In order to maintain the stability of [Primary Tested](http://trac.trilinos.org/wiki/TribitsLifecycleModelOverview#test_categories) packages and code on the 'develop' branch of Trilinos, the checkin-test.py script should always be used to test code before any push that changes source code.  A standard environment for running the pre-push CI tests has been set up based on the [SEMS Development Environment](https://github.com/trilinos/Trilinos/wiki/SEMS-Dev-Env) encapsulated in the script [checkin-test-sems.sh](https://github.com/trilinos/Trilinos/blob/develop/cmake/std/sems/checkin-test-sems.sh).  This script makes it easy to run on any machine that has the SEMS Dev Env mounted.  To set up to use this script to test and push changes to Trilinos, do:

```
$ cd Trilinos/
$ mkdir CHECKIN/  # Or put this anywhere you want on Linux systems
$ ln -s ../cmake/std/sems/checkin-test-sems.sh .
``` 

Then when one wants to test and push changes (and the local git repo is "clean" with no modified or untracked files), one can do:

```
$ cd Trilinos/CHECKIN/
$ ./checkin-test-sems.sh --do-all --push
```

That will automatically figure out what packages are changed and will enable those packages and all of their downstream packages and [BASIC](https://tribits.org/doc/TribitsDevelopersGuide.html#test-test-category) tests.  Then if everything passes, it will rebase the commits on top of `origin/develop`, amend the top commit message with the test results summary, and push the commits (i.e. it implements the [simple centralized workflow](https://github.com/trilinos/Trilinos/wiki/VC-%7C-Simple-Centralized-Workflow) by default).

If one changes a given Trilinos package and one knows that it only impacts that one package (such as when only modifying tests but no changes to library code), then one can run the build and tests for only the directly modified packages with:

```
$ ./checkin-test-sems.sh --enable-all-packages=off --no-enable-fwd-packages --do-all --push
```

Also, one can test any set of packages `<pkg0>`, `<pkg1>`, etc. (and not downstream packages) with the local git repo in any arbitrary state (i.e. has modified or untracked files, is in a detached-head state, etc.) using:

```
$ ./checkin-test-sems.sh --enable-all-packages=off --enable-packages=<pkg0>,<pkg1>,... --no-enable-fwd-packages \
    --local-do-all
```

One can also run any individual step by replacing `--do-all` and `--local-do-all` with the action commands `--pull`, `--configure`, `--build`, and/or `--test` (the script keep memory of what steps have been performed already).

If the configure, build or any tests fail, then one can easily reproduce them just like for any regular build of Trilinos by sourcing the script [load_ci_sems_dev_env.sh](https://github.com/trilinos/Trilinos/wiki/SEMS-Dev-Env#load_ci_sems_dev_env.sh) and doing the following (in an `sh` compatible shell, not (t)csh):

```
$ cd Trilinos/CHECKIN/
$ source ../cmake/load_ci_sems_dev_env.sh
$ cd MPI_RELEASE_DEBUG_SHARED_PT/
$ ./do-configure               # Reproduce configure failure
$ make -j<N>                   # Reproduce build failure
$ ctest -j<N> -R <test-name>   # Reproduce failing test(s)
```

Many other use cases are also supported.  Some detailed documentation on the checkin-test.py script can be obtained using [checkin-test.py --help](https://tribits.org/doc/TribitsDevelopersGuide.html#checkin-test-py-help).

NOTES:
* Using the checkin-test-sems.sh script to push is critical in order to mark known-tested commits to allow for [robust usage of git bisect](https://tribits.org/doc/TribitsDevelopersGuide.html#using-git-bisect-with-checkin-test-py-workflows).  Without this, git bisection is very hard to do with Trilinos.
* The `checkin-test-sems.sh` script, by default, runs a single build called `MPI_RELEASE_DEBUG_SHARED_PT` (see CMake options in `MPI_RELEASE_DEBUG_SHARED_PT/do-configure.base`) with the env defined by the source script [load_ci_sems_dev_env.sh](https://github.com/trilinos/Trilinos/blob/develop/cmake/load_ci_sems_dev_env.sh).  This is a build designed to best protect developers and users of Trilinos.  This build allows the enable of any Primary Tested (PT) packages and enables several TPLs provided by the SEMS env (see `Final set of enabled TPLs` in `MPI_RELEASE_DEBUG_SHARED_PT/configure.out`).
* The standard pre-push CI build is currently chosen to be the Sandia Linux RHEL 6 or 7 COE with the SEMS env.  It is recommended to Trilinos from one of these machines, or ask someone else with access to one of these machines to push for your.  (Please contact trilinos-framework at software.sandia.gov if you do not have access and time on one of these systems.  Most Sandia staff can be given lab-support systems for this purpose.).
* One can do development on any machine with any env one wishes and then use a remote SNL Linux RHEL 6 machine with the SEMS env to do a [remote pull, test, and push](https://github.com/trilinos/Trilinos/wiki/Local-development-with-remote-pull%2C-test%2C-and-push) of the commits.
<a name="raw_checkin_test_py"/>
* The raw script `Trilinos/checkin-test.py` can still be used for doing local development work and testing or driving any workflow.  It can even be used for pushing (but please prefer to use the `checkin-test-sems.sh` script on a RHEL 6 machine with the SEMS env when pushing to the main Trilinos 'develop' branch).  However, unless you can define the `MPI_RELEASE_DEBUG_SHARED_PT.config` file to support the full set of PT packages and TPLs, then just disable that build by running with your own pre-defined builds (i.e. creating `<build0>.config`, `<build1>.config`) by passing in `--default-builds= --st-extra-builds=<build0>,<build1>,...`.  (See the checkin-test-sems.sh script for an example of how to do this.)
* You can see if the CI build is currently clean by [looking at the CI build on CDash](http://testing.sandia.gov/cdash/index.php?project=Trilinos&date=2016-11-30&filtercount=3&showfilters=1&filtercombine=and&field1=buildname&compare1=61&value1=Linux-GCC-4.7.2-MPI_RELEASE_DEBUG_SHARED_PT_CI&field2=groupname&compare2=61&value2=Continuous&field3=buildstarttime&compare3=84&value3=now).  If you see failing tests then you can disable those tests when invoking the checkin-test-sems.sh script by passing in `--extra-cmake-options="-D<failing_test_0>_DISABLE=ON -D<failing_test_1>_DISABLE=ON ..."`.  That will allow you to push by ignoring those failing tests.  If entire packages are failing to build or have catastrophic test failures, then you can disable those using `--disable-packages=<pkg0>,<pkg1>,...`. 
* The checkin-test-sems.sh script is not supported on systems like Mac OSX or Windows.  It only works and has been tested on Linux machines (in particular on RHEL 6 with the SEMS env at present).  Sorry :-(