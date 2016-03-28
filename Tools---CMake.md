# Tools - CMake

#### Recommended CMake version: 2.8.4
#### Minimum supported CMake version: 2.8.0

The Trilinos build system is based on CMake. CTest, used for Trilinos testing, is included in the CMake distribution.

### CMake Links

+ [CMake homepage](https://cmake.org/)
+ [Building Trilinos](https://trilinos.org/oldsite/TrilinosBuildQuickRef.html)
+ [Linking CMake-aware software against Trilinos libraries](https://trilinos.org/oldsite/Finding_Trilinos.txt)

### Installing CMake
An easy way to install CMake is to use the $TrilinosHome/cmake/python/download-cmake.py script. The instructions for using the script can be found by going to the top level Trilinos directory and running:

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
