Git Common-Flow 1.0.0-draft.8
=============================

Summary
-------

Common-Flow is an attempt to gather a sensible selection of the most common
usage patterns of git into a single and concise specification. It is based on
the [original variant](http://scottchacon.com/2011/08/31/github-flow.html)
of [GitHub Flow](https://guides.github.com/introduction/flow/), while taking
into account how a lot of open source projects use git.

TL;DR: Common-Flow is basically GitHub Flow with the addition of versioned
releases, maintenance releases for old versions, and without the requirement to
deploy to production all the time.

Terminology
-----------

- **Master Branch** - Must always have passing tests, is considered bleeding
  edge, and must be named `master`.
- **Change Branches** - Any branch that introduces changes like a new feature, a
  bug fix, etc.
- **Source Branch** - The branch that a change branch was created from. New
  changes in the source branch should be incorporated into the change branch via
  rebasing.
- **Merge Target** - A branch that is the intended merge target for a change
  branch. Typically the merge target branch will be the same as the source
  branch.
- **Maintenance Branches** - Used for maintaining old versions and releasing
  PATCH updates when the master branch has moved on. Should follow a
  `stable-X.Y` naming pattern, where `X` is MAJOR version and `Y` is MINOR
  version.
- **Pull Request** - A means of requesting that a change branch is merged in to
  its merge target, allowing others to review, discuss and approve the changes.
- **Release** - Consists of a version bump commit directly on the master branch,
  and a git tag named according to the new version string placed on said commit.
- **Maintenance Release** - Just like a regular release, except the version bump
  commit and release tag are on a maintenance branch instead of the master
  branch.

Git Common-Flow Specification (Common-Flow)
-------------------------------------------

The key words "MUST", "MUST NOT", "REQUIRED", "SHALL", "SHALL NOT", "SHOULD",
"SHOULD NOT", "RECOMMENDED", "MAY", and "OPTIONAL" in this document are to be
interpreted as described in [RFC 2119](https://tools.ietf.org/html/rfc2119).

1. The Master Branch
    1. A branch named "master" MUST exist and it MUST be referred to as the
       "master branch".
    2. The master branch MUST be considered bleeding edge.
    3. The master branch MUST always be in a non-broken state with its test
       suite passing.
    4. The master branch SHOULD always be in a "as near as possible ready for
       release/production" state to reduce the friction of creating a new
       release.
2. Changes
    1. Changes MUST be performed on a separate branch that SHOULD be referred to
       as a "change branch". All change branches MUST have descriptive names. It
       is RECOMMENDED that you commit often locally, and you SHOULD regularly
       push your work to the same named branch on the remote server.
    2. When a change branch is created, the branch that it is created from
       SHOULD be referred to as the "source branch". Each change branch also
       needs a designated "merge target branch", typically this will be the same
       as the source branch.
    3. Change branches MUST be regularly updated with any changes from their
       source branch. This MUST be done by rebasing the change branch on top of
       the source branch. To be clear you MUST NOT merge a source branch into a
       change branch.
    4. After rebasing a change branch on top of its source branch you MUST push
       the change branch to the remote server. This will require you do a force
       push, and you SHOULD use the "--force-with-lease" git push option.
    5. To merge a change branch into its merge target branch, you MUST open a
       "pull request" (or equivalent) so others can review and approve your
       changes.
    6. A pull request MUST only be merged when the change branch is up-to-date
       with its source branch, the test suite is passing, and you and others are
       happy with the change. This is especially important if the merge target
       is the master branch.
    7. To get feedback, help, or generally just discuss a change branch with
       others, it is RECOMMENDED you do this by creating a pull request and
       discuss the changes with others there.
3. Git Best Practices
    1. All commit messages SHOULD follow the Commit Guidelines and format from
       the official git documentation:
       https://git-scm.com/book/en/v2/Distributed-Git-Contributing-to-a-Project
    2. You SHOULD always use "--force-with-lease" when doing a force push. The
       plain "--force" option is dangerous and destructive. More information:
       https://developer.atlassian.com/blog/2015/04/force-with-lease/
    3. You SHOULD understand and be comfortable with rebasing:
       https://git-scm.com/book/en/v2/Git-Branching-Rebasing
    4. It is RECOMMENDED that you always do "git pull --rebase" instead of "git
       pull" to avoid unnecessary merge commits. You can make this the default
       behavior of "git pull" with "git config --global pull.rebase true".
    5. It is RECOMMENDED that all branches be merged using "git merge --no-ff".
       This makes sure the reference to the original branch is kept in the commits,
       allows one to revert a merge by reverting a single merge commit, and creates
       a merge commit to mark the integration of the branch with master.
4. Versioning
    1. The project MUST have its version hard-coded somewhere in the
       code-base. It is RECOMMENDED that this is done in a file called "VERSION"
       located in the root of the project.
    2. If you are using a "VERSION" file in the root of the project, this MUST
       only contain the exact version string.
    3. The version string SHOULD follow the Semantic Versioning
       (http://semver.org/) format. Use of Semantic Versioning is OPTIONAL, but
       the version string MUST NOT have a "v" prefix. For example "v2.11.4" is
       bad, and "2.11.4" is good.
5. Releases
    1. To create a new release, you MUST create a "version bump" commit directly
       on the master branch which changes the hard-coded version value of the
       project. The version bump commit MUST have a git tag created on it and
       named as the exact version string.
    2. A version bump commit MUST have a commit message title of "Bump version
       to VERSION". For example, if the new version string is "2.11.4", the
       first line of the commit message MUST read: "Bump version to 2.11.4"
    3. The release tag on the version bump commit MUST be named exactly the same
       as the version string. The tag name can OPTIONALLY be prefixed with
       "v". For example the tag name can be either "2.11.4" or "v2.11.4".
    4. It is RECOMMENDED that release tags are lightweight tags, but you can
       OPTIONALLY use annotated tags if you want to include changelog
       information in the release tag itself.
    5. If you use annotated release tags, the first line of the annotation MUST
       read "Release VERSION". For example for version "2.11.4" the first line
       of the tag annotation would read "Release 2.11.4". The second line must
       be blank, and the changelog MUST start on the third line.
6. Bug Fixes & Rollback
    1. You MUST NOT under any circumstances force push to the master branch.
    2. If a change branch which has been merged in to the master branch is found
       to have a bug in it, the bug fix work MUST be done as a new separate
       change branch and MUST follow the same workflow as any other change
       branch.
    3. If a change branch is wrongfully merged in to master, or for any other
       reason the merge must be undone, you MUST undo the merge by reverting the
       merge commit itself. Effectively creating a new commit that reverses all
       the relevant changes.
7. Maintenance Releases
    1. Any branch that has a name starting with "stable-" SHOULD be referred to
       as a "maintenance branch".
    2. Maintenance branches are used for managing new releases of older
       versions. Typically this is used to provide security updates for older
       versions when the master branch has moved on to a point that a new
       release for the old version cannot be made from the master branch.
    3. A "maintenance release" is identical to a regular release, except the
       version bump commit and the release tag are placed on the maintenance
       branch instead of on the master branch.
    3. A maintenance branch SHOULD follow a "stable-X.Y" naming pattern, where
       "X" is the MAJOR version and "Y" is the minor version.
    4. A maintenance branch MUST be created from the relevant release tag. For
       example if there is a security fix for all 2.9.x releases, the latest of
       which is "2.9.7", we create a new branch called "stable-2.9" off of the
       "2.9.7" release tag. The security fix release will then end up being
       version "2.9.8".
    5. When working on a maintenance release, the relevant maintenance branch
       MUST be thought of as the master branch for that maintenance work.
    6. Changes in a maintenance branch SHOULD typically come from work being
       done against the master branch. Meaning changes SHOULD only trickle
       downwards from the master branch. If a change needs to trickle back up
       into the master branch, that work should have happened against the master
       branch in the first place.

About
-----

The Git Common-Flow specification is authored
by [Jim Myhrberg](http://jimeh.me).

If you'd like to leave feedback,
please [open an issue on GitHub](https://github.com/jimeh/common-flow/issues).

License
-------

[Creative Commons - CC BY 3.0](http://creativecommons.org/licenses/by/3.0/)
