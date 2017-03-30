## Overview

With a little setup and some basic comfort with git workflows involving multiple repositories, you can do development of Trilinos on any machine with any environment you want and then use a different standard Linux COE RHEL 6 machine with the SEMS Env (e.g. any CEE LAN machine at Sandia) to pull, test, and push the changes to the Trilinos GitHub 'develop' branch using the [checkin-test-sems.sh](https://github.com/trilinos/Trilinos/wiki/Policies-%7C-Safe-Checkin-Testing) script.  This is a minor variation of the [simple centralized workflow](https://github.com/trilinos/Trilinos/wiki/VC-%7C-Simple-Centralized-Workflow) on the 'develop' branch and does not require any deep understanding of topic branch workflows.

After a [one-time initial setup](https://github.com/trilinos/Trilinos/wiki/Local-development-with-remote-pull%2C-test%2C-and-push#initial_setup) is performed, you can invoke a remote pull/test/push process using a local script on your local machine as:

```
$ ./remote-pull-test-push-<remote-machine>.sh
```

(see details [below](https://github.com/trilinos/Trilinos/wiki/Local-development-with-remote-pull%2C-test%2C-and-push#remote_pull_test_push)).

The basic process is:

1. Create commits on your local machine (on the 'develop' branch or a topic branch)
2. Push the local branch from the local machine to an intermediate git repo (e.g. your GitHub fork of Trilinos)
3. Pull and merge your branch from the intermediate git repo to Trilinos git repo 'develop' branch on the remote machine
4. Run `checkin-test-sems.sh` script on remote machine to test and push your branch to the Trilinos 'develop' branch.

If all goes well, this will automatically merge in your branch and push to the Trilinos 'develop' branch.  If anything fails, it will send you email messages about the failures.  If that happens, you can SSH to `<remote-machine>` and inspect the configure, build, or test failures yourself.

The advantages of using this process are:

* You can develop Trilinos on any machine you want but still do a solid pre-push test with the standard CI build to avoid breaking things for other developers and customers.
* As soon as your local commits are pushed to the intermediate git repo from your local machine, you can keep working.  (The checkin-test-sems.sh script will be running on the remote machine in the background.)
* Your local machine is not loaded down running the full Trilinos CI build before your push.
* If anything fails on the remote machine, you can quickly examine the failures, rerun the configure, build, or individual tests since these artifacts live in your own directory on the remote machine (see [resolving problems on `<remote-machine>`](https://github.com/trilinos/Trilinos/wiki/Local-development-with-remote-pull%2C-test%2C-and-push#resolving_problems)).

## Process

Process Outline

* [A) Initial setup on `<remote-machine>` and `<local-machine>`](https://github.com/trilinos/Trilinos/wiki/Local-development-with-remote-pull%2C-test%2C-and-push#initial_setup)

* [B) Local development then remote pull, test, and push](https://github.com/trilinos/Trilinos/wiki/Local-development-with-remote-pull%2C-test%2C-and-push#local_dev_remote_pull_test_push)

* [C) Resolving problems on `<remote-machine>`](https://github.com/trilinos/Trilinos/wiki/Local-development-with-remote-pull%2C-test%2C-and-push#resolving_problems)

For these instructions, define the following:

* `<local-machine>`: Name (and URL) of your machine (i.e. Mac OSX laptop, nonstandard Linux workstation, etc.) where you regularly do some Trilinos development.

* `<local_trilinos_base_dir>`: Base location of your local Trilinos git repo on `<local-machine>` (e.g. `$HOME/Trilinos.base`) where your are doing your development.

* `<remote-machine>`: Name (and URL) of a standard SNL Linux COE RHEL 6 machine (e.g. Sandia CEE LAN machine `ceerws1113`) with the SEMS env that can be directly reached by SSH from `<local-machine>`. (Using SSH tunneling, this can almost always be done from any machine.)

* `<remote_trilinos_base_dir>`: Location of base Trilinos git repo on the remote pull/test/push server `<remote-machine>`. (e.g. on a CEE LAN machine, this can be `/scratch/$USER/TRILINOS_PUSH_SERVER`).  (**WARNING**: Don't use this Trilinos repo on `<remote-machine>` for development.  Instead, make this dedicated to servicing remote pull/test/push processes.)

* `intermediate-repo`: A git repo remote which is used to push and pull your local branch to communicate between your Trilinos git repos on `<local-machine>` and `<remote-machine>`.  (The easiest thing to use is your own fork of the Trilinos GitHub repo your GitHub account `<your-github-id>`, and this is what is assumed below in the setup but it can be changed to anything you want.)

<a name="initial_setup"/>

### A) Initial setup on `<remote-machine>` and `<local-machine>`

To get started, one needs to do a minimal one-time setup.  Once this setup is complete, it never needs to be done again.  The setup steps are given below.

**A.1) Set up [minimal Git settings](https://github.com/trilinos/Trilinos/wiki/VC-|-Initial-Git-Setup#minimal_git_settings) for your account on `<remote-machine>`:**

```
$ ssh <remote-machine>
$ git config --global user.name "First M. Last"
$ git config --global user.email "youremail@someurl.com"
$ git config --global color.ui true         # Use color in git output to terminal
$ git config --global push.default simple   # Or 'tracking' with older versions of git
$ git config --global rerere.enabled 1      # Auto resolve of same conflicts on rebase!
```

**A.2) Set up a Trilinos clone and CHECKIN build directory on `<remote machine>`:**

```
$ ssh <remote-machine>
$ mkdir <remote_trilinos_base_dir>
$ cd <remote_trilinos_base_dir>/
$ git clone git@github.com:trilinos/Trilinos.git
$ cd Trilinos/
$ git checkout --track origin/develop
$ git branch -d master
$ git remote add intermediate-repo git@github.com:<your-github-id>/Trilinos.git  # e.g. your Trilinos fork
$ mkdir CHECKIN
$ cd CHECKIN/
$ ln -s ../cmake/std/sems/checkin-test-sems.sh .
```

(NOTE: You can use a different remote git repo for `intermediate-repo` other than your GitHub fork of Trilinos.)

Test that remote `intermediate-repo` is set up correctly and is accessible

```
$ cd <remote_trilinos_base_dir>/Trilinos/
$ git fetch intermediate-repo
```

Run the checkin-test-sems.sh script once and then adjust the number of processors to use for builds and testing:

```
$ cd <remote_trilinos_base_dir>/Trilinos/CHECKIN/
$ ./checkin-test-sems.sh --help
$ emacs -nw local-checkin-test-defaults.py  # e.g. change -j4 to -j16
```

**A.3) Set up local git repo on `<local-machine>`:**

Add git remote `intermediate-repo` (e.g. your Trilinos GitHub fork for your account `<your-github-id>`) in your local git repo:

```
$ cd <local_trilinos_base_dir>/Trilinos/
$ git remote add intermediate-repo git@github.com:<your-github-id>/Trilinos.git  # e.g. your Trilinos fork
```

Test that the access to `intermediate-repo` is working correctly:

```
$ git fetch intermediate-repo
```

Create a driver script for the remote pull/test/push process `remote-pull-test-push-<remote-machine>.sh` on `<local-machine>` in base directory `<local_trilinos_base_dir>`:

```
$ cd <local_trilinos_base_dir>/
$ touch remote-pull-test-push-<remote-machine>.sh
$ chmod u+x remote-pull-test-push-<remote-machine>.sh
$ emacs -nw remote-pull-test-push-<remote-machine>.sh   # or use vi
```

This script `remote-pull-test-push-<remote-machine>.sh` should have the contents:

```
#!/bin/bash -e

cd <local_trilinos_base_dir>/

./Trilinos/cmake/std/sems/remote-pull-test-push.sh \
  <remote-machine> \
  <remote_trilinos_base_dir>    # Abs dir!
```

For example, a script using remote machine `ceerws1113` called `remote-pull-test-push-ceerws1113.sh` might look like:

```
#!/bin/bash -e

cd $HOME/Trilinos.base/

./Trilinos/cmake/std/sems/remote-pull-test-push.sh \
  ceerws1113 \
  /scratch/$USER/TRILINOS_PUSH_SERVER
```

**A.4) Set up SSH key access from `<local-machine>` to `<remote-machine>`:**

```
$ ssh <local-machine>
$ scp ~/.ssh/id_rsa.pub <remote-machine>:~/.ssh/id_rsa.pub.<local-machine>
$ ssh <remote-machine>
$ cd ~/.ssh
$ cat id_rsa.pub.<local-machine> >> authorized_keys
```

NOTES:
* This allows you to SSH from `<local-machine>` to `<remote-machine>` and run commands on `<remote-machine>` invoked from `<local-machine>` without requiring a password.
* This step needs to be performed for each new `<local-machine>` that will use `<remote-machine>` as a remote pull, test, and push machine.

<a name="local_dev_remote_pull_test_push"/>

### B) Local development then remote pull, test, and push

Once the above one-time [initial setup](https://github.com/trilinos/Trilinos/wiki/Local-development-with-remote-pull%2C-test%2C-and-push#initial_setup) is performed, you can use the following process to develop changes on your local machine and use the remote machine to pull, test, and push your new commits.

**B.1) Do development of Trilinos on `<local-machine>`**

* Do development, make commits, do local testing as you normally would to the git repo on `<local-machine>` (see [simple centralized workflow](https://github.com/trilinos/Trilinos/wiki/VC-%7C-Simple-Centralized-Workflow) except for the push).  (NOTE: This can be done on the local 'develop' branch or in a local topic branch.  It works either way.)
* Clean up your local branch commits on `<local-machine>` with `git rebase -i origin/develop` **[Optional but Recommended]**.

NOTE: Doing `git rebase -i` removes all of the merge commits that might have been created when doing `git pull` to keep your local branch up to date and avoid a merge conflict on `<remote-machine>`.

<a name="remote_pull_test_push"/>

**B.2) Invoke a remote pull/test/push process:**

Once you are ready for your current branch to be tested and pushed to Trilinos 'develop', run your local driver script as:

```
$ <local_trilinos_base_dir>/remote-pull-test-push-<remote-machine>.sh
```

This prints output like, for example:

```
[rabartl@crf450 Trilinos.base (master)]$ ./remote-pull-test-push-ceerws1113.sh

**************************************************************************
*** Running remote pull/test/push using server 

Remote server: 'ceerws1113'
Remote base dir: '/scratch/rabartl/TRILINOS_PUSH_SERVER'

***
*** 1) Force push local branch 'develop' to 'intermediate-repo'
***

Counting objects: 56, done.
Delta compression using up to 32 threads.
Compressing objects: 100% (55/55), done.
Writing objects: 100% (56/56), 7.16 KiB | 0 bytes/s, done.
Total 56 (delta 43), reused 0 (delta 0)
remote: Resolving deltas: 100% (43/43), completed with 28 local objects.
To git@github.com:bartlettroscoe/Trilinos.git
   23879fb..dfa989f  develop -> develop

***
*** 2) Hard reset the 'develop' branch in Trilinos repo on ceerws1113
***

Already on 'develop'
HEAD is now at c761a11 MiniTensor, ROL: Fix error introduced by making some data members

***
*** 3) Doing non-blocking remote pull/tets/push on 'ceerws1113' (se log file checkin-test-remote.log)
***

You may now keep working on your local machine and wait for email notifications!  (final result will be printed when complete)
```

To keep tabs on the remote pull/test/push progress, one can follow this up on `<local-machine>` with:

```
$ tail -f checkin-test-remote.log
```

Otherwise, if everything passes and the push happens (or if there are any failures), then emails will be sent to your email account (as set by git on `<remote-machine>`).

Once the remote `checkin-test-sems.sh` script completes, the final status is printed to the terminal on the local machine, like for example:

```
DID PUSH: Trilinos: ceerws1113



Total time for checkin-test.py = 100.78 min

Final time: Tue Mar 21 07:23:09 MDT 2017

REQUESTED ACTIONS: PASSED
```

Therefore, it is a good idea to run `remote-pull-test-push-<remote-machine>.sh` in its own terminal.

**NOTES:**

* The script `remote-pull-test-push.sh` does a forced push of the local branch to your `intermediate-repo `(e.g. your GitHub fork) and does a hard reset on the 'develop' branch on `<remote-machine>` to the 'origin/develop' branch.  Therefore, you can rebase your local commits on `<local-machine>` and the run the script `remote-pull-test-push-<remote-machine>.sh` again and again and it will discard the old branch and start over correctly each time fresh.

* **WARNING:** As described above, because this script will hard reset the 'develop' branch on `<remote-machine>`, don't make any uncommitted changes in that remote repo that you want to keep and then run the `remote-pull-test-push-<remote-machine>.sh` script again.  That will wipe out those uncommitted changes!

* One may need to open a new terminal on `<local-machine>` to avoid a timeout for public/private SSH key access on `<remote-machine>`.

* If there are no packages changed by your commits, then no push will be performed and no email will go out and the final result printed will be `ABORTED DUE TO NO ENABLES`.  If that happens, then one can just directly push the changes to the 'develop' branch from `<local-machine>` or `<remote-machine>`.

* **WARNING:** Until the `checkin-test-sems.sh` script finishes running on `<remote-machine>` do not try to invoke another remote pull/test/push process.  (Otherwise, the two `checkin-test-sems.sh` process will run on top of each other and create a huge mess.)

<a name="resolving_problems"/>

### C) Resolving problems on `<remote-machine>`

If the configure, build, or any tests failed in the invocation of `checkin-test-sems.sh` on `<remote-machine>`, then the errors must be resolved before a final push can occur.  Problems can either be resolved on `<local-machine>` followed up with another call to `remote-pull-test-push.sh` **or** the problems can be directly addressed on `<remote-machine>`.  However, it may not be possible to reproduce the errors on `<local-machine>`.  Therefore, it may be better to reproduce, fix, and push the changes from `<remote-machine>` where they are guaranteed to be reproduced.

The following steps describe how to reproduce and address failures on `<remote-machine>`.

**C.1) Log onto `<remote-machine>` and reproduce the problem:**

```
$ ssh <remote-machine>
$ cd <remote_trilinos_base_dir>/Trilinos/CHECKIN/
$ source ../cmake/load_ci_sems_dev_env.sh
$ cd MPI_RELEASE_DEBUG_SHARED_PT/
$ ./do-configure               # Reproduce configure failure
$ make -j<N>                   # Reproduce build failure
$ ctest -j<N> -R <test-name>   # Reproduce failing test(s)
```

**C.2) Add new fixing commits on `<remote-machine>`**

```
$ cd <remote_trilinos_base_dir>/Trilinos/
$ emacs -nw <broken-files>  # or other editors
$ git commit -m "Blah blah blah (#1234)"
```

NOTES:

* Obviously one would iteratively make changes, run the build, and run the affected tests in the build dir on `<remote-machine>` over and over until the problem was fixed before making the final commits.

* For simplicity, you should fix the problems by **adding new commits, not amending old commits** on `<remote-machine>`.  (Amending existing commits on `<remote-machine>` may result in merge conflicts when pulling the main `develop` branch from GitHub back on `<local-machine>` using `git pull` or `git pull --rebase`.)

* However, if you are conformable with git, you can amend, squash and otherwise alter commits not yet pushed to GitHub 'develop' all you want. (Then one will just need to make the necessary adjustments back on `<local-machine>` when pulling the updated GitHub 'develop' branch.)

**C.3) Run final remote test/push on `<remote-machine>`:**

```
$ cd <remote_trilinos_base_dir>/Trilinos/CHECKIN/
$ ./checkin-test-sems.sh --no-rebase --do-all --push
```

NOTES:
* One can remove the `--no-rebase` option to allow the rebase in case where merge conflicts are not a concern or with a `git rebase -i @{u}` cleanup has recently been done.  (Doing a final rebase creates a nice linear history on the 'develop' branch, if that is what you want.)
