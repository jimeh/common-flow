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

Branch types:

- Master Branch - Should always be deployable / usable, is considered "bleeding
  edge", and must be named `master`.
- Change Branches - Any branch that introduces changes (new feature, bug fix,
  etc), should be created off of `master`, and must have a descriptive name.
- Maintenance Branches - Used to maintain old versions, and should follow a
  `stable-X.Y` naming pattern, where `X` is MAJOR version and `Y` is MINOR
  version.

Rules:

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

Git Common-Flow Specification (Common-Flow)
-------------------------------------------

The key words "MUST", "MUST NOT", "REQUIRED", "SHALL", "SHALL NOT", "SHOULD",
"SHOULD NOT", "RECOMMENDED", "MAY", and "OPTIONAL" in this document are to be
interpreted as described in [RFC 2119](https://tools.ietf.org/html/rfc2119).

1. A branch called "master" MUST exist, and it SHOULD be referred to as the
   "master branch". The master branch MUST always be in a non-broken state,
   meaning it MUST always be in a good enough state that, depending on your
   deployment/release flow, a new release can be built from master, or that
   master can safely be deployed to production.
2. Changes MUST be performed on a separate branch that SHOULD be referred to as
   a "change branch". All change branches MUST have descriptive names. You
   SHOULD commit locally often, and you MUST regularly push your work to the
   same named branch on the remote server.
