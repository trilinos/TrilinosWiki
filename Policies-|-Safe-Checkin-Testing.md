#Proper Safe Checkin Testing

In order to maintain the stability of "Primary Stable" Code in Trilinos, the checkin-test.py script should always be used to test code before any checkin that changes source code. The script can be found inside of the Trilinos source tree at:

    $TRILINOS_HOME/checkin-test.py

To see how to get started and access documentation type:

    $TRILINOS_HOME/checkin-test.py --help

See static (perhaps outdated) copy of the ouptut [checking-test.py --help](https://software.sandia.gov/trilinos/developer/checkin-test.help.out)

If CMake can find BLAS, LAPACK, and MPI, doing a solid checkin test, along with the push, is as easy as:

    $ cd $TRILINOS_HOME
    $ mkdir CHECKIN
    $ echo CHECKIN >> .git/info/exclude
    $ cd CHECKIN
    $ ../checkin-test.py --do-all -j8 --push


NOTE: If nothing else, please at least run with the options '--enable-all-packages=off' '--no-enable-fwd-packages' '--without-serial-release'. That will enable only the packages you change and will only do an 'MPI_DEBUG' build. This should not take any longer than a regular test build that you should already be doing but at least we would be getting a consistent configuration between different Trilinos developers.

NOTE: To only do a build and test without messing with git use '--local-do-all' instead of '--do-all'.

A set of slides on the motivation and usage for the checkin-test.py script are available in PPT and PDF formats.

