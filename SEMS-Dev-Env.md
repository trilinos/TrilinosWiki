For machines that have access to the SEMS development environment, Trilinos has easy built-in support for taking advantage of this env.

Just source the script [load_sems_dev_env.sh](https://github.com/trilinos/Trilinos/blob/develop/cmake/load_sems_dev_env.sh) as:

```
$ source $TRILINOS_DIR/cmake/load_sems_dev_env.sh
``` 

This loads the default compiler, MPI, CMake, Python, and many TPLs that can be used for Trilinos like Boost, Zlib, HDF5, and Netcdf.  Different compilers and versions of the compiler and OpenMPI can be selected.  See the default compilers and versions of everything and more documentation at the top of the script [load_sems_dev_env.sh](https://github.com/trilinos/Trilinos/blob/develop/cmake/load_sems_dev_env.sh).  (Also see [Trilinos Issue #158](https://github.com/trilinos/Trilinos/issues/158) for background.)

Once a SEMS Dev Evn has been loaded, one can configure, build, and test simply with:

```
$ cmake \
  -DBUILD_SHARED_LIBS=ON \
  -DCMAKE_BUILD_TYPE=DEBUG \
  -DTPL_ENABLE_MPI \
  -DTrilinos_ENABLE_<PKG0>=ON \
  -DTrilinos_ENABLE_<PKG0>=ON \
  [...] \
  $TRILNOS_HOME
$ make -j10
$ ctest -j10
 ```

All of the TPLs supported by the loaded SEMS Dev Env will be automatically picked up and enabled.

Also, the same loaded SEMS Dev Env can be used for MPI and serial (non-MPI) builds and shared and static lib builds.  The SEMS Dev Env only needs to be changed when wants a different compiler/version and/or MPI/version and/or CMake/version, etc.
