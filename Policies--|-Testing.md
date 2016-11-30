(Note this page needs to be reviewed after the move to GitHub wiki as it is partially out of date.)

# Trilinos Test Harness

## Overview

Trilinos uses CTest and CDash for testing and display of test results. With CTest/CDash, a testing day is from 10 p.m. to 10 p.m. MST. CDash pages for current results can be found below.

NOTE: Many of the Trilinos Nightly builds are temporarily being sent to the following site CDash site:

http://my.cdash.org/index.php?project=Trilinos

Some builds are also temporarily secondarily being submitted to the protected site:

http://testing.sandia.gov/extended/cdash/index.php?project=Trilinos

Links to main Trilinos CDash dashboard results and query tools:

+ [Main projects dashboard page for the current day](http://testing.sandia.gov/cdash)
+ [Subprojects (packages) dashboard page for Trilinos for the current day](http://testing.sandia.gov/cdash/viewSubProjects.php?project=Trilinos) [**Warning**: can be slow to come up]
  + Tip: To get a quick summary for all the packages that had configure, build, or test failures, sort the packages in descending order by holding down the Shift key and then click twice on the column headers **Configure: Error, Build: Error**, then **Test: Fail**.
+ [Subproject (package) dashboard page Trilinos package Teuchos for the current day](http://testing.sandia.gov/cdash/index.php?project=Trilinos&subproject=Teuchos)
   + Tip: To query other packages change 'Teuchos' in the URL to some another package name (e.g. 'Epetra', 'Tpetra', etc.)
+ [Summary of Trilinos builds for the current day](http://testing.sandia.gov/cdash/index.php?project=Trilinos&display=project)
   + Tip: To get a quick summary for all the build groups that had configure, build, or test failures, sort the build group rows in descending order by holding down the Shift key and then click twice on the column headers **Configure: Error, Build: Error**, then **Test: Fail**.
   + Tip: To see the individual package builds/tests, click on the name of the build in the **Build Name** field.
   + Tip: To see what files got updated, click the number in the **Update** field.
   + Tip: To exclude bad packages (i.e. packages with build and/or test failures) from the build summaries, click [Show Filters] and then select the filter ('Subprojects', 'exclude', '<BAD_PACKAGE_NAME>').
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

**Pre-checkin Testing**: Primary Tested Code [[Checkin test mailing list](https://software.sandia.gov/pipermail/trilinos-checkin-tests/)]

Before every push to the global Trilinos git repository, the Trilinos [checkin-test-sems.sh](https://github.com/trilinos/Trilinos/wiki/Policies-%7C-Safe-Checkin-Testing) script should be used to test all affected [Primary Tested (PT) Code](http://trac.trilinos.org/wiki/TribitsLifecycleModelOverview#test_categories). The purpose for this type of testing is to a) do a basic smoke test to make sure nothing significant has been broken, and b) provide a consistent basis of comparison across all developers to determine if it is safe to push.

The main purpose of this type of testing is to protect other developers so they can continue their development work after pulling changes from the 'develop' branch. The selection of PT packages is done based on usage by important Trilinos customers (see [Trilinos GitHub Issue #410](https://github.com/trilinos/Trilinos/issues/482)).  However, 

NOTE: By default, all pushes done through `checkin-test.py [other options] --push` get archived to the [trilinos-checkin-tests@software.sandia.gov Mailman list](https://software.sandia.gov/pipermail/trilinos-checkin-tests/). If your pushes are not getting archived to this list please send email to trilinos-framework@software.sandia.gov to have your URL added the list of accepted submitters.

The default test category set by the Trilinos checkin-test.py script is `Trilinos_TEST_CATEGORIES=BASIC` which matches to the `CATEGORIES` option `BASIC` only (not `NIGHTLY`) passed into [`TRIBITS_ADD_TEST()`](https://tribits.org/doc/TribitsDevelopersGuide.html#tribits-add-test) and [`TRIBITS_ADD_ADVANCED_TEST()`](https://tribits.org/doc/TribitsDevelopersGuide.html#tribits-add-advanced-test) in the test CMakeLists.txt files.

**Continuous Integration Testing**: Primary Tested Code [[Continuous Integration Build](http://testing.sandia.gov/cdash/index.php?project=Trilinos&date=2016-11-30&filtercount=3&showfilters=1&filtercombine=and&field1=buildname&compare1=61&value1=Linux-GCC-4.7.2-MPI_RELEASE_DEBUG_SHARED_PT_CI&field2=groupname&compare2=61&value2=Continuous&field3=buildstarttime&compare3=84&value3=now)]

A Continuous Integration (CI) server is set up that run the build [Linux-GCC-4.7.2-MPI_RELEASE_DEBUG_SHARED_PT_CI](http://testing.sandia.gov/cdash/index.php?project=Trilinos&date=2016-11-30&filtercount=3&showfilters=1&filtercombine=and&field1=buildname&compare1=61&value1=Linux-GCC-4.7.2-MPI_RELEASE_DEBUG_SHARED_PT_CI&field2=groupname&compare2=61&value2=Continuous&field3=buildstarttime&compare3=84&value3=now) in a continuous loop looking for updates to the global repository and when it finds them, it enables the affected packages and runs the builds and tests.  This build is an exact duplicate of the default build MPI_RELEASE_DEBUG_SHARED_PT_CI which is run by the checkin-test-sems.sh script.

Emails are sent out by CDash to the different package lists `PACKAGE-regression@software.sandia.gov` when errors are detected. The first build of the day is a build from scratch while every subsequent CI iteration is a rebuild and retesting of only the affected packages.  (This greatly speeds up the response time of the CI server compared to always building and testing of of Trilinos at every iteration.)

The main purposes for having a post-push CI server running in addition to pre-push CI testing are to: a) quickly catch errors from sloppy (or absent) checkin testing, b) catch violations of the [additive test assumption of branches](https://docs.google.com/document/d/1uVQYI2cmNx09fDkHDA136yqDTqayhxqfvjFiuUue7wo/edit#bookmark=id.d1jneh8ubsyn) which can occur when multiple test/pushes overlap, c) catch problems so they can be fixed before the nightly or weekly tests fire off.

**Nightly Regression Testing**: Secondary Tested Code [[Nightly Regression Dashboard](http://testing.sandia.gov/cdash/index.php?project=Trilinos&date=2016-11-30&filtercount=1&showfilters=1&field1=groupname&compare1=61&value1=Nightly)]

Each night starting at 12 Midnight MST/MDT, a wide range of configurations of Trilinos are tested on a variety of different platforms for [Secondary Tested (PT) Code](http://trac.trilinos.org/wiki/TribitsLifecycleModelOverview#test_categories). MPI and SERIAL are varied along with DEBUG and RELEASE. Different compilers are used. Builds are performed with shared or static libraries.

The test category set for nightly testing is `Trilinos_TEST_CATEGORIES=NIGHTLY` which matches to the `CATEGORIES` options `BASIC` or `NIGHTLY` passed into passed into [`TRIBITS_ADD_TEST()`](https://tribits.org/doc/TribitsDevelopersGuide.html#tribits-add-test) and [`TRIBITS_ADD_ADVANCED_TEST()`](https://tribits.org/doc/TribitsDevelopersGuide.html#tribits-add-advanced-test) in the test CMakeLists.txt files.

**Nightly Performance Testing**: Secondary Tested Code [[Nightly Performance Dashboard](http://testing.sandia.gov/cdash/index.php?project=Trilinos&display=project&filtercount=2&showfilters=1&filtercombine=and&field1=buildname/string&compare1=63&value1=PERF&field2=groupname/string&compare2=61&value2=Nightly)]

Performance tests are specially designed and specially run tests to assess the serial performance of code. These tests do timing of different types of operations and have strong timing pass/fail test success criteria. If the relative or absolute time limit is violated, then the test fails. These tests must typically be targeted to a specific computer and manually verified on the machine before they can set as nightly tests.

The test category set for performance testing is `Trilinos_TEST_CATEGORIES=PERFORMANCE` which matches to the `CATEGORIES` option `PERFORMANCE` only passed into [`TRIBITS_ADD_TEST()`](https://tribits.org/doc/TribitsDevelopersGuide.html#tribits-add-test) and [`TRIBITS_ADD_ADVANCED_TEST()`](https://tribits.org/doc/TribitsDevelopersGuide.html#tribits-add-advanced-test) in the test CMakeLists.txt files.

**Installation Testing**: Primary Tested Code

**NOTE: INSTALLATION TESTING IS CURRENTLY DISABLED!!!**

Installation testing is currently done on Primary Tested code to test whether or not an installation contains all necessary files. It does this by compiling Trilinos package tests against an installation of Trilinos that was built from a release-like tarball generated from the development branch. This helps ensure that both the installation and the tarball are not missing critical files.

Installation testing can also be run manually so developers can check if recent changes affected the installation, or reproduce issues found in the nightly testing if necessary. To generate a release-like tarball of Trilinos (on a Linux system), configure Trilinos as for a normal build, and then type make package_source. Next, expand the tarball and configure, build, and install Trilinos, using CMAKE_INSTALL_PREFIX to specify the installation directory. Next, using the git clone used to create the release-like tarball, configure Trilinos again, this time setting the variable Trilinos_INSTALLATION_DIR to the the path that CMAKE_INSTALL_PREFIX was set to when installing Trilinos. All other configuration options should be the same for both builds to prevent incompatibilities (e.g. trying to build MPI tests against a serial installation). Finally, type make, and run the tests (using ctest.

The second build will not compile Trilinos libraries, but rather link against the libraries built for the tarball-based build. The header files in the source trees for Trilinos packages will also be ignored in favor of the headers in the installation directory.

The first build can be based directly on a git clone, rather than a release-like tarball, but some errors will only be detected when building from a tarball.

**Experimental Testing**: Experimental Code [[Experimental Dashboard](http://testing.sandia.gov/cdash/index.php?project=Trilinos&date=2016-11-30&filtercount=1&showfilters=1&field1=groupname&compare1=61&value1=Experimental)]

The experimental dashboard contains a host of experimental builds where no emails are sent out for failed builds. Some of these experimental builds are run by cron jobs every night. Regularly scheduled builds that are run as experimental builds usually have some issues that result in several failed package builds or test failures and are not ready for "prime time" yet. The goal is to get these builds to a state where they can be moved over to regular "Nightly" builds where emails will be sent out reporting failures.

Other experimental builds are run by individual developers (for running coverage testing for instance). A good way to post experimental builds from a local git repo and build directory is to use the 'dashbaord' target run with [`make dashboard`](https://trilinos.org/docs/files/TrilinosBuildReference.html#dashboard-submissions).

By default, the test category set for experimental testing is `Trilinos_TEST_CATEGORIES=NIGHTLY` which matches to the `CATEGORIES` options `BASIC` or `NIGHTLY` passed into [`TRIBITS_ADD_TEST()`](https://tribits.org/doc/TribitsDevelopersGuide.html#tribits-add-test) and [`TRIBITS_ADD_ADVANCED_TEST()`](https://tribits.org/doc/TribitsDevelopersGuide.html#tribits-add-advanced-test) in the test CMakeLists.txt files.

**Weekly Coverage Testing**: Secondary Tested Code [[Current coverage results](http://testing.sandia.gov/extended/cdash/index.php?project=Trilinos&display=project&filtercount=2&showfilters=1&filtercombine=and&field1=buildstarttime/date&compare1=83&value1=7%20days%20ago&field2=buildname/string&compare2=66&value2=_COV) (as of last Saturday)]

**NOTE: REGULAR COVERAGE TESTING IS CURRENTLY DISABLED!!!**

Every Saturday morning, a set of coverage tests is run after the other nightly tests are finished. An MPI and a SERIAL build are performed where the coverage for each package is accumulated after running each of the package's test suite.

Note that this coverage will typically be lower than the actual coverage after running the rest the downstream package tests that test more functionality. Currently, we are not accumulated these higher coverage numbers. Trilinos needs to strive to improve the coverage of the native packages test suites for each and every package.

The test category set for coverage testing is `Trilinos_TEST_CATEGORIES=NIGHTLY` which matches to the `CATEGORIES` options `BASIC` or `NIGHTLY` passed into [`TRIBITS_ADD_TEST()`](https://tribits.org/doc/TribitsDevelopersGuide.html#tribits-add-test) and [`TRIBITS_ADD_ADVANCED_TEST()`](https://tribits.org/doc/TribitsDevelopersGuide.html#tribits-add-advanced-test) in the test CMakeLists.txt files.

Note that coverage results are reported to a separate dashboard. Developers requiring access to coverage results from outside of Sandia should send a note to the trilinos-framework mail list.

**Memory Testing**: Most Secondary Tested Code [ [Current Memory testing Results](https://testing.sandia.gov/cdash/index.php?project=Trilinos&display=project&filtercount=2&showfilters=1&filtercombine=and&field1=buildname/string&compare1=66&value1=_MEMCHECK&field2=buildstarttime/date&compare2=83&value2=1%20days%20ago&collapse=0)]

**NOTE: REGULAR MEMORY/VALGRIND TESTING IS CURRENTLY DISABLED!!!**

Every day a subset of Trilinos packages and tests are run through Valgrind and posed to CDash to find some forms of memory issues. Note that the memory checking tests can be quite long which is the reason why only a portion of memory issues are being checked currently.

Currently the test category used for Memory testing is `Trilinos_TEST_CATEGORIES=NIGHTLY` which matches to the `CATEGORIES` options `BASIC` or` NIGHTLY` passed into [`TRIBITS_ADD_TEST()`](https://tribits.org/doc/TribitsDevelopersGuide.html#tribits-add-test) and [`TRIBITS_ADD_ADVANCED_TEST()`](https://tribits.org/doc/TribitsDevelopersGuide.html#tribits-add-advanced-test) in the test CMakeLists.txt files.

**Weekly Scalability Testing**: Secondary Tested Code

**NOT DEFINED YET:** There is currently no support for automated scalability testing in the Trilinos CMake/CTest/CDash system.