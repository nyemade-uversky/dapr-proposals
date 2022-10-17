## What is this?

This proposal is to formalize the structure and lifecycle to proposals with two primary goals: first, make it easier for contributors to both put forth proposals as well as review them and, second, increase the clarity and focus of proposals themselves. In order to do this, I propose that we make two changes to the way proposals are made:

1. Break up the proposal process into two discrete phases 
2. Create a template for each of these two phases

## Proposal Phases: Design vs Build

This proposal suggests that we break the proposal process down into two separate phases - the design phase, and the build phase. The design phase is where an idea is put forth regarding new functionality or changes to Dapr; the goal of this step is for the proposal to express to the community the "what" and "why" of the proposal. The second phase is intended to focus on the "how" and the "when" (and possibly even the "who") of the idea. The intent here is to make it easier to not only introduce new ideas but to evaluate them as well by providing a small amount of structure and reducing the scope of the proposal. And, for each of these phases, we will create a template to keep proposals consistent. 

### Design Phase

"Your scientists were so preoccupied with whether they could, they didn’t stop to think if they should." - Dr. Ian Malcolm (Jurassic Park)

The intent of the design phase is to focus on two things: the idea that is being proposed, and why the idea is being proposed. In this step, a proposal would provide both the background on the idea: what it is and how it works (at a high level) as well as  why it should be introduced to Dapr. The why part of this proposal is as important (or more so) than what is being proposed, as it lets the reviewers understand better what kinds of use cases that the feature will enable, how it will make Dapr better or improve the experience of Dapr users. In addition, it needs to also convey what is in scope and what is out of scope (i.e what things have been deliberately omitted) along with any alternatives that have been considered and why they were not a good fit.

### Build Phase

The second phase of the process focuses on the implementation of the proposal - the goal of this part is to show the community not only how it will operate, but also provide information on how success will be measured and also include a list of activities that need to be completed in order for this proposal to be complete. This is where the technical implementation of the new feature will be discussed with the intention that, because the what and why of the idea have been evaluated ahead of time, the discussion can focus on the technical details of the proposal. 

### Templates

The templates for this will include the following structure:

#### Design Template

1. What problem is being addressed in this proposal?
2. What is being proposed to address this problem?
3. What is in scope for this proposal?
4. What is deliberately *not* in scope? 
5. What alternatives have been considered?


#### Build Template

1. How will this work, technically?
  * Design documents
  * System diagrams
  * Code examples
2. How will success be measured?
  * Performance targets
  * Compabitility
3. Checklist of things that will need to be done before this proposal is complete
  * Code changes
  * Tests added (e2e, unit)
  * SDK changes (if needed)
  * Documentation

## Why do we want to do this?
 
As a community project, Dapr relies on contributors to help advance the project the goal of this proposal is to simplify the process of contributing, and evaluating, new ideas. We want to make it more inviting for community members to propose new ideas (or evaluate them) as well as ensure that the time being spent evaluating proposals or working on new features is well spent.


Adding clarity to the proposal process, as well as some amount of structure, will hopefully make it easier for contributors of all experience levels to both contribute and review new ideas. Splitting the proposal process into two phases, making each step more focused and smaller in scope, would benefit the community by making the process more approachable for new contributors who have ideas to share and also improve the process for those who are reviewing them because it would ensure that a proposal meets a certain set of criteria.

As a new contributor it can sometimes feel a bit daunting to propose a new idea -- not knowing quite where to start, whether or not what you are proposing has the right level of information, etc. Having structure makes it easier for a new contributor who wants to propose a new idea to know what is expected and feel confident that their proposal meets those expectations. In addition, for anyone putting forward a proposal, the structure proposed prompts thinking about how someone else would use the feature, how they might benefit from it, and what other ways the feature they are proposing might be solved using existing features (or other technology).  

On the other side of the equation are the community members who are reviewing those proposals; it can be challenging to review something if you feel that information or context is lacking. A consistent structure means that reviewers can know what to expect out of a proposal document and clearly ask for more information if some is missing. And, the suggested structure would make sure that reviewers have the right information needed to have a conversation about the proposal (as well as reduce the scope of the review). 

As a community, we want to be welcoming of new people and also respectful of the time and energy that everyone devotes to make this project great. I believe that adding this small amount of structure to the proposal process will help not only make it easier to propose new ideas, but also ensure that everyone who is participating can make the best use of the time they have available to improve Dapr!





















































