### Code reviews and checkins

To help ensure that only the highest quality code makes its way into the project, please submit all your code changes to GitHub as PRs. This includes runtime code changes, unit test updates, and updates to official samples (e.g. Music Store and Bug Tracker). For example, sending a PR for just an update to a unit test might seem like a waste of time but the unit tests are just as important as the product code and as such, reviewing changes to them is also just as important.

The advantages are numerous: improving code quality, more visibility on changes and their potential impact, avoiding duplication of effort, and creating general awareness of progress being made in various areas.

In general a PR should be signed off (using the :shipit: `:shipit:` emoticon) by the Subject Matter Expert (SME) of that code. For example, a change to the Banana project should be signed off by `@MrMonkey`, and not by `@MrsGiraffe`. If you don't know the SME, please talk to one of the engineering leads and they will be happy to help you identify the SME. Of course, sometimes it's the SME who is making a change, in which case a secondary person will have to sign off on the change (e.g. `@JuniorMonkey`).

To commit the PR to the repo **do not use the Big Green Button**. Instead, do a typical push that you would use with Git (e.g. local pull, rebase, merge, push).

### Branch strategy

In general:

* `master` has the code for the latest release to staging.
* `dev` has the code that is being worked on but not yet released. This is the branch into which devs normally submit pull requests and merge changes into.