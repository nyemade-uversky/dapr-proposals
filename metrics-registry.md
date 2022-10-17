### What is this proposing?

Currently, daprd emits a small number of metrics, mainly around the internals of the process (memory usage, goroutines, etc.). This proposal suggests that we add a central registry to Dapr components (daprd, placement service, etc.) that can be used anywhere in the Dapr codebase to record, and report, application-level metrics in order to increase observability of Dapr and its various components.  

### Why do we need it?

"Metrics, Metrics, Everywhere!" - Coda Hale 

In order for Dapr to improve its performance we need to have a baseline for existing performance characteristics. For example, how do we know whether a recent change in the Dapr codebase has not had a negative impact on the performance of service invocation? Currently this is not a part of Dapr's core behavior but if we were to export timing metrics around service invocations we could observe whether or not the performance has been affected as compared to existing baseline data. 

> If you don't measure it, you can't fix it. 

In addition to being able to identify performance regressions, it will also help us to find areas where we can improve performance and demonstrate to ourselves, as well as users, that these changes have actually improved performance. 

## Technical Design

### How will it work?

Fortunately, this is a relatively straightforward operation; Dapr already supports exporting internal runtime metrics to Prometheus using an exporter so it already has a metrics exporting endpoint available to Prometheus. We can leverage that, combined with a metrics library (Uber's `tally` library, in this case -- https://github.com/uber-go/tally), to create a central application-level metrics registry where new metrics instruments (counters, timers, gauges, etc.) can be created and automatically made available to the existing prometheus export system. 

With this change in place, developers working on Dapr can add new metrics to anything they want to observe with very minimal overhead. Programmatically it would look something like:


```go

import "github.com/dapr/dapr/metrics"

func doActualWork() {

	c := metrics.GetCounter("happy-people")
	c.Inc(1)
	
}
```


Inside of the `dapr/metrics` package, there will be wrappers around the underlying metrics library to take care of some internal bookkeeping and configuration, tagging, etc. to export a consistent interface for metrics. For example, automatically adding the `app-id` to the tags for the metrics initially, but  adding any other metadata we deem valuable over time. A simple example of how this would be implemented would look something like the following: 

```go 

package metrics 

import (
  "io"
  "github.com/uber-go/tally"
)

var metricsScope tally.Scope
var metricsCloser io.Closer 

type Counter struct {
	underlying tally.Counter
}

func (c *Counter) Inc(val int) {
	c.underlying.Inc()
}

func Init(opts *MetricsOpts) {
	tags := opt.Tags
	tags["app-id"] = opts.AppId
	
	reporter := // use existing prometheus config to create prometheus reporter
	metricsScope, metricsCloser := tally.NewRootScope(tally.ScopeOptions{
		Prefix:   opts.Prefix, // `daprd` for example
		Tags:     tags,
		Reporter: reporter,
	}, opts.Interval)

}

func GetCounter(name string) Counter {
	return Counter { underlying: metricsScope.Counter(name) }
}
```

### Performance / compatibility 

As this is a new feature, the only compatibility that we will need is to be able to use the existing metrics configuration (prometheus exporter / port / flag) to avoid adding new configuration options for the user. 

Performance is a little tricky with this one because, _Quis custodiet ipsos custodes?_ ("Who watches the watchers?"). Fortunately the performance part is largely taken care of by Uber's metrics benchmarks -- taken from their GitHub readme, the metrics invocations are measured on the scale of ns/op so they are "more-or-less" free (in comparison to most other work we are doing already anyhow). 

```
BenchmarkCounterInc-8               	200000000	         7.68 ns/op
BenchmarkReportCounterNoData-8      	300000000	         4.88 ns/op
BenchmarkReportCounterWithData-8    	100000000	         21.6 ns/op
BenchmarkGaugeSet-8                 	100000000	         16.0 ns/op
BenchmarkReportGaugeNoData-8        	100000000	         10.4 ns/op
BenchmarkReportGaugeWithData-8        	 50000000	         27.6 ns/op
BenchmarkTimerInterval-8            	 50000000	         37.7 ns/op
BenchmarkTimerReport-8              	300000000	         5.69 ns/op
```


### What needs to be done?

- [ ] Implement metrics registry in Dapr 
- [ ] Create stable metrics interfaces inside Dapr (lightweight wrappers)
- [ ] Wire metrics registry to existing Prometheus reporter
- [ ] Add metrics-consumption information to docs (i.e how does an end-user consume the metrics that Dapr exports)
- [ ] Add metrics-exporting information to developer notes / docs (i.e naming conventions, when to use gauges, counters, timers, etc.)
