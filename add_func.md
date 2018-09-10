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

* include
  * af/\<domain\>.h
    * If you are working on a function that already belongs to an existing
      group such as image processing, statistics etc. add your declaration
      to that group.
    * Otherwise, create a header and include it in include/arrayfire.h
    * Add both the C and C++ declarations for your new function.
    * Prefix the C function name with "af_".
    * If the C++ function has any arguments with default values, initialize
      those arguments in this declaration
    * Add the C and C++ function's documentation here (Doxygen style)

  * arrayfire.h
    * Include your new af/\<domain\>.h header file here if you do create a
      new one

* src
  * api
    * c
      * exampleFunction.cpp
        * If arrayfire already has all the functions needed for this
          new function, try first to keep all your implementation
          code here rather than adding code to src/backend
      * CMakeLists.txt
        * Add your include/af/\<domain\>.h header and
          exampleFunction.cpp in their appropriate sections here
    * cpp
      * \<domain\>.cpp
        * Most of the time, all you essentially have to do here is
          call the C*API function.
          See the other C++ functions in the same \<domain\>.cpp file
          for examples
      * CMakeLists.txt (should already exist)
        * Add your \<domain\>.cpp file here
    * unified
      * \<domain\>.cpp
        * Contains API delegation from unified level to C*API level
          See the other C++ functions in the same \<domain\>.cpp file
          for examples

      * CMakeLists.txt (should already exist)
        * Add your \<domain\>.cpp file here
 * backend
   * cpu
     * exampleFunction.[hpp|cpp]
       * Wrapper level for the CPU algorithm, which can call your
         own and/or other existing CPU kernels
  
     * kernel
       * exampleFunction.hpp
         * Add your CPU algorithm here
  
     * CMakeLists.txt (should already exist)
       * Add your exampleFunction.[hpp cpp] and
         kernel/exampleFunction.hpp to their appropriate sections
         here
     * cuda
       * exampleFunction.[hpp|cu]
         * Wrapper level for the CUDA algorithm, which can call your
           own and/or other existing CUDA kernels
       * kernel
         * exampleFunction.hpp
         * Add your custom CUDA kernels here
       * CMakeLists.txt (should already exist)
         * Add your exampleFunction.[hpp cu] and
           kernel/exampleFunction.hpp to their appropriate sections
           here
  
     * opencl
       * exampleFunction.[hpp|cpp]
         * Wrapper level for the OpenCL algorithm, which can call your
           own and/or other existing OpenCL kernel wrappers
       * kernel
         * exampleFunction.hpp
           * OpenCL kernel wrappers
           * Note that because of the nature of how OpenCL
             operates, this file does not actually link with the
             exampleFunction.cl file. Rather, this includes
             kernel_headers/exampleFunction.hpp, which contains
             the OpenCL kernel in the form of a runtime char array.
             This char array is passed to OpenCL API calls to
             dispatch the actual OpenCL kernel.
         * exampleFunction.cl (OpenCL Kernel file)
           * Add your actual OpenCL kernels here
           * See the note above about how this file is "linked"
             with exampleFunction.hpp. To support the described
             scheme above, a header file (in kernel_headers/) is
             generated during the build process, which basically
             creates a char array object representation of this
             file.
       * CMakeLists.txt (should already exist)
         * Add your exampleFunction.[hpp cpp] and
           kernel/exampleFunction.hpp to their appropriate sections
           here
* test
  * exampleFunction.cpp
    * Add all your unit tests here. Refer to the "Writing unit tests"
      section of the wiki for this part.
      It is good practice to add a test for the documentation's code
      snippets here, to verify that the public facing sample indeed works
  
    * data
      * exampleFunction
        * <test data name>.test
        * <another test data name>.test
          * Refer to the "Unit test data format" section of the wiki for
            this part 

* docs
  * details/<domain>.dox
    * Add your functions' brief and detailed descriptions here.
      Note that you can add references to code snippets here for
      demonstration purposes, which are actually displayed in the
      description when rendered. Ideally they are part of the test suite
      for the new function(s)
      See existing documentation sections for examples

For example, if you are about to implement median filter feature, copy all the files mentioned above and replace the file name `exampleFunction` with `medianfilter`. Now open the files one after another using your favorite editor starting at the top of the hierarchy and change the content to fit your needs.

**All the exampleFunction.* files shown below are documented to explain how the user facing API (C/C++) talks to each backend and how a new developer might go about to implement a new function in ArrayFire.**

**Important Note:** This is only a simple example of how a function is added to ArrayFire. You may have to modify other files some times depending on the situation. You can always discuss with us through [Gitter](https://gitter.im/arrayfire/arrayfire), our [mailing list](https://groups.google.com/forum/#!forum/arrayfire-users), or [email](mailto:technical@arrayfire.com).
