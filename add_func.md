We have an example function built into the library itself. All you need to do is to copy the files with appropriate file names corresponding to the new feature and add your implementation.

Given below is the layout of the example function inside the library.

    include
    |__ af/ <domain.h> - If you are working on a function that already belongs 
                         to an existing group such as image processing, 
                         statistics etc. add your declaration to that group. 
                         Otherwise, create a header and include it in the 
                         file `include/arrayfire.h`
                       - Don't forget to add both the C and C++ declarations
                         for your new function
                       - Prefix the C function name with "af_"
                       - If the C++ declaration has any arguments with default
                         values, declare those values here

    src
     |__ api
     |    |__ c/ exampleFunction.cpp
     |    |   - If arrayfire already has all the functions needed for this new function, try first
     |    |     to keep all your implementation code here rather than adding code to backend/
     |    |
     |    |__ cpp/ <header_name>.cpp
     |    |   - Most of the time, all you essentially have to do here is call the C-API function.
     |    |     See the other C++ functions in the file for examples
     |    |
     |    |__ unified/ <header_name>.cpp (contains API delegation from unified level to C-API level)
     |
     |__backend
          |__ cpu
          |    |__ exampleFunction.[hpp|cpp] (Wrapper level for CPU Algorithm)
          |    |__ kernel
          |          |__ exampleFunction.hpp (CPU Backend core algorithm)
          |
          |__ cuda
          |    |__ exampleFunction.[hpp|cu]
          |    |__ kernel
          |          |__ exampleFunction.hpp (CUDA kernels & wrapper function)
          |
          |__ opencl
               |__ exampleFunction.[hpp|cpp]
               |__ kernel
                     |__ exampleFunction.hpp (OpenCL kernel wrapper function)
                     |__ example.cl (OpenCL Kernel file)

For example, if you are about to implement median filter feature, copy all the files mentioned above and replace the file name `exampleFunction` with `medianfilter`. Now open the files one after another using your favorite editor starting at the top of the hierarchy and change the content to fit your needs.

**All the exampleFunction.* files shown below are documented to explain how the user facing API (C/C++) talks to each backend and how a new developer might go about to implement a new function in ArrayFire.**

**Important Note:** This is only a simple example of how a function is added to ArrayFire. You may have to modify other files some times depending on the situation. You can always discuss with us through [Gitter](https://gitter.im/arrayfire/arrayfire), our [mailing list](https://groups.google.com/forum/#!forum/arrayfire-users), or [email](mailto:technical@arrayfire.com).