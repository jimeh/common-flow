Git Common-Flow 1.0.0-draft.0
=============================

Summary
-------

Common-Flow is an attempt to gather a sensible selection of the most common
usage patterns of git out in the wild into a single and concise
specification. It is based on
the [original variant](http://scottchacon.com/2011/08/31/github-flow.html)
of [GitHub Flow](https://guides.github.com/introduction/flow/), while taking
into account how a lot of open source projects use git.

Terminology:

- Master Branch - Should always be deployable/usable, is considered bleeding
  edge, and must be named `master`.
- Change Branches - Any branch that introduces changes (new feature, bug fix,
  etc), should be created off of the master branch, and must have a descriptive
  name.
- Maintenance Branches - Used to maintain old versions, and should follow a
  `stable-X.Y` naming pattern, where `X` is MAJOR version and `Y` is MINOR
  version.
- Pull Request - A means of requesting that a change branch is merged in to the
  master branch, allowing others to review, discuss and approve the changes.
- Release - Consists of a version bump commit directly on the master branch, and
  a git tag named according to the new version number placed on said commit.

Basic Requirements:

- The "master" branch should always be deployable/usable, while also
  considered to be bleeding edge.
- New work must be done on a descriptively named change branch created off of
  the master branch.
- Commit to the change branch locally, and regularly push your work to the same
  named branch on the remote server.
- When you need feedback, help, or think the branch is ready for merging, open a
  pull request.
- After someone else has reviewed and signed off on the change, you can merge it
  in to the master branch.
- New releases are created by committing a version bump commit directly to the
  master branch, and then tagging that commit with the version.
- Maintenance branches are updated by manually merging and/or back-porting
  relevant change branches in to them.

Git Common-Flow Specification (Common-Flow)
-------------------------------------------

The key words "MUST", "MUST NOT", "REQUIRED", "SHALL", "SHALL NOT", "SHOULD",
"SHOULD NOT", "RECOMMENDED", "MAY", and "OPTIONAL" in this document are to be
interpreted as described in [RFC 2119](https://tools.ietf.org/html/rfc2119).

1. A branch named "master" MUST exist, and it SHOULD be referred to as the
   "master branch". The master branch MUST always be in a non-broken state, but
   MUST be considered to be "bleeding edge". That means the master branch MUST
   always be in a good enough state that a new release can always be built from
   master, or that master can always be safely deployed to production.
2. Changes MUST be performed on a separate branch that SHOULD be referred to as
   a "change branch". All change branches SHOULD be created off of the master
   branch, and they MUST have descriptive names. You SHOULD commit often
   locally, and you SHOULD regularly push your work to the same named branch on
   the remote server.
3. To merge a change branch back in to the master branch, you MUST open a Pull
   Request (or equivalent) so others can review and approve your changes. A
   change branch MUST only be merged in to master when you and others are
   certain that it is ready for release and/or deployment to production.
4. To get feedback, help, or generally just discuss a change branch with others,
   you SHOULD create a pull request (or equivalent).
5. The project MUST have it's version hard-coded somewhere in project. It is
   RECOMMENDED that this is done in a "VERSION" file in the root of the project,
   which only contains the exact version string.
6. The version string SHOULD follow the Semantic Versioning (http://semver.org/)
   format. Use of Semantic Versioning is OPTIONAL, but the version string MUST
   NOT have a "v" prefix. For example "v2.11.4" is bad, and "2.11.4" is good.
7. To create a new release, you MUST create a "version bump" commit directly on
   the master branch which changes the hard-coded version value of the
   project. The version bump commit MUST then have a git tag created on it named
   as the exact version string.
8. A version bump commit MUST have a commit message title of "Bump version to
   <VERSION>". For example, if the new version string is "2.11.4", the first
   line of the commit message MUST read "Bump version to 2.11.4".
9. The release tag on the version bump commit MUST be named exactly the same as
   the version string. The release tag name MUST NOT have a "v" prefix.
