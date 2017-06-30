Git Common-Flow 1.0.0-draft.1
=============================

Summary
-------

Common-Flow is an attempt to gather a sensible selection of the most common
usage patterns of git out in the wild into a single and concise
specification. It is based on
the [original variant](http://scottchacon.com/2011/08/31/github-flow.html)
of [GitHub Flow](https://guides.github.com/introduction/flow/), while taking
into account how a lot of open source projects use git.

Core rules:

- The `master` branch should always be deployable / usable.
- New work must be done on a descriptively named change branch created off of
  `master`.
- Commit to the change branch locally, and regularly push your work to the same
  named branch on the remote server.
- When you need feedback, help, or think the branch is ready for merging, open a
  pull request.
- After someone else has reviewed and signed off on the change, you can merge it
  in to `master`.
- New releases are created by committing a version bump commit directly to
  `master`, and then tagging that commit with the version.

Branch types:

- Master Branch - Should always be deployable / usable, is considered "bleeding
  edge", and must be named `master`.
- Change Branches - Any branch that introduces changes (new feature, bug fix,
  etc), should be created off of `master`, and must have a descriptive name.
- Maintenance Branches - Used to maintain old versions, and should follow a
  `stable-X.Y` naming pattern, where `X` is MAJOR version and `Y` is MINOR
  version.

