For machines that are set up with SEMS development environment modules (e.g. usually nfs mounted or rsynced under `/project/sems/`), Trilinos has built-in support for easy automatic configuration.  Just source the script [load_sems_dev_env.sh](https://github.com/trilinos/Trilinos/blob/develop/cmake/load_sems_dev_env.sh) as:

```
$ cd <some-build-dir>/
$ source $TRILINOS_DIR/cmake/load_sems_dev_env.sh
``` 

(where `TRILINOS_DIR` points to the base Trilinos git repo source dir, e.g. `$HOME/Trilinos`).  This does a `module purge` then loads SEMS modules for compilers, MPI, CMake, Python, and several key TPLs that can be used by Trilinos like Boost, Zlib, HDF5, Netcdf, ParMETIS, and SuperLU (run `module list` to see what is loaded).

Once a SEMS Dev Env has been loaded, then one can configure, build, and test as (for example):

```
$ cmake \
  -DBUILD_SHARED_LIBS=ON \
  -DCMAKE_BUILD_TYPE=DEBUG \
  -DTPL_ENABLE_MPI=ON \
  -DTrilinos_ENABLE_<PKG0>=ON \
  -DTrilinos_ENABLE_<PKG1>=ON \
  $TRILNOS_DIR
$ make -j10
$ ctest -j10
 ```

The search paths for all of the TPLs supported by the loaded SEMS Dev Env will automatically be set (all that is needed is to enable the TPLs).  The same loaded SEMS Dev Env can be used for MPI or serial (non-MPI) builds and shared lib or static lib builds.  The SEMS Dev Env only needs to be changed when one wants a different compiler/version and/or MPI/version and/or CMake/version using, for example:

```
$ source $TRILINOS_DIR/cmake/load_sems_dev_env.sh \
   sems-gcc/5.3.0  sems-openmpi/1.10.1  sems-cmake/3.3.2
```

Different compilers (GCC, Clang, Intel) and several versions of each can be selected.  Different versions of OpenMPI and CMake can also be selected (see modules starting with `sems-` from `module avail`).  See the default versions for each of these and more documentation in the script [load_sems_dev_env.sh](https://github.com/trilinos/Trilinos/blob/develop/cmake/load_sems_dev_env.sh) itself.  The defaults for GCC, OpenMPI, and CMake selected in the script provide the best combination of build speed, error checking, and portability checking (see [Trilinos Issue #158](https://github.com/trilinos/Trilinos/issues/158) for more background).

NOTES:

* **WARNING:** Sourcing `load_sems_dev_env.sh` clears out any modules one may have set before loading the new SEMS modules.  Therefore, to avoid messing up your current env, one may want to start a new shell.

* The Trilinos CMake configure is set up to look for the env var `TRILINOS_SEMS_DEV_ENV_LOADED` that is set by the `load_sems_dev_env.sh` script.  If it finds it, then it will automatically include the file [SEMSDevEnv.cmake](https://github.com/trilinos/Trilinos/blob/develop/cmake/std/sems/SEMSDevEnv.cmake) which then reads in all of the information from the env to set the compilers, MPI implementation and the location of the various TPLs.  In this case, it prints to the CMake output something like:
  ```
  -- SEMS: Env var TRILINOS_SEMS_DEV_ENV_LOADED={sems-gcc/4.7.2 sems-openmpi/1.6.5 sems-cmake/3.5.2} => Allowing load of SEMS Build Env by default!
  -- Trilinos_USE_BUILD_ENV='SEMS'
  -- SEMS: Loading SEMSDevEnv.cmake to set compilers and TPL paths (To skip, set -DTrilinos_USE_BUILD_ENV=) ...
  ```

* If the script `load_sems_dev_env.sh` is not used, then one can load the various SEMS modules manually and then configure with `-DTrilinos_USE_BUILD_ENV=SEMS`.  This results in the loading of the file [SEMSDevEnv.cmake](https://github.com/trilinos/Trilinos/blob/develop/cmake/std/sems/SEMSDevEnv.cmake).  In this case, it prints to the CMake output something like:
  ```
  -- Trilinos_USE_BUILD_ENV='SEMS'
  -- SEMS: Loading SEMSDevEnv.cmake to set compilers and TPL paths (To skip, set -DTrilinos_USE_BUILD_ENV=) ...
  ```

* To unload a loaded dev env and get back to no loaded modules, source the script [unload_sems_dev_env.sh](https://github.com/trilinos/Trilinos/blob/develop/cmake/unload_sems_dev_env.sh).  This will do `module purge` and then wipe out `TRILINOS_SEMS_DEV_ENV_LOADED`.

<a name="load_ci_sems_dev_env.sh"/>
* A standard pre-push CI Dev Env  based on `load_sems_dev_env.sh` is selected using the source script [load_ci_sems_dev_env.sh](https://github.com/trilinos/Trilinos/blob/develop/cmake/load_ci_sems_dev_env.sh).  See [checkin-test-sems.sh](https://github.com/trilinos/Trilinos/wiki/Policies-%7C-Safe-Checkin-Testing).

* On Mac OSX, this script will load an env but it loads a different env from Linux and Trilinos currently does not build with that env.  This script can really only be used for Linux machines with the SEMS env and `sh` compatible shells (e.g. `bash`, sorry no (t)csh versions are currently available).