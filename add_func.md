We have an example function built into the library itself. All you need to do is to copy the files with appropriate file names corresponding to the new feature and add your implementation.

Given below is the layout of the example function inside the library.

    include
     |__ af/<domain>.h
     |    - If you are working on a function that already belongs to an existing
     |      group such as image processing, statistics etc. add your declaration
     |      to that group.
     |    - Otherwise, create a header and include it in include/arrayfire.h
     |    - Add both the C and C++ declarations for your new function.
     |    - Prefix the C function name with "af_".
     |    - If the C++ function has any arguments with default values, initialize
     |      those arguments in this declaration
     |    - Add the C and C++ function's documentation here (Doxygen style)
     |
     |__ arrayfire.h
         - Include your new af/<domain>.h header file here if you do create a
           new one

    src
     |__ api
     |    |__ c
     |    |    |__ exampleFunction.cpp
     |    |    |    - If arrayfire already has all the functions needed for this
     |    |    |      new function, try first to keep all your implementation
     |    |    |      code here rather than adding code to src/backend
     |    |    |
     |    |    |__ CMakeLists.txt (should already exist)
     |    |         - Add your include/af/<domain>.h header and
     |    |           exampleFunction.cpp in their appropriate sections here
     |    |
     |    |__ cpp
     |    |    |__ <domain>.cpp
     |    |    |    - Most of the time, all you essentially have to do here is
     |    |    |      call the C-API function.
     |    |    |    - See the other C++ functions in the same <domain>.cpp file
     |    |    |      for examples
     |    |    |
     |    |    |__ CMakeLists.txt (should already exist)
     |    |         - Add your <domain>.cpp file here
     |    |
     |    |__ unified
     |         |__ <domain>.cpp
     |         |    - Contains API delegation from unified level to C-API level
     |         |    - See the other C++ functions in the same <domain>.cpp file
     |         |      for examples
     |         |
     |         |__ CMakeLists.txt (should already exist)
     |              - Add your <domain>.cpp file here
     |
     |__ backend
          |__ cpu
          |    |__ exampleFunction.[hpp|cpp]
          |    |    - Wrapper level for the CPU algorithm
          |    |
          |    |__ kernel
          |    |    |__ exampleFunction.hpp
          |    |         - CPU Backend core algorithm
          |    |
          |    |__ CMakeLists.txt (should already exist)
          |         - Add your exampleFunction.[hpp|cpp] and
          |           kernel/exampleFunction.hpp to their appropriate sections
          |           here
          |
          |__ cuda
          |    |__ exampleFunction.[hpp|cu]
          |    |    - Wrapper level for the CUDA algorithm, which can call your
          |    |      own and/or other existing CUDA kernels
          |    |
          |    |__ kernel
          |    |    |__ exampleFunction.hpp
          |    |         - Add your custom CUDA kernels here
          |    |
          |    |__ CMakeLists.txt (should already exist)
          |         - Add your exampleFunction.[hpp|cu] and
          |           kernel/exampleFunction.hpp to their appropriate sections
          |           here
          |
          |__ opencl
               |__ exampleFunction.[hpp|cpp]
               |    - Wrapper level for the OpenCL algorithm, which can call your
               |      own and/or other existing OpenCL kernel wrappers
               |__ kernel
               |    |__ exampleFunction.hpp
               |    |    - OpenCL kernel wrappers
               |    |    - Note that because of the nature of how OpenCL
               |    |      operates, this file does not actually link with the
               |    |      exampleFunction.cl file. Rather, this includes
               |    |      kernel_headers/exampleFunction.hpp, which contains
               |    |      the OpenCL kernel in the form of a runtime char array.
               |    |      This char array is passed to OpenCL API calls to
               |    |      dispatch the actual OpenCL kernel.
               |    |__ exampleFunction.cl (OpenCL Kernel file)
               |         - Actual OpenCL kernels
               |         - See the note above about how this file is "linked"
               |           with exampleFunction.hpp. To support the described
               |           scheme above, a header file (in kernel_headers/) is
               |           generated during the build process, which basically
               |           creates a char array object representation of this
               |           file.
               |__ CMakeLists.txt (should already exist)
                    - Add your exampleFunction.[hpp|cpp] and
                      kernel/exampleFunction.hpp to their appropriate sections
                      here

    test
     |__ exampleFunction.cpp
     |   - Put all your unit tests here. Refer to the "Writing unit tests" section of the wiki                    
     |     for this part
     |__ data
          |__ exampleFunction
               |__ <test_data_name>.test
               |__ <another_test_data_name>.test
                   - Refer to the "Unit test data format" section of the wiki for this part 

For example, if you are about to implement median filter feature, copy all the files mentioned above and replace the file name `exampleFunction` with `medianfilter`. Now open the files one after another using your favorite editor starting at the top of the hierarchy and change the content to fit your needs.

**All the exampleFunction.* files shown below are documented to explain how the user facing API (C/C++) talks to each backend and how a new developer might go about to implement a new function in ArrayFire.**

**Important Note:** This is only a simple example of how a function is added to ArrayFire. You may have to modify other files some times depending on the situation. You can always discuss with us through [Gitter](https://gitter.im/arrayfire/arrayfire), our [mailing list](https://groups.google.com/forum/#!forum/arrayfire-users), or [email](mailto:technical@arrayfire.com).
