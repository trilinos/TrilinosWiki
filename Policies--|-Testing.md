(Note this page needs to be reviewed after the move to GitHub wiki as it is partially out of date.)

# Trilinos Test Harness

## Overview

Trilinos now uses CTest and CDash for testing and display of test results. With CTest/CDash, a testing day is from 10 p.m. to 10 p.m. MST. CDash pages for current results can be found below.

NOTE: Many of the Trilinos Nightly builds are temporarily being sent to the following site CDash site:

http://my.cdash.org/index.php?project=Trilinos

Links to main Trilinos CDash dashboard results and query tools:

+ [Main projects dashboard page for the current day](http://testing.sandia.gov/cdash)
+ [Subprojects (packages) dashboard page for Trilinos for the current day](http://testing.sandia.gov/cdash/index.php?project=Trilinos) [**Warning**: can be slow to come up]
  + Tip: To get a quick summary for all the packages that had configure, build, or test failures, sort the packages in descending order by holding down the Shift key and then click twice on the column headers **Configure: Error, Build: Error**, then **Test: Fail**.
+ [Subproject (package) dashboard page Trilinos package Teuchos for the current day](http://testing.sandia.gov/cdash/index.php?project=Trilinos&subproject=Teuchos)
   + Tip: To query other packages change 'Teuchos' in the URL to some another package name (e.g. 'Epetra', 'Tpetra', etc.)
+ [Summary of Trilinos builds for the current day](http://testing.sandia.gov/cdash/index.php?project=Trilinos&display=project)
   + Tip: To get a quick summary for all the build groups that had configure, build, or test failures, sort the build group rows in descending order by holding down the Shift key and then click twice on the column headers **Configure: Error, Build: Error**, then **Test: Fail**.
   + Tip: To see the individual package builds/tests, click on the name of the build in the **Build Name** field.
   + Tip: To see what files got updated, see the **Update** for the first package (usually Teuchos)
   + Tip: To exclude bad packages (i.e. packages with build and/or test failures) from the build summaries, click [Show Filters] and then select the filter ('Label', 'is not', 'BADPACKAGE').
+ Test query page:
 + [All tests for current day](http://testing.sandia.gov/cdash/queryTests.php?project=Trilinos&showfilters=1) (allows any type of query) [**Warning**: very slow to come up]
 + [Failing tests for current day](http://testing.sandia.gov/cdash/queryTests.php?project=Trilinos&filtercount=1&showfilters=1&field1=status/string&compare1=61&value1=failed) (can only query failed tests) [Comes up fast]
   + Tip: To query results for PACKAGEX, select the filter ('TestName', 'starts with', 'PACKAGEX_')
   + Tip: To query passing tests, select the filter ('Status', 'is', 'passed') [Note the lower case 'passed')
   + Tip: To query failing tests, select the filter ('Status', 'is', 'failed') [Note the lower case 'failed')
   + Tip: To see tests for the last two weeks, select the filter ('Build Time', 'is after', '2 weeks ago')
+ [Current coverage results](http://testing.sandia.gov/extended/cdash/index.php?project=Trilinos&display=project&filtercount=2&showfilters=1&filtercombine=and&field1=buildstarttime/date&compare1=83&value1=7%20days%20ago&field2=buildname/string&compare2=66&value2=_COV) (as of last Saturday)
 + Tip: Click on the Build Name for the MPI or Serial build under 'Nightly' and then go to 'Coverage' to the coverage results for each Trilinos package.

## Categories of Tests

+ Pre-checkin Testing
+ Continuous Integration Testing
+ Nightly Regression Testing
+ Nightly Performance Testing
+ Installation Testing
+ Experimental Testing
+ Weekly Coverage Testing
+ Memory Testing
+ Weekly Scalability Testing

There are many different types of testing that are performed:

**Pre-checkin Testing**: Primary Stable Code [[Checkin test mailing list](https://software.sandia.gov/pipermail/trilinos-checkin-tests/)]

Before every checkin and push to the global Trilinos git repository, the checkin-test.py script should be used to test all affected Primary Stable Code. The purpose for this type of testing is to a) do a basic smoke test to make sure nothing significant has been broken, and b) provide a consistent basis of comparison across all developers to determine if it is safe to commit/push.

The main purpose of this type of testing is to protect other developers so they can continue their development work after pulling your changes. The amount of code tested in this build should be keep to a bare minimum in order to speed up the pre-checkin testing (subject to not skipping critical tests to ensure basic functionality remains working).

NOTE: By default, all pushes done through checkin-test.py [other options] --push get archived to the [trilinos-checkin-tests@software.sandia.gov Mailman list](https://software.sandia.gov/pipermail/trilinos-checkin-tests/). If your pushes are not getting archived to this list please send email to trilinos-framework@software.sandia.gov to have your URL added the list of accepted submitters.

The test category set by the checkin-test.py script is Trilinos_TEST_CATEGORIES=BASIC which matches to the CATEGORIES option BASIC only (not NIGHTLY) passed into PACKAGE_ADD_[ADVANCED]_TEST(...) in the test CMakeLists.txt files.

**Continuous Integration Testing**: Secondary Stable Code [ [Continuous Integration Dashboard](http://testing.sandia.gov/cdash/index.php?project=Trilinos&display=project&filtercount=1&showfilters=1&field1=groupname/string&compare1=61&value1=Continuous)]

A Continuous Integration (CI) server is set up that runs in a continuous loop looking for updates to the global repository and when it finds them, it enables the affected packages and runs the builds and tests. Emails are sent out to the different package lists PACKAGE-regression@software.sandia.gov when problems occur. The first build of the day is a build from scratch while every subsequent CI iteration is a rebuild and retesting of only the affected packages.

The main purposes for having a CI server running are: a) to quickly catch errors from sloppy (or absent) checkin testing, b) to catch problems with Secondary Stable Code so they can be fixed before the nightly or weekly tests fire off.

NOTE: Just because all the Secondary Stable Code builds and all the tests pass does not automatically imply that all Primary Stable Code will build and all the tests will pass. For example, if code in ifdefs for Secondary Stable Code are needed to build Primary Stable code by accident, Primary Stable Code will fail to build. Therefore, just because the CI server is running does not imply that developers can or should skip pre-checkin testing using the checkin-test.py script.

The test category set for CI testing is Trilinos_TEST_CATEGORIES=NIGHTLY which matches to the CATEGORIES options BASIC or NIGHTLY passed into PACKAGE_ADD_[ADVANCED]_TEST(...) in the test CMakeLists.txt files.

**Nightly Regression Testing**: Secondary Stable Code [ [Nightly Regression Dashboard](http://testing.sandia.gov/cdash/index.php?project=Trilinos&display=project&filtercount=1&showfilters=1&field1=groupname/string&compare1=61&value1=Nightly)]

Each night starting at 12 Midnight MST/MDT, a wide range of configurations of Trilinos are tested on a variety of different platforms. MPI and SERIAL are varied along with DEBUG and RELEASE. Different compilers are used. Builds are performed with shared or static libraries.

The test category set for nightly testing is Trilinos_TEST_CATEGORIES=NIGHTLY which matches to the CATEGORIES options BASIC or NIGHTLY passed into PACKAGE_ADD_[ADVANCED]_TEST(...) in the test CMakeLists.txt files.

**Nightly Performance Testing**: Secondary Stable Code [ [ Nightly Performance Dashboard](http://testing.sandia.gov/cdash/index.php?project=Trilinos&display=project&filtercount=2&showfilters=1&filtercombine=and&field1=buildname/string&compare1=63&value1=PERF&field2=groupname/string&compare2=61&value2=Nightly)]

Performance tests are specially designed and specially run tests to assess the serial performance of code. These tests do timing of different types of operations and have strong timing pass/fail test success criteria. If the relative or absolute time limit is violated, then the test fails. These tests must typically be targeted to a specific computer and manually verified on the machine before they can set as nightly tests.

The test category set for performance testing is Trilinos_TEST_CATEGORIES=PERFORMANCE which matches to the CATEGORIES option PERFORMANCE only passed into PACKAGE_ADD_[ADVANCED]_TEST(...) in the test CMakeLists.txt files.

**Installation Testing**: Primary Stable Code

Installation testing is currently done on Primary Stable code to test whether or not an installation contains all necessary files. It does this by compiling Trilinos package tests against an installation of Trilinos that was built from a release-like tarball generated from the development branch. This helps ensure that both the installation and the tarball are not missing critical files.

Installation testing can also be run manually so developers can check if recent changes affected the installation, or reproduce issues found in the nightly testing if necessary. To generate a release-like tarball of Trilinos (on a Linux system), configure Trilinos as for a normal build, and then type make package_source. Next, expand the tarball and configure, build, and install Trilinos, using CMAKE_INSTALL_PREFIX to specify the installation directory. Next, using the git clone used to create the release-like tarball, configure Trilinos again, this time setting the variable Trilinos_INSTALLATION_DIR to the the path that CMAKE_INSTALL_PREFIX was set to when installing Trilinos. All other configuration options should be the same for both builds to prevent incompatibilities (e.g. trying to build MPI tests against a serial installation). Finally, type make, and run the tests (using ctest.

The second build will not compile Trilinos libraries, but rather link against the libraries built for the tarball-based build. The header files in the source trees for Trilinos packages will also be ignored in favor of the headers in the installation directory.

The first build can be based directly on a git clone, rather than a release-like tarball, but some errors will only be detected when building from a tarball.

**Experimental Testing**: Experimental Code [ [Experimental Dashboard](http://testing.sandia.gov/cdash/index.php?project=Trilinos&display=project&filtercount=1&showfilters=1&field1=groupname/string&compare1=61&value1=Experimental)]

The experimental dashboard contains a host of experimental builds where no emails are sent out for failed builds. Some of these experimental builds are run by cron jobs every night. Regularly scheduled builds that are run as experimental builds usually have some issues that result in several failed package builds or test failures and are not ready for "prime time" yet. he goal is to get these builds to a state where they can be moved over to regular nightly builds where emails will be sent out reporting failures.

Other experimental builds are run by individual developers (for running coverage testing for instance). A good way to post experimental builds is with the dashbaord CMake target run with make dashboard (see TrilinosCMakeQuickstart.txt).

By default, the test category set for nightly testing is Trilinos_TEST_CATEGORIES=NIGHTLY which matches to the CATEGORIES options BASIC or NIGHTLY passed into PACKAGE_ADD_[ADVANCED]_TEST(...) in the test CMakeLists.txt files.

**Weekly Coverage Testing**: Secondary Stable Code [ [Current coverage results](http://testing.sandia.gov/extended/cdash/index.php?project=Trilinos&display=project&filtercount=2&showfilters=1&filtercombine=and&field1=buildstarttime/date&compare1=83&value1=7%20days%20ago&field2=buildname/string&compare2=66&value2=_COV) (as of last Saturday)]

Every Saturday morning, a set of coverage tests is run after the other nightly tests are finished. An MPI and a SERIAL build are performed where the coverage for each package is accumulated after running each of the package's test suite.

Note that this coverage will typically be lower than the actual coverage after running the rest the downstream package tests that test more functionality. Currently, we are not accumulated these higher coverage numbers. Trilinos needs to strive to improve the coverage of the native packages test suites for each and every package.

The test category set for coverage testing is Trilinos_TEST_CATEGORIES=NIGHTLY which matches to the CATEGORIES options BASIC or NIGHTLY passed into PACKAGE_ADD_[ADVANCED]_TEST(...) in the test CMakeLists.txt files.

Note that coverage results are reported to a separate dashboard. Developers requiring access to coverage results from outside of Sandia should send a note to the trilinos-framework mail list

**Memory Testing**: Most Secondary Stable Code [ [Current Memory testing Results](https://testing.sandia.gov/cdash/index.php?project=Trilinos&display=project&filtercount=2&showfilters=1&filtercombine=and&field1=buildname/string&compare1=66&value1=_MEMCHECK&field2=buildstarttime/date&compare2=83&value2=1%20days%20ago&collapse=0)]

Every day a subset of Trilinos packages and tests are run through Valgrind to find some forms of memory issues. Note that the memory checking tests can be quite long which is the reason why only a portion of memory issues are being checked currently.

Currently the test category used for Memory testing is Trilinos_TEST_CATEGORIES=NIGHTLY which matches to the CATEGORIES options BASIC or NIGHTLY passed into PACKAGE_ADD_[ADVANCED]_TEST(...) in the test CMakeLists.txt files.

**Weekly Scalability Testing**: Secondary Stable Code

**NOT DEFINED YET**

There is currently no support for automated scalability testing in the Trilinos CMake/CTest/CDash system.