#### Building on Windows Visual Studio
##### Prerequisites
* Install Visual Studio 2013:
  
  **Note**: `Visual Studio Express` and `Visual Studio 2015` are not supported! 
  Update(2016.6): Now CUDA 8 RC could support VS2015 Update 1

  [Nvidia reference](http://docs.nvidia.com/cuda/cuda-getting-started-guide-for-microsoft-windows/index.html#axzz44vwAc5Qx)

* Install Cuda 7.5:
  Also refer to above link. [download](https://developer.nvidia.com/cuda-downloads)

* Install CMake:
  The latest version is OK. [download](https://cmake.org/) 

* Install OpenCV:
  I installed 3.1, other versions should also work. [download](http://opencv.org/)
  * Run the EXE to extract the files. This EXE does not have an installer. Instead, you put your files where you want, and then add an environment variable
  * Adding the environment variable named "OpenCV_DIR" (no quotes) to the "build" subfolder in the folder where you extracted.(The exact folder you need will have one very important file in it: OpenCVConfig.cmake - this tells CMake which variables to set for you.)
  * Add a dir of "OpenCV binary DLLs" to Windows $PATH.(like f:/software/opencv/build/x64/vc12/bin)
  * If we run CUDA build, Opencv should be recompiled with CUDA options on, as the pre-built version does not support CUDA functions. Options suggested:
    * -DWITH_CUDA=ON -DBUILD_DOCS=OFF -DBUILD_TESTS=OFF -DBUILD_PERF_TESTS=OFF -DBUILD_SHARED_LIBS=OFF -DBUILD_WITH_STATIC_CRT=OFF -DBUILD_opencv_apps=OFF -DWITH_TBB=ON -DWITH_OPENMP=ON -DWITH_IPP=ON

##### Compile the solution
* Use the cmake-gui tool:

  Open the cmake-gui tool and selete the location of **Source and Build**.
  Then press configure to specify the generator: In this case, it's **Visual Studio 12 2013 Win64**.
  Last, press generate.

* Use the cmake command line:

  mkdir build && cd build/ && cmake -G "Visual Studio 12 2013 Win64"

**Done!** Just use Visual Studio to open the project-solution in dir build/ and compile everything.

=======
### Original README.md 

AprilTags library

Detect April tags (2D bar codes) in images; reports unique ID of each
detection, and optionally its position and orientation relative to a
calibrated camera.

See examples/apriltags_demo.cpp for a simple example that detects
April tags (see tags/pdf/tag36h11.pdf) in laptop or webcam images and
marks any tags in the live image.

Ubuntu dependencies:
sudo apt-get install subversion cmake libopencv-dev libeigen3-dev libv4l-dev

Mac dependencies:
sudo port install pkgconfig opencv eigen3

Uses the pods build system in connection with cmake, see:
http://sourceforge.net/p/pods/

Michael Kaess
October 2012

----------------------------

AprilTags were developed by Professor Edwin Olson of the University of
Michigan.  His Java implementation is available on this web site:
  http://april.eecs.umich.edu.

Olson's Java code was ported to C++ and integrated into the Tekkotsu
framework by Jeffrey Boyland and David Touretzky.

See this Tekkotsu wiki article for additional links and references:
  http://wiki.tekkotsu.org/index.php/AprilTags

----------------------------

This C++ code was further modified by
Michael Kaess (kaess@mit.edu) and Hordur Johannson (hordurj@mit.edu)
and the code has been released under the LGPL 2.1 license.

- converted to standalone library
- added stable homography recovery using OpenCV
- robust tag code table that does not require a terminating 0
  (omission results in false positives by illegal codes being accepted)
- changed example tags to agree with Ed Olson's Java version and added
  all his other tag families
- added principal point as parameter as in original code - essential
  for homography
- added some debugging code (visualization using OpenCV to show
  intermediate detection steps)
- added fast approximation of arctan2 from Ed's original Java code
- using interpolation instead of homography in Quad: requires less
  homography computations and provides a small improvement in correct
  detections

todo:
- significant speedup could be achieved by performing image operations
  using OpenCV (Gaussian filter, but also operations in
  TagDetector.cc)
- replacing arctan2 by precomputed lookup table
- converting matrix operations to Eigen (mostly for simplifying code,
  maybe some speedup)
