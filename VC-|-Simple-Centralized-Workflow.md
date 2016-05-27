No matter what git-related workflow that is being used, developers need to understand how to maintain a simple linear shared remote tracking branch.  This shared remote branch may be the ['develop' branch](https://github.com/trilinos/Trilinos/wiki/VC-|-'develop'-'master'-workflow), a shared topic branch, or a Trilinos release branch.  This workflow should be used when a single local commit (or a very small number of local commits) are pushed to the shared remote tracking branch.  In these cases, one should rebase the local commits on top of the updated remote tracking branch before the push in order to maintain a nice linear history.  The motivation of this simple workflow is explained in more detail at the [Simple Centralized CI Workflow](https://docs.google.com/document/d/1uVQYI2cmNx09fDkHDA136yqDTqayhxqfvjFiuUue7wo/edit#heading=h.7z34akh7lsvp).

## The simple centralized workflow

The simple centralized workflow (i.e. the git equivalent of SVN) is outlined on the Atlassian page [Git Workflows](https://www.atlassian.com/pt/git/workflows#!workflow-centralized) as the most basic valid git workflow.  On this Atlassian page, it is stated:

* “Your team can develop projects in the exact same way as they do with Subversion.”
* “Before the developer can publish their feature, they need to fetch the updated central commits and rebase their changes on top of them. This is like saying, “I want to add my changes to what everyone else has already done.” The result is a perfectly linear history, just like in traditional SVN workflows”

Another description is the [“Simple Centralized CI Workflow”](https://docs.google.com/document/d/1uVQYI2cmNx09fDkHDA136yqDTqayhxqfvjFiuUue7wo/edit#heading=h.7z34akh7lsvp).  That link gives the summary (for example, where the branch is assumed to be 'master' but it could be 'develop' or any other shared remote tracking branch):

| Step                                          | Git Commands |
| ---                                           | --- |
| **Get latest updates from the central repo:** | `$ git pull` (from origin/master) |
| **Make local changes and create commits:**    | `$ emacs <files>`   (or other editor) |
|                                               | `$ git commit -a` |
| **Examine state of local branch:**            | `$ git status` |
|                                               | `$ git log --name-status @{u}..HEAD` |
| **Test changes locally:**                     | `$ git pull` (from origin/master) |
|                                               | ... Test changes ( using checkin-test.py) ... |
| **Push changes to central repo:**             | `$ git pull --rebase` (from origin/master) |
|                                               | `$ git push`  (to origin/master) |

The key to keeping a nice linear history is the `--rebase` argument in the `git pull --rebase` command right before the final `git push` command.  That is always safe if these commits are created locally and were not shared with any other repo (which is the typical SVN workflow depicted above).

NOTE: The checkin-test.py script performs all of the steps starting with "Test changes locally" automatically by default.

To avoid having to address the same merge conflicts more than once on the subsequent rebase, just enable “git rerere” using:

```
$ git config --global rerere.enabled 1
```

If you turn on git rerere before you address merge conflicts the first time, then git will remember these conflict resolutions and take care of that automatically in all future rebase commands in that local git repo.  (NOTE: git rerere should have been enabled as part of your [initial git setup](https://github.com/trilinos/Trilinos/wiki/VC-|-Initial-Setup).)

If those references don’t make sense, please don’t give up.  Take a look at [ways to avoid merge commits in git](http://kernowsoul.com/blog/2012/06/20/4-ways-to-avoid-merge-commits-in-git/) (but don’t set up for auto rebase, that will come to bite you later!).  Or look at [this StackOverflow Question](http://stackoverflow.com/questions/25614345/rebase-onto-upstream-changes-with-non-trivial-merge-commits-present-locally).

## Justification

Many beginner developers new to git use a simple git workflow like:

```
$ cd Trilinos/   # Old version of 'master' :-)
… change files, do testing, etc. …
$ git commit -a
$ git push   # Fails because someone pushed to origin/mater since your last pull
$ git pull   # Creates a trivial merge commit!
$ git push
```

The problem with the above process, is that the later ‘git pull’ creates what is often called a trivial merge commit.  Such trivial merge commits make the git history difficult to look at and follow and the mess up the nice linear history that you get using a tool like Subversion (SVN).  (As a result, many development projects that use git will not even let you push these trivial commits.  For example, the push hooks for the SIERRA git repos do not allow them.  Also, PETSc does not allow them.)

Another problem with these trivial merge commits is that GitHub push emails show all of the changed files associated with these trivial merge commits.  This results in getting a bunch of false matches for custom email filters for the GitHub push emails.  For example, many Trilinos developers have an Outlook filter that only shows emails that contain package names such as “teuchos”, “thyra”, "kokkos" and "tpetra".  But these people get spammed because of these trivial merge commits involving commits that have nothing to do with these keywords.  Therefore, routinely using trivial merge commits makes these git email filters are almost worthless.
