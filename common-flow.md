git common-flow 1.0.0-draft.1
=============================

Summary
-------

Common-Flow is an attempt to gather the most common usage patterns of git out in
the wild into a single and concise specification. Based on
the [original variant](http://scottchacon.com/2011/08/31/github-flow.html)
of [GitHub Flow](https://guides.github.com/introduction/flow/), while taking
into account how a lot of open source projects use git.

- The `master` branch should always be deployable / usable.
- New work must be done on a descriptively named change branch created off of
  `master`.
- Commit to the change branch locally, and regularly push your work to the same
  named branch on the remote server.
- When you need feedback, help, or think the branch is ready for merging, open a
  pull request.
- After someone else has reviewed and signed off on the change, you can merge
  it in to `master`.
- New releases are created by committing a version bump commit directly to
  `master`, and then tagging that commit with the version.

### Branch Types

- **Master Branch:**
    - Should always be deployable / usable
    - Is considered "bleeding edge"
    - Must be named `master`
- **Change Branches:**
    - Any branch that introduces changes (new feature, bug fix, etc)
    - Should be cut from `master` (in most cases)
    - Must have a descriptive name
- **Maintenance Branches:**
    - Used to maintain old versions (back-porting security patches, etc)
    - Should follow a `stable-X.Y` naming pattern, where `X` is MAJOR version
      and `Y` is MINOR version
