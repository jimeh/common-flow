Git Common-Flow 1.0.0-rc.1
==============================

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
- **Pull Request** - A means of requesting that a change branch is merged in to
  its merge target, allowing others to review, discuss and approve the changes.
- **Release** - Consists of a version bump commit, and a git tag named according
  to the new version string placed on said commit.
- **Release Branches** - Used both for short-term preparations of a release, and
  also for long-term maintenance of older version.

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
2. Change Branches
    1. Changes MUST be performed on a separate branch that SHOULD be referred to
       as a "change branch". All change branches MUST have descriptive names. It
       is RECOMMENDED that you commit often locally, and you SHOULD regularly
       push your work to the same named branch on the remote server.
    2. When a change branch is created, the branch that it is created from
       SHOULD be referred to as the "source branch". Each change branch also
       needs a designated "merge target" branch, typically this will be the same
       as the source branch.
    3. Change branches MUST be regularly updated with any changes from their
       source branch. This MUST be done by rebasing the change branch on top of
       the source branch.
    4. After rebasing a change branch on top of its source branch you MUST push
       the change branch to the remote server. This will require you to do a
       force push, and you SHOULD use the "--force-with-lease" git push option.
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
3. Versioning
    1. The project MUST have its version hard-coded somewhere in the
       code-base. It is RECOMMENDED that this is done in a file called "VERSION"
       located in the root of the project.
    2. If you are using a "VERSION" file in the root of the project, this MUST
       only contain the exact version string.
    3. The version string SHOULD follow the Semantic Versioning
       (<http://semver.org/>) format. Use of Semantic Versioning is OPTIONAL,
       but the version string MUST NOT have a "v" prefix. For example "v2.11.4"
       is bad, and "2.11.4" is good.
4. Releases
    1. To create a new release, you MUST create a "version bump" commit which
       changes the hard-coded version string of the project. The version bump
       commit MUST have a git tag created on it and named as the exact version
       string.
    2. If you are not using a release branch, then the version bump commit MUST
       be created directly on the master branch.
    3. The version bump commit MUST have a commit message title of "Bump version
       to VERSION". For example, if the new version string is "2.11.4", the
       first line of the commit message MUST read: "Bump version to 2.11.4"
    4. The release tag on the version bump commit MUST be named exactly the same
       as the version string. The tag name can OPTIONALLY be prefixed with
       "v". For example the tag name can be either "2.11.4" or "v2.11.4".
    5. It is RECOMMENDED that release tags are lightweight tags, but you can
       OPTIONALLY use annotated tags if you want to include changelog
       information in the release tag itself.
    6. If you use annotated release tags, the first line of the annotation MUST
       read "Release VERSION". For example for version "2.11.4" the first line
       of the tag annotation would read "Release 2.11.4". The second line must
       be blank, and the changelog MUST start on the third line.
5. Release Branches
    1. Any branch that has a name starting with "release-" SHOULD be referred to
       as a "release branch".
    2. Use of release branches is OPTIONAL.
    3. Changes in a release branch SHOULD typically come from work being
       done against the master branch. Meaning changes SHOULD only trickle
       downwards from the master branch. If a change needs to trickle back up
       into the master branch, that work should have happened against the master
       branch in the first place. One exception to this is version bump commits.
    4. There are two types of release branches; short-term, and long-term.
6. Short-Term Release Branches
    1. Used for creating a specific versioned release.
    2. A short-term release branch is RECOMMENDED if there is a lengthy
       pre-release verification process to avoid a code freeze on the master
       branch.
    3. MUST have a name of "release-VERSION". For example for version
       "2.11.4" the release branch name MUST be "release-2.11.4".
    4. When using a short-term release branch, the version bump commit and
       release tag MUST be made directly on the release branch itself.
    5. Only very minor changes should be performed on a short-term release
       branch directly. Any larger changes SHOULD be done in the master branch,
       and SHOULD be pulled into the release branch by rebasing it on top of the
       master branch the same was a change branch pulls in updates from its
       source branch.
    6. After the version bump commit and release tag have been created, the
       release branch MUST be merged back into its source branch and then
       deleted. Typically the source branch will be the master branch.
7. Long-Term Release Branches
    1. Used for work on versions which are not currently part of the master
       branch. Typically this is useful when you need to create a new
       maintenance release for a older version.
    2. The branch name MUST have a non-specific version number. For example
       a long-term release branch for creating new 2.9.x releases would be
       named "release-2.9".
    3. To create a new release from a long-term release branch, you MUST
       create a version bump commit and release tag directly on the release
       branch.
    4. A long-term release branch MUST be created from the relevant release
       tag. For example if there is a security fix for all 2.9.x releases,
       the latest of which is "2.9.7", we create a new branch called
       "release-2.9" off of the "2.9.7" release tag. The security fix
       release will then end up being version "2.9.8".
8. Bug Fixes & Rollback
    1. You MUST NOT under any circumstances force push to the master branch.
    2. If a change branch which has been merged into the master branch is found
       to have a bug in it, the bug fix work MUST be done as a new separate
       change branch and MUST follow the same workflow as any other change
       branch.
    3. If a change branch is wrongfully merged into master, or for any other
       reason the merge must be undone, you MUST undo the merge by reverting the
       merge commit itself. Effectively creating a new commit that reverses all
       the relevant changes.
9. Git Best Practices
    1. All commit messages SHOULD follow the Commit Guidelines and format from
       the official git
       documentation:
       <https://git-scm.com/book/en/v2/Distributed-Git-Contributing-to-a-Project>
    2. You SHOULD always use "--force-with-lease" when doing a force push. The
       plain "--force" option is dangerous and destructive. More
       information:
       <https://developer.atlassian.com/blog/2015/04/force-with-lease/>
    3. You SHOULD understand and be comfortable with
       rebasing: <https://git-scm.com/book/en/v2/Git-Branching-Rebasing>
    4. It is RECOMMENDED that you always do "git pull --rebase" instead of "git
       pull" to avoid unnecessary merge commits. You can make this the default
       behavior of "git pull" with "git config --global pull.rebase true".
    5. It is RECOMMENDED that all branches be merged using "git merge --no-ff".
       This makes sure the reference to the original branch is kept in the
       commits, allows one to revert a merge by reverting a single merge commit,
       and creates a merge commit to mark the integration of the branch with
       master.

About
-----

The Git Common-Flow specification is authored
by [Jim Myhrberg](http://jimeh.me).

If you'd like to leave feedback,
please [open an issue on GitHub](https://github.com/jimeh/common-flow/issues).

License
-------

[Creative Commons - CC BY 3.0](http://creativecommons.org/licenses/by/3.0/)
