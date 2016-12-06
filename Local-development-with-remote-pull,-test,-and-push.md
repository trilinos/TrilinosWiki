## Overview

With a little setup and some basic comfort with git workflows involving multiple repositories, one can do development of Trilinos on any machine with any environment they want and then use a different standard Linux COE RHEL 6 machine with the SEMS Env to pull, test, and push the changes to the Trilinos GitHub 'develop' branch using the [checkin-test-sems.sh](https://github.com/trilinos/Trilinos/wiki/Policies-%7C-Safe-Checkin-Testing) script.

For these instructions, define:
* `<remote-machine>`: name of a standard SNL Linux COE RHEL 6 machine with the SEMS env.
* `<local-machine>`: name of any machine (i.e. Mac OSX laptop, nonstandard Linux workstation, etc.) where you regularly do some Trilinos development and that can be reached by SSH from `<remote-machine>`.
* `$TRILNIOS_DIR`: location of the Trilinos git repo on `<local-machine>`.

## Process

* [Initial setup on `<remote-machine>` and `<local-machine>`](https://github.com/trilinos/Trilinos/wiki/Local-development-with-remote-pull%2C-test%2C-and-push#initial_setup)
* [Local development then remote pull, test, and push](https://github.com/trilinos/Trilinos/wiki/Local-development-with-remote-pull%2C-test%2C-and-push#local_dev_remote_pull_test_push)
* [Resolving problems on `<remote-machine>`](https://github.com/trilinos/Trilinos/wiki/Local-development-with-remote-pull%2C-test%2C-and-push#resolving_problems)

<a name="initial_setup"/>
### Initial setup on `<remote-machine>` and `<local-machine>`

**1) Set up a Trilinos clone and CHECKIN build directory on the remote machine:**

```
$ ssh <remote-machine>
$ git clone git@github.com:trilinos/Trilinos.git
$ mkdir CHECKIN
$ cd CHECKIN/
$ ln -s ../cmake/std/sems/checkin-test-sems.sh .
```

NOTE: You can use any directory structure you want, just make the same modifications in the below steps.

**2) Set up SSH key access from `<remote-machine>` to `<local-machine>`:**

```
$ ssh <remote-machine>
$ scp ~/.ssh/id_rsa.pub <local-machine>:~/.ssh/id_rsa.pub.<remote-machine>
$ ssh <local-machine>
$ cd ~/.ssh
$ cat id_rsa.pub.<remote-machine> >> authorized_keys
```

NOTE: This allows you to SSH from `<remote-machine>` to `<local-machine>` without requiring a password.

**3) Set up git remote from `<remote-machine>` to `<local-machine>`**

```
$ ssh <remote-machine>
$ ssh Trilinos/
$ git remote add <local-machine> <local-machine>:$TRILNIOS_DIR
```

NOTE: One can test that password less remote repo access works by running `git fetch <local-machine>`.

<a name="local_dev_remote_pull_test_push"/>
### Local development then remote pull, test, and push

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
* The `--no-rebase` option avoids rebasing that might otherwise result in merge conflicts (i.e. git rerere info does not automatically transfer from your Trilinos repo on `<local-machine>` to `<remote-machine>`).  (However, if one is not concerned about merge conflicts or if one did the `git rebase -i @{u}` cleanup as described above, then one can leave off the `--no-rebase` option and let the checkin-test script rebase, keeping a nice linear history.)
* Once the pull has been completed by the checkin-test script, then while the configure, build, and testing is being performed, you can go back to `<local-machine>` and keep developing and adding new commits (but not modifying any existing commits).
* If everything passes, the checkin-test script will push to the GitHub 'develop' branch and send you an email about what happened.  However, if there are any failures (reported by an auto email), then they need to be resolved as described below.
* When pulling on `<local-machine>`, it is a good idea to use `git pull --rebase` to remove duplicate commits that were pushed from `<remote-machine>` in case the checkin-test.py script rebased or amended the top commit (which it does by default).

<a name="resolving_problems"/>
### Resolving problems on `<remote-machine>`

If the configure, build, or any tests failed in the invocation of `checkin-test-sems.sh` on `<remote-machine>`, then the errors must be resolved before a final push can occur.  Problems can either be resolved on `<local-machine>` **or** on `<remote-machine>`.  If working on `<local-machine>`, just make new commits there and then repeat the above [remote pull, test, push command](https://github.com/trilinos/Trilinos/wiki/Local-development-with-remote-pull%2C-test%2C-and-push#remote_pull_test_push).  However, below, we will describe how to fix the problems on `<remote-machine>` where they are guaranteed to be reproduced.

**1) Log onto `<remote-machine>` and reproduce the problem:**

```
$ ssh <remote-machine>
$ cd Trilinos/CHECKIN/
$ source ../Trilinos/cmake/load_ci_sems_dev_env.sh
$ cd MPI_RELEASE_DEBUG_SHARED_PT/
$ make -j<N>                   # Reproduce the build failure
$ ctest -j<N> -R <test-name>   # Reproduce the test failure(s)
```

**2) Add new fixing commits on `<remote-machine>`**

```
$ cd ~/Trilinos
$ emacs -nw <broken-files>  # or other editors
$ git commit -m "Blah blah blah (#1234)"
```

NOTES:
* Obviously one would iteratively make changes, run the build, and run the affected tests in the build dir over and over until the problem was fixed before making the final commits.
* For simplicity, you should fix the problems by adding new commits, not amending old commits.  (Amending existing commits will result in merge conflicts when pulling the `develop` branch from GitHub back on `<local-machine>` using `git pull` or `git pull --rebase`.)
* However, if you are conformable with git, you can amend, squash and otherwise alter commits not yet pushed to GitHub 'develop' all you want. (Then one will just need to make the necessary adjustments back on `<local-machine>` when pulling the updated GitHub 'develop' branch.)

**3) Run final remote test/push:**

```
$ cd Trilinos/CHECKIN/
$ ./checkin-test-sems.sh --no-rebase --do-all --push
```

NOTES:
* Again, one can remove the `--no-rebase` option to allow the rebase in case where merge conficts are not a concern or with a `git rebase -i @{u}` cleanup has recently been done.  (Doing a final rebase creates a nice linear history on the 'develop' branch.)
