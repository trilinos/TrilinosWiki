## Overview

With a little setup and some basic comfort with git workflows involving multiple repositories, you can do development of Trilinos on any machine with any environment you want and then use a different standard Linux COE RHEL 6 machine with the SEMS Env (e.g. any CEE LAN machine at Sandia) to pull, test, and push the changes to the Trilinos GitHub 'develop' branch using the [checkin-test-sems.sh](https://github.com/trilinos/Trilinos/wiki/Policies-%7C-Safe-Checkin-Testing) script.  This is a minor variation of the [simple centralized workflow](https://github.com/trilinos/Trilinos/wiki/VC-%7C-Simple-Centralized-Workflow) on the 'develop' branch and does not require any deep understanding of topic branch workflows.

After a [one-time initial setup](https://github.com/trilinos/Trilinos/wiki/Local-development-with-remote-pull%2C-test%2C-and-push#initial_setup) is performed, the only extra commands that a developer must run in order to safely test and push from the remote machine are:

```
$ ssh <remote-machine>
$ cd Trilinos/CHECKIN/
$ ./checkin-test-sems.sh --extra-pull-from=<local-machine>:develop \
  --no-rebase --do-all --push
```

(see [below](https://github.com/trilinos/Trilinos/wiki/Local-development-with-remote-pull%2C-test%2C-and-push#remote_pull_test_push)).  And this set of remote commands can be performed in an SSH invocation script as a single command from the local machine.

For these instructions, define:
* `<remote-machine>`: name (and URL) of a standard SNL Linux COE RHEL 6 machine with the SEMS env.
* `<local-machine>`: name (and URL) of any machine (i.e. Mac OSX laptop, nonstandard Linux workstation, etc.) where you regularly do some Trilinos development and that can be reached by SSH from `<remote-machine>` (see relaxation of this requirement described below in [Alternative branch workflow involving GitHub repo.](https://github.com/trilinos/Trilinos/wiki/Local-development-with-remote-pull%2C-test%2C-and-push#alternative_branch_workflow))
* `$TRILNIOS_DIR`: location of the Trilinos git repo on `<local-machine>`.

## Process

* [Initial setup on `<remote-machine>` and `<local-machine>`](https://github.com/trilinos/Trilinos/wiki/Local-development-with-remote-pull%2C-test%2C-and-push#initial_setup)
* [Local development then remote pull, test, and push](https://github.com/trilinos/Trilinos/wiki/Local-development-with-remote-pull%2C-test%2C-and-push#local_dev_remote_pull_test_push)
* [Resolving problems on `<remote-machine>`](https://github.com/trilinos/Trilinos/wiki/Local-development-with-remote-pull%2C-test%2C-and-push#resolving_problems)

Note [Alternative branch workflow involving GitHub repo](https://github.com/trilinos/Trilinos/wiki/Local-development-with-remote-pull%2C-test%2C-and-push#alternative_branch_workflow).

<a name="initial_setup"/>

### Initial setup on `<remote-machine>` and `<local-machine>`

To get started, one needs to do a one-time minimal setup.  Once this setup is complete, it never needs to be done again.

**1) Set up [minimal Git settings](https://github.com/trilinos/Trilinos/wiki/VC-|-Initial-Git-Setup#minimal_git_settings) for your account on `<remote-machine>`:**

```
$ git config --global user.name "First M. Last"
$ git config --global user.email "youremail@someurl.com"
$ git config --global color.ui true         # Use color in git output to terminal
$ git config --global push.default simple   # or 'tracking' with older versions of git
$ git config --global rerere.enabled 1      # auto resolve of same conflicts on rebase!
```

**2) Set up a Trilinos clone and CHECKIN build directory on `<remote machine>`:**

```
$ ssh <remote-machine>
$ git clone git@github.com:trilinos/Trilinos.git
$ cd Trilinos/
$ git checkout --track origin/develop
$ git branch -d master
$ mkdir CHECKIN
$ cd CHECKIN/
$ ln -s ../cmake/std/sems/checkin-test-sems.sh .
```

NOTE: You can use any directory structure you want, just make the same modifications in the below steps.

**3) Set up SSH key access from `<remote-machine>` to `<local-machine>`:**

```
$ ssh <remote-machine>
$ scp ~/.ssh/id_rsa.pub <local-machine>:~/.ssh/id_rsa.pub.<remote-machine>
$ ssh <local-machine>
$ cd ~/.ssh
$ cat id_rsa.pub.<remote-machine> >> authorized_keys
```

NOTES:
* This allows you to SSH from `<remote-machine>` to `<local-machine>` without requiring a password.
* This step needs to be performed for each new `<local-machine>` that will use `<remote-machine>` as a remote pull, test, and push machine.

**4) Set up git remote from `<remote-machine>` to `<local-machine>`**

```
$ ssh <remote-machine>
$ cd Trilinos/
$ git remote add <local-machine> <local-machine>:$TRILNIOS_DIR
```

NOTES:
* You many need to use the full URL for the machine when setting up the remote depending on your network connection between `<remote-machine>` and `<local-machine>`.  For example `git remote add crf450 crf450.srn.sandia.gov:Trilinos.base/Trilinos`.
* You can test that passwordless remote repo access works by running `git fetch <local-machine>`.
* This step needs to be performed for each new `<local-machine>` that will use `<remote-machine>` as a remote pull, test, and push machine.

<a name="local_dev_remote_pull_test_push"/>

### Local development then remote pull, test, and push

Once the above one-time [initial setup](https://github.com/trilinos/Trilinos/wiki/Local-development-with-remote-pull%2C-test%2C-and-push#initial_setup) is performed, you can use the following process to develop changes on your local machine and use the remote machine to pull, test, and push your new commits.

**1) Do development of Trilinos on `<local-machine>` on local 'develop' branch**

* Do development, make commits, do local testing as you normally would to the git repo `$TRILNIOS_DIR` on `<local-machine>` (see [simple centralized workflow](https://github.com/trilinos/Trilinos/wiki/VC-%7C-Simple-Centralized-Workflow) except for the push).
* Clean up your local 'develop' branch commits on `<local-machine>` with `git rebase -i @{u}` [Optional but Recommended].

NOTE: Doing `git rebase -i @{u}` removes all of the merge commits that might have been created when doing `git pull` to keep your local branch up to date and avoid a merge conflict on `<remote-machine>` when not using the `--no-rebase` argument with checkin-test-sems.sh below.

<a name="remote_pull_test_push"/>

**2) SSH to `<remote-machine>` and invoke checkin-test script to pull, test, and push:**

```
$ ssh <remote-machine>
$ cd Trilinos/CHECKIN/
$ ./checkin-test-sems.sh --extra-pull-from=<local-machine>:develop \
  --no-rebase --do-all --push
```

NOTES:

* The `--no-rebase` option avoids rebasing that might otherwise result in merge conflicts (i.e. [git rerere](https://github.com/trilinos/Trilinos/wiki/VC-%7C-Simple-Centralized-Workflow#git_rerere) info does not automatically transfer from your Trilinos repo on `<local-machine>` to `<remote-machine>`).

<a name="rebase_okay"/>

* However, if your is not concerned about merge conflicts or if you did the `git rebase -i @{u}` cleanup as described above, then you can leave off the `--no-rebase` option and let the checkin-test script rebase, keeping a nice linear history.
* Once the pull has been completed by the checkin-test script, then (while the configure, build, and testing is being performed on `<remote-machine>`) you can go back to `<local-machine>` and keep developing and adding new commits (but not modifying any existing commits).

* If everything passes, the checkin-test script will push to the GitHub 'develop' branch and send you an email.  However, if there are any failures (reported to you by email), then they need to be resolved as described below.

* When later pulling on `<local-machine>` to continue development, it is a good idea to use [`git pull --rebase`](https://github.com/trilinos/Trilinos/wiki/VC-%7C-Simple-Centralized-Workflow#anchor) to remove duplicate commits that were pushed from `<remote-machine>` in case the checkin-test.py script rebased or amended the top commit (which it does by default).

* **WARNING**: Until the `checkin-test-sems.sh` script finishes running on `<remote-machine>` do not try to run it again.  (Otherwise, the two `checkin-test-sems.sh` process will run on top of each other and create a huge mess.)

<a name="resolving_problems"/>

### Resolving problems on `<remote-machine>`

If the configure, build, or any tests failed in the invocation of `checkin-test-sems.sh` on `<remote-machine>`, then the errors must be resolved before a final push can occur.  Problems can either be resolved on `<local-machine>` **or** on `<remote-machine>`.  If working on `<local-machine>`, just make new commits there and then repeat the above [remote pull, test, push command](https://github.com/trilinos/Trilinos/wiki/Local-development-with-remote-pull%2C-test%2C-and-push#remote_pull_test_push).  However, below, we will describe how to fix the problems on `<remote-machine>` where they are guaranteed to be reproduced.

**1) Log onto `<remote-machine>` and reproduce the problem:**

```
$ ssh <remote-machine>
$ cd Trilinos/CHECKIN/
$ source ../cmake/load_ci_sems_dev_env.sh
$ cd MPI_RELEASE_DEBUG_SHARED_PT/
$ ./do-configure               # Reproduce configure failure
$ make -j<N>                   # Reproduce build failure
$ ctest -j<N> -R <test-name>   # Reproduce failing test(s)
```

**2) Add new fixing commits on `<remote-machine>`**

```
$ cd ~/Trilinos
$ emacs -nw <broken-files>  # or other editors
$ git commit -m "Blah blah blah (#1234)"
```

NOTES:
* Obviously one would iteratively make changes, run the build, and run the affected tests in the build dir over and over until the problem was fixed before making the final commits.
* For simplicity, you should fix the problems by **adding new commits, not amending old commits**.  (Amending existing commits will result in merge conflicts when pulling the `develop` branch from GitHub back on `<local-machine>` using `git pull` or `git pull --rebase`.)
* However, if you are conformable with git, you can amend, squash and otherwise alter commits not yet pushed to GitHub 'develop' all you want. (Then one will just need to make the necessary adjustments back on `<local-machine>` when pulling the updated GitHub 'develop' branch.)

**3) Run final remote test/push on `<remote-machine>`:**

```
$ cd Trilinos/CHECKIN/
$ ./checkin-test-sems.sh --no-rebase --do-all --push
```

NOTES:
* Again, one can remove the `--no-rebase` option to allow the rebase in case where merge conficts are not a concern or with a `git rebase -i @{u}` cleanup has recently been done.  (Doing a final rebase creates a nice linear history on the 'develop' branch.)

<a name="alternative_branch_workflow"/>

## Alternative branch workflow involving GitHub repo

The above workflow assumes that commits will be pulled directly from `<local-machine>` to `<remote-machine>`.  However, one can accomplish the move of commits by pushing the 'develop' branch (or perhaps well-named topic branch `<branch-name>`) to the developer's own Trilinos GitHub fork from `<local-machine>` and then pulling from the GitHub fork to `<remote-machine>`.  The advantages of this approach is that it provides a backup on the branch on GitHub and it works for cases where `<remote-machine>` can't SSH to `<local-machine>` but both `<remote-machine>` and `<local-machine>` can reach GitHub.

A brief outline of this alternative workflow involving the GitHub fork is:

* One-time setup
  * Create remote `intermediate-repo` from the local Trilinos repos on `<local-machine>` and `<remote-machine>` pointing to your GitHub fork of Trilinos.
* Local development then remote pull, test, and push:
  * Push changes from local branch on `<local-machine>` to GitHub fork `intermediate-repo` with branch name `<branch-name>` ('develop' is okay but typically a unique topic branch name is better).
  * On `<remote-machine>` run the `checkin-test-sems.sh` script as described [above](https://github.com/trilinos/Trilinos/wiki/Local-development-with-remote-pull%2C-test%2C-and-push#remote_pull_test_push) but instead use `--extra-pull-from=intermediate-repo:<branch-name>`.
  * Once the push is complete, remove the temp branch on github with `git push intermediate-repo --delete <branch-name>`