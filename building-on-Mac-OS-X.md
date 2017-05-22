# Configuring Mac OS X to Build Trilinos Using Macports

| Author:  | Tim Fuller |
| :------- | :--------- |
| Contact: | @tjfulle   |
| OS X Version: | Instructions have been tested on OS X 10.11 |

## Introduction

One of the largest obstacles to building [Trilinos](https://www.github.com/trilinos/Trilinos) is properly configuring and installing the different third party libraries (TPLs) needed by many of its packages.  On a Linux platform, package management tools such as [`yum`](https://access.redhat.com/solutions/9934) for [redhat](https://access.redhat.com) and its derivatives or [`apt-get`](https://manpages.debian.org/jessie/apt/apt-get.8.en.html) for [Debian](https://debian.org) and its derivatives ease the burden of installing the third party software.  Most unix platforms provide similar package managers, such as [FreeBSD's](https://freebsd.org) [ports](https://www.freebsd.org/doc/handbook/ports.html) system.  Additionaly, many Linux/unix distributions come prepackaged with most of the necessary developer tools and third party libraries needed for building and working with Trilinos.

Mac OS X, on the other hand, does not provide a native package manager (though third party tools such as [Homebrew](https://brew.sh) and [MacPorts](https://www.macports.org) help fill the gap).  In fact, factory builds of Mac OS X do not even provide compilers so that users can build the necessary TPLs.  Developers/users who want to build Trilinos on Mac OS X must first install the Apple provided [Developer Tools](https://developer.apple.com/xcode) (which includes a compiler suite) and any required third party software before building Trilinos.

In this guide, steps for preparing a Mac OS X machine to build Trilinos are presented.  The MacPorts package manager will be employed for third party software management.  A local MacPorts port index will be set up that provides the proper versions/patches for each of the installed TPLs.  The guide finishes with some Mac OS X specific configuration suggestions for building Trilinos.

This guide *does not* provide instructions for building Trilinos.  For instructions on how to build Trilinos, see the [install notes](https://github.com/trilinos/Trilinos/blob/master/INSTALL.rst) and [installation instructions](https://trilinos.org/docs/files/TrilinosBuildReference.html).

### Why MacPorts?

Homebrew seems to have won mindshare over Macports in recent years (see, for example, this article in [slant](https://www.slant.co/topics/511/~best-mac-package-managers)).  However, MacPorts offers a clear advantage over Homebrew by allowing the user to install multiple versions of the same compiler, multiple MPI vendors/versions, and to build third party software against the compiler/mpi combination of their choice.  For help building Trilinos using Homebrew installed TPLs, see the [instructions](https://github.com/spdomin/Nalu/wiki/mac_osx) on the [Nalu](https://github.com/spdomin/Nalu) project website.  For those interested in even more configurability, [spack](https://spack.readthedocs.io/en/latest/) may be the ultimate choice for managing TPLs needed for scientific software.

### A Note for Sandia Developers

For developers with access to [Sandia National Labs'](http://www.sandia.gov) cee lan, SEMs provides compilers and third party libraries that can be used for building Trilinos.  Search for "SEMs" in the internal TechWeb.

## Install the Apple Developer Tools

[Xcode](https://developer.apple.com/xcode/) is the application provided by Apple that contains the necessary suite of tools to develop on OS X (including compilers, libraries, etc.).  Download the latest version of Xcode using the [Mac App Store](https://itunes.apple.com/us/app/xcode/id497799835).  After Xcode is installed, run `xcode-select --install` in a terminal (`/Applications/Utilities/Terminal.app`).  Click the `Install` button to install the required command line developer tools.

See the [MacPorts guide](https://guide.macports.org/chunked/installing.html#installing.xcode) for more detailed instructions.

## Install MacPorts

Download and run the [MacPorts OS X package installer](https://www.macports.org/install.php).  See the MacPorts [installation guide](https://guide.macports.org/chunked/installing.macports.html) for complete instructions.

## Set Up a Local Port Index

A local port index is a local set of instructions for building specific third party software using MacPorts.  The set of ports needed for building Trilinos are housed at the authors' [ports](https://github.com/tjfulle/ports.git) github repository.  The included ports came originally from the MacPorts project. In some cases, a port is just a copy of the most recent port without modification. In others, the port is modified to build the version/variant needed by Trilinos. Unmodified Ports are included so that Portfiles for all of the necessary TPLs for Trilinos live in one place.

### Included Ports

* `devel/boost` 1.58.0: At the time of this writing, boost provided by MacPorts is at version 1.59.0. A bug in that version prevents using the variants `+openmpi` and `-no_single`, as needed by Trilinos.
* `devel/yaml-cpp` 0.5.3: The default version of yaml-cpp in the Macports repository is 0.5.1.  This port is still not fully automated.  It *should* get the right tarball from github.  But, it does not.  Consequently, a zipfile from github must be downloaded and placed in `/opt/local/var/macports/distfiles/yaml-cpp`.  The downloaded version *must* be the 0.5.3 release version.  Newer versions of `yaml-cpp` require `c++11` and this port has not yet been updated to reflect that change.  The checksums in the `Portfile` will almost assuredly need to be modified to reflect the checksums of the actual downloaded file.

* `math/SuiteSparse` 4.2.1: Port not modified.
* `math/metis` 1.51.0: Port modified to build with `+longindex`, `+double`, and `+openmp` by default
* `math/parmetis` 4.0.3: Port not modified, but uses the modified metis port.
* `math/superlu` 4.3: MacPorts provides v5.2.1 but Trilinos is only guaranteed to work with v4.3
* `math/superlu_dist` 4.3: MacPorts provides a newer version, this version is consistent with superlu 4.3

* `science/hdf5` 1.10: Fixes for fortran and parallel variants
* `science/pnetcdf` 1.7.0: Build of parallel-netcdf, needed for `+openmpi` variant of `netcdf`
* `science/hdf5-18` 1.8.17: Fixes for fortran and parallel variants to be consistent with netcdf. Other options as required by [seacas](https://github.com/gsjaardema/seacas) are turned on by default.
* `science/netcdf` 4.4.1: Modified to properly build parallel netcdf for `+openmpi` variant.  `NC_MAX_VARS` and `NC_MAX_DIMS` modified for use in [seacas](https://github.com/gsjaardema/seacas).
* `science/scotch`: `+longindex` variant turned on by default to be consistent with `metis`.

### Installation

*In the following, assume the `ports` directory was cloned to `/Users/user/Documents/ports` and MacPorts is installed in `opt/local`. Adjust paths as necessary for your system.*

#### Clone the Local Ports

```sh
cd /Users/user/Documents
git clone https://www.github.com/tjfulle/ports
```

#### Add the directory to MacPorts `sources.conf` configuration file

Add the line

```sh
file:///Users/user/Documents/ports
```

*before* the default `rsync://rsync.macports.org/release/tarballs/ports.tar` in `/opt/local/etc/macports/sources.conf`.  See [Installing Local Ports](https://guide.macports.org/chunked/development.local-repositories.html).

Now add the ports to the port index:

```sh
cd /Users/user/Documents/ports
/opt/local/bin/portindex
```

## Installing the Local Ports

The modified ports should appear first in the port index and will take precedence over the ports packaged with MacPorts.  Ports can be installed by the standard

```
port install <port name> [port variants]
```

The following script will install the `gcc` compiler (version 6.3), the default version of `openmpi` (version 1.10.1 at the time of this writing), and each of the above listed ports.  As you can see, several of the ports are built with and without mpi.  After installation, to switch back and forth between versions use `port activate <port name> @<port version>`.

```sh
#!/bin/sh
set -x

port=/opt/local/bin/port

# Set the compiler vendor, major, and minor versions.
# If clang, the compiler name needs to be modified
compiler_vendor=gcc
compiler_major_version=6
compiler_minor_version=3
if [ $compiler_vendor == clang ]; then
    compiler=$compiler_vendor$compiler_major_version$compiler_minor_version
else
    compiler=$compiler_vendor$compiler_major_version
fi

# Compiler dependent ports are: superlu, boost, metis, hdf5, netcdf, SuiteSparse
$port install $compiler || exit
$port install superlu +$compiler || exit
$port install boost +$compiler configure.compiler=macports-$compiler_vendor-$compiler_major_version || exit
$port install yaml-cpp +$compiler || exit
$port install metis +$compiler || exit
$port install hdf5-18 +$compiler +fortran || exit
$port install netcdf +$compiler || exit
$port install hdf5 +$compiler +fortran || exit
$port install SuiteSparse +$compiler || exit

# OPENMPI Ports installed are: openmpi, boost, parmetis, scotch, hdf5, pnetcdf, netcdf
mpi=openmpi
$port install $mpi-$compiler +threads || exit
$port install boost +$compiler +$mpi configure.compiler=macports-$compiler_vendor-$compiler_major_version configure.mpi=$mpi-$compiler-fortran || exit
$port install yaml-cpp +$compiler +$mpi || exit
$port install parmetis +$compiler +$mpi || exit
$port install scotch +$compiler +$mpi || exit
$port install hdf5-18 +$compiler +fortran +$mpi || exit
$port install pnetcdf +$compiler +$mpi || exit
$port install netcdf +$compiler +$mpi || exit
$port install hdf5 +$compiler +fortran +$mpi || exit
```

## Install Trilinos

If the installation of the TPLs proceded successfully, Trilinos can now be installed.  See the [install notes](https://github.com/trilinos/Trilinos/blob/master/INSTALL.rst) and [installation instructions](https://trilinos.org/docs/files/TrilinosBuildReference.html).

Using the above outlined TPLs, the author has successfully built Trilinos and many applications requiring Trilinos (like [Albany](https://github.com/gahansen/Albany)).  Cmake and the [TriBITs](https://tribits.org) build system used by Trilinos take care of many of the Mac OS X specific build instructions.  However, at the time of this writing the author has had the following issues:

- The [stk](https://trilinos.org/packages/stk/) package fails to build dynamic libraries.
- Many tests fail.
