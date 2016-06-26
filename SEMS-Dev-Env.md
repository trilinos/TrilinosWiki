For machines that are set up with SEMS development environment modules (e.g. usually nfs mounted under `/project/sems/`), Trilinos has built-in support for easy automatic configuration.  Just source the script [load_sems_dev_env.sh](https://github.com/trilinos/Trilinos/blob/develop/cmake/load_sems_dev_env.sh) as:

```
$ cd <some-build-dir>/
$ source $TRILINOS_DIR/cmake/load_sems_dev_env.sh
``` 

(where `TRILNIOS_DIR` points to the base Trilinos git repo source dir, e.g. `$HOME/Trilinos`).  This loads the default compiler, MPI, CMake, Python, and many TPLs that can be used by Trilinos like Boost, Zlib, HDF5, and Netcdf (run `module list` to see what is loaded).  Different compilers (GCC, Clang, Intel) and several versions of each can be selected.  Different versions of OpenMPI and CMake can also be selected (see modules starting with `sems-` from `module avail`).  See the default versions for each of these and more documentation in the script [load_sems_dev_env.sh](https://github.com/trilinos/Trilinos/blob/develop/cmake/load_sems_dev_env.sh) itself.  The defaults for GCC, OpenMPI, and CMake selected in the script provide the best combination of build speed, error checking, and portability checking (see [Trilinos Issue #158](https://github.com/trilinos/Trilinos/issues/158) for more background).

Once a SEMS Dev Env has been loaded, then one can configure, build, and test with (for example):

```
$ cmake \
  -DBUILD_SHARED_LIBS=ON \
  -DCMAKE_BUILD_TYPE=DEBUG \
  -DTPL_ENABLE_MPI=ON \
  -DTrilinos_ENABLE_<PKG0>=ON \
  -DTrilinos_ENABLE_<PKG1>=ON \
  [other options] \
  $TRILNOS_DIR
$ make -j10
$ ctest -j10
 ```

All of the TPLs supported by the loaded SEMS Dev Env will be automatically picked up and enabled (see `Final set of enabled TPLs` in the cmake STDOUT).  The same loaded SEMS Dev Env can be used for MPI or serial (non-MPI) builds and shared lib or static lib builds.  The SEMS Dev Env only needs to be changed when one wants a different compiler/version and/or MPI/version and/or CMake/version, etc.

NOTES:
* If the script `load_sems_dev_env.sh` is not used, then one can load the various modules manually and then configure with `-DTrilinos_USE_SEMS_DEV_ENV=TRUE`.   
