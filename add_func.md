We have an example function built into the library itself. All you need to do is to copy the files with appropriate file names corresponding to the new feature and add your implementation.

Given below is the layout of the example function inside the library.

### Quick layout:

    include
     |__ af/<domain>.h
     |__ arrayfire.h

    src
     |__ api
     |    |__ c
     |    |    |__ exampleFunction.cpp
     |    |    |__ CMakeLists.txt (should already exist)
     |    |
     |    |__ cpp
     |    |    |__ <domain>.cpp
     |    |    |__ CMakeLists.txt (should already exist)
     |    |
     |    |__ unified
     |         |__ <domain>.cpp
     |         |__ CMakeLists.txt (should already exist)
     |
     |__ backend
          |__ cpu
          |    |__ exampleFunction.[hpp|cpp]
          |    |__ kernel
          |    |    |__ exampleFunction.hpp
          |    |
          |    |__ CMakeLists.txt (should already exist)
          |
          |__ cuda
          |    |__ exampleFunction.[hpp|cu]
          |    |
          |    |__ kernel
          |    |    |__ exampleFunction.hpp
          |    |
          |    |__ CMakeLists.txt (should already exist)
          |
          |__ opencl
               |__ exampleFunction.[hpp|cpp]
               |__ kernel
               |    |__ exampleFunction.hpp
               |    |__ exampleFunction.cl (OpenCL Kernel file)
               |
               |__ CMakeLists.txt (should already exist)

    test
     |__ exampleFunction.cpp
     |__ data
          |__ exampleFunction

### Detailed Layout

#### Include

* `include/af/<domain>.h`
  * Contains the C and C++ declarations of your new function
  * If you are working on a function that already belongs to an existing
    group such as image processing, statistics etc. add your declaration
    to that group. Otherwise, create a header and include it in include/arrayfire.h
  * Add both the C and C++ declarations for your new function.
  * Prefix the C function name with `af_`.
  * If the C++ function has any arguments with default values, initialize
    those arguments here
  * Add the C and C++ function's documentation here (Doxygen style)

* `include/arrayfire.h`
  * Contains the includes for the domain header files
  * Include your new af/\<domain\>.h header file here if you do create a
    new one

#### C API

* `src/api/c/exampleFunction.cpp`
  * Contains the definition of the function's C API.
  * If arrayfire already has all the functions needed for this new function,
    **try first to keep all your implementation code here** rather than adding
    code to `src/backend`

* `src/api/c/CMakeLists.txt`
  * Add your `include/af/<domain>.h` header and
    `src/api/c/exampleFunction.cpp` in their appropriate sections here

#### C++ API

* `src/api/cpp/<domain>.cpp`
  * Contains the definition of the function's C++ API. Most of the time, all you
    essentially have to do here is call the C API function.
  * See the other C++ functions in the same `<domain>.cpp` file
    for examples

* `src/api/cpp/CMakeLists.txt` (should already exist)
  * Add your `<domain>.cpp` file here

#### Unified Backend

* `src/api/cpp/unified/<domain>.cpp`
  * Contains API delegation from unified level to C API level
  * See the other C++ functions in the same `<domain>.cpp` file
    for examples

* `src/api/cpp/unified/CMakeLists.txt` (should already exist)
  * Add your \<domain\>.cpp file here

#### CPU Backend 

* `src/backend/cpu/exampleFunction.[hpp|cpp]`
  * Wrapper level for the CPU algorithm, which can call your
    own and/or other existing CPU kernels
  * You can write the whole implementation here if all the necessary
    kernels already exist
  
* `src/backend/cpu/kernel/exampleFunction.hpp`
  * Contains the CPU-specific implementation of the algorithm here
  
* `src/backend/cpu/CMakeLists.txt` (should already exist)
  * Add your exampleFunction.[hpp|cpp] and
    `kernel/exampleFunction.hpp` to their appropriate sections
    here

#### CUDA Backend 

* `src/backend/cuda/exampleFunction.[hpp|cu]`
  * Wrapper level for the CUDA algorithm, which can call your
    own and/or other existing CUDA kernels
  * You can write the whole implementation here if all the necessary
    kernels already exist

* `src/backend/cuda/kernel/exampleFunction.hpp`
  * Contains the CUDA-specific implementation of the algorithm here

* `src/backend/cuda/CMakeLists.txt` (should already exist)
  * Add your exampleFunction.[hpp|cu] and
    `kernel/exampleFunction.hpp` to their appropriate sections
    here

#### OpenCL Backend 

* `src/backend/opencl/exampleFunction.[hpp|cpp]`
  * C++ wrapper level for the OpenCL kernel wrappers, which can call your
    own and/or other existing OpenCL kernel wrappers
  * You can write the whole implementation here if all the necessary
    OpenCL kernel wrappers already exist

* `src/backend/opencl/kernel/exampleFunction.hpp`
  * Contains the OpenCL kernel wrappers
  * Note that because of the nature of how OpenCL operates, this file does not
    actually link with the exampleFunction.cl file. Rather, this includes
    kernel_headers/exampleFunction.hpp, which contains the OpenCL kernel in the
    form of a runtime char array. This char array is passed to OpenCL API calls
    to dispatch the actual OpenCL kernel.

* `src/backend/opencl/kernel/exampleFunction.cl` (OpenCL Kernel file)
  * Contains the actual OpenCL kernels
  * See the note above about how this file is "linked" with exampleFunction.hpp.
    To support the described scheme above, a header file (in `kernel_headers/`)
    is generated during the build process, which basically creates a char array
    object representation of this file.

* `src/backend/opencl/CMakeLists.txt` (should already exist)
  * Add your exampleFunction.[hpp|cpp] and kernel/exampleFunction.hpp to their
    appropriate sections here

#### Tests 

* `test/exampleFunction.cpp`
  * Contains unit tests for the new function. Refer to the
    [Writing unit tests](https://github.com/arrayfire/arrayfire/wiki/Writing-Unit-Tests)
    section of the wiki for this part.
  * It is good practice to add a test for the documentation's code
    snippets here, to verify that the public-facing example indeed works
  
* `test/data/exampleFunction/<test_data_name>.test`
  * Refer to the
    [Unit test data format](https://github.com/arrayfire/arrayfire/wiki/Unit-Tests-Data-Format)
    section of the wiki for this part. You can have multiple test files if you need

#### Documentation 

* `docs/details/<domain>.dox`
  * Contains the Doxygen markup for the domain's functions
  * Add your functions' brief and detailed descriptions here.
    Note that you can add references to code snippets here for
    demonstration purposes, which are actually displayed in the
    description when rendered. Ideally they are part of the test suite
    for the new function(s)
  * See existing documentation sections for examples

For example, if you are about to implement median filter feature, copy all the files mentioned above and replace the file name `exampleFunction` with `medianfilter`. Now open the files one after another using your favorite editor starting at the top of the hierarchy and change the content to fit your needs.

**All the exampleFunction.\* files shown below are documented to explain how the user facing API (C/C++) talks to each backend and how a new developer might go about to implement a new function in ArrayFire.**

**Important Note:** This is only a simple example of how a function is added to ArrayFire. You may have to modify other files some times depending on the situation. You can always discuss with us through [Gitter](https://gitter.im/arrayfire/arrayfire), our [mailing list](https://groups.google.com/forum/#!forum/arrayfire-users), or [email](mailto:technical@arrayfire.com).
