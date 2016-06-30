**NOTE: THIS PAGE WILL BE UPDATED SOON ONCE AN IMPROVED PRE-PUSH CI TESTING PROCESS IS ESTABLISHED!**

In order to maintain the stability of "Primary Tested" packages and code on the 'develop' branch of Trilinos, the checkin-test.py script should always be used to test code before any push that changes source code. The script can be found inside of the Trilinos source tree at:

    $TRILINOS_HOME/checkin-test.py

To see how to get started and access documentation type:

    $TRILINOS_HOME/checkin-test.py --help

See static copy of the ouptut [checking-test.py --help](https://tribits.org/doc/TribitsDevelopersGuide.html#checkin-test-py-help).

If CMake can find BLAS, LAPACK, and MPI, doing a solid checkin test, along with the push, is as easy as:

```
$ cd $TRILINOS_HOME
$ mkdir CHECKIN
$ echo CHECKIN >> .git/info/exclude
$ cd CHECKIN
$ ../checkin-test.py --do-all -j8 --push
```

If one has access to the [SEMS development environment](https://github.com/trilinos/Trilinos/wiki/SEMS-Dev-Env), then the wrapper script [checkin-test-sems.sh](https://github.com/trilinos/Trilinos/blob/develop/cmake/std/sems/checkin-test-sems.sh) should be used instead as:

```
$ cd $TRILINOS_HOME/CHECKIN/
$ ln -s ../Trilinos/cmake/std/sems/checkin-test-sems.sh .
$ ./checkin-test-sems.sh --do-all -j8 --push
```

NOTES:
*  If nothing else, please at least run with the options '--enable-all-packages=off' '--no-enable-fwd-packages' '--without-serial-release'. That will enable only the packages you change and will only do an 'MPI_DEBUG' build. This should not take any longer than a regular test build that you should already be doing but at least we would be getting a consistent configuration between different Trilinos developers and the tested commits will get marked which will allow for [robust usage of git bisect](https://tribits.org/doc/TribitsDevelopersGuide.html#using-git-bisect-with-checkin-test-py-workflows).
* To only do a local build and test without messing with git use '--local-do-all' instead of '--do-all'.

A set of slides on the motivation and usage for the checkin-test.py script are available in PPT and PDF formats.
