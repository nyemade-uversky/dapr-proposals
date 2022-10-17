### What does this propose?

This proposal suggests that we create and maintain a repository specifically for proposals related to Dapr (ie github.com/dapr/proposals) This follows the model that a number of other projects use in order to keep proposals and their lifecycle separate from the day-to-day issues and pull requests that the project, and its related repositories, see. 

### Why should we do this?

Doing this would have two primary benefits (feel free to suggest others):

1. Keeps the lifecycle of proposals separate from daily codebase operations (pull requests, issues, etc.)
2. Provides a centralized, predictable, place for proposals related to any part of the Dapr ecosystem (SDKs, components, core services, etc.) 

Keeping proposal centralized means that they are much more discoverable and also easier to follow in that they provide a clear repository for historical design choices and decision. This is also beneficial when looking to add a new feature proposal or other design because it makes it easier to see previous conversations and examples all in one central place. It also means that the proposals themselves will be Markdown files and, as such, any changes to the proposals over time can be viewed as well. 


### How will we implement this?

Create a new repository (`dapr/proposals`) and begin to store new proposals there as Markdown files. New proposals will come in the form of pull requests to the repository, and once they are accepted / completed the PR will be merged and the proposal / design will be available in the repository. 


### What needs to be done?

[ ] Create a new repository
[ ] Create a README.md in the new repository outlining the process for creating a new proposal with pointers to examples, templates, etc. 
[ ] Move any in-flight proposals to PRs against this new repository
[ ] (Optional) move previous proposals to this repository as Markdown files
[ ] Update any documentation to point to this new repository
