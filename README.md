The Open Motion Planning Library (OMPL)
=======================================

Linux / macOS [![Build Status](https://travis-ci.org/ompl/ompl.svg?branch=main)](https://travis-ci.org/ompl/ompl)
Windows [![Build status](https://ci.appveyor.com/api/projects/status/valuv9sabye1y35n/branch/main?svg=true)](https://ci.appveyor.com/project/mamoll/ompl/branch/main)

Visit the [OMPL installation page](https://ompl.kavrakilab.org/core/installation.html) for
detailed installation instructions.

OMPL has the following required dependencies:

* [Boost](https://www.boost.org) (version 1.58 or higher)
* [CMake](https://www.cmake.org) (version 3.5 or higher)
* [Eigen](http://eigen.tuxfamily.org) (version 3.3 or higher)

The following dependencies are optional:

* [ODE](http://ode.org) (needed to compile support for planning using the Open Dynamics Engine)
* [Py++](https://github.com/ompl/ompl/blob/main/doc/markdown/installPyPlusPlus.md) (needed to generate Python bindings)
* [Doxygen](http://www.doxygen.org) (needed to create a local copy of the documentation at
  https://ompl.kavrakilab.org/core)

Once dependencies are installed, you can build OMPL on Linux, macOS,
and MS Windows. Go to the top-level directory of OMPL and type the
following commands:

    mkdir -p build/Release
    cd build/Release
    cmake ../..
    # next step is optional
    make -j 4 update_bindings # if you want Python bindings
    make -j 4 # replace "4" with the number of cores on your machine

Build ompl on ubuntu 20.04 but within a python 3.9 conda environment:
```
macro(find_boost_python)
    if (PYTHON_FOUND)
        foreach(_bp_libname
            "python-py${PYTHON_VERSION_MAJOR}${PYTHON_VERSION_MINOR}"
            "python${PYTHON_VERSION_MAJOR}${PYTHON_VERSION_MINOR}"
            "python${PYTHON_VERSION_MAJOR}" "python")
            string(TOUPPER ${_bp_libname} _bp_upper)
            set(_Boost_${_bp_upper}_HEADERS "boost/python.hpp")
            find_package(Boost COMPONENTS ${_bp_libname} PATHS "${PYTHON_PREFIX}/lib") # path to the lib folder in python, so it does not try to find the apt-installed boost.python
            set(_bplib "${Boost_${_bp_upper}_LIBRARY}")
            if (_bplib)
                set(Boost_PYTHON_LIBRARY "${_bplib}")
                break()
            endif()
        endforeach()
    endif()
endmacro(find_boost_python)

cmake ../../ -DPYTHON_EXEC=`which python` -DOMPL_BUILD_PYBINDINGS=1 -DBoost_USE_DEBUG_RUNTIME=FALSE
```