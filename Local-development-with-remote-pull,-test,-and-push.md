## Overview

With a little setup and some basic comfort with git workflows involving multiple repositories, one can do development of Trilinos on any machine they want and then use a different standard Linux COE RHEL 6 machine with the SEMS Env to pull, test, and push the changes to the Trilinos GitHub 'develop' branch.

For the below instructions:
* Let `<remote-machine>` be the name of a standard SNL Linux COE RHEL 6 machine with the SEMS env.
* Let `<local-machine>` be the name of any machine (i.e. Mac OSX laptop, nonstandard Linux workstation, etc.) where you regularly do some Trilinos development and that can be reached by SSH from `<remote-machine>`.
* Let `$TRILNIOS_DIR` be the location of the Trilinos git repo on `<local-machine>`.

## Process

* [Initial setup on `<remote-machine>` and `<local-machine>`](https://github.com/trilinos/Trilinos/wiki/Local-development-with-remote-pull%2C-test%2C-and-push#initial_setup)
* ???
* ???

<a name="initial_setup"/>
### Initial setup on `<remote-machine>` and `<local-machine>`

**1) Set up a Trilinos clone and checkin build directory on the remote machine:**

```
$ ssh <remote-machine>
$ git clone git@github.com:trilinos/Trilinos.git
$ mkdir CHECKIN
$ cd CHECKIN/
$ ln -s ../cmake/std/sems/checkin-test-sems.sh .
```

NOTE: You can use any directory structure you want, you just need to make the same modifications in the below steps.

**2) Set up SSH key access from `<remote-machine>` to `<local-machine>`:**

```
$ ssh <remote-machine>
$ scp .ssh/id_rsa.pub <local-machine>:~/.ssh/id_rsa.pub.<remote-machine>
$ ssh <local-machine>
$ cd ~/.ssh
$ cat id_rsa.pub.<remote-machine> >> authorized_keys
```

NOTE: This allows your account on `<remote-machine>` to use SSH to `<local-machine>` without requiring type to type in a password every time.

**3) Set up git remote from `<remote-machine>` to `<local-machine>`**

```
$ ssh <remote-machine>
$ ssh Trilinos/
$ git remote add <local-machine> <local-machine>:$TRILNIOS_DIR
```

<a name="local_dev_remote_pull_test_push"/>
### Local development then remote pull/test/push (after initial setup)

**1) Do development on `<local-machine>` to `$TRILNIOS_DIR/` and make commits to local 'develop' branch**

* Do development, make commits, do local testing as you normally would to the git repo `<local-machine>:$TRILNIOS_DIR`
* Clean up your local 'develop' branch commits with `git rebase -i @{u}` (Optional but Recommended).  (This removes all of the merge commits from doing `git pull` to keep your local branch up to date and avoid a merge conflict on `<remote-machine>` when not using the `--no-rebase` argument.)

**2) SSH to `<remote-machine>` and invoke checkin-test script:**

```
$ ssh <remote-machine>
$ cd Trilinos/CHECKIN/
$ ./checkin-test-sems.sh --extra-pull-from=<local-machine>:develop \
  --no-rebase --do-all --push
```

NOTES:
* The `--no-rebase` option avoids rebasing that might otherwise result in merge conflicts (i.e. git rerere info does not automatically transfer from your Trilinos repo on `<local-machine>` to `<remote-machine>`).  (However, if one is not concerned about merge conflicts or if one did the `git rebase -i @{u}` cleanup as described above, then one can leave off the `--no-rebase` option and let the checkin-test script rebase, keeping a nice linear history.)
* Once the pull has been completed by the checkin-test script, then one can go back to their `<local-machine>` and keep developing, adding new commits (but not modifying any existing commits). 
* When pulling on `<local-machine>`, it is a good idea to use `git pull --rebase` to remove duplicate commits that were pushed from `<remote-machine>` in case the checkin-test.py script rebased or amended the top commit (which it does by default).
* If everything passes, the checkin-test script will push to the GitHub 'develop' branch and send you an email about what happened.  However, if there are any failures, then they need to be resolved as described below.

<a name="resolving_problems"/>
### Resolving problems on `<remote-machine>`

If the configure, build, or any tests fail, then the errors much be resolved.  That can either be done on `<local-machine>` **or** on `<remote-machine>`.  If fixed on the `<local-machine>`, just make new commits there and then repeat the above remote pull/test/push commands.  However, below, we will describe how to fix the problems on the `<remote-machine>` where the are guaranteed to be reproduced.

**1) Log onto `<remote-machine>` and reproduce the problem:**

```
$ ssh <remote-machine>
$ cd Trilinos/CHECKIN/
$ source ../Trilinos/cmake/load_ci_sems_dev_env.sh
$ cd MPI_RELEASE_DEBUG_SHARED_PT/
$ make   # Reproduce the build failure
$ ctest -j<N> -R <test-name>  # Reproduce the test failure
```

**2) Add new fixing commits on `<remote-machine>`**

```
$ cd ~/Trilinos
$ emacs -nw <broken-files>  # or other editors
$ git commit -m "Blah blah blah (#1234)"
```

NOTES:
* Obviously one would make changes, run the build and run the affected tests in the build dir over and over until the problem was fixed.
* For simplicity, fix the problems by adding new commits, not amending old commits.  Amending existing commits will result in merge conflicts when pulling the `develop` branch from GitHub back on `<local-machine>` using `git pull` or `git pull --rebase`.  (However, if you are conformable with git, you can amend, squash and otherwise alter commits not yet pushed to GitHub 'develop' all you want.  Then one will just need to make the necessary adjustments back on `<local-machine>` when pulling the updated GitHub 'develop' branch.)

**3) Run final remote test/push:**

```
$ cd Trilinos/CHECKIN/
$ ./checkin-test-sems.sh --no-rebase --do-all --push
```

NOTES:
* Again, one can remove the `--no-rebase` option to allow the rebase in case where merge conficts are not a concern or with a `git rebase -i @{u}` cleanup has recently been done.
