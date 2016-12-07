In order to maintain the stability of [Primary Tested](http://trac.trilinos.org/wiki/TribitsLifecycleModelOverview#test_categories) packages and code on the 'develop' branch of Trilinos, the checkin-test.py script should always be used to test code before any push that changes source code.  A standard environment for running the pre-push CI tests has been set up based on the [SEMS Development Environment](https://github.com/trilinos/Trilinos/wiki/SEMS-Dev-Env) encapsulated in the script [checkin-test-sems.sh](https://github.com/trilinos/Trilinos/blob/develop/cmake/std/sems/checkin-test-sems.sh).  This script makes it easy to run on any machine that has the SEMS Dev Env mounted.  To set up to use this script to test and push changes to Trilinos, do:

```
$ cd Trilinos/
$ mkdir CHECKIN/  # Or put this anywhere you want on Linux systems
$ ln -s ../cmake/std/sems/checkin-test-sems.sh .
``` 

Then when one wants to test and push changes (and local git repo is "clean" with no modified or untracked files), one can do:

```
$ ./checkin-test-sems.sh --do-all --push
```

That will automatically figure out what packages are changed and will enable those packages and all of their downstream packages and tests.  Then if everything passes, it will rebase the commits on top of `origin/develop` and push the commits (i.e. it implements the [simple centralized workflow](https://github.com/trilinos/Trilinos/wiki/VC-%7C-Simple-Centralized-Workflow)).

One can also test changes to any packages locally (with the local repo in any arbitrary state with modified or untracked files) using:

```
$ ./checkin-test-sems.sh --enable-all-packages=off --enable-packages=<pkg0>,<pkg1>,... \
    --local-do-all
```

Many other use cases are also supported.  Some detailed documentation on the checkin-test.py script can be obtained using [checkin-test.py --help](https://tribits.org/doc/TribitsDevelopersGuide.html#checkin-test-py-help).

NOTES:
* The `checkin-test-sems.sh` script, by default, runs a single build called `MPI_RELEASE_DEBUG_SHARED_PT` with the env defined by the source script [load_ci_sems_dev_env.sh](https://github.com/trilinos/Trilinos/blob/develop/cmake/load_ci_sems_dev_env.sh).  This is a build designed to best protect developers and users of Trilinos.  This build allows the enable of any Primary Tested (PT) packages and enables several TPLs provided by the SEMS env (see the exactly list produced by 'Final set of enabled TPLs' in CMake output).
* The standard pre-push CI build is currently chosen to be the Sandia Linux RHEL 6 or 7 COE with the SEMS env.  If at all possible, one should only push to Trilinos from one of these machines, or ask someone else with access to one of these machines to push.  (Please contact trilinos-framework at software.sandia.gov if you do not have access and time on one of these systems.  Most Sandia staff can be given lab-support systems for this purpose.) 
* If nothing else, please at least run with the options `--enable-all-packages=off --no-enable-fwd-packages` . That will enable only the packages you change. This should not take any longer than a regular test build that you should already be doing but at least we would be getting a consistent configuration between different Trilinos developers and the tested commits will get marked which will allow for [robust usage of git bisect](https://tribits.org/doc/TribitsDevelopersGuide.html#using-git-bisect-with-checkin-test-py-workflows).
* One can do development on any machine with any env and then use a remote SNL Linux RHEL 6 machine with the SEMS env to a [remote pull, test, and push](https://github.com/trilinos/Trilinos/wiki/Local-development-with-remote-pull%2C-test%2C-and-push) of the commits.
