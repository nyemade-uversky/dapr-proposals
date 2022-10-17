### What is this proposing?

Currently, daprd emits a number of metrics mainly around the internals of the process (memory usage, goroutines, etc.) as well as some operational data like sidecar injection counters. This proposal suggests that we improve this by making it easier for developers to add metrics to almost anything with minimal effort. This would be done with a central convenience wrapper / registry that is available to any service (daprd, placement service, etc.) and can be used anywhere in the Dapr codebase to easily record, and report, application-level metrics in order to increase observability of Dapr and its various components.  

### Why do we need it?

"Metrics, Metrics, Everywhere!" - Coda Hale 

In order for Dapr to improve its performance we need to have a baseline for existing performance characteristics. For example, how do we know whether a recent change in the Dapr codebase has not had a negative impact on the performance of service invocation? Currently this is not a part of Dapr's core behavior but if we were to export timing metrics around service invocations we could observe whether or not the performance has been affected as compared to existing baseline data. 

> If you don't measure it, you can't fix it. 

In addition to being able to identify performance regressions, it will also help us to find areas where we can improve performance and demonstrate to ourselves, as well as users, that these changes have actually improved performance. 

## Technical Design

### How will it work?

Fortunately, this is a relatively straightforward operation; Dapr already supports exporting internal runtime metrics to Prometheus using an exporter so it already has a metrics exporting endpoint available to Prometheus along with OpenCensus monitoring data. The primary change will be that we add some convenience to this so that Dapr developers don't need to do quite as much work. 

With this change in place, developers working on Dapr can add new metrics to anything they want to observe with very minimal overhead. Programmatically it would look something like:


```go

import "github.com/dapr/dapr/metrics"

func doActualWork() {

	c := metrics.GetCounter("happy-people")
	c.Inc(1)
	
}
```


Inside of the `dapr/metrics` package, there will be wrappers around the underlying metrics library to take care of some internal bookkeeping and configuration, tagging, etc. to export a consistent interface for metrics. For example, automatically adding the `app-id` to the tags for the metrics initially, but  adding any other metadata we deem valuable over time. A simple example of how this would be implemented would look something like the following rough outline code: 

```go 

package metrics 

import (
	"go.opencensus.io/stats"
	"go.opencensus.io/stats/view"
	"go.opencensus.io/tag"
	diagUtils "github.com/dapr/dapr/pkg/diagnostics/utils"

)

var tags = nil // set base tags here 
var ctx = // ... 

type Counter struct {
	underlying *stats.Int64Measure
}

func (c *Counter) Inc(val int) {
	stats.RecordWithTags(ctx, diagUtils.WithTags(tags...), c.underlying.M(val))
}

func Init(opts *MetricsOpts) {
	// Setup reporter, add default tags, context, etc.

}

func GetCounter(name string, description string) Counter {
	return Counter { underlying: stats.Int64(
		name,
		description,
		stats.UnitDimensionless) }
}
```

### Performance / compatibility 

As this is a new feature, the only compatibility that we will need is to be able to use the existing metrics configuration (prometheus exporter / port / flag) to avoid adding new configuration options for the user. 

Performance is a little tricky with this one because, _Quis custodiet ipsos custodes?_ ("Who watches the watchers?") so we will have to rely a bit on the upstream implementation to be performant (and possibly measure this ourselves via unit tests) 

### What needs to be done?

- [ ] Implement metrics registry in Dapr with consistent metadata present (i.e app-id, etc.) 
- [ ] Create stable metrics interfaces inside Dapr (lightweight wrappers)
- [ ] Add metrics-exporting information to developer notes / docs (i.e naming conventions, when to use gauges, counters, timers, etc.)
