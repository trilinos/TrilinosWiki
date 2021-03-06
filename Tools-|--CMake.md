# Tools - CMake

#### Recommended CMake version: 3.3.2 (patched)

The patched version of CMake 3.3.2 installed using:

```
Trilinos/cmake/tribits/devtools_install/install-cmake.py --cmake-version=3.3.2 ...
```

is currently the fastest released version of CMake that works with Trilinos.

#### Minimum supported CMake version: 2.8.11

The Trilinos build system is based on CMake. CTest, used for Trilinos testing, is included in the CMake distribution.

### Trilinos CMake Links

+ [CMake homepage](https://cmake.org/)
+ [Building, Testing, and Installing Trilinos](http://trilinos.org/docs/files/TrilinosBuildReference.html)
+ [TriBITS Developers Guide](https://tribits.org/doc/TribitsDevelopersGuide.html)
  + See "Howtos" section for performing common tasks like adding a new package dependency or adding a new TPL
  + For working and tested examplars of how to use TriBITS, browse the [TriBITS Example Project](https://github.com/TriBITSPub/TriBITS/tree/master/tribits/examples/TribitsExampleProject) (which you can search locally in your cloned Trilinos repo under [Trilinos/cmake/tribits/examples/TribitsExampleProject](https://github.com/trilinos/Trilinos/tree/master/cmake/tribits/examples/TribitsExampleProject)).
    + NOTE: Much of the usage of TriBITS in Trilinos is deprecated or non-examplar so Trilinos packages don't always provide good examples of how to use TriBITS.
  + If any of that documentation is lacking, has typos, or has other problems, please create a new issue on the [TriBITS GitHub Site](https://github.com/tribitspub/tribits/issues).
  + If the TriBITS documentation for Trilinos is lacking and now addressing your, please contact the Trilinos Framework team (trilinos-framework@software.sandia.gov) for guidance.
+ [Linking CMake-aware software against Trilinos libraries](http://trilinos.org/docs/files/Finding_Trilinos.txt)

### Installing CMake

An easy way to build and install various versions of CMake from source on many machines is to use the script:

   Trilinos/cmake/tribits/devtools_install/install-cmake.py

For example:

```
$ cd Trilinos/
$ cmake/tribits/devtools_install/install-cmake.py --cmake-version=3.3.2 \
  --install-dir=~/cmake-3.3.2 --parallel=<num-procs> --do-all
$ export PATH=$HOME/cmake-3.3.2/bin:$PATH
```
See `install-cmake.py --help` for more details.  This script will apply patches to certain versions of CMake to address bugs or performance issues.

Another way to install binary versions of CMake is to use the $TrilinosHome/cmake/python/download-cmake.py script. The instructions for using the script can be found by going to the top level Trilinos directory and running:

    cmake/python/download-cmake.py --help

Another installation option is to use a pre-built binary from the [CMake download page](https://cmake.org/download/). The source code can be found on the same page.

Download the latest released installer for your system and install it anywhere you like. For the *.tar.gz files, you can untar them into any directory. Under that path, you'll find cmake, ctest and cpack in the "bin" directory. You may add this location to your PATH environment value for easy command line use if you wish.

You should uninstall your existing installation of CMake first, or put the new version in a different directory than the old version.

### Updating the recommended version of CMake for Trilinos

A Trilinos Framework developer should use the process below to change the recommended version of CMake for Trilinos (and the "release" version that is installed by default when using the download-cmake.py script and used for nightly testing):

First re-familiarize yourself with the process by running:

    cmake/python/download-cmake.py --help

in your Trilinos source tree. The versions of CMake designated as 'min', 'release', 'rc', and 'dev' are controlled by the file cmake/python/CMakeVersions.py.
Running the command:

    cmake/python/download-cmake.py --skip-download --skip-extract --skip-install

generates a new file called "download_area/CMakeVersions.py" in your current directory - the 'rc' and 'dev' numbers in this new file should reflect the latest versions available from www.cmake.org. You may copy this CMakeVersion.py to cmake/python in the Trilinos tree, overwriting the old one.
You should also update the 'release' version number in CMakeVersions.py: 'min' and 'release' numbers are only ever updated manually by a Trilinos developer.

If source code changes in Trilinos require an increase in the minimum version of CMake, you should update both the 'min' version number in CMakeVersions.py and the cmake_minimum_required call in the top level Trilinos CMakeLists.txt file.

When updating the recommended version of CMake, also update the version displayed on the left menu bar of the developer website. When updating the recommended or minimum version, update the version numbers listed at the top of this webpage.

### Version of CMake used for Trilinos testing

All those running Trilinos dashboards should use the TrilinosDriver dashboard to run the Trilinos dashboards. The cron_driver.py file that initiates a TrilinosDriver dashboard automatically downloads the 'dev' version of cmake to drive the TrilinosDriver dashboard cmake script. If you want to use a different version, set TDD_CMAKE_INSTALLER_TYPE in the environment prior to calling cron_driver.py. Then, for each Trilinos dashboard run on that machine, each one downloads and installs its own CMake to use to run and submit the actual Trilinos dashboards. The version used for each dashboard is controlled in the machine's TrilinosDriver CMakeLists.txt file with the CTEST_INSTALLER_TYPE argument to the TRILINOS_DRIVER_ADD_DASHBOARD function.
