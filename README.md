# Install caffe (Python3) on Ubuntu/Mac OS

The [official installation guide](http://caffe.berkeleyvision.org/installation.html) lacks of configuration details regarding Python3. But it serves as a useful basic reference before and after we modify the Makefile.config and CmakeCache.txt. We need to install the dependent libraries as instructed in the "Ubuntu Installation" link and python dependencies as instructed in the "Python and/or MATLAB Caffe (optional)" part.

## Installation on Ubuntu 16.04 (Python 3.5 with GPU)

### Installation Environment 

Ubuntu: 16.04

Python: 3.5.2

OpenCV: 3.4.2

CUDA: 8.0

### 1. Install Dependencies

```
$ sudo apt-get install libprotobuf-dev libleveldb-dev libsnappy-dev libopencv-dev libhdf5-serial-dev protobuf-compiler
$ sudo apt-get install --no-install-recommends libboost-all-dev
```

CUDA 8 is required on Ubuntu 16.04.

### 2. Install Caffe

#### Download Caffe

Type in the following commands in Terminal:

```
$ cd ~/
$ git clone https://github.com/BVLC/caffe.git
$ cd caffe/
$ cp Makefile.config.example Makefile.config
```

#### Modify `Makefile.config`

Type in the following commands in Terminal to open the file `Makefile.config`:

```
$ cd ~/caffe/
```

Use your favourite code editer to open `Makefile.config`, modify the code following the instructions below:

1. Uncomment `USE_CUDNN`;
2. Uncomment `USE_OPENCV` and change its value to 1;
3. Uncomment `USE_HDF5` and change its value to 1;
4. Uncomment `OPENCV_VERSION`;
5. According to your python3 library path, replace the library path.
6. Uncomment `WITH_PYTHON_LAYER`;

Sample code (only showing the part modified):

```
# cuDNN acceleration switch (uncomment to build with cuDNN).
USE_CUDNN := 1

# uncomment to disable IO dependencies and corresponding data layers
USE_OPENCV := 1
# USE_LEVELDB := 0
# USE_LMDB := 0
# This code is taken from https://github.com/sh1r0/caffe-android-lib
USE_HDF5 := 1

# Uncomment if you're using OpenCV 3
OPENCV_VERSION := 3

# NOTE: this is required only if you will compile the python interface.
# We need to be able to find Python.h and numpy/arrayobject.h.
# PYTHON_INCLUDE := /usr/include/python2.7 \
#		/usr/lib/python2.7/dist-packages/numpy/core/include

# Uncomment to use Python 3 (default is Python 2)
PYTHON_LIBRARIES := boost_python3 python3.5m
PYTHON_INCLUDE := /usr/include/python3.5m \
				 /usr/local/lib/python3.5/dist-packages/numpy/core/include

# We need to be able to find libpythonX.X.so or .dylib.
PYTHON_LIB := /usr/lib

# Uncomment to support layers written in Python (will link against Python libs)
WITH_PYTHON_LAYER := 1
```

Save the file.

#### Pre-make

Type in the following commands in Terminal:

```
$ cd ~/caffe
$ mkdir build
$ cd build
$ cmake ..
```

#### Modify `CMakeCache.txt`

Use your favourite code editer to open `CMakeCache.txt`; find and modify the following code:

```
//Path to a program.
PYTHON_EXECUTABLE:FILEPATH=/usr/bin/python3.5

//Path to a file.
PYTHON_INCLUDE_DIR:PATH=/usr/include/python3.5

//Path to a library.
PYTHON_LIBRARY:FILEPATH=/usr/lib/x86_64-linux-gnu/libpython3.5m.so

//Flags used by the compiler during all build types.
CMAKE_CXX_FLAGS:STRING=-std=c++11

//Boost python library (debug)
Boost_PYTHON_LIBRARY_DEBUG:FILEPATH=/usr/lib/x86_64-linux-gnu/libboost_python-py35.so

//Boost python library (release)
Boost_PYTHON_LIBRARY_RELEASE:FILEPATH=/usr/lib/x86_64-linux-gnu/libboost_python-py35.so
```

#### Make `caffe` again

```
$ cmake ..
$ sudo make all -j8
$ sudo make pycaffe
```

#### Introduce the path of caffe module to bash profile

Type in the following commands in Terminal to edit `~/.bashrc`.

```
$ vim ~/.bash_profile
```

Add the following command at the end.

```
export PYTHONPATH=~/caffe/python:$PYTHONPATH
```

## Installation on Mac OS Mojave 10.14

Follow the instruction of this [blog][mac-caffe]. 

Minor modifications have been applied according to default installation directory of Python3.7.

## Use Ready Files

This repo contains the actual Makefile.config and CMakeCahe.txt files that work for the compilation, both for Ubuntu 16.04 and MacOS 10.14.

Rename `Makefile.config.xxx.xxx` to `Makefile.config` and place it under the root of caffe folder. 

Pre-make by running the following commands in Terminal.

```
$ $ cd ~/caffe/build
$ cmake ..
```

Rename `CMakeCache.txt.xxx.xxx` to `CMakeCache.txt` and place it under the build subfolder under caffe root folder.

Then run the following commands.

```
$ cmake ..
$ sudo make all -j8
$ sudo make pycaffe
```

## Reference

This installation guide is mainly based on this [blog][mac-caffe].

[mac-caffe]:https://www.dazhuanlan.com/2019/08/15/5d5514f5efcdc/