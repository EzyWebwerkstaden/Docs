### Code reviews and checkins

To help ensure that only the highest quality code makes its way into the project, please submit all your code changes to GitHub as PRs. This includes runtime code changes, unit test updates. For example, sending a PR for just an update to a unit test might seem like a waste of time but the unit tests are just as important as the product code and as such, reviewing changes to them is also just as important.

The advantages are numerous: improving code quality, more visibility on changes and their potential impact, avoiding duplication of effort, and creating general awareness of progress being made in various areas.

In general a PR should be signed off (using the :shipit: `:shipit:` emoticon) by any developer. As long it's not your self signing it off.

### Branch strategy

In general:

* `master` has the code for the latest release to staging.
* `dev` has the code that is being worked on but not yet released. This is the branch into which devs normally submit pull requests and merge changes into.

### Commit Message

When doing an commit ther should always be an TAG and if there is an item associated to the commit it should also be added.

- :bug: `:bug:` For an bugfix
- :gift: `:gift:` for something new
- :wrench: `:wrench:` for refactoring
- :construction: `:construction:` when just editing tests
- :recycle: `:recycle:` when testing temporary changes
- :feet: `:feet:` when only updating submodule
- :memo: `:memo:` When changing a config file or setting

So en example commit should look like this:
- `PRJ-1 :bug: Made changes to web.config so it's correct`
- `PRJ-1 :feet: Updated ezyRadixxBase`
- `PRJ-1 :recycle: Made changes to web.config in order to test SSR code




