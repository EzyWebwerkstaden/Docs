### Code reviews and checkins

To help ensure that only the highest quality code makes its way into the project, please submit all your code changes to GitHub as PRs. This includes runtime code changes, unit test updates. For example, sending a PR for just an update to a unit test might seem like a waste of time but the unit tests are just as important as the product code and as such, reviewing changes to them is also just as important.

The advantages are numerous: improving code quality, more visibility on changes and their potential impact, avoiding duplication of effort, and creating general awareness of progress being made in various areas.

In general a PR should be signed off (using the :shipit: `:shipit:` emoticon) by any developer. As long it's not your self signing it off.

After a PR is merged it should be deleted.

### Branch strategy

* Using the [Github workflow] (https://guides.github.com/introduction/flow/)

**In general:**
Use feature-branches as much as possible.  
The pattern is `{epic or type of branch (fix,feature or refactor)}/{Jira item number}/{title}`  

* _{epic or type of branch (fix,feature or refactor}_ prefixing a branch makes it easier to search and filter among branches.  
* _{Jira item number}_ makes the branch and PR be integrated with Jira.  
* _{title}_ use short title as possible, whitespace should be replaced with a dash.  

**Branch explanation**  
* `paymentflow/fa-313/xsl-embedded-resource` with epic
* `feature/fa-338/free-priority-boarding` with type of branch
* `master` has the code for the latest release to staging.
* `dev` has the code that is being worked on but not yet released. This is the branch into which devs normally submit pull requests and merge changes into.

### Commit Message

An commit message should always contain. ITEM-X [TAG] **message**.

For the commit **message** we have the following guidlines.
- a shorter description of what is done
- A summary of the intent of the code (Describe WHY and WHAT in a clear language)

Also a good practies if your using the shell. Associate your favorite text editors with Git. Cause then if you don't use  `-m`
You will get up a blank file where you can easly structure your commit message in your editor of choose. https://help.github.com/articles/associating-text-editors-with-git/


We are using the following [TAG]'s.

- 🐛 `:bug:` For an bugfix
- ✨ `:sparkles:` for something new
- 🏭 `:factory:` for refactoring
- 💄 `:lipstick:` for visual changes
- 🚦 `:traffic_light:` when just editing tests
- 🚧 `:construction:` when testing temporary changes
- ⛓ `:chains:` when only updating submodule
- ⚙ `:gear:` When changing a config file or setting
- 💻 `:computer:` Automatic changes made by IDE settings or a script 
- 🔥 `:fire:` Something hotfixed (pushed directly to master for example)
- 🔈  ´:speaker:´ when adding logging

So en example commit should look like this:
- `PRJ-1 :bug: Made changes to web.config so it's correct`
- `PRJ-1 :chains: Updated ezyRadixxBase`
- `PRJ-1 :gear: Made changes to web.config in order to test SSR code`

Here is a good real-world example.  
![Realworld](https://cloud.githubusercontent.com/assets/2648767/13316486/96c2d7ec-dbb0-11e5-9017-af5b16845e09.png)  



