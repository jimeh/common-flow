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

Basics:

- The "master" branch should always be deployable/usable, while also considered
  to be bleeding edge.
- New work must be done on a descriptively named change branch created off of
  the master branch.
- Commit to the change branch locally, and regularly push your work to the same
  named branch on the remote server.
- The change branch MUST be kept up to date with the master branch by rebasing
  it on top of the remote master branch.
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
   MUST be considered to be bleeding edge. That means the master branch MUST
   always be in a good enough state that a new release can always be built from
   master, or that master can always be safely deployed to production.
2. Changes MUST be performed on a separate branch that SHOULD be referred to as
   a "change branch". All change branches SHOULD be created off of the master
   branch, and they MUST have descriptive names. You SHOULD commit often
   locally, and you SHOULD regularly push your work to the same named branch on
   the remote server.
3. Change branches MUST be regularly updated with any changes on the master
   branch. This MUST be done by rebasing the change branch on top of the remote
   master branch ("git rebase origin/master"). To be clear you MUST NOT merge
   the master branch into a change branch.
4. To merge a change branch back in to the master branch, you MUST open a "pull
   request" (or equivalent) so others can review and approve your changes. A
   change branch MUST only be merged in to the master branch when you and others
   are certain that it is ready for release and/or deployment to production.
5. To get feedback, help, or generally just discuss a change branch with others,
   you SHOULD also create a pull request and discuss the changes with others
   there.
6. The project MUST have its version hard-coded somewhere in the code-base. It
   is RECOMMENDED that this is done in a file called "VERSION" located in the
   root of the project, and that it only contains the exact version string.
7. The version string SHOULD follow the Semantic Versioning (http://semver.org/)
   format. Use of Semantic Versioning is OPTIONAL, but the version string MUST
   NOT have a "v" prefix. For example "v2.11.4" is bad, and "2.11.4" is good.
8. To create a new release, you MUST create a "version bump" commit directly on
   the master branch which changes the hard-coded version value of the
   project. The version bump commit MUST have a lightweight git tag created on
   it and named as the exact version string.
9. A version bump commit MUST have a commit message title of "Bump version to
   VERSION". For example, if the new version string is "2.11.4", the first line
   of the commit message MUST read: "Bump version to 2.11.4"
10. The release tag on the version bump commit MUST be named exactly the same as
    the version string. And its name MUST NOT have a "v" prefix.
11. OPTIONALLY you can create the release tag as a annotated tag where the
    annotation message is the changelog since the last release.
12. If a change branch which has been merged in to master is found to have a bug
    in it, the bug fix work MUST be done as a new separate change branch and
    MUST follow the same workflow as any other change branch.
13. If a change branch is wrongfully merged in to master, or for any other
    reason the merge must be undone, you MUST undo the merge by reverting the
    merge commit self. Effectively creating a new commit that reverses all the
    relevant changes. The master branch MUST NOT under any circumstance be
    rewound to a earlier commit.
14. Maintenance branches are used for managing new releases for older
    versions. Typically this is used to provide security updates for older
    versions when the master branch has moved on. A maintenance branch SHOULD
    follow a "stable-X.Y" naming pattern, where "X" is the MAJOR version and "Y"
    is the minor version.
15. Updating a maintenance branch is done by cherry picking the relevant commits
    if possible. If not possible it MUST be done via a change branch created off
    of the maintenance branch, and reviewed through a pull request like any
    other change request.
16. Creating a release from a maintenance branch is done the same way as a
    normal release, except everything is done on the maintenance branch instead
    of the master branch.

About
-----

The Git Common-Flow specification is authored
by [Jim Myhrberg](http://jimeh.me).

If you'd like to leave feedback,
please [open an issue on GitHub](https://github.com/jimeh/common-flow/issues).

License
-------

[Creative Commons - CC BY 3.0](http://creativecommons.org/licenses/by/3.0/)
