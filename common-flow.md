Git Common-Flow {{version}}
===========================

Introduction
------------

Common-Flow is an attempt to gather a sensible selection of the most common
usage patterns of git into a single and concise specification. It is based on
the [original variant](http://scottchacon.com/2011/08/31/github-flow.html)
of [GitHub Flow](https://guides.github.com/introduction/flow/), while taking
into account how a lot of open source projects most commonly use git.

In short, Common-Flow is essentially GitHub Flow with the addition of versioned
releases, optional release branches, and without the requirement to deploy to
production all the time.

Terminology
-----------

- **Master Branch** - Must be named "master", must always have passing tests,
  and is not guaranteed to always work in production environments.
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
- **Release** - May be considered safe to use in production environments. Is
  effectively just a git tag named after the version of the release.
- **Release Branches** - Used both for short-term preparations of a release, and
  also for long-term maintenance of older version.

Summary
-------

- The "master" branch is the mainline branch with latest changes, and must not
  be broken.
- Changes (features, bugfixes, etc.) are done on "change branches" created from
  the master branch.
- [Rebase early and often.](https://i.imgur.com/1RS8x2d.png)
- When a change branch is stable and ready, it is merged back in to master.
- A release is just a git tag who's name is the exact release version string
  (e.g. "2.11.4").
- Release branches can be used to avoid a change freezes on master. They are not
  required, instead they are available if you do need them.

Git Common-Flow Specification (Common-Flow)
-------------------------------------------

The key words "MUST", "MUST NOT", "REQUIRED", "SHALL", "SHALL NOT", "SHOULD",
"SHOULD NOT", "RECOMMENDED", "MAY", and "OPTIONAL" in this document are to be
interpreted as described in [RFC 2119](https://tools.ietf.org/html/rfc2119).

1. TL;DR
    1. Do not break the master branch.
    2. A release is a git tag.
2. The Master Branch
    1. A branch named "master" MUST exist and it MUST be referred to as the
       "master branch".
    2. The master branch MUST always be in a non-broken state with its test
       suite passing.
    4. The master branch IS NOT guaranteed to always work in production
       environments. Despite test suites passing it may at times contain
       unfinished work. Only releases may be considered safe for production use.
    5. The master branch SHOULD always be in a "as near as possibly ready for
       release/production" state to reduce any friction with creating a new
       release.
3. Change Branches
    1. Each change (feature, bugfix, etc.) MUST be performed on separate
       branches that SHOULD be referred to as "change branches".
    2. All change branches MUST have descriptive names.
    3. It is RECOMMENDED that you commit often locally, and that you try and
       keep the commits reasonably structured to avoid a messy and confusing git
       history.
    4. You SHOULD regularly push your work to the same named branch on the
       remote server.
    5. You SHOULD create separate change branches for each distinctly different
       change. You SHOULD NOT include multiple unrelated changes into a single
       change branch.
    6. When a change branch is created, the branch that it is created from
       SHOULD be referred to as the "source branch". Each change branch also
       needs a designated "merge target" branch, typically this will be the same
       as the source branch.
    7. Change branches MUST be regularly updated with any changes from their
       source branch. This MUST be done by rebasing the change branch on top of
       the source branch.
    8. After updating a change branch from its source branch you MUST push the
       change branch to the remote server. Due to the nature of rebasing, you
       will be required to do a force push, and you MUST use the
       "--force-with-lease" git push option when doing so instead of the regular
       "--force".
    9. If there is a truly valid technical reason to not use rebase when
       updating change branches, then you can update change branches via merge
       instead of rebase. The decision to use merge MUST only be taken after all
       possible options to use rebase have been tried and failed. People not
       understanding how to use rebase is NOT a valid reason to use merge. If
       you do decide to use merge instead of rebase, you MUST NOT use a mixture
       of both methods, pick one and stick to it.
4. Pull Requests
    1. To merge a change branch into its merge target, you MUST open a "pull
       request" (or equivalent).
    2. The purpose of a pull request is to allow others to review your changes
       and give feedback. You can then fix any issues, complaints, and more that
       might arise, and then let people review again.
    3. Before creating a pull request, it is RECOMMENDED that you consider the
       state of your change branch's commit history. If it is messy and
       confusing, it might be a good idea to rebase your branch with "git rebase
       -i" to present a cleaner and easier to follow commit history for your
       reviewers.
    4. A pull request MUST only be merged when the change branch is up-to-date
       with its source branch, the test suite is passing, and you and others are
       happy with the change. This is especially important if the merge target
       is the master branch.
    5. To get feedback, help, or generally just discuss a change branch with
       others, it is RECOMMENDED you create a pull request and discuss the
       changes with others there. This leaves a clear and visible history of
       how, when, and why the code looks and behaves the way it does.
5. Versioning
    1. A "version string" is a typically mostly numeric string that identifies a
       specific version of a project. The version string itself MUST NOT have a
       "v" prefix, but the version string can be displayed with a "v" prefix to
       indicate it is a version that is being referred to.
    2. The source of truth for a project's version MUST be a git tag with a name
       based on the version string. This kind of tag MUST be referred to as a
       "release tag".
    3. It is OPTIONAL, but RECOMMENDED to also keep the version string
       hard-coded somewhere in the project code-base.
    4. If you hard-code the version string into the code-base, it is RECOMMENDED
       that you do so in a file called "VERSION" located in the root of the
       project. But be mindful of the conventions of your programming language
       and community when choosing if, where and how to hard-code the version
       string.
    5. If you are using a "VERSION" file in the root of the project, this file
       MUST only contain the exact version string, meaning it MUST NOT have a
       "v" prefix. For example "v2.11.4" is bad, and "2.11.4" is good.
    6. It is OPTIONAL, but RECOMMENDED that that the version string follows
       Semantic Versioning (<http://semver.org/>).
6. Releases
    1. To create a new release, you MUST create a git tag named as the exact
       version string of the release. This kind of tag MUST be referred to as a
       "release tag".
    2. The release tag name can OPTIONALLY be prefixed with "v". For example the
       tag name can be either "2.11.4" or "v2.11.4". It is however RECOMMENDED
       that you do not use a "v" prefix. You MUST NOT use a mixture of "v"
       prefixed and non-prefixed tags. Pick one form and stick to it.
    3. If the version string is hard-coded into the code-base, you MUST create a
       "version bump" commit which changes the hard-coded version string of the
       project.
    4. When using version bump commits, the release tag MUST be placed on the
       version bump commit.
    5. If you are not using a release branch, then the release tag, and if
       relevant the version bump commit, MUST be created directly on the master
       branch.
    6. The version bump commit SHOULD have a commit message title of "Bump
       version to VERSION". For example, if the new version string is "2.11.4",
       the first line of the commit message SHOULD read: "Bump version to
       2.11.4"
    7. It is RECOMMENDED that release tags are lightweight tags, but you can
       OPTIONALLY use annotated tags if you want to include changelog
       information in the release tag itself.
    8. If you use annotated release tags, the first line of the annotation
       SHOULD read "Release VERSION". For example for version "2.11.4" the first
       line of the tag annotation SHOULD read "Release 2.11.4". The second line
       MUST be blank, and the changelog MUST start on the third line.
7. Short-Term Release Branches
    1. Any branch that has a name starting with "release-" SHOULD be referred to
       as a "release branch".
    2. Any release branch which has a name ending with a specific version
       string, MUST be referred to as a "short-term release branch".
    3. Use of short-term release branches are OPTIONAL, and intended to be used
       to create a specific versioned release.
    4. A short-term release branch is RECOMMENDED if there is a lengthy
       pre-release verification process to avoid a code freeze on the master
       branch.
    5. Short-term release branches MUST have a name of "release-VERSION". For
       example for version "2.11.4" the release branch name MUST be
       "release-2.11.4".
    6. When using a short-term release branch to create a release, the release
       tag and if used, version bump commit, MUST be placed directly on the
       short-term release branch itself.
    7. Only very minor changes should be performed on a short-term release
       branch directly. Any larger changes SHOULD be done in the master branch,
       and SHOULD be pulled into the release branch by rebasing it on top of the
       master branch the same way a change branch pulls in updates from its
       source branch.
    8. After a release tag has been created, the release branch MUST be merged
       back into its source branch and then deleted. Typically the source branch
       will be the master branch.
8. Long-term Release Branches
    1. Any release branch which has a name ending with a non-specific version
       string, MUST be referred to as a "long-term release branch". For example
       "release-2.11" is a long-term release branch, while "release-2.11.4" is a
       short-term release branch.
    2. Use of long-term release branches are OPTIONAL, and intended for work on
       versions which are not currently part of the master branch. Typically
       this is useful when you need to create a new maintenance release for a
       older version.
    3. A long-term release branch MUST have a name with a non-specific version
       number. For example a long-term release branch for creating new 2.9.x
       releases MUST be named "release-2.9".
    4. Long-term release branches for maintenance releases of older versions
       MUST be created from the relevant release tag. For example if the master
       branch is on version 2.11.4 and there is a security fix for all 2.9.x
       releases, the latest of which is "2.9.7". Create a new branch called
       "release-2.9" from the "2.9.7" release tag. The security fix release will
       then end up being version "2.9.8".
    5. To create a new release from a long-term release branch, you MUST follow
       the same process as a release from the master branch, except the
       long-term release branch takes the place of the master branch.
    7. A long-term release branch should be treated with the same respect as the
       master branch. It is effectively the master branch for the release series
       in question. Meaning it MUST always be in a non-broken state, MUST NOT be
       force pushed to, etc.
9. Bug Fixes & Rollback
    1. You MUST NOT under any circumstances force push to the master branch or
       to long-term release branches.
    2. If a change branch which has been merged into the master branch is found
       to have a bug in it, the bug fix work MUST be done as a new separate
       change branch and MUST follow the same workflow as any other change
       branch.
    3. If a change branch is wrongfully merged into master, or for any other
       reason the merge must be undone, you MUST undo the merge by reverting the
       merge commit itself. Effectively creating a new commit that reverses all
       the relevant changes.
10. Git Best Practices
    1. All commit messages SHOULD follow the Commit Guidelines and format from
       the official git
       documentation:
       <https://git-scm.com/book/en/v2/Distributed-Git-Contributing-to-a-Project#_commit_guidelines>
    2. You SHOULD never blindly commit all changes with "git commit -a". It is
       RECOMMENDED you use "git add -i" or "git add -p" to add individual
       changes to the staging area so you are fully aware of what you are
       committing.
    3. You SHOULD always use "--force-with-lease" when doing a force push. The
       regular "--force" option is dangerous and destructive. More
       information:
       <https://developer.atlassian.com/blog/2015/04/force-with-lease/>
    4. You SHOULD understand and be comfortable with
       rebasing: <https://git-scm.com/book/en/v2/Git-Branching-Rebasing>
    5. It is RECOMMENDED that you always do "git pull --rebase" instead of "git
       pull" to avoid unnecessary merge commits. You can make this the default
       behavior of "git pull" with "git config --global pull.rebase true".
    6. It is RECOMMENDED that all branches be merged using "git merge --no-ff".
       This makes sure the reference to the original branch is kept in the
       commits, allows one to revert a merge by reverting a single merge commit,
       and creates a merge commit to mark the integration of the branch with
       master.

FAQ
---

### Why use Common-Flow instead of Git Flow, and how does it differ?

Common-Flow tries to be a lot less complicated than Git Flow by having fewer
types of branches, and simpler rules. Normal day to day development doesn't
really change much:

- You create change branches instead of feature branches, without the need of a
  "feature/" or "change/" prefix in the branch name.
- Change branches are typically created from and merged back into "master"
  instead of "develop".
- Creating a release is done by simply creating a git tag, typically on the
  master branch.

In detail, the main differences between Git Flow and Common-Flow are:

- There is no "develop" branch, there is only a "master" branch which contains
  the latest work. In Git Flow the master branch effectively ends up just being
  a pointer to the latest release, despite the fact that Git Flow includes
  release tags too. In Common-Flow you just look at the tags to find the latest
  release.
- There are no "feature" or "hotfix" branches, there's only "change"
  branches. Any branch that is not master and introduces changes is a change
  branch. Change branches also don't have a enforced naming convention, they
  just have to have a "descriptive name". This makes things simpler and allows
  more flexibility.
- Release branches are available, but optional. Instead of enforcing the use of
  release branches like Git Flow, Common-Flow only recommends the use of release
  branches when it makes things easier. If creating a new release by tagging
  "master" works for you, great, do that.

### Why use Common-Flow instead of GitHub Flow, and how does it differ?

Common-Flow is essentially GitHub Flow with the addition of a "Release" concept
that uses tags. It also attempts to define how certain common tasks are done,
like updating change/feature branches from their source branches for
example. This is to help end arguments about how such things are done.

If a deployment/release for you is just getting the latest code in the master
branch out, without caring about bumping version numbers or anything, then
GitHub Flow is a good fit for you, and you probably don't need the extras of
Common-Flow.

However if your deployments/releases have specific version numbers, then
Common-Flow gives you a simple set of rules of how to create and manage
releases, on top of what GitHub Flow already does.

### What does "descriptive name" mean for change branches?

It means what it sounds like. The name should be descriptive, as in by just
reading the name of the branch you should understand what the branch's purpose
is and what it does. Here's a few examples:

- add-2fa-support
- fix-login-issue
- remove-sort-by-middle-name-functionality
- update-font-awesome
- change-search-behavior
- tweak-footer-style

Notice how none of these have any prefixes like "feature/" or "hotfix/", they're
not needed when branch names are properly descriptive. However there's nothing
to say you can't use such prefixes if you want.

You can also add ticket numbers to the branch name if your team/org has that as
part of it's process. But it is recommended that ticket numbers are added to the
end of the branch name. The ticket number is essentially metadata, so put it at
the end and out of the way of humans trying to read the descriptive name from
left to right.

### How do we release an emergency hotfix when the master branch is broken?

This should ideally never happen, however if it does you can do one of the
following:

- Review why the master branch is broken and revert the changes that caused the
  issues. Then apply the hotfix and release.
- Or use a short-term release branch created from the latest release tag instead
  of the master branch. Apply the hotfix to the release branch, create a release
  tag on the release branch, and then merge it back into master.

In this situation, it is recommended you try to revert the offending changes
that's preventing a new release from master. But if that proves to be a
complicated task and you're short on time, a short-term release branch gives you
a instant fix to the situation at hand, and let's you resolve the issues with
the master branch when you have more time on your hands.

About
-----

The Git Common-Flow specification is authored
by [Jim Myhrberg](https://jimeh.me/).

If you'd like to leave feedback,
please [open an issue on GitHub](https://github.com/jimeh/common-flow/issues).

License
-------

[Creative Commons - CC BY 4.0](https://creativecommons.org/licenses/by/4.0/)
