# WIP

Whenever a new commit is pushed into the `main` branch, either via a merge or directly, the release
creation workflow will be executed. The workflow will then tag the new commit and create a new
release.

It won't be executed for every commit in a `push`, you can for example merge a PR into the `main`
branch which contains 10+ commits and only one release will be created for all of them.

The workflow will increment the `patch` version by default if none of the commit messages being pushed
into the `main` branch contains version specific annotations.

Supported annotations are:
* `[release-manual]` - will skip the tagging and release creation, the release must then be created
manually. This overrides any other `[release-*]` annotations if they're present in a multi-commit
merge.
* `[release-patch]` - this is the default behaviour and does not need to be specified in a commit,
it will increment the `patch` version number, e.g. `vX.Y.Z` becomes `vX.Y.Z+1`.
* `[release-minor]` - this will increment the `minor` version number, e.g. `vX.Y.Z` becomes `vX.Y+1.0`.
It will override any `[release-patch]` annotations if they're present in a multi-commit merge.
* `[release-major]` - this will increment the `major` version number, e.g. `vX.Y.Z` becomes `vX+1.0.0`.
It will override any `[release-patch]` or `[release-minor]` annotations if they're present in a
multi-commit merge.

The current workflow has some issues if multiple people merge changes at the same time. The workflows
are started concurrently so they can fail due to race conditions on obtaining the previous release
tag. Can be addressed via a new GitHub Action which implement a queueing system. Should not be a
problem initially though.
