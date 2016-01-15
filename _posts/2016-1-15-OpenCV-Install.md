---
layout: post
title: OpenCV Install from Github (Ubuntu)
---

- Clone OpenCV repository to your machine.  

``` git clone https://github.com/Itseez/opencv.git ```

>![_config.yml]({{ site.baseurl}}/images/opencv_github.png)

- Go to the root directory.

``` cd /home/path/to/your/opencv ```

- Create a build directory

``` mkdir build ``` 

- Go to the build directory.

``` cd build ```

- Generate the makefile with cmake.

``` cmake .. ```

- Compile with make.

``` make -j4 ``` or ``` make -j8``` 

- Install

``` sudo make install ```

##Example:

```
$ mkdir build
PhD:~/opencv$ cd build/
PhD:~/opencv/build$ cmake ..
-- The CXX compiler identification is GNU 4.9.2
-- The C compiler identification is GNU 4.8.1
-- Check for working CXX compiler: /usr/bin/c++
-- Check for working CXX compiler: /usr/bin/c++ -- works
-- Detecting CXX compiler ABI info
-- Detecting CXX compiler ABI info - done
-- Detecting CXX compile features
-- Detecting CXX compile features - done
-- Check for working C compiler: /usr/bin/cc
-- Check for working C compiler: /usr/bin/cc -- works
-- Detecting C compiler ABI info
-- Detecting C compiler ABI info - done
-- Detecting C compile features
-- Detecting C compile features - done
-- Detected version of GNU GCC: 49 (409)
-- Performing Test HAVE_CXX_FSIGNED_CHAR
-- Performing Test HAVE_CXX_FSIGNED_CHAR - Success
-- Performing Test HAVE_C_FSIGNED_CHAR
-- Performing Test HAVE_C_FSIGNED_CHAR - Success
-- Performing Test HAVE_CXX_W
-- Performing Test HAVE_CXX_W - Success
-- Performing Test HAVE_C_W
-- Performing Test HAVE_C_W - Success
-- Performing Test HAVE_CXX_WALL
-- Performing Test HAVE_CXX_WALL - Success
-- Performing Test HAVE_C_WALL
-- Performing Test HAVE_C_WALL - Success
-- Performing Test HAVE_CXX_WERROR_RETURN_TYPE
-- Performing Test HAVE_CXX_WERROR_RETURN_TYPE - Success
-- Performing Test HAVE_C_WERROR_RETURN_TYPE
-- Performing Test HAVE_C_WERROR_RETURN_TYPE - Success
-- Performing Test HAVE_CXX_WERROR_NON_VIRTUAL_DTOR
-- Performing Test HAVE_CXX_WERROR_NON_VIRTUAL_DTOR - Success
-- Performing Test HAVE_C_WERROR_NON_VIRTUAL_DTOR
-- Performing Test HAVE_C_WERROR_NON_VIRTUAL_DTOR - Success
-- Performing Test HAVE_CXX_WERROR_ADDRESS
-- Performing Test HAVE_CXX_WERROR_ADDRESS - Success
-- Performing Test HAVE_C_WERROR_ADDRESS
-- Performing Test HAVE_C_WERROR_ADDRESS - Success
-- Performing Test HAVE_CXX_WERROR_SEQUENCE_POINT
-- Performing Test HAVE_CXX_WERROR_SEQUENCE_POINT - Success
-- Performing Test HAVE_C_WERROR_SEQUENCE_POINT
-- Performing Test HAVE_C_WERROR_SEQUENCE_POINT - Success
-- Performing Test HAVE_CXX_WFORMAT
-- Performing Test HAVE_CXX_WFORMAT - Success
-- Performing Test HAVE_C_WFORMAT
-- Performing Test HAVE_C_WFORMAT - Success
-- Performing Test HAVE_CXX_WERROR_FORMAT_SECURITY
-- Performing Test HAVE_CXX_WERROR_FORMAT_SECURITY - Success
-- Performing Test HAVE_C_WERROR_FORMAT_SECURITY
-- Performing Test HAVE_C_WERROR_FORMAT_SECURITY - Success
-- Performing Test HAVE_CXX_WMISSING_DECLARATIONS
-- Performing Test HAVE_CXX_WMISSING_DECLARATIONS - Success
-- Performing Test HAVE_C_WMISSING_DECLARATIONS
-- Performing Test HAVE_C_WMISSING_DECLARATIONS - Success
-- Performing Test HAVE_CXX_WMISSING_PROTOTYPES
-- Performing Test HAVE_CXX_WMISSING_PROTOTYPES - Failed
-- Performing Test HAVE_C_WMISSING_PROTOTYPES
-- Performing Test HAVE_C_WMISSING_PROTOTYPES - Success
-- Performing Test HAVE_CXX_WSTRICT_PROTOTYPES
-- Performing Test HAVE_CXX_WSTRICT_PROTOTYPES - Failed
-- Performing Test HAVE_C_WSTRICT_PROTOTYPES
-- Performing Test HAVE_C_WSTRICT_PROTOTYPES - Success
-- Performing Test HAVE_CXX_WUNDEF
-- Performing Test HAVE_CXX_WUNDEF - Success
-- Performing Test HAVE_C_WUNDEF
-- Performing Test HAVE_C_WUNDEF - Success
-- Performing Test HAVE_CXX_WINIT_SELF
-- Performing Test HAVE_CXX_WINIT_SELF - Success
-- Performing Test HAVE_C_WINIT_SELF
-- Performing Test HAVE_C_WINIT_SELF - Success
-- Performing Test HAVE_CXX_WPOINTER_ARITH
-- Performing Test HAVE_CXX_WPOINTER_ARITH - Success
-- Performing Test HAVE_C_WPOINTER_ARITH
-- Performing Test HAVE_C_WPOINTER_ARITH - Success
-- Performing Test HAVE_CXX_WSHADOW
-- Performing Test HAVE_CXX_WSHADOW - Success
-- Performing Test HAVE_C_WSHADOW
-- Performing Test HAVE_C_WSHADOW - Success
-- Performing Test HAVE_CXX_WSIGN_PROMO
-- Performing Test HAVE_CXX_WSIGN_PROMO - Success
-- Performing Test HAVE_C_WSIGN_PROMO
-- Performing Test HAVE_C_WSIGN_PROMO - Failed
-- Performing Test HAVE_CXX_WNO_NARROWING
-- Performing Test HAVE_CXX_WNO_NARROWING - Success
-- Performing Test HAVE_C_WNO_NARROWING
-- Performing Test HAVE_C_WNO_NARROWING - Success
-- Performing Test HAVE_CXX_WNO_DELETE_NON_VIRTUAL_DTOR
-- Performing Test HAVE_CXX_WNO_DELETE_NON_VIRTUAL_DTOR - Success
-- Performing Test HAVE_C_WNO_DELETE_NON_VIRTUAL_DTOR
-- Performing Test HAVE_C_WNO_DELETE_NON_VIRTUAL_DTOR - Failed
-- Performing Test HAVE_CXX_WNO_UNNAMED_TYPE_TEMPLATE_ARGS
-- Performing Test HAVE_CXX_WNO_UNNAMED_TYPE_TEMPLATE_ARGS - Failed
-- Performing Test HAVE_C_WNO_UNNAMED_TYPE_TEMPLATE_ARGS
-- Performing Test HAVE_C_WNO_UNNAMED_TYPE_TEMPLATE_ARGS - Failed
-- Performing Test HAVE_CXX_FDIAGNOSTICS_SHOW_OPTION
-- Performing Test HAVE_CXX_FDIAGNOSTICS_SHOW_OPTION - Success
-- Performing Test HAVE_C_FDIAGNOSTICS_SHOW_OPTION
-- Performing Test HAVE_C_FDIAGNOSTICS_SHOW_OPTION - Success
-- Performing Test HAVE_CXX_PTHREAD
-- Performing Test HAVE_CXX_PTHREAD - Success
-- Performing Test HAVE_C_PTHREAD
-- Performing Test HAVE_C_PTHREAD - Success
-- Performing Test HAVE_CXX_MARCH_I686
-- Performing Test HAVE_CXX_MARCH_I686 - Success
-- Performing Test HAVE_C_MARCH_I686
-- Performing Test HAVE_C_MARCH_I686 - Success
-- Performing Test HAVE_CXX_FOMIT_FRAME_POINTER
-- Performing Test HAVE_CXX_FOMIT_FRAME_POINTER - Success
-- Performing Test HAVE_C_FOMIT_FRAME_POINTER
-- Performing Test HAVE_C_FOMIT_FRAME_POINTER - Success
-- Performing Test HAVE_CXX_MSSE
-- Performing Test HAVE_CXX_MSSE - Success
-- Performing Test HAVE_C_MSSE
-- Performing Test HAVE_C_MSSE - Success
-- Performing Test HAVE_CXX_MSSE2
-- Performing Test HAVE_CXX_MSSE2 - Success
-- Performing Test HAVE_C_MSSE2
-- Performing Test HAVE_C_MSSE2 - Success
-- Performing Test HAVE_CXX_MNO_AVX
-- Performing Test HAVE_CXX_MNO_AVX - Success
-- Performing Test HAVE_C_MNO_AVX
-- Performing Test HAVE_C_MNO_AVX - Success
-- Performing Test HAVE_CXX_MSSE3
-- Performing Test HAVE_CXX_MSSE3 - Success
-- Performing Test HAVE_C_MSSE3
-- Performing Test HAVE_C_MSSE3 - Success
-- Performing Test HAVE_CXX_MNO_SSSE3
-- Performing Test HAVE_CXX_MNO_SSSE3 - Success
-- Performing Test HAVE_C_MNO_SSSE3
-- Performing Test HAVE_C_MNO_SSSE3 - Success
-- Performing Test HAVE_CXX_MNO_SSE4_1
-- Performing Test HAVE_CXX_MNO_SSE4_1 - Success
-- Performing Test HAVE_C_MNO_SSE4_1
-- Performing Test HAVE_C_MNO_SSE4_1 - Success
-- Performing Test HAVE_CXX_MNO_SSE4_2
-- Performing Test HAVE_CXX_MNO_SSE4_2 - Success
-- Performing Test HAVE_C_MNO_SSE4_2
-- Performing Test HAVE_C_MNO_SSE4_2 - Success
-- Performing Test HAVE_CXX_MFPMATH_SSE
-- Performing Test HAVE_CXX_MFPMATH_SSE - Success
-- Performing Test HAVE_C_MFPMATH_SSE
-- Performing Test HAVE_C_MFPMATH_SSE - Success
-- Performing Test HAVE_CXX_FFUNCTION_SECTIONS
-- Performing Test HAVE_CXX_FFUNCTION_SECTIONS - Success
-- Performing Test HAVE_C_FFUNCTION_SECTIONS
-- Performing Test HAVE_C_FFUNCTION_SECTIONS - Success
-- Performing Test HAVE_CXX_FVISIBILITY_HIDDEN
-- Performing Test HAVE_CXX_FVISIBILITY_HIDDEN - Success
-- Performing Test HAVE_C_FVISIBILITY_HIDDEN
-- Performing Test HAVE_C_FVISIBILITY_HIDDEN - Success
-- Performing Test HAVE_CXX_FVISIBILITY_INLINES_HIDDEN
-- Performing Test HAVE_CXX_FVISIBILITY_INLINES_HIDDEN - Success
-- Performing Test HAVE_C_FVISIBILITY_INLINES_HIDDEN
-- Performing Test HAVE_C_FVISIBILITY_INLINES_HIDDEN - Failed
-- Looking for pthread.h
-- Looking for pthread.h - found
-- Check if the system is big endian
-- Searching 16 bit integer
-- Looking for sys/types.h
-- Looking for sys/types.h - found
-- Looking for stdint.h
-- Looking for stdint.h - found
-- Looking for stddef.h
-- Looking for stddef.h - found
-- Check size of unsigned short
-- Check size of unsigned short - done
-- Using unsigned short
-- Check if the system is big endian - little endian
-- Found ZLIB: /usr/lib/libz.so (found suitable version "1.2.8", minimum required is "1.2.3") 
-- Found TIFF: /usr/local/lib/libtiff.so (found version "4.0.3") 
-- Found JPEG: /usr/lib/libjpeg.so  
-- Performing Test HAVE_C_WNO_UNUSED_VARIABLE
-- Performing Test HAVE_C_WNO_UNUSED_VARIABLE - Success
-- Performing Test HAVE_C_WNO_UNUSED_FUNCTION
-- Performing Test HAVE_C_WNO_UNUSED_FUNCTION - Success
-- Performing Test HAVE_C_WNO_SHADOW
-- Performing Test HAVE_C_WNO_SHADOW - Success
-- Performing Test HAVE_C_WNO_MAYBE_UNINITIALIZED
-- Performing Test HAVE_C_WNO_MAYBE_UNINITIALIZED - Success
-- Found Jasper: /usr/lib/i386-linux-gnu/libjasper.so (found version "1.900.1") 
-- Found ZLIB: /usr/lib/libz.so (found version "1.2.8") 
-- Found PNG: /usr/local/lib/libpng.so (found version "1.6.18") 
-- Looking for /usr/local/include/libpng/png.h
-- Looking for /usr/local/include/libpng/png.h - not found
-- Found OpenEXR: /usr/lib/i386-linux-gnu/libIlmImf.so
-- checking for module 'gtk+-3.0'
--   package 'gtk+-3.0' not found
-- checking for module 'gtk+-2.0'
--   found gtk+-2.0, version 2.24.10
-- checking for module 'gthread-2.0'
--   found gthread-2.0, version 2.32.4
-- checking for module 'gstreamer-base-1.0'
--   found gstreamer-base-1.0, version 1.2.1
-- checking for module 'gstreamer-video-1.0'
--   found gstreamer-video-1.0, version 1.4.4
-- checking for module 'gstreamer-app-1.0'
--   found gstreamer-app-1.0, version 1.4.4
-- checking for module 'gstreamer-riff-1.0'
--   found gstreamer-riff-1.0, version 1.4.4
-- checking for module 'gstreamer-pbutils-1.0'
--   found gstreamer-pbutils-1.0, version 1.4.4
-- checking for module 'libdc1394-2'
--   found libdc1394-2, version 2.2.0
-- Looking for linux/videodev.h
-- Looking for linux/videodev.h - not found
-- Looking for linux/videodev2.h
-- Looking for linux/videodev2.h - found
-- Looking for sys/videoio.h
-- Looking for sys/videoio.h - not found
-- checking for module 'libavcodec'
--   found libavcodec, version 56.14.100
-- checking for module 'libavformat'
--   found libavformat, version 56.15.103
-- checking for module 'libavutil'
--   found libavutil, version 54.15.100
-- checking for module 'libswscale'
--   found libswscale, version 3.1.101
-- checking for module 'libavresample'
--   package 'libavresample' not found
-- Looking for libavformat/avformat.h
-- Looking for libavformat/avformat.h - found
-- Looking for ffmpeg/avformat.h
-- Looking for ffmpeg/avformat.h - not found
-- checking for module 'libgphoto2'
--   package 'libgphoto2' not found
-- On 32-bit Linux IPP can not currently be used with dynamic libs because of linker errors. Set BUILD_SHARED_LIBS=OFF
-- Found Doxygen: /usr/bin/doxygen (found version "1.7.6.1") 
-- To enable PlantUML support, set PLANTUML_JAR environment variable or pass -DPLANTUML_JAR=<filepath> option to cmake
-- Found PythonInterp: /usr/bin/python2.7 (found suitable version "2.7.3", minimum required is "2.7") 
-- Found PythonLibs: /usr/lib/libpython2.7.so (found suitable exact version "2.7.3") 
-- Found PythonInterp: /home/cobalt/anaconda3/bin/python3.4 (found suitable version "3.4.3", minimum required is "3.4") 
-- Found PythonLibs: /usr/local/lib/libpython3.4m.so (found suitable exact version "3.4.3") 
-- Found apache ant 1.9.2: /usr/bin/ant
-- Found JNI: /usr/lib/jvm/java-7-oracle/jre/lib/i386/libjawt.so  
-- Could NOT find Matlab (missing:  MATLAB_MEX_SCRIPT MATLAB_INCLUDE_DIRS MATLAB_LIBRARIES MATLAB_LIBRARY_DIRS MATLAB_MEXEXT MATLAB_ARCH MATLAB_BIN) 
-- Found VTK ver. 5.8.0 (usefile: /usr/lib/vtk-5.8/UseVTK.cmake)
-- Performing Test HAVE_CXX_WNO_UNDEF
-- Performing Test HAVE_CXX_WNO_UNDEF - Success
-- Performing Test HAVE_CXX_WNO_SHADOW
-- Performing Test HAVE_CXX_WNO_SHADOW - Success
-- Performing Test HAVE_CXX_WNO_DEPRECATED_DECLARATIONS
-- Performing Test HAVE_CXX_WNO_DEPRECATED_DECLARATIONS - Success
-- 
-- General configuration for OpenCV 3.1.0-dev =====================================
--   Version control:               3.1.0-84-g243c513-dirty
-- 
--   Platform:
--     Host:                        Linux 3.5.0-27-generic i686
--     CMake:                       3.2.1
--     CMake generator:             Unix Makefiles
--     CMake build tool:            /usr/bin/make
--     Configuration:               Release
-- 
--   C/C++:
--     Built as dynamic libs?:      YES
--     C++ Compiler:                /usr/bin/c++  (ver 4.9.2)
--     C++ flags (Release):         -fsigned-char -W -Wall -Werror=return-type -Werror=non-virtual-dtor -Werror=address -Werror=sequence-point -Wformat -Werror=format-security -Wmissing-declarations -Wundef -Winit-self -Wpointer-arith -Wshadow -Wsign-promo -Wno-narrowing -Wno-delete-non-virtual-dtor -fdiagnostics-show-option -pthread -march=i686 -fomit-frame-pointer -msse -msse2 -mno-avx -msse3 -mno-ssse3 -mno-sse4.1 -mno-sse4.2 -mfpmath=sse -ffunction-sections -fvisibility=hidden -fvisibility-inlines-hidden -O2 -DNDEBUG  -DNDEBUG
--     C++ flags (Debug):           -fsigned-char -W -Wall -Werror=return-type -Werror=non-virtual-dtor -Werror=address -Werror=sequence-point -Wformat -Werror=format-security -Wmissing-declarations -Wundef -Winit-self -Wpointer-arith -Wshadow -Wsign-promo -Wno-narrowing -Wno-delete-non-virtual-dtor -fdiagnostics-show-option -pthread -march=i686 -fomit-frame-pointer -msse -msse2 -mno-avx -msse3 -mno-ssse3 -mno-sse4.1 -mno-sse4.2 -mfpmath=sse -ffunction-sections -fvisibility=hidden -fvisibility-inlines-hidden -g  -O0 -DDEBUG -D_DEBUG
--     C Compiler:                  /usr/bin/cc
--     C flags (Release):           -fsigned-char -W -Wall -Werror=return-type -Werror=non-virtual-dtor -Werror=address -Werror=sequence-point -Wformat -Werror=format-security -Wmissing-declarations -Wmissing-prototypes -Wstrict-prototypes -Wundef -Winit-self -Wpointer-arith -Wshadow -Wno-narrowing -fdiagnostics-show-option -pthread -march=i686 -fomit-frame-pointer -msse -msse2 -mno-avx -msse3 -mno-ssse3 -mno-sse4.1 -mno-sse4.2 -mfpmath=sse -ffunction-sections -fvisibility=hidden -O2 -DNDEBUG  -DNDEBUG
--     C flags (Debug):             -fsigned-char -W -Wall -Werror=return-type -Werror=non-virtual-dtor -Werror=address -Werror=sequence-point -Wformat -Werror=format-security -Wmissing-declarations -Wmissing-prototypes -Wstrict-prototypes -Wundef -Winit-self -Wpointer-arith -Wshadow -Wno-narrowing -fdiagnostics-show-option -pthread -march=i686 -fomit-frame-pointer -msse -msse2 -mno-avx -msse3 -mno-ssse3 -mno-sse4.1 -mno-sse4.2 -mfpmath=sse -ffunction-sections -fvisibility=hidden -g  -O0 -DDEBUG -D_DEBUG
--     Linker flags (Release):
--     Linker flags (Debug):
--     Precompiled headers:         YES
--     Extra dependencies:          /usr/local/lib/libpng.so /usr/lib/libz.so /usr/local/lib/libtiff.so /usr/lib/i386-linux-gnu/libjasper.so /usr/lib/libjpeg.so /usr/lib/i386-linux-gnu/libImath.so /usr/lib/i386-linux-gnu/libIlmImf.so /usr/lib/i386-linux-gnu/libIex.so /usr/lib/i386-linux-gnu/libHalf.so /usr/lib/i386-linux-gnu/libIlmThread.so gtk-x11-2.0 gdk-x11-2.0 atk-1.0 gio-2.0 pangoft2-1.0 pangocairo-1.0 gdk_pixbuf-2.0 cairo pango-1.0 freetype fontconfig gthread-2.0 gstvideo-1.0 gstapp-1.0 gstbase-1.0 gstriff-1.0 gstpbutils-1.0 gstreamer-1.0 gobject-2.0 glib-2.0 dc1394 avcodec avformat avutil swscale /usr/lib/i386-linux-gnu/libbz2.so vtkCommon vtkFiltering vtkImaging vtkGraphics vtkGenericFiltering vtkIO vtkRendering vtkVolumeRendering vtkHybrid vtkWidgets vtkParallel vtkInfovis vtkGeovis vtkViews vtkCharts dl m pthread rt
--     3rdparty dependencies:       libwebp
-- 
--   OpenCV modules:
--     To be built:                 core flann imgproc ml photo video viz imgcodecs shape videoio highgui objdetect superres ts features2d calib3d java stitching videostab python2 python3
--     Disabled:                    world
--     Disabled by dependency:      -
--     Unavailable:                 cudaarithm cudabgsegm cudacodec cudafeatures2d cudafilters cudaimgproc cudalegacy cudaobjdetect cudaoptflow cudastereo cudawarping cudev
-- 
--   GUI: 
--     QT:                          NO
--     GTK+ 2.x:                    YES (ver 2.24.10)
--     GThread :                    YES (ver 2.32.4)
--     GtkGlExt:                    NO
--     OpenGL support:              NO
--     VTK support:                 YES (ver 5.8.0)
-- 
--   Media I/O: 
--     ZLib:                        /usr/lib/libz.so (ver 1.2.8)
--     JPEG:                        /usr/lib/libjpeg.so (ver )
--     WEBP:                        build (ver 0.3.1)
--     PNG:                         /usr/local/lib/libpng.so (ver 1.6.18)
--     TIFF:                        /usr/local/lib/libtiff.so (ver 42 - 4.0.3)
--     JPEG 2000:                   /usr/lib/i386-linux-gnu/libjasper.so (ver 1.900.1)
--     OpenEXR:                     /usr/lib/i386-linux-gnu/libImath.so /usr/lib/i386-linux-gnu/libIlmImf.so /usr/lib/i386-linux-gnu/libIex.so /usr/lib/i386-linux-gnu/libHalf.so /usr/lib/i386-linux-gnu/libIlmThread.so (ver 2.1.0)
--     GDAL:                        NO
-- 
--   Video I/O:
--     DC1394 1.x:                  NO
--     DC1394 2.x:                  YES (ver 2.2.0)
--     FFMPEG:                      YES
--       codec:                     YES (ver 56.14.100)
--       format:                    YES (ver 56.15.103)
--       util:                      YES (ver 54.15.100)
--       swscale:                   YES (ver 3.1.101)
--       resample:                  NO
--       gentoo-style:              YES
--     GStreamer:                   
--       base:                      YES (ver 1.2.1)
--       video:                     YES (ver 1.4.4)
--       app:                       YES (ver 1.4.4)
--       riff:                      YES (ver 1.4.4)
--       pbutils:                   YES (ver 1.4.4)
--     OpenNI:                      NO
--     OpenNI PrimeSensor Modules:  NO
--     OpenNI2:                     NO
--     PvAPI:                       NO
--     GigEVisionSDK:               NO
--     UniCap:                      NO
--     UniCap ucil:                 NO
--     V4L/V4L2:                    NO/YES
--     XIMEA:                       NO
--     Xine:                        NO
--     gPhoto2:                     NO
-- 
--   Parallel framework:            pthreads
-- 
--   Other third-party libraries:
--     Use IPP:                     IPP not found or implicitly disabled
--     Use IPP Async:               NO
--     Use VA:                      NO
--     Use Intel VA-API/OpenCL:     NO
--     Use Eigen:                   YES (ver 3.0.5)
--     Use Cuda:                    NO
--     Use OpenCL:                  YES
--     Use custom HAL:              NO
-- 
--   OpenCL:
--     Version:                     dynamic
--     Include path:                /home/cobalt/opencv/3rdparty/include/opencl/1.2
--     Use AMDFFT:                  NO
--     Use AMDBLAS:                 NO
-- 
--   Python 2:
--     Interpreter:                 /usr/bin/python2.7 (ver 2.7.3)
--     Libraries:                   /usr/lib/libpython2.7.so (ver 2.7.3)
--     numpy:                       /usr/lib/python2.7/dist-packages/numpy/core/include (ver 1.6.1)
--     packages path:               lib/python2.7/dist-packages
-- 
--   Python 3:
--     Interpreter:                 /home/cobalt/anaconda3/bin/python3.4 (ver 3.4.3)
--     Libraries:                   /usr/local/lib/libpython3.4m.so (ver 3.4.3)
--     numpy:                       /home/cobalt/anaconda3/lib/python3.4/site-packages/numpy/core/include (ver 1.9.3)
--     packages path:               lib/python3.4/site-packages
-- 
--   Python (for build):            /usr/bin/python2.7
-- 
--   Java:
--     ant:                         /usr/bin/ant (ver 1.9.2)
--     JNI:                         /usr/lib/jvm/java-7-oracle/include /usr/lib/jvm/java-7-oracle/include/linux /usr/lib/jvm/java-7-oracle/include
--     Java wrappers:               YES
--     Java tests:                  YES
-- 
--   Matlab:                        Matlab not found or implicitly disabled
-- 
--   Documentation:
--     Doxygen:                     /usr/bin/doxygen (ver 1.7.6.1)
--     PlantUML:                    NO
-- 
--   Tests and samples:
--     Tests:                       YES
--     Performance tests:           YES
--     C/C++ Examples:              NO
-- 
--   Install path:                  /usr/local
-- 
--   cvconfig.h is in:              /home/cobalt/opencv/build 
-- -----------------------------------------------------------------
-- 
-- Configuring done
-- Generating done
-- Build files have been written to: /home/cobalt/opencv/build 

PhD:~/opencv/build$ make -j4
[  0%] [  0%] [  0%] Generating opencv_core_pch_dephelp.cxx
Generating opencv_ts_pch_dephelp.cxx
Scanning dependencies of target libwebp
Generating opencv_imgproc_pch_dephelp.cxx
Scanning dependencies of target opencv_core_pch_dephelp
Scanning dependencies of target opencv_imgproc_pch_dephelp
Scanning dependencies of target opencv_ts_pch_dephelp
[  0%] Building C object 3rdparty/libwebp/CMakeFiles/libwebp.dir/dec/buffer.c.o
[  0%] Building C object 3rdparty/libwebp/CMakeFiles/libwebp.dir/dec/quant.c.o
[  0%] Building C object 3rdparty/libwebp/CMakeFiles/libwebp.dir/dec/layer.c.o
[  0%] Building C object 3rdparty/libwebp/CMakeFiles/libwebp.dir/dec/frame.c.o
[  0%] [  0%] Building CXX object modules/ts/CMakeFiles/opencv_ts_pch_dephelp.dir/opencv_ts_pch_dephelp.cxx.o
Building CXX object modules/imgproc/CMakeFiles/opencv_imgproc_pch_dephelp.dir/opencv_imgproc_pch_dephelp.cxx.o
[  0%] Building C object 3rdparty/libwebp/CMakeFiles/libwebp.dir/dec/io.c.o
[  1%] Building CXX object modules/core/CMakeFiles/opencv_core_pch_dephelp.dir/opencv_core_pch_dephelp.cxx.o
[  1%] Building C object 3rdparty/libwebp/CMakeFiles/libwebp.dir/dec/vp8l.c.o
[  1%] Building C object 3rdparty/libwebp/CMakeFiles/libwebp.dir/dec/alpha.c.o
[  1%] Building C object 3rdparty/libwebp/CMakeFiles/libwebp.dir/dec/webp.c.o
[  1%] Building C object 3rdparty/libwebp/CMakeFiles/libwebp.dir/dec/tree.c.o
[  1%] Building C object 3rdparty/libwebp/CMakeFiles/libwebp.dir/dec/vp8.c.o
Linking CXX static library ../../lib/libopencv_imgproc_pch_dephelp.a
[  1%] Built target opencv_imgproc_pch_dephelp
[  1%] Generating opencv_imgcodecs_pch_dephelp.cxx
Scanning dependencies of target opencv_imgcodecs_pch_dephelp
Linking CXX static library ../../lib/libopencv_core_pch_dephelp.a
[  1%] Built target opencv_core_pch_dephelp
[  1%] Generating opencv_videoio_pch_dephelp.cxx
Scanning dependencies of target opencv_videoio_pch_dephelp
[  1%] Building CXX object modules/imgcodecs/CMakeFiles/opencv_imgcodecs_pch_dephelp.dir/opencv_imgcodecs_pch_dephelp.cxx.o
Linking CXX static library ../../lib/libopencv_ts_pch_dephelp.a
[  1%] [  1%] Building CXX object modules/videoio/CMakeFiles/opencv_videoio_pch_dephelp.dir/opencv_videoio_pch_dephelp.cxx.o
Built target opencv_ts_pch_dephelp
[  1%] Generating opencv_highgui_pch_dephelp.cxx
Scanning dependencies of target opencv_highgui_pch_dephelp
[  1%] Building C object 3rdparty/libwebp/CMakeFiles/libwebp.dir/dec/idec.c.o
[  2%] Building CXX object modules/highgui/CMakeFiles/opencv_highgui_pch_dephelp.dir/opencv_highgui_pch_dephelp.cxx.o
[  2%] Building C object 3rdparty/libwebp/CMakeFiles/libwebp.dir/dsp/upsampling_sse2.c.o
[  2%] Building C object 3rdparty/libwebp/CMakeFiles/libwebp.dir/dsp/dec_neon.c.o
[  3%] Building C object 3rdparty/libwebp/CMakeFiles/libwebp.dir/dsp/lossless.c.o
Linking CXX static library ../../lib/libopencv_imgcodecs_pch_dephelp.a
[  3%] Built target opencv_imgcodecs_pch_dephelp
[  3%] Generating opencv_perf_core_pch_dephelp.cxx
Scanning dependencies of target opencv_perf_core_pch_dephelp
Linking CXX static library ../../lib/libopencv_videoio_pch_dephelp.a
[  3%] Building CXX object modules/core/CMakeFiles/opencv_perf_core_pch_dephelp.dir/opencv_perf_core_pch_dephelp.cxx.o
[  3%] Built target opencv_videoio_pch_dephelp
[  3%] Generating opencv_test_core_pch_dephelp.cxx
Scanning dependencies of target opencv_test_core_pch_dephelp
Linking CXX static library ../../lib/libopencv_highgui_pch_dephelp.a
[  3%] [  3%] Building CXX object modules/core/CMakeFiles/opencv_test_core_pch_dephelp.dir/opencv_test_core_pch_dephelp.cxx.o
Built target opencv_highgui_pch_dephelp
[  3%] Generating opencv_flann_pch_dephelp.cxx
Scanning dependencies of target opencv_flann_pch_dephelp
[  3%] Building CXX object modules/flann/CMakeFiles/opencv_flann_pch_dephelp.dir/opencv_flann_pch_dephelp.cxx.o
[  3%] Building C object 3rdparty/libwebp/CMakeFiles/libwebp.dir/dsp/enc_neon.c.o
[  3%] Building C object 3rdparty/libwebp/CMakeFiles/libwebp.dir/dsp/enc.c.o
[  3%] Building C object 3rdparty/libwebp/CMakeFiles/libwebp.dir/dsp/dec_sse2.c.o
Linking CXX static library ../../lib/libopencv_perf_core_pch_dephelp.a
[  3%] Built target opencv_perf_core_pch_dephelp
[  3%] Generating opencv_test_flann_pch_dephelp.cxx
Scanning dependencies of target opencv_test_flann_pch_dephelp
[  4%] Building CXX object modules/flann/CMakeFiles/opencv_test_flann_pch_dephelp.dir/opencv_test_flann_pch_dephelp.cxx.o
[  4%] Building C object 3rdparty/libwebp/CMakeFiles/libwebp.dir/dsp/dec.c.o
Linking CXX static library ../../lib/libopencv_test_core_pch_dephelp.a
[  4%] Built target opencv_test_core_pch_dephelp
[  4%] Building C object 3rdparty/libwebp/CMakeFiles/libwebp.dir/dsp/yuv.c.o
[  4%] Building C object 3rdparty/libwebp/CMakeFiles/libwebp.dir/dsp/cpu.c.o
[  4%] Building C object 3rdparty/libwebp/CMakeFiles/libwebp.dir/dsp/upsampling.c.o
Linking CXX static library ../../lib/libopencv_flann_pch_dephelp.a
[  4%] Built target opencv_flann_pch_dephelp
[  4%] Building C object 3rdparty/libwebp/CMakeFiles/libwebp.dir/dsp/upsampling_neon.c.o
[  4%] Building C object 3rdparty/libwebp/CMakeFiles/libwebp.dir/dsp/enc_sse2.c.o
[  4%] Building C object 3rdparty/libwebp/CMakeFiles/libwebp.dir/enc/analysis.c.o
[  4%] Building C object 3rdparty/libwebp/CMakeFiles/libwebp.dir/enc/cost.c.o
[  4%] Generating opencv_perf_imgproc_pch_dephelp.cxx
Scanning dependencies of target opencv_perf_imgproc_pch_dephelp
[  4%] Building CXX object modules/imgproc/CMakeFiles/opencv_perf_imgproc_pch_dephelp.dir/opencv_perf_imgproc_pch_dephelp.cxx.o
[  4%] Generating opencv_test_imgproc_pch_dephelp.cxx
Scanning dependencies of target opencv_test_imgproc_pch_dephelp
[  4%] Building CXX object modules/imgproc/CMakeFiles/opencv_test_imgproc_pch_dephelp.dir/opencv_test_imgproc_pch_dephelp.cxx.o
[  4%] Building C object 3rdparty/libwebp/CMakeFiles/libwebp.dir/enc/token.c.o
Linking CXX static library ../../lib/libopencv_test_flann_pch_dephelp.a
[  4%] Built target opencv_test_flann_pch_dephelp
[  4%] Generating opencv_ml_pch_dephelp.cxx
Scanning dependencies of target opencv_ml_pch_dephelp
[  4%] Building CXX object modules/ml/CMakeFiles/opencv_ml_pch_dephelp.dir/opencv_ml_pch_dephelp.cxx.o
[  6%] Building C object 3rdparty/libwebp/CMakeFiles/libwebp.dir/enc/histogram.c.o
[  6%] Building C object 3rdparty/libwebp/CMakeFiles/libwebp.dir/enc/webpenc.c.o
[  6%] Building C object 3rdparty/libwebp/CMakeFiles/libwebp.dir/enc/quant.c.o
Linking CXX static library ../../lib/libopencv_perf_imgproc_pch_dephelp.a
[  6%] Built target opencv_perf_imgproc_pch_dephelp
[  6%] Building C object 3rdparty/libwebp/CMakeFiles/libwebp.dir/enc/iterator.c.o
[  6%] Building C object 3rdparty/libwebp/CMakeFiles/libwebp.dir/enc/layer.c.o
[  6%] Building C object 3rdparty/libwebp/CMakeFiles/libwebp.dir/enc/frame.c.o
[  6%] Generating opencv_test_ml_pch_dephelp.cxx
Scanning dependencies of target opencv_test_ml_pch_dephelp
Linking CXX static library ../../lib/libopencv_test_imgproc_pch_dephelp.a
[  6%] [  6%] Built target opencv_test_imgproc_pch_dephelp
[  6%] Building CXX object modules/ml/CMakeFiles/opencv_test_ml_pch_dephelp.dir/opencv_test_ml_pch_dephelp.cxx.o
Generating opencv_photo_pch_dephelp.cxx
Scanning dependencies of target opencv_photo_pch_dephelp
Linking CXX static library ../../lib/libopencv_ml_pch_dephelp.a
[  6%] Built target opencv_ml_pch_dephelp
[  6%] Generating opencv_perf_photo_pch_dephelp.cxx
Scanning dependencies of target opencv_perf_photo_pch_dephelp
[  6%] Building CXX object modules/photo/CMakeFiles/opencv_photo_pch_dephelp.dir/opencv_photo_pch_dephelp.cxx.o
[  6%] Building CXX object modules/photo/CMakeFiles/opencv_perf_photo_pch_dephelp.dir/opencv_perf_photo_pch_dephelp.cxx.o
[  6%] Building C object 3rdparty/libwebp/CMakeFiles/libwebp.dir/enc/filter.c.o
[  6%] Building C object 3rdparty/libwebp/CMakeFiles/libwebp.dir/enc/vp8l.c.o
Linking CXX static library ../../lib/libopencv_test_ml_pch_dephelp.a
Linking CXX static library ../../lib/libopencv_photo_pch_dephelp.a
[  6%] Built target opencv_test_ml_pch_dephelp
[  6%] [  6%] Built target opencv_photo_pch_dephelp
[  6%] Generating opencv_test_photo_pch_dephelp.cxx
Generating opencv_video_pch_dephelp.cxx
[  6%] Building C object 3rdparty/libwebp/CMakeFiles/libwebp.dir/enc/backward_references.c.o
Scanning dependencies of target opencv_video_pch_dephelp
Scanning dependencies of target opencv_test_photo_pch_dephelp
[  6%] Building CXX object modules/photo/CMakeFiles/opencv_test_photo_pch_dephelp.dir/opencv_test_photo_pch_dephelp.cxx.o
Linking CXX static library ../../lib/libopencv_perf_photo_pch_dephelp.a
[  6%] Building CXX object modules/video/CMakeFiles/opencv_video_pch_dephelp.dir/opencv_video_pch_dephelp.cxx.o
[  6%] Built target opencv_perf_photo_pch_dephelp
[  7%] Generating opencv_perf_video_pch_dephelp.cxx
Scanning dependencies of target opencv_perf_video_pch_dephelp
[  7%] Building CXX object modules/video/CMakeFiles/opencv_perf_video_pch_dephelp.dir/opencv_perf_video_pch_dephelp.cxx.o
[  7%] Building C object 3rdparty/libwebp/CMakeFiles/libwebp.dir/enc/alpha.c.o
[  7%] Building C object 3rdparty/libwebp/CMakeFiles/libwebp.dir/enc/config.c.o
[  7%] Building C object 3rdparty/libwebp/CMakeFiles/libwebp.dir/enc/tree.c.o
[  7%] Building C object 3rdparty/libwebp/CMakeFiles/libwebp.dir/enc/picture.c.o
Linking CXX static library ../../lib/libopencv_video_pch_dephelp.a
[  7%] Built target opencv_video_pch_dephelp
[  7%] Generating opencv_test_video_pch_dephelp.cxx
Linking CXX static library ../../lib/libopencv_test_photo_pch_dephelp.a
Scanning dependencies of target opencv_test_video_pch_dephelp
[  7%] Built target opencv_test_photo_pch_dephelp
[  7%] Generating opencv_viz_pch_dephelp.cxx
[  7%] Building CXX object modules/video/CMakeFiles/opencv_test_video_pch_dephelp.dir/opencv_test_video_pch_dephelp.cxx.o
Scanning dependencies of target opencv_viz_pch_dephelp
Linking CXX static library ../../lib/libopencv_perf_video_pch_dephelp.a
[  7%] Built target opencv_perf_video_pch_dephelp
[  8%] Building C object 3rdparty/libwebp/CMakeFiles/libwebp.dir/enc/syntax.c.o
[  9%] Generating opencv_test_viz_pch_dephelp.cxx
Scanning dependencies of target opencv_test_viz_pch_dephelp
[  9%] Building C object 3rdparty/libwebp/CMakeFiles/libwebp.dir/mux/muxedit.c.o
[  9%] Building CXX object modules/viz/CMakeFiles/opencv_test_viz_pch_dephelp.dir/opencv_test_viz_pch_dephelp.cxx.o
[  9%] Building CXX object modules/viz/CMakeFiles/opencv_viz_pch_dephelp.dir/opencv_viz_pch_dephelp.cxx.o
[  9%] Building C object 3rdparty/libwebp/CMakeFiles/libwebp.dir/mux/muxread.c.o
[  9%] Building C object 3rdparty/libwebp/CMakeFiles/libwebp.dir/mux/muxinternal.c.o
[  9%] Building C object 3rdparty/libwebp/CMakeFiles/libwebp.dir/utils/bit_writer.c.o
Linking CXX static library ../../lib/libopencv_test_video_pch_dephelp.a
[  9%] Built target opencv_test_video_pch_dephelp
[  9%] Generating opencv_perf_imgcodecs_pch_dephelp.cxx
[  9%] Scanning dependencies of target opencv_perf_imgcodecs_pch_dephelp
Building C object 3rdparty/libwebp/CMakeFiles/libwebp.dir/utils/huffman.c.o
[  9%] Building CXX object modules/imgcodecs/CMakeFiles/opencv_perf_imgcodecs_pch_dephelp.dir/opencv_perf_imgcodecs_pch_dephelp.cxx.o
[  9%] Building C object 3rdparty/libwebp/CMakeFiles/libwebp.dir/utils/thread.c.o
[  9%] Building C object 3rdparty/libwebp/CMakeFiles/libwebp.dir/utils/filters.c.o
[  9%] Building C object 3rdparty/libwebp/CMakeFiles/libwebp.dir/utils/quant_levels.c.o
[  9%] Building C object 3rdparty/libwebp/CMakeFiles/libwebp.dir/utils/huffman_encode.c.o
Linking CXX static library ../../lib/libopencv_test_viz_pch_dephelp.a
[  9%] Built target opencv_test_viz_pch_dephelp
[  9%] Building C object 3rdparty/libwebp/CMakeFiles/libwebp.dir/utils/color_cache.c.o
[  9%] Building C object 3rdparty/libwebp/CMakeFiles/libwebp.dir/utils/utils.c.o
[  9%] Building C object 3rdparty/libwebp/CMakeFiles/libwebp.dir/utils/bit_reader.c.o
[  9%] Generating opencv_test_imgcodecs_pch_dephelp.cxx
Scanning dependencies of target opencv_test_imgcodecs_pch_dephelp
[  9%] Building C object 3rdparty/libwebp/CMakeFiles/libwebp.dir/utils/rescaler.c.o
[  9%] Building CXX object modules/imgcodecs/CMakeFiles/opencv_test_imgcodecs_pch_dephelp.dir/opencv_test_imgcodecs_pch_dephelp.cxx.o
[ 11%] Building C object 3rdparty/libwebp/CMakeFiles/libwebp.dir/utils/quant_levels_dec.c.o
Linking C static library ../lib/liblibwebp.a
[ 11%] Built target libwebp
[ 11%] Generating opencv_shape_pch_dephelp.cxx
Scanning dependencies of target opencv_shape_pch_dephelp
[ 11%] Building CXX object modules/shape/CMakeFiles/opencv_shape_pch_dephelp.dir/opencv_shape_pch_dephelp.cxx.o
Linking CXX static library ../../lib/libopencv_perf_imgcodecs_pch_dephelp.a
[ 11%] Built target opencv_perf_imgcodecs_pch_dephelp
[ 11%] Generating opencv_test_shape_pch_dephelp.cxx
Scanning dependencies of target opencv_test_shape_pch_dephelp
[ 11%] Building CXX object modules/shape/CMakeFiles/opencv_test_shape_pch_dephelp.dir/opencv_test_shape_pch_dephelp.cxx.o
Linking CXX static library ../../lib/libopencv_viz_pch_dephelp.a
[ 11%] Built target opencv_viz_pch_dephelp
[ 11%] Generating opencv_perf_videoio_pch_dephelp.cxx
Linking CXX static library ../../lib/libopencv_shape_pch_dephelp.a
Scanning dependencies of target opencv_perf_videoio_pch_dephelp
[ 11%] Built target opencv_shape_pch_dephelp
[ 11%] Generating opencv_test_videoio_pch_dephelp.cxx
Scanning dependencies of target opencv_test_videoio_pch_dephelp
[ 11%] Building CXX object modules/videoio/CMakeFiles/opencv_perf_videoio_pch_dephelp.dir/opencv_perf_videoio_pch_dephelp.cxx.o
[ 11%] Building CXX object modules/videoio/CMakeFiles/opencv_test_videoio_pch_dephelp.dir/opencv_test_videoio_pch_dephelp.cxx.o
Linking CXX static library ../../lib/libopencv_test_imgcodecs_pch_dephelp.a
[ 11%] Built target opencv_test_imgcodecs_pch_dephelp
[ 11%] Generating opencv_test_highgui_pch_dephelp.cxx
Scanning dependencies of target opencv_test_highgui_pch_dephelp
[ 11%] Building CXX object modules/highgui/CMakeFiles/opencv_test_highgui_pch_dephelp.dir/opencv_test_highgui_pch_dephelp.cxx.o
Linking CXX static library ../../lib/libopencv_test_shape_pch_dephelp.a
[ 11%] Built target opencv_test_shape_pch_dephelp
[ 11%] Generating opencv_objdetect_pch_dephelp.cxx
Scanning dependencies of target opencv_objdetect_pch_dephelp
[ 11%] Building CXX object modules/objdetect/CMakeFiles/opencv_objdetect_pch_dephelp.dir/opencv_objdetect_pch_dephelp.cxx.o
Linking CXX static library ../../lib/libopencv_perf_videoio_pch_dephelp.a
[ 11%] Built target opencv_perf_videoio_pch_dephelp
[ 11%] Generating opencv_perf_objdetect_pch_dephelp.cxx
Scanning dependencies of target opencv_perf_objdetect_pch_dephelp
[ 12%] Building CXX object modules/objdetect/CMakeFiles/opencv_perf_objdetect_pch_dephelp.dir/opencv_perf_objdetect_pch_dephelp.cxx.o
Linking CXX static library ../../lib/libopencv_test_highgui_pch_dephelp.a
[ 12%] Built target opencv_test_highgui_pch_dephelp
[ 12%] Generating opencv_test_objdetect_pch_dephelp.cxx
Scanning dependencies of target opencv_test_objdetect_pch_dephelp
[ 12%] Building CXX object modules/objdetect/CMakeFiles/opencv_test_objdetect_pch_dephelp.dir/opencv_test_objdetect_pch_dephelp.cxx.o
Linking CXX static library ../../lib/libopencv_test_videoio_pch_dephelp.a
[ 12%] Built target opencv_test_videoio_pch_dephelp
[ 12%] Generating opencv_superres_pch_dephelp.cxx
Scanning dependencies of target opencv_superres_pch_dephelp
[ 13%] Building CXX object modules/superres/CMakeFiles/opencv_superres_pch_dephelp.dir/opencv_superres_pch_dephelp.cxx.o
Linking CXX static library ../../lib/libopencv_objdetect_pch_dephelp.a
[ 13%] Built target opencv_objdetect_pch_dephelp
[ 13%] Generating opencv_perf_superres_pch_dephelp.cxx
Scanning dependencies of target opencv_perf_superres_pch_dephelp
[ 13%] Building CXX object modules/superres/CMakeFiles/opencv_perf_superres_pch_dephelp.dir/opencv_perf_superres_pch_dephelp.cxx.o
Linking CXX static library ../../lib/libopencv_perf_objdetect_pch_dephelp.a
[ 13%] Built target opencv_perf_objdetect_pch_dephelp
[ 13%] Generating opencv_test_superres_pch_dephelp.cxx
Scanning dependencies of target opencv_test_superres_pch_dephelp
[ 13%] Building CXX object modules/superres/CMakeFiles/opencv_test_superres_pch_dephelp.dir/opencv_test_superres_pch_dephelp.cxx.o
Linking CXX static library ../../lib/libopencv_test_objdetect_pch_dephelp.a
[ 13%] Built target opencv_test_objdetect_pch_dephelp
[ 14%] Generating opencv_features2d_pch_dephelp.cxx
Scanning dependencies of target opencv_features2d_pch_dephelp
[ 14%] Building CXX object modules/features2d/CMakeFiles/opencv_features2d_pch_dephelp.dir/opencv_features2d_pch_dephelp.cxx.o
Linking CXX static library ../../lib/libopencv_superres_pch_dephelp.a
[ 14%] Built target opencv_superres_pch_dephelp
[ 14%] Generating opencv_perf_features2d_pch_dephelp.cxx
Scanning dependencies of target opencv_perf_features2d_pch_dephelp
[ 14%] Building CXX object modules/features2d/CMakeFiles/opencv_perf_features2d_pch_dephelp.dir/opencv_perf_features2d_pch_dephelp.cxx.o
Linking CXX static library ../../lib/libopencv_perf_superres_pch_dephelp.a
[ 14%] Built target opencv_perf_superres_pch_dephelp
[ 14%] Generating opencv_test_features2d_pch_dephelp.cxx
Scanning dependencies of target opencv_test_features2d_pch_dephelp
[ 14%] Building CXX object modules/features2d/CMakeFiles/opencv_test_features2d_pch_dephelp.dir/opencv_test_features2d_pch_dephelp.cxx.o
Linking CXX static library ../../lib/libopencv_test_superres_pch_dephelp.a
[ 14%] Built target opencv_test_superres_pch_dephelp
[ 14%] Generating opencv_calib3d_pch_dephelp.cxx
Scanning dependencies of target opencv_calib3d_pch_dephelp
[ 14%] Building CXX object modules/calib3d/CMakeFiles/opencv_calib3d_pch_dephelp.dir/opencv_calib3d_pch_dephelp.cxx.o
Linking CXX static library ../../lib/libopencv_features2d_pch_dephelp.a
[ 14%] Built target opencv_features2d_pch_dephelp
[ 14%] Generating opencv_perf_calib3d_pch_dephelp.cxx
Scanning dependencies of target opencv_perf_calib3d_pch_dephelp
[ 14%] Building CXX object modules/calib3d/CMakeFiles/opencv_perf_calib3d_pch_dephelp.dir/opencv_perf_calib3d_pch_dephelp.cxx.o
Linking CXX static library ../../lib/libopencv_perf_features2d_pch_dephelp.a
[ 14%] Built target opencv_perf_features2d_pch_dephelp
[ 14%] Generating opencv_test_calib3d_pch_dephelp.cxx
Scanning dependencies of target opencv_test_calib3d_pch_dephelp
[ 14%] Building CXX object modules/calib3d/CMakeFiles/opencv_test_calib3d_pch_dephelp.dir/opencv_test_calib3d_pch_dephelp.cxx.o
Linking CXX static library ../../lib/libopencv_test_features2d_pch_dephelp.a
[ 14%] Built target opencv_test_features2d_pch_dephelp
[ 14%] Generating opencv_perf_stitching_pch_dephelp.cxx
Scanning dependencies of target opencv_perf_stitching_pch_dephelp
[ 14%] Building CXX object modules/stitching/CMakeFiles/opencv_perf_stitching_pch_dephelp.dir/opencv_perf_stitching_pch_dephelp.cxx.o
Linking CXX static library ../../lib/libopencv_calib3d_pch_dephelp.a
[ 14%] Built target opencv_calib3d_pch_dephelp
[ 14%] Generating opencv_stitching_pch_dephelp.cxx
Scanning dependencies of target opencv_stitching_pch_dephelp
[ 14%] Building CXX object modules/stitching/CMakeFiles/opencv_stitching_pch_dephelp.dir/opencv_stitching_pch_dephelp.cxx.o
Linking CXX static library ../../lib/libopencv_perf_calib3d_pch_dephelp.a
[ 14%] Built target opencv_perf_calib3d_pch_dephelp
[ 14%] Generating opencv_test_stitching_pch_dephelp.cxx
Scanning dependencies of target opencv_test_stitching_pch_dephelp
[ 14%] Building CXX object modules/stitching/CMakeFiles/opencv_test_stitching_pch_dephelp.dir/opencv_test_stitching_pch_dephelp.cxx.o
Linking CXX static library ../../lib/libopencv_test_calib3d_pch_dephelp.a
[ 14%] Built target opencv_test_calib3d_pch_dephelp
[ 14%] Generating opencv_videostab_pch_dephelp.cxx
Scanning dependencies of target opencv_videostab_pch_dephelp
[ 14%] Building CXX object modules/videostab/CMakeFiles/opencv_videostab_pch_dephelp.dir/opencv_videostab_pch_dephelp.cxx.o
Linking CXX static library ../../lib/libopencv_perf_stitching_pch_dephelp.a
[ 14%] Built target opencv_perf_stitching_pch_dephelp
Scanning dependencies of target pch_Generate_opencv_core
[ 14%] Generating precomp.hpp
[ 14%] Generating precomp.hpp.gch/opencv_core_Release.gch
Linking CXX static library ../../lib/libopencv_stitching_pch_dephelp.a
[ 14%] Built target opencv_stitching_pch_dephelp
Scanning dependencies of target pch_Generate_opencv_ts
[ 14%] Generating precomp.hpp
[ 14%] Generating precomp.hpp.gch/opencv_ts_Release.gch
Linking CXX static library ../../lib/libopencv_test_stitching_pch_dephelp.a
[ 14%] Built target opencv_test_stitching_pch_dephelp
Scanning dependencies of target pch_Generate_opencv_imgproc
Linking CXX static library ../../lib/libopencv_videostab_pch_dephelp.a
[ 14%] Generating precomp.hpp
[ 14%] Generating precomp.hpp.gch/opencv_imgproc_Release.gch
[ 14%] Built target opencv_videostab_pch_dephelp
Scanning dependencies of target pch_Generate_opencv_imgcodecs
[ 14%] Generating precomp.hpp
[ 16%] Generating precomp.hpp.gch/opencv_imgcodecs_Release.gch
[ 16%] Built target pch_Generate_opencv_core
Scanning dependencies of target pch_Generate_opencv_videoio
[ 16%] Generating precomp.hpp
[ 16%] Generating precomp.hpp.gch/opencv_videoio_Release.gch
[ 16%] Built target pch_Generate_opencv_imgcodecs
Scanning dependencies of target pch_Generate_opencv_highgui
[ 16%] Generating precomp.hpp
[ 16%] Generating precomp.hpp.gch/opencv_highgui_Release.gch
[ 16%] Built target pch_Generate_opencv_imgproc
Scanning dependencies of target pch_Generate_opencv_perf_core
[ 16%] Generating perf_precomp.hpp
[ 16%] Generating perf_precomp.hpp.gch/opencv_perf_core_Release.gch
[ 16%] Built target pch_Generate_opencv_ts
Scanning dependencies of target pch_Generate_opencv_test_core
[ 16%] Generating test_precomp.hpp
[ 17%] Generating test_precomp.hpp.gch/opencv_test_core_Release.gch
[ 17%] Built target pch_Generate_opencv_videoio
Scanning dependencies of target pch_Generate_opencv_flann
[ 17%] Generating precomp.hpp
[ 17%] Generating precomp.hpp.gch/opencv_flann_Release.gch
[ 17%] Built target pch_Generate_opencv_highgui
Scanning dependencies of target pch_Generate_opencv_test_flann
[ 17%] Generating test_precomp.hpp
[ 17%] Generating test_precomp.hpp.gch/opencv_test_flann_Release.gch
[ 17%] Built target pch_Generate_opencv_perf_core
Scanning dependencies of target pch_Generate_opencv_perf_imgproc
[ 17%] Generating perf_precomp.hpp
[ 17%] Generating perf_precomp.hpp.gch/opencv_perf_imgproc_Release.gch
[ 17%] Built target pch_Generate_opencv_test_core
Scanning dependencies of target pch_Generate_opencv_test_imgproc
[ 17%] Generating test_precomp.hpp
[ 17%] Generating test_precomp.hpp.gch/opencv_test_imgproc_Release.gch
[ 17%] Built target pch_Generate_opencv_flann
Scanning dependencies of target pch_Generate_opencv_ml
[ 17%] Generating precomp.hpp
[ 17%] Generating precomp.hpp.gch/opencv_ml_Release.gch
[ 17%] Built target pch_Generate_opencv_perf_imgproc
Scanning dependencies of target pch_Generate_opencv_test_ml
[ 18%] Generating test_precomp.hpp
[ 18%] Generating test_precomp.hpp.gch/opencv_test_ml_Release.gch
[ 18%] Built target pch_Generate_opencv_test_flann
Scanning dependencies of target pch_Generate_opencv_photo
[ 18%] Generating precomp.hpp
[ 18%] Generating precomp.hpp.gch/opencv_photo_Release.gch
[ 18%] Built target pch_Generate_opencv_ml
Scanning dependencies of target pch_Generate_opencv_perf_photo
[ 18%] Generating perf_precomp.hpp
[ 18%] Generating perf_precomp.hpp.gch/opencv_perf_photo_Release.gch
[ 18%] [ 18%] [ 18%] Built target pch_Generate_opencv_test_imgproc
Built target pch_Generate_opencv_photo
Built target pch_Generate_opencv_test_ml
Scanning dependencies of target pch_Generate_opencv_test_photo
Scanning dependencies of target pch_Generate_opencv_video
Scanning dependencies of target pch_Generate_opencv_perf_video
[ 18%] [ 18%] [ 18%] Generating test_precomp.hpp
Generating perf_precomp.hpp
Generating precomp.hpp
[ 18%] [ 19%] [ 19%] Generating test_precomp.hpp.gch/opencv_test_photo_Release.gch
Generating perf_precomp.hpp.gch/opencv_perf_video_Release.gch
Generating precomp.hpp.gch/opencv_video_Release.gch
[ 19%] Built target pch_Generate_opencv_perf_photo
Scanning dependencies of target pch_Generate_opencv_test_video
[ 19%] Generating test_precomp.hpp
[ 19%] Generating test_precomp.hpp.gch/opencv_test_video_Release.gch
[ 19%] Built target pch_Generate_opencv_perf_video
Scanning dependencies of target pch_Generate_opencv_viz
[ 20%] Generating precomp.hpp
[ 20%] Built target pch_Generate_opencv_video
[ 20%] Generating precomp.hpp.gch/opencv_viz_Release.gch
Scanning dependencies of target pch_Generate_opencv_test_viz
[ 20%] Generating test_precomp.hpp
[ 20%] Generating test_precomp.hpp.gch/opencv_test_viz_Release.gch
[ 20%] Built target pch_Generate_opencv_test_photo
Scanning dependencies of target pch_Generate_opencv_perf_imgcodecs
[ 20%] Generating perf_precomp.hpp
[ 20%] Generating perf_precomp.hpp.gch/opencv_perf_imgcodecs_Release.gch
[ 20%] Built target pch_Generate_opencv_test_video
Scanning dependencies of target pch_Generate_opencv_test_imgcodecs
[ 20%] Generating test_precomp.hpp
[ 20%] Generating test_precomp.hpp.gch/opencv_test_imgcodecs_Release.gch
[ 20%] [ 20%] Built target pch_Generate_opencv_perf_imgcodecs
Built target pch_Generate_opencv_test_viz
Scanning dependencies of target pch_Generate_opencv_shape
[ 20%] Scanning dependencies of target pch_Generate_opencv_test_shape
Generating precomp.hpp
[ 20%] [ 20%] Generating precomp.hpp.gch/opencv_shape_Release.gch
Generating test_precomp.hpp
[ 20%] Generating test_precomp.hpp.gch/opencv_test_shape_Release.gch
[ 20%] Built target pch_Generate_opencv_viz
Scanning dependencies of target pch_Generate_opencv_perf_videoio
[ 20%] Generating perf_precomp.hpp
[ 20%] Generating perf_precomp.hpp.gch/opencv_perf_videoio_Release.gch
[ 20%] Built target pch_Generate_opencv_test_imgcodecs
Scanning dependencies of target pch_Generate_opencv_test_videoio
[ 20%] Generating test_precomp.hpp
[ 22%] Generating test_precomp.hpp.gch/opencv_test_videoio_Release.gch
[ 22%] Built target pch_Generate_opencv_shape
Scanning dependencies of target pch_Generate_opencv_test_highgui
[ 22%] Generating test_precomp.hpp
[ 22%] Generating test_precomp.hpp.gch/opencv_test_highgui_Release.gch
[ 22%] Built target pch_Generate_opencv_test_shape
Scanning dependencies of target pch_Generate_opencv_objdetect
[ 22%] Generating precomp.hpp
[ 22%] Generating precomp.hpp.gch/opencv_objdetect_Release.gch
[ 22%] Built target pch_Generate_opencv_perf_videoio
Scanning dependencies of target pch_Generate_opencv_perf_objdetect
[ 22%] Generating perf_precomp.hpp
[ 22%] Generating perf_precomp.hpp.gch/opencv_perf_objdetect_Release.gch
[ 22%] Built target pch_Generate_opencv_test_highgui
Scanning dependencies of target pch_Generate_opencv_test_objdetect
[ 22%] Generating test_precomp.hpp
[ 22%] Generating test_precomp.hpp.gch/opencv_test_objdetect_Release.gch
[ 22%] Built target pch_Generate_opencv_test_videoio
Scanning dependencies of target pch_Generate_opencv_superres
[ 22%] Generating precomp.hpp
[ 22%] Generating precomp.hpp.gch/opencv_superres_Release.gch
[ 22%] Built target pch_Generate_opencv_objdetect
Scanning dependencies of target pch_Generate_opencv_perf_superres
[ 22%] Generating perf_precomp.hpp
[ 22%] Generating perf_precomp.hpp.gch/opencv_perf_superres_Release.gch
[ 22%] Built target pch_Generate_opencv_perf_objdetect
Scanning dependencies of target pch_Generate_opencv_test_superres
[ 22%] Generating test_precomp.hpp
[ 22%] Generating test_precomp.hpp.gch/opencv_test_superres_Release.gch
[ 22%] Built target pch_Generate_opencv_test_objdetect
Scanning dependencies of target pch_Generate_opencv_features2d
[ 22%] Generating precomp.hpp
[ 22%] Generating precomp.hpp.gch/opencv_features2d_Release.gch
[ 22%] Built target pch_Generate_opencv_superres
Scanning dependencies of target pch_Generate_opencv_perf_features2d
[ 23%] Generating perf_precomp.hpp
[ 23%] Generating perf_precomp.hpp.gch/opencv_perf_features2d_Release.gch
[ 23%] Built target pch_Generate_opencv_perf_superres
Scanning dependencies of target pch_Generate_opencv_test_features2d
[ 23%] Generating test_precomp.hpp
[ 23%] Generating test_precomp.hpp.gch/opencv_test_features2d_Release.gch
[ 23%] Built target pch_Generate_opencv_test_superres
Scanning dependencies of target pch_Generate_opencv_calib3d
[ 23%] Generating precomp.hpp
[ 23%] Generating precomp.hpp.gch/opencv_calib3d_Release.gch
[ 23%] Built target pch_Generate_opencv_features2d
Scanning dependencies of target pch_Generate_opencv_perf_calib3d
[ 23%] Generating perf_precomp.hpp
[ 23%] Generating perf_precomp.hpp.gch/opencv_perf_calib3d_Release.gch
[ 23%] Built target pch_Generate_opencv_perf_features2d
Scanning dependencies of target pch_Generate_opencv_test_calib3d
[ 23%] Generating test_precomp.hpp
[ 23%] Generating test_precomp.hpp.gch/opencv_test_calib3d_Release.gch
[ 23%] Built target pch_Generate_opencv_test_features2d
Scanning dependencies of target pch_Generate_opencv_perf_stitching
[ 23%] Generating perf_precomp.hpp
[ 23%] Generating perf_precomp.hpp.gch/opencv_perf_stitching_Release.gch
[ 23%] Built target pch_Generate_opencv_calib3d
Scanning dependencies of target pch_Generate_opencv_stitching
[ 23%] Generating precomp.hpp
[ 23%] Generating precomp.hpp.gch/opencv_stitching_Release.gch
[ 23%] Built target pch_Generate_opencv_perf_calib3d
Scanning dependencies of target pch_Generate_opencv_test_stitching
[ 23%] Generating test_precomp.hpp
[ 23%] Generating test_precomp.hpp.gch/opencv_test_stitching_Release.gch
[ 23%] Built target pch_Generate_opencv_test_calib3d
Scanning dependencies of target pch_Generate_opencv_videostab
[ 23%] Generating precomp.hpp
[ 23%] Generating precomp.hpp.gch/opencv_videostab_Release.gch
[ 23%] Built target pch_Generate_opencv_perf_stitching
[ 23%] Generating opencl_kernels_core.cpp, opencl_kernels_core.hpp
Scanning dependencies of target opencv_core
[ 23%] Building CXX object modules/core/CMakeFiles/opencv_core.dir/src/glob.cpp.o
[ 23%] [ 23%] [ 23%] Built target pch_Generate_opencv_stitching
Built target pch_Generate_opencv_videostab
Built target pch_Generate_opencv_test_stitching
[ 23%] [ 23%] [ 23%] Building CXX object modules/core/CMakeFiles/opencv_core.dir/src/cuda_gpu_mat.cpp.o
Building CXX object modules/core/CMakeFiles/opencv_core.dir/src/kmeans.cpp.o
Building CXX object modules/core/CMakeFiles/opencv_core.dir/src/rand.cpp.o
[ 23%] Building CXX object modules/core/CMakeFiles/opencv_core.dir/src/cuda_stream.cpp.o
[ 23%] Building CXX object modules/core/CMakeFiles/opencv_core.dir/src/parallel_pthreads.cpp.o
[ 23%] Building CXX object modules/core/CMakeFiles/opencv_core.dir/src/lda.cpp.o
[ 24%] Building CXX object modules/core/CMakeFiles/opencv_core.dir/src/gl_core_3_1.cpp.o
[ 24%] Building CXX object modules/core/CMakeFiles/opencv_core.dir/src/downhill_simplex.cpp.o
[ 24%] Building CXX object modules/core/CMakeFiles/opencv_core.dir/src/convert.cpp.o
[ 24%] Building CXX object modules/core/CMakeFiles/opencv_core.dir/src/types.cpp.o
[ 24%] Building CXX object modules/core/CMakeFiles/opencv_core.dir/src/tables.cpp.o
[ 24%] Building CXX object modules/core/CMakeFiles/opencv_core.dir/src/matop.cpp.o
[ 24%] Building CXX object modules/core/CMakeFiles/opencv_core.dir/src/out.cpp.o
[ 24%] Building CXX object modules/core/CMakeFiles/opencv_core.dir/src/mathfuncs.cpp.o
[ 24%] Building CXX object modules/core/CMakeFiles/opencv_core.dir/src/dxt.cpp.o
[ 24%] Building CXX object modules/core/CMakeFiles/opencv_core.dir/src/lpsolver.cpp.o
[ 24%] Building CXX object modules/core/CMakeFiles/opencv_core.dir/src/persistence.cpp.o
[ 24%] Building CXX object modules/core/CMakeFiles/opencv_core.dir/src/cuda_host_mem.cpp.o
[ 24%] Building CXX object modules/core/CMakeFiles/opencv_core.dir/src/va_intel.cpp.o
[ 25%] Building CXX object modules/core/CMakeFiles/opencv_core.dir/src/matrix_decomp.cpp.o
[ 25%] Building CXX object modules/core/CMakeFiles/opencv_core.dir/src/ocl.cpp.o
[ 25%] Building CXX object modules/core/CMakeFiles/opencv_core.dir/src/conjugate_gradient.cpp.o
[ 25%] Building CXX object modules/core/CMakeFiles/opencv_core.dir/src/matmul.cpp.o
[ 25%] Building CXX object modules/core/CMakeFiles/opencv_core.dir/src/stl.cpp.o
[ 25%] Building CXX object modules/core/CMakeFiles/opencv_core.dir/src/system.cpp.o
[ 25%] Building CXX object modules/core/CMakeFiles/opencv_core.dir/src/command_line_parser.cpp.o
[ 25%] Building CXX object modules/core/CMakeFiles/opencv_core.dir/src/array.cpp.o
[ 25%] Building CXX object modules/core/CMakeFiles/opencv_core.dir/src/mathfuncs_core.cpp.o
[ 25%] Building CXX object modules/core/CMakeFiles/opencv_core.dir/src/opengl.cpp.o
[ 25%] Building CXX object modules/core/CMakeFiles/opencv_core.dir/src/cuda_info.cpp.o
[ 25%] Building CXX object modules/core/CMakeFiles/opencv_core.dir/src/split.cpp.o
[ 25%] Building CXX object modules/core/CMakeFiles/opencv_core.dir/src/umatrix.cpp.o
[ 27%] Building CXX object modules/core/CMakeFiles/opencv_core.dir/src/lapack.cpp.o
[ 27%] Building CXX object modules/core/CMakeFiles/opencv_core.dir/src/alloc.cpp.o
[ 27%] Building CXX object modules/core/CMakeFiles/opencv_core.dir/src/matrix.cpp.o
[ 27%] Building CXX object modules/core/CMakeFiles/opencv_core.dir/src/algorithm.cpp.o
[ 27%] Building CXX object modules/core/CMakeFiles/opencv_core.dir/src/copy.cpp.o
[ 27%] Building CXX object modules/core/CMakeFiles/opencv_core.dir/src/pca.cpp.o
[ 27%] Building CXX object modules/core/CMakeFiles/opencv_core.dir/src/parallel.cpp.o
[ 27%] Building CXX object modules/core/CMakeFiles/opencv_core.dir/src/stat.cpp.o
[ 27%] Building CXX object modules/core/CMakeFiles/opencv_core.dir/src/directx.cpp.o
[ 27%] Building CXX object modules/core/CMakeFiles/opencv_core.dir/src/datastructs.cpp.o
[ 27%] Building CXX object modules/core/CMakeFiles/opencv_core.dir/src/merge.cpp.o
[ 27%] Building CXX object modules/core/CMakeFiles/opencv_core.dir/src/opencl/runtime/opencl_clamdblas.cpp.o
[ 27%] Building CXX object modules/core/CMakeFiles/opencv_core.dir/src/opencl/runtime/opencl_core.cpp.o
[ 27%] Building CXX object modules/core/CMakeFiles/opencv_core.dir/src/opencl/runtime/opencl_clamdfft.cpp.o
[ 28%] Building CXX object modules/core/CMakeFiles/opencv_core.dir/src/arithm.cpp.o
[ 28%] Building CXX object modules/core/CMakeFiles/opencv_core.dir/opencl_kernels_core.cpp.o
Linking CXX shared library ../../lib/libopencv_core.so
[ 28%] Built target opencv_core
Scanning dependencies of target opencv_flann
Scanning dependencies of target opencv_ml
[ 28%] Scanning dependencies of target opencv_viz
Generating opencl_kernels_imgproc.cpp, opencl_kernels_imgproc.hpp
[ 28%] Building CXX object modules/flann/CMakeFiles/opencv_flann.dir/src/miniflann.cpp.o
[ 28%] Building CXX object modules/ml/CMakeFiles/opencv_ml.dir/src/tree.cpp.o
Scanning dependencies of target opencv_imgproc
[ 28%] Building CXX object modules/viz/CMakeFiles/opencv_viz.dir/src/types.cpp.o
[ 28%] Building CXX object modules/imgproc/CMakeFiles/opencv_imgproc.dir/src/shapedescr.cpp.o
[ 28%] Building CXX object modules/imgproc/CMakeFiles/opencv_imgproc.dir/src/linefit.cpp.o
[ 28%] Building CXX object modules/viz/CMakeFiles/opencv_viz.dir/src/widget.cpp.o
[ 29%] Building CXX object modules/imgproc/CMakeFiles/opencv_imgproc.dir/src/imgwarp.cpp.o
[ 29%] Building CXX object modules/ml/CMakeFiles/opencv_ml.dir/src/testset.cpp.o
[ 29%] Building CXX object modules/ml/CMakeFiles/opencv_ml.dir/src/em.cpp.o
[ 30%] Building CXX object modules/viz/CMakeFiles/opencv_viz.dir/src/vizcore.cpp.o
[ 30%] Building CXX object modules/ml/CMakeFiles/opencv_ml.dir/src/kdtree.cpp.o
[ 30%] Building CXX object modules/viz/CMakeFiles/opencv_viz.dir/src/clouds.cpp.o
[ 30%] Building CXX object modules/ml/CMakeFiles/opencv_ml.dir/src/inner_functions.cpp.o
[ 30%] Building CXX object modules/flann/CMakeFiles/opencv_flann.dir/src/flann.cpp.o
[ 30%] Building CXX object modules/ml/CMakeFiles/opencv_ml.dir/src/rtrees.cpp.o
Linking CXX shared library ../../lib/libopencv_flann.so
[ 30%] Built target opencv_flann
[ 30%] Building CXX object modules/ml/CMakeFiles/opencv_ml.dir/src/knearest.cpp.o
[ 30%] Building CXX object modules/ml/CMakeFiles/opencv_ml.dir/src/gbt.cpp.o
[ 30%] Building CXX object modules/viz/CMakeFiles/opencv_viz.dir/src/shapes.cpp.o
[ 30%] Building CXX object modules/imgproc/CMakeFiles/opencv_imgproc.dir/src/intersection.cpp.o
[ 32%] Building CXX object modules/ml/CMakeFiles/opencv_ml.dir/src/lr.cpp.o
[ 32%] Building CXX object modules/ml/CMakeFiles/opencv_ml.dir/src/boost.cpp.o
[ 32%] Building CXX object modules/imgproc/CMakeFiles/opencv_imgproc.dir/src/tables.cpp.o
[ 32%] Building CXX object modules/imgproc/CMakeFiles/opencv_imgproc.dir/src/contours.cpp.o
[ 32%] Building CXX object modules/ml/CMakeFiles/opencv_ml.dir/src/ann_mlp.cpp.o
[ 32%] Building CXX object modules/imgproc/CMakeFiles/opencv_imgproc.dir/src/gabor.cpp.o
[ 32%] Building CXX object modules/imgproc/CMakeFiles/opencv_imgproc.dir/src/featureselect.cpp.o
[ 32%] Building CXX object modules/viz/CMakeFiles/opencv_viz.dir/src/vizimpl.cpp.o
[ 32%] Building CXX object modules/ml/CMakeFiles/opencv_ml.dir/src/svm.cpp.o
[ 32%] Building CXX object modules/imgproc/CMakeFiles/opencv_imgproc.dir/src/utils.cpp.o
[ 32%] Building CXX object modules/viz/CMakeFiles/opencv_viz.dir/src/vtk/vtkCloudMatSink.cpp.o
[ 32%] Building CXX object modules/imgproc/CMakeFiles/opencv_imgproc.dir/src/emd.cpp.o
[ 32%] Building CXX object modules/imgproc/CMakeFiles/opencv_imgproc.dir/src/segmentation.cpp.o
[ 32%] Building CXX object modules/ml/CMakeFiles/opencv_ml.dir/src/nbayes.cpp.o
[ 32%] Building CXX object modules/imgproc/CMakeFiles/opencv_imgproc.dir/src/deriv.cpp.o
[ 32%] Building CXX object modules/viz/CMakeFiles/opencv_viz.dir/src/vtk/vtkTrajectorySource.cpp.o
[ 32%] Building CXX object modules/ml/CMakeFiles/opencv_ml.dir/src/data.cpp.o
[ 32%] Building CXX object modules/imgproc/CMakeFiles/opencv_imgproc.dir/src/lsd.cpp.o
[ 32%] Building CXX object modules/imgproc/CMakeFiles/opencv_imgproc.dir/src/connectedcomponents.cpp.o
[ 32%] Building CXX object modules/viz/CMakeFiles/opencv_viz.dir/src/vtk/vtkXYZWriter.cpp.o
[ 32%] Building CXX object modules/viz/CMakeFiles/opencv_viz.dir/src/vtk/vtkVizInteractorStyle.cpp.o
[ 32%] Building CXX object modules/imgproc/CMakeFiles/opencv_imgproc.dir/src/drawing.cpp.o
Linking CXX shared library ../../lib/libopencv_ml.so
[ 33%] Building CXX object modules/imgproc/CMakeFiles/opencv_imgproc.dir/src/filter.cpp.o
[ 33%] Built target opencv_ml
[ 33%] Building CXX object modules/viz/CMakeFiles/opencv_viz.dir/src/vtk/vtkXYZReader.cpp.o
[ 33%] Building CXX object modules/viz/CMakeFiles/opencv_viz.dir/src/vtk/vtkCloudMatSource.cpp.o
[ 33%] Building CXX object modules/imgproc/CMakeFiles/opencv_imgproc.dir/src/colormap.cpp.o
[ 33%] Building CXX object modules/imgproc/CMakeFiles/opencv_imgproc.dir/src/approx.cpp.o
[ 33%] Building CXX object modules/imgproc/CMakeFiles/opencv_imgproc.dir/src/morph.cpp.o
[ 33%] Building CXX object modules/imgproc/CMakeFiles/opencv_imgproc.dir/src/corner.cpp.o
[ 33%] Building CXX object modules/viz/CMakeFiles/opencv_viz.dir/src/vtk/vtkImageMatSource.cpp.o
[ 33%] Building CXX object modules/viz/CMakeFiles/opencv_viz.dir/src/vtk/vtkOBJWriter.cpp.o
[ 33%] Building CXX object modules/viz/CMakeFiles/opencv_viz.dir/src/viz3d.cpp.o
[ 33%] Building CXX object modules/imgproc/CMakeFiles/opencv_imgproc.dir/src/hershey_fonts.cpp.o
[ 33%] Building CXX object modules/imgproc/CMakeFiles/opencv_imgproc.dir/src/blend.cpp.o
[ 33%] Building CXX object modules/imgproc/CMakeFiles/opencv_imgproc.dir/src/thresh.cpp.o
[ 33%] Building CXX object modules/imgproc/CMakeFiles/opencv_imgproc.dir/src/templmatch.cpp.o
Linking CXX shared library ../../lib/libopencv_viz.so
[ 33%] Building CXX object modules/imgproc/CMakeFiles/opencv_imgproc.dir/src/samplers.cpp.o
[ 33%] Built target opencv_viz
[ 33%] Building CXX object modules/imgproc/CMakeFiles/opencv_imgproc.dir/src/main.cpp.o
[ 33%] Building CXX object modules/imgproc/CMakeFiles/opencv_imgproc.dir/src/clahe.cpp.o
[ 33%] Building CXX object modules/imgproc/CMakeFiles/opencv_imgproc.dir/src/histogram.cpp.o
[ 34%] Building CXX object modules/imgproc/CMakeFiles/opencv_imgproc.dir/src/generalized_hough.cpp.o
[ 34%] Building CXX object modules/imgproc/CMakeFiles/opencv_imgproc.dir/src/spatialgradient.cpp.o
[ 34%] Building CXX object modules/imgproc/CMakeFiles/opencv_imgproc.dir/src/geometry.cpp.o
[ 34%] Building CXX object modules/imgproc/CMakeFiles/opencv_imgproc.dir/src/grabcut.cpp.o
[ 34%] Building CXX object modules/imgproc/CMakeFiles/opencv_imgproc.dir/src/matchcontours.cpp.o
[ 34%] Building CXX object modules/imgproc/CMakeFiles/opencv_imgproc.dir/src/phasecorr.cpp.o
[ 34%] Building CXX object modules/imgproc/CMakeFiles/opencv_imgproc.dir/src/smooth.cpp.o
[ 34%] Building CXX object modules/imgproc/CMakeFiles/opencv_imgproc.dir/src/undistort.cpp.o
[ 34%] Building CXX object modules/imgproc/CMakeFiles/opencv_imgproc.dir/src/sumpixels.cpp.o
[ 34%] Building CXX object modules/imgproc/CMakeFiles/opencv_imgproc.dir/src/distransform.cpp.o
[ 34%] Building CXX object modules/imgproc/CMakeFiles/opencv_imgproc.dir/src/min_enclosing_triangle.cpp.o
[ 34%] Building CXX object modules/imgproc/CMakeFiles/opencv_imgproc.dir/src/color.cpp.o
[ 34%] Building CXX object modules/imgproc/CMakeFiles/opencv_imgproc.dir/src/accum.cpp.o
[ 34%] Building CXX object modules/imgproc/CMakeFiles/opencv_imgproc.dir/src/convhull.cpp.o
[ 35%] Building CXX object modules/imgproc/CMakeFiles/opencv_imgproc.dir/src/hough.cpp.o
[ 35%] Building CXX object modules/imgproc/CMakeFiles/opencv_imgproc.dir/src/demosaicing.cpp.o
[ 35%] Building CXX object modules/imgproc/CMakeFiles/opencv_imgproc.dir/src/cornersubpix.cpp.o
[ 35%] Building CXX object modules/imgproc/CMakeFiles/opencv_imgproc.dir/src/pyramids.cpp.o
[ 35%] Building CXX object modules/imgproc/CMakeFiles/opencv_imgproc.dir/src/floodfill.cpp.o
[ 35%] Building CXX object modules/imgproc/CMakeFiles/opencv_imgproc.dir/src/subdivision2d.cpp.o
[ 35%] Building CXX object modules/imgproc/CMakeFiles/opencv_imgproc.dir/src/rotcalipers.cpp.o
[ 35%] Building CXX object modules/imgproc/CMakeFiles/opencv_imgproc.dir/src/moments.cpp.o
[ 35%] Building CXX object modules/imgproc/CMakeFiles/opencv_imgproc.dir/src/canny.cpp.o
[ 35%] Building CXX object modules/imgproc/CMakeFiles/opencv_imgproc.dir/opencl_kernels_imgproc.cpp.o
Linking CXX shared library ../../lib/libopencv_imgproc.so
[ 35%] Built target opencv_imgproc
[ 35%] [ 35%] Scanning dependencies of target opencv_imgcodecs
Generating opencl_kernels_video.cpp, opencl_kernels_video.hpp
Generating opencl_kernels_photo.cpp, opencl_kernels_photo.hpp
Scanning dependencies of target opencv_photo
Scanning dependencies of target opencv_video
[ 35%] [ 37%] Building CXX object modules/video/CMakeFiles/opencv_video.dir/src/ecc.cpp.o
Building CXX object modules/video/CMakeFiles/opencv_video.dir/src/lkpyramid.cpp.o
[ 37%] Building CXX object modules/photo/CMakeFiles/opencv_photo.dir/src/hdr_common.cpp.o
[ 37%] Building CXX object modules/imgcodecs/CMakeFiles/opencv_imgcodecs.dir/src/loadsave.cpp.o
[ 37%] Building CXX object modules/photo/CMakeFiles/opencv_photo.dir/src/seamless_cloning.cpp.o
[ 37%] Building CXX object modules/photo/CMakeFiles/opencv_photo.dir/src/tonemap.cpp.o
[ 37%] Building CXX object modules/imgcodecs/CMakeFiles/opencv_imgcodecs.dir/src/utils.cpp.o
[ 37%] Building CXX object modules/video/CMakeFiles/opencv_video.dir/src/optflowgf.cpp.o
[ 37%] Building CXX object modules/imgcodecs/CMakeFiles/opencv_imgcodecs.dir/src/grfmt_hdr.cpp.o
[ 37%] Building CXX object modules/photo/CMakeFiles/opencv_photo.dir/src/contrast_preserve.cpp.o
[ 37%] Building CXX object modules/imgcodecs/CMakeFiles/opencv_imgcodecs.dir/src/grfmt_exr.cpp.o
[ 37%] Building CXX object modules/video/CMakeFiles/opencv_video.dir/src/bgfg_KNN.cpp.o
[ 37%] Building CXX object modules/video/CMakeFiles/opencv_video.dir/src/tvl1flow.cpp.o
[ 37%] Building CXX object modules/imgcodecs/CMakeFiles/opencv_imgcodecs.dir/src/grfmt_bmp.cpp.o
[ 37%] Building CXX object modules/photo/CMakeFiles/opencv_photo.dir/src/inpaint.cpp.o
[ 37%] Building CXX object modules/video/CMakeFiles/opencv_video.dir/src/kalman.cpp.o
[ 37%] Building CXX object modules/imgcodecs/CMakeFiles/opencv_imgcodecs.dir/src/grfmt_jpeg.cpp.o
[ 37%] Building CXX object modules/photo/CMakeFiles/opencv_photo.dir/src/npr.cpp.o
[ 37%] Building CXX object modules/imgcodecs/CMakeFiles/opencv_imgcodecs.dir/src/grfmt_gdal.cpp.o
[ 37%] Building CXX object modules/imgcodecs/CMakeFiles/opencv_imgcodecs.dir/src/grfmt_tiff.cpp.o
[ 37%] Building CXX object modules/video/CMakeFiles/opencv_video.dir/src/camshift.cpp.o
[ 37%] [ 37%] Building CXX object modules/imgcodecs/CMakeFiles/opencv_imgcodecs.dir/src/grfmt_sunras.cpp.o
Building CXX object modules/imgcodecs/CMakeFiles/opencv_imgcodecs.dir/src/grfmt_webp.cpp.o
[ 37%] Building CXX object modules/video/CMakeFiles/opencv_video.dir/src/bgfg_gaussmix2.cpp.o
[ 37%] Building CXX object modules/imgcodecs/CMakeFiles/opencv_imgcodecs.dir/src/grfmt_base.cpp.o
[ 37%] Building CXX object modules/imgcodecs/CMakeFiles/opencv_imgcodecs.dir/src/grfmt_png.cpp.o
[ 38%] Building CXX object modules/photo/CMakeFiles/opencv_photo.dir/src/denoising.cpp.o
[ 38%] Building CXX object modules/photo/CMakeFiles/opencv_photo.dir/src/align.cpp.o
[ 38%] Building CXX object modules/video/CMakeFiles/opencv_video.dir/src/compat_video.cpp.o
[ 39%] Building CXX object modules/imgcodecs/CMakeFiles/opencv_imgcodecs.dir/src/grfmt_pxm.cpp.o
[ 39%] Building CXX object modules/video/CMakeFiles/opencv_video.dir/opencl_kernels_video.cpp.o
Linking CXX shared library ../../lib/libopencv_video.so
[ 39%] Built target opencv_video
[ 39%] Building CXX object modules/photo/CMakeFiles/opencv_photo.dir/src/calibrate.cpp.o
[ 39%] Building CXX object modules/imgcodecs/CMakeFiles/opencv_imgcodecs.dir/src/grfmt_jpeg2000.cpp.o
[ 39%] Building CXX object modules/imgcodecs/CMakeFiles/opencv_imgcodecs.dir/src/bitstrm.cpp.o
Scanning dependencies of target opencv_shape
[ 39%] Building CXX object modules/shape/CMakeFiles/opencv_shape.dir/src/emdL1.cpp.o
[ 39%] Building CXX object modules/imgcodecs/CMakeFiles/opencv_imgcodecs.dir/src/rgbe.cpp.o
[ 39%] Building CXX object modules/imgcodecs/CMakeFiles/opencv_imgcodecs.dir/src/jpeg_exif.cpp.o
[ 39%] Building CXX object modules/photo/CMakeFiles/opencv_photo.dir/src/seamless_cloning_impl.cpp.o
Linking CXX shared library ../../lib/libopencv_imgcodecs.so
[ 39%] Built target opencv_imgcodecs
[ 39%] Building CXX object modules/shape/CMakeFiles/opencv_shape.dir/src/sc_dis.cpp.o
[ 39%] Building CXX object modules/shape/CMakeFiles/opencv_shape.dir/src/hist_cost.cpp.o
[ 39%] Building CXX object modules/photo/CMakeFiles/opencv_photo.dir/src/denoising.cuda.cpp.o
[ 39%] Building CXX object modules/photo/CMakeFiles/opencv_photo.dir/src/merge.cpp.o
[ 39%] Building CXX object modules/photo/CMakeFiles/opencv_photo.dir/src/denoise_tvl1.cpp.o
[ 39%] Building CXX object modules/shape/CMakeFiles/opencv_shape.dir/src/aff_trans.cpp.o
[ 39%] Building CXX object modules/shape/CMakeFiles/opencv_shape.dir/src/tps_trans.cpp.o
Scanning dependencies of target opencv_videoio
[ 39%] Building CXX object modules/shape/CMakeFiles/opencv_shape.dir/src/precomp.cpp.o
[ 39%] Building CXX object modules/shape/CMakeFiles/opencv_shape.dir/src/haus_dis.cpp.o
[ 39%] Building CXX object modules/videoio/CMakeFiles/opencv_videoio.dir/src/cap.cpp.o
[ 39%] Building CXX object modules/videoio/CMakeFiles/opencv_videoio.dir/src/cap_images.cpp.o
Linking CXX shared library ../../lib/libopencv_shape.so
[ 39%] Built target opencv_shape
[ 39%] Building CXX object modules/videoio/CMakeFiles/opencv_videoio.dir/src/cap_mjpeg_encoder.cpp.o
[ 39%] Building CXX object modules/photo/CMakeFiles/opencv_photo.dir/opencl_kernels_photo.cpp.o
[ 39%] Building CXX object modules/videoio/CMakeFiles/opencv_videoio.dir/src/cap_mjpeg_decoder.cpp.o
[ 40%] Building CXX object modules/videoio/CMakeFiles/opencv_videoio.dir/src/cap_dc1394_v2.cpp.o
[ 40%] Building CXX object modules/videoio/CMakeFiles/opencv_videoio.dir/src/cap_gstreamer.cpp.o
[ 40%] Building CXX object modules/videoio/CMakeFiles/opencv_videoio.dir/src/cap_v4l.cpp.o
[ 40%] Building CXX object modules/videoio/CMakeFiles/opencv_videoio.dir/src/cap_ffmpeg.cpp.o
Linking CXX shared library ../../lib/libopencv_photo.so
[ 40%] Built target opencv_photo
Linking CXX shared library ../../lib/libopencv_videoio.so
[ 40%] Built target opencv_videoio
[ 40%] Scanning dependencies of target opencv_highgui
Generating opencl_kernels_superres.cpp, opencl_kernels_superres.hpp
Scanning dependencies of target opencv_superres
[ 40%] [ 40%] [ 40%] Building CXX object modules/superres/CMakeFiles/opencv_superres.dir/src/frame_source.cpp.o
Building CXX object modules/superres/CMakeFiles/opencv_superres.dir/src/optical_flow.cpp.o
Building CXX object modules/superres/CMakeFiles/opencv_superres.dir/src/btv_l1.cpp.o
[ 40%] Building CXX object modules/highgui/CMakeFiles/opencv_highgui.dir/src/window.cpp.o
[ 41%] Building CXX object modules/superres/CMakeFiles/opencv_superres.dir/src/btv_l1_cuda.cpp.o
[ 41%] Building CXX object modules/highgui/CMakeFiles/opencv_highgui.dir/src/window_gtk.cpp.o
[ 41%] Building CXX object modules/superres/CMakeFiles/opencv_superres.dir/src/input_array_utility.cpp.o
[ 41%] Building CXX object modules/superres/CMakeFiles/opencv_superres.dir/src/super_resolution.cpp.o
[ 41%] Building CXX object modules/superres/CMakeFiles/opencv_superres.dir/opencl_kernels_superres.cpp.o
Linking CXX shared library ../../lib/libopencv_highgui.so
[ 41%] Built target opencv_highgui
[ 41%] Generating opencl_kernels_objdetect.cpp, opencl_kernels_objdetect.hpp
[ 41%] Scanning dependencies of target opencv_ts
Generating opencl_kernels_features2d.cpp, opencl_kernels_features2d.hpp
Scanning dependencies of target opencv_objdetect
Scanning dependencies of target opencv_features2d
Linking CXX shared library ../../lib/libopencv_superres.so
[ 41%] [ 43%] Building CXX object modules/objdetect/CMakeFiles/opencv_objdetect.dir/src/cascadedetect.cpp.o
Building CXX object modules/ts/CMakeFiles/opencv_ts.dir/src/ocl_test.cpp.o
[ 43%] Built target opencv_superres
[ 43%] Building CXX object modules/objdetect/CMakeFiles/opencv_objdetect.dir/src/haar.cpp.o
[ 43%] Building CXX object modules/features2d/CMakeFiles/opencv_features2d.dir/src/fast_score.cpp.o
[ 43%] Building CXX object modules/features2d/CMakeFiles/opencv_features2d.dir/src/kaze.cpp.o
[ 43%] Building CXX object modules/ts/CMakeFiles/opencv_ts.dir/src/cuda_perf.cpp.o
[ 43%] Building CXX object modules/features2d/CMakeFiles/opencv_features2d.dir/src/feature2d.cpp.o
Scanning dependencies of target opencv_annotation
[ 43%] Building CXX object apps/annotation/CMakeFiles/opencv_annotation.dir/opencv_annotation.cpp.o
Linking CXX executable ../../bin/opencv_annotation
[ 43%] Building CXX object modules/features2d/CMakeFiles/opencv_features2d.dir/src/bagofwords.cpp.o
[ 43%] Building CXX object modules/ts/CMakeFiles/opencv_ts.dir/src/ts_perf.cpp.o
[ 43%] Built target opencv_annotation
[ 43%] Building CXX object modules/ts/CMakeFiles/opencv_ts.dir/src/cuda_test.cpp.o
[ 43%] Building CXX object modules/objdetect/CMakeFiles/opencv_objdetect.dir/src/detection_based_tracker.cpp.o
[ 43%] Building CXX object modules/features2d/CMakeFiles/opencv_features2d.dir/src/agast.cpp.o
[ 43%] Building CXX object modules/objdetect/CMakeFiles/opencv_objdetect.dir/src/hog.cpp.o
[ 43%] Building CXX object modules/features2d/CMakeFiles/opencv_features2d.dir/src/blobdetector.cpp.o
[ 43%] Building CXX object modules/ts/CMakeFiles/opencv_ts.dir/src/ocl_perf.cpp.o
[ 43%] Building CXX object modules/ts/CMakeFiles/opencv_ts.dir/src/ts_arrtest.cpp.o
[ 43%] Building CXX object modules/features2d/CMakeFiles/opencv_features2d.dir/src/mser.cpp.o
[ 44%] Building CXX object modules/objdetect/CMakeFiles/opencv_objdetect.dir/src/main.cpp.o
[ 44%] Building CXX object modules/ts/CMakeFiles/opencv_ts.dir/src/ts_func.cpp.o
[ 44%] Building CXX object modules/objdetect/CMakeFiles/opencv_objdetect.dir/src/cascadedetect_convert.cpp.o
[ 44%] Building CXX object modules/features2d/CMakeFiles/opencv_features2d.dir/src/fast.cpp.o
[ 44%] Building CXX object modules/ts/CMakeFiles/opencv_ts.dir/src/ts.cpp.o
[ 44%] Building CXX object modules/objdetect/CMakeFiles/opencv_objdetect.dir/opencl_kernels_objdetect.cpp.o
[ 44%] Building CXX object modules/features2d/CMakeFiles/opencv_features2d.dir/src/main.cpp.o
Linking CXX shared library ../../lib/libopencv_objdetect.so
[ 44%] Built target opencv_objdetect
[ 44%] Building CXX object modules/ts/CMakeFiles/opencv_ts.dir/src/ts_gtest.cpp.o
[ 45%] Building CXX object modules/features2d/CMakeFiles/opencv_features2d.dir/src/orb.cpp.o
[ 45%] Building CXX object modules/features2d/CMakeFiles/opencv_features2d.dir/src/agast_score.cpp.o
[ 45%] Building CXX object modules/features2d/CMakeFiles/opencv_features2d.dir/src/brisk.cpp.o
[ 45%] Building CXX object modules/features2d/CMakeFiles/opencv_features2d.dir/src/dynamic.cpp.o
[ 45%] Building CXX object modules/features2d/CMakeFiles/opencv_features2d.dir/src/evaluation.cpp.o
[ 45%] Building CXX object modules/features2d/CMakeFiles/opencv_features2d.dir/src/kaze/AKAZEFeatures.cpp.o
[ 45%] Building CXX object modules/features2d/CMakeFiles/opencv_features2d.dir/src/kaze/KAZEFeatures.cpp.o
[ 45%] Building CXX object modules/features2d/CMakeFiles/opencv_features2d.dir/src/kaze/nldiffusion_functions.cpp.o
Linking CXX static library ../../lib/libopencv_ts.a
[ 45%] Built target opencv_ts
[ 45%] Building CXX object modules/features2d/CMakeFiles/opencv_features2d.dir/src/kaze/fed.cpp.o
Scanning dependencies of target opencv_perf_core
[ 45%] Building CXX object modules/core/CMakeFiles/opencv_perf_core.dir/perf/perf_stat.cpp.o
Scanning dependencies of target opencv_test_core
[ 45%] Building CXX object modules/features2d/CMakeFiles/opencv_features2d.dir/src/draw.cpp.o
[ 45%] Building CXX object modules/core/CMakeFiles/opencv_test_core.dir/test/test_ds.cpp.o
Scanning dependencies of target opencv_test_flann
[ 45%] Building CXX object modules/flann/CMakeFiles/opencv_test_flann.dir/test/test_main.cpp.o
[ 45%] Building CXX object modules/features2d/CMakeFiles/opencv_features2d.dir/src/akaze.cpp.o
[ 45%] Building CXX object modules/flann/CMakeFiles/opencv_test_flann.dir/test/test_lshtable_badarg.cpp.o
[ 45%] Building CXX object modules/core/CMakeFiles/opencv_perf_core.dir/perf/perf_minmaxloc.cpp.o
[ 45%] Building CXX object modules/core/CMakeFiles/opencv_test_core.dir/test/test_downhill_simplex.cpp.o
[ 45%] Building CXX object modules/features2d/CMakeFiles/opencv_features2d.dir/src/keypoint.cpp.o
Linking CXX executable ../../bin/opencv_test_flann
[ 45%] Built target opencv_test_flann
[ 45%] Building CXX object modules/core/CMakeFiles/opencv_test_core.dir/test/test_eigen.cpp.o
[ 45%] Building CXX object modules/features2d/CMakeFiles/opencv_features2d.dir/src/gftt.cpp.o
[ 46%] Building CXX object modules/features2d/CMakeFiles/opencv_features2d.dir/src/matchers.cpp.o
[ 46%] Building CXX object modules/core/CMakeFiles/opencv_perf_core.dir/perf/perf_convertTo.cpp.o
[ 46%] Building CXX object modules/core/CMakeFiles/opencv_perf_core.dir/perf/perf_abs.cpp.o
[ 46%] Building CXX object modules/core/CMakeFiles/opencv_test_core.dir/test/test_dxt.cpp.o
[ 46%] Building CXX object modules/features2d/CMakeFiles/opencv_features2d.dir/opencl_kernels_features2d.cpp.o
[ 46%] Building CXX object modules/core/CMakeFiles/opencv_test_core.dir/test/test_rand.cpp.o
[ 46%] Building CXX object modules/core/CMakeFiles/opencv_perf_core.dir/perf/perf_cvround.cpp.o
Linking CXX shared library ../../lib/libopencv_features2d.so
[ 46%] Building CXX object modules/core/CMakeFiles/opencv_test_core.dir/test/test_conjugate_gradient.cpp.o
[ 46%] Built target opencv_features2d
[ 46%] Building CXX object modules/core/CMakeFiles/opencv_perf_core.dir/perf/perf_addWeighted.cpp.o
[ 46%] Building CXX object modules/core/CMakeFiles/opencv_test_core.dir/test/test_io.cpp.o
[ 46%] Building CXX object modules/core/CMakeFiles/opencv_test_core.dir/test/test_misc.cpp.o
[ 46%] Building CXX object modules/core/CMakeFiles/opencv_test_core.dir/test/test_ptr.cpp.o
[ 48%] Building CXX object modules/core/CMakeFiles/opencv_perf_core.dir/perf/perf_mat.cpp.o
Scanning dependencies of target opencv_perf_imgproc
[ 48%] Building CXX object modules/core/CMakeFiles/opencv_perf_core.dir/perf/perf_dot.cpp.o
[ 48%] Building CXX object modules/imgproc/CMakeFiles/opencv_perf_imgproc.dir/perf/perf_corners.cpp.o
[ 48%] Building CXX object modules/core/CMakeFiles/opencv_test_core.dir/test/test_ippasync.cpp.o
[ 49%] Building CXX object modules/core/CMakeFiles/opencv_test_core.dir/test/test_umat.cpp.o
[ 49%] Building CXX object modules/core/CMakeFiles/opencv_perf_core.dir/perf/perf_arithm.cpp.o
[ 49%] Building CXX object modules/imgproc/CMakeFiles/opencv_perf_imgproc.dir/perf/perf_remap.cpp.o
[ 49%] Building CXX object modules/core/CMakeFiles/opencv_perf_core.dir/perf/perf_dft.cpp.o
[ 50%] Building CXX object modules/imgproc/CMakeFiles/opencv_perf_imgproc.dir/perf/perf_spatialgradient.cpp.o
[ 50%] Building CXX object modules/imgproc/CMakeFiles/opencv_perf_imgproc.dir/perf/perf_sepfilters.cpp.o
[ 50%] Building CXX object modules/core/CMakeFiles/opencv_perf_core.dir/perf/perf_math.cpp.o
[ 50%] Building CXX object modules/core/CMakeFiles/opencv_test_core.dir/test/test_utils.cpp.o
[ 50%] Building CXX object modules/core/CMakeFiles/opencv_perf_core.dir/perf/perf_umat.cpp.o
[ 50%] Building CXX object modules/core/CMakeFiles/opencv_test_core.dir/test/ocl/test_channels.cpp.o
[ 50%] Building CXX object modules/core/CMakeFiles/opencv_perf_core.dir/perf/perf_norm.cpp.o
[ 50%] Building CXX object modules/core/CMakeFiles/opencv_perf_core.dir/perf/cuda/perf_gpumat.cpp.o
[ 50%] Building CXX object modules/imgproc/CMakeFiles/opencv_perf_imgproc.dir/perf/perf_goodFeaturesToTrack.cpp.o
[ 50%] Building CXX object modules/core/CMakeFiles/opencv_test_core.dir/test/ocl/test_dft.cpp.o
[ 50%] Building CXX object modules/imgproc/CMakeFiles/opencv_perf_imgproc.dir/perf/perf_resize.cpp.o
[ 50%] Building CXX object modules/core/CMakeFiles/opencv_perf_core.dir/perf/perf_reduce.cpp.o
[ 50%] Building CXX object modules/core/CMakeFiles/opencv_perf_core.dir/perf/perf_compare.cpp.o
[ 50%] Building CXX object modules/core/CMakeFiles/opencv_test_core.dir/test/ocl/test_gemm.cpp.o
[ 50%] Building CXX object modules/imgproc/CMakeFiles/opencv_perf_imgproc.dir/perf/perf_integral.cpp.o
[ 50%] Building CXX object modules/imgproc/CMakeFiles/opencv_perf_imgproc.dir/perf/perf_distanceTransform.cpp.o
[ 50%] Building CXX object modules/core/CMakeFiles/opencv_perf_core.dir/perf/perf_inRange.cpp.o
[ 50%] Building CXX object modules/core/CMakeFiles/opencv_test_core.dir/test/ocl/test_matrix_expr.cpp.o
[ 50%] Building CXX object modules/core/CMakeFiles/opencv_perf_core.dir/perf/perf_sort.cpp.o
[ 50%] Building CXX object modules/imgproc/CMakeFiles/opencv_perf_imgproc.dir/perf/perf_warp.cpp.o
Scanning dependencies of target opencv_test_imgproc
[ 50%] Building CXX object modules/imgproc/CMakeFiles/opencv_test_imgproc.dir/test/test_grabcut.cpp.o
[ 50%] Building CXX object modules/core/CMakeFiles/opencv_test_core.dir/test/ocl/test_arithm.cpp.o
[ 50%] Building CXX object modules/core/CMakeFiles/opencv_perf_core.dir/perf/opencl/perf_gemm.cpp.o
[ 50%] Building CXX object modules/imgproc/CMakeFiles/opencv_test_imgproc.dir/test/test_histograms.cpp.o
[ 50%] Building CXX object modules/imgproc/CMakeFiles/opencv_perf_imgproc.dir/perf/perf_cvt_color.cpp.o
[ 50%] Building CXX object modules/imgproc/CMakeFiles/opencv_test_imgproc.dir/test/test_imgproc_umat.cpp.o
[ 51%] Building CXX object modules/core/CMakeFiles/opencv_perf_core.dir/perf/opencl/perf_channels.cpp.o
[ 53%] Building CXX object modules/imgproc/CMakeFiles/opencv_test_imgproc.dir/test/test_thresh.cpp.o
[ 53%] Building CXX object modules/imgproc/CMakeFiles/opencv_test_imgproc.dir/test/test_convhull.cpp.o
[ 53%] Building CXX object modules/imgproc/CMakeFiles/opencv_perf_imgproc.dir/perf/perf_phasecorr.cpp.o
[ 53%] Building CXX object modules/core/CMakeFiles/opencv_perf_core.dir/perf/opencl/perf_usage_flags.cpp.o
[ 53%] Building CXX object modules/imgproc/CMakeFiles/opencv_perf_imgproc.dir/perf/perf_threshold.cpp.o
[ 53%] Building CXX object modules/core/CMakeFiles/opencv_perf_core.dir/perf/opencl/perf_dxt.cpp.o
[ 53%] Building CXX object modules/imgproc/CMakeFiles/opencv_test_imgproc.dir/test/test_emd.cpp.o
[ 53%] Building CXX object modules/imgproc/CMakeFiles/opencv_test_imgproc.dir/test/test_contours.cpp.o
[ 53%] Building CXX object modules/imgproc/CMakeFiles/opencv_perf_imgproc.dir/perf/perf_blur.cpp.o
[ 53%] Building CXX object modules/core/CMakeFiles/opencv_test_core.dir/test/ocl/test_image2d.cpp.o
[ 53%] Building CXX object modules/core/CMakeFiles/opencv_perf_core.dir/perf/opencl/perf_arithm.cpp.o
[ 53%] Building CXX object modules/imgproc/CMakeFiles/opencv_test_imgproc.dir/test/ocl/test_match_template.cpp.o
[ 53%] Building CXX object modules/core/CMakeFiles/opencv_test_core.dir/test/ocl/test_matrix_operation.cpp.o
[ 53%] Building CXX object modules/imgproc/CMakeFiles/opencv_test_imgproc.dir/test/ocl/test_filter2d.cpp.o
[ 53%] Building CXX object modules/imgproc/CMakeFiles/opencv_perf_imgproc.dir/perf/perf_moments.cpp.o
[ 53%] Building CXX object modules/core/CMakeFiles/opencv_test_core.dir/test/test_main.cpp.o
[ 53%] Building CXX object modules/imgproc/CMakeFiles/opencv_perf_imgproc.dir/perf/perf_houghLines.cpp.o
[ 53%] Building CXX object modules/core/CMakeFiles/opencv_test_core.dir/test/test_mat.cpp.o
[ 53%] Building CXX object modules/imgproc/CMakeFiles/opencv_test_imgproc.dir/test/ocl/test_warp.cpp.o
[ 54%] Building CXX object modules/imgproc/CMakeFiles/opencv_perf_imgproc.dir/perf/perf_canny.cpp.o
[ 54%] Building CXX object modules/imgproc/CMakeFiles/opencv_perf_imgproc.dir/perf/perf_morph.cpp.o
[ 54%] Building CXX object modules/core/CMakeFiles/opencv_test_core.dir/test/test_arithm.cpp.o
[ 54%] Building CXX object modules/imgproc/CMakeFiles/opencv_test_imgproc.dir/test/ocl/test_pyramids.cpp.o
[ 54%] Building CXX object modules/imgproc/CMakeFiles/opencv_perf_imgproc.dir/perf/perf_histogram.cpp.o
[ 54%] Building CXX object modules/imgproc/CMakeFiles/opencv_test_imgproc.dir/test/ocl/test_blend.cpp.o
[ 54%] Building CXX object modules/core/CMakeFiles/opencv_perf_core.dir/perf/opencl/perf_bufferpool.cpp.o
[ 54%] Building CXX object modules/imgproc/CMakeFiles/opencv_perf_imgproc.dir/perf/perf_bilateral.cpp.o
[ 54%] Building CXX object modules/core/CMakeFiles/opencv_perf_core.dir/perf/opencl/perf_matop.cpp.o
[ 54%] Building CXX object modules/imgproc/CMakeFiles/opencv_test_imgproc.dir/test/ocl/test_filters.cpp.o
[ 54%] Building CXX object modules/core/CMakeFiles/opencv_test_core.dir/test/test_intrin.cpp.o
[ 54%] Building CXX object modules/imgproc/CMakeFiles/opencv_perf_imgproc.dir/perf/perf_pyramids.cpp.o
[ 54%] Building CXX object modules/core/CMakeFiles/opencv_perf_core.dir/perf/perf_main.cpp.o
[ 54%] Building CXX object modules/core/CMakeFiles/opencv_perf_core.dir/perf/perf_merge.cpp.o
[ 54%] Building CXX object modules/imgproc/CMakeFiles/opencv_perf_imgproc.dir/perf/perf_filter2d.cpp.o
[ 54%] Building CXX object modules/imgproc/CMakeFiles/opencv_test_imgproc.dir/test/ocl/test_accumulate.cpp.o
[ 54%] Building CXX object modules/core/CMakeFiles/opencv_perf_core.dir/perf/perf_split.cpp.o
[ 54%] Building CXX object modules/imgproc/CMakeFiles/opencv_perf_imgproc.dir/perf/opencl/perf_imgproc.cpp.o
[ 54%] Building CXX object modules/imgproc/CMakeFiles/opencv_test_imgproc.dir/test/ocl/test_histogram.cpp.o
[ 54%] Building CXX object modules/core/CMakeFiles/opencv_perf_core.dir/perf/perf_bitwise.cpp.o
Linking CXX executable ../../bin/opencv_perf_core
[ 54%] Built target opencv_perf_core
[ 54%] Building CXX object modules/imgproc/CMakeFiles/opencv_perf_imgproc.dir/perf/opencl/perf_accumulate.cpp.o
[ 54%] Building CXX object modules/imgproc/CMakeFiles/opencv_test_imgproc.dir/test/ocl/test_boxfilter.cpp.o
[ 54%] Building CXX object modules/imgproc/CMakeFiles/opencv_perf_imgproc.dir/perf/opencl/perf_3vs4.cpp.o
[ 55%] Building CXX object modules/core/CMakeFiles/opencv_test_core.dir/test/test_hal_core.cpp.o
[ 55%] Building CXX object modules/imgproc/CMakeFiles/opencv_perf_imgproc.dir/perf/opencl/perf_imgwarp.cpp.o
[ 55%] Building CXX object modules/core/CMakeFiles/opencv_test_core.dir/test/test_countnonzero.cpp.o
[ 56%] Building CXX object modules/imgproc/CMakeFiles/opencv_test_imgproc.dir/test/ocl/test_imgproc.cpp.o
[ 56%] Building CXX object modules/imgproc/CMakeFiles/opencv_perf_imgproc.dir/perf/opencl/perf_pyramid.cpp.o
[ 56%] Building CXX object modules/core/CMakeFiles/opencv_test_core.dir/test/test_lpsolver.cpp.o
[ 56%] Building CXX object modules/core/CMakeFiles/opencv_test_core.dir/test/test_operations.cpp.o
[ 56%] Building CXX object modules/imgproc/CMakeFiles/opencv_perf_imgproc.dir/perf/opencl/perf_moments.cpp.o
[ 56%] Building CXX object modules/imgproc/CMakeFiles/opencv_perf_imgproc.dir/perf/opencl/perf_houghLines.cpp.o
Scanning dependencies of target opencv_test_ml
[ 56%] Building CXX object modules/ml/CMakeFiles/opencv_test_ml.dir/test/test_gbttest.cpp.o
[ 56%] Building CXX object modules/imgproc/CMakeFiles/opencv_test_imgproc.dir/test/ocl/test_medianfilter.cpp.o
[ 56%] Building CXX object modules/ml/CMakeFiles/opencv_test_ml.dir/test/test_main.cpp.o
[ 56%] Building CXX object modules/ml/CMakeFiles/opencv_test_ml.dir/test/test_lr.cpp.o
[ 58%] Building CXX object modules/imgproc/CMakeFiles/opencv_perf_imgproc.dir/perf/opencl/perf_blend.cpp.o
[ 58%] Building CXX object modules/ml/CMakeFiles/opencv_test_ml.dir/test/test_emknearestkmeans.cpp.o
[ 58%] Building CXX object modules/imgproc/CMakeFiles/opencv_test_imgproc.dir/test/ocl/test_color.cpp.o
[ 58%] Building CXX object modules/imgproc/CMakeFiles/opencv_perf_imgproc.dir/perf/opencl/perf_gftt.cpp.o
[ 58%] Building CXX object modules/core/CMakeFiles/opencv_test_core.dir/test/test_math.cpp.o
[ 58%] Building CXX object modules/ml/CMakeFiles/opencv_test_ml.dir/test/test_mltests2.cpp.o
In file included from /home/cobalt/opencv/modules/ts/include/opencv2/ts.hpp:28:0,
                 from /home/cobalt/opencv/modules/core/test/test_precomp.hpp:13,
                 from /home/cobalt/opencv/modules/core/test/test_math.cpp:5:
/home/cobalt/opencv/modules/ts/include/opencv2/ts/ts_gtest.h: In instantiation of ‘testing::AssertionResult testing::internal::CmpHelperEQ(const char*, const char*, const T1&, const T2&) [with T1 = int; T2 = unsigned int]’:
/home/cobalt/opencv/modules/ts/include/opencv2/ts/ts_gtest.h:18962:30:   required from ‘static testing::AssertionResult testing::internal::EqHelper<lhs_is_null_literal>::Compare(const char*, const char*, const T1&, const T2&) [with T1 = int; T2 = unsigned int; bool lhs_is_null_literal = false]’
/home/cobalt/opencv/modules/core/test/test_math.cpp:2387:9:   required from here
/home/cobalt/opencv/modules/ts/include/opencv2/ts/ts_gtest.h:18925:16: warning: comparison between signed and unsigned integer expressions [-Wsign-compare]
   if (expected == actual) {
                ^
[ 58%] Building CXX object modules/imgproc/CMakeFiles/opencv_perf_imgproc.dir/perf/opencl/perf_filters.cpp.o
[ 59%] Building CXX object modules/ml/CMakeFiles/opencv_test_ml.dir/test/test_mltests.cpp.o
[ 59%] Building CXX object modules/ml/CMakeFiles/opencv_test_ml.dir/test/test_svmtrainauto.cpp.o
[ 59%] Building CXX object modules/ml/CMakeFiles/opencv_test_ml.dir/test/test_save_load.cpp.o
[ 59%] Building CXX object modules/imgproc/CMakeFiles/opencv_test_imgproc.dir/test/ocl/test_canny.cpp.o
Linking CXX executable ../../bin/opencv_test_ml
[ 59%] Built target opencv_test_ml
[ 59%] [ 59%] Building CXX object modules/imgproc/CMakeFiles/opencv_test_imgproc.dir/test/ocl/test_sepfilter2D.cpp.o
Building CXX object modules/imgproc/CMakeFiles/opencv_perf_imgproc.dir/perf/opencl/perf_matchTemplate.cpp.o
[ 59%] Building CXX object modules/core/CMakeFiles/opencv_test_core.dir/test/test_rotatedrect.cpp.o
[ 59%] Building CXX object modules/imgproc/CMakeFiles/opencv_perf_imgproc.dir/perf/opencl/perf_color.cpp.o
[ 59%] Building CXX object modules/core/CMakeFiles/opencv_test_core.dir/test/test_concatenation.cpp.o
Scanning dependencies of target opencv_perf_photo
[ 59%] Building CXX object modules/photo/CMakeFiles/opencv_perf_photo.dir/perf/perf_cuda.cpp.o
[ 59%] Building CXX object modules/imgproc/CMakeFiles/opencv_test_imgproc.dir/test/ocl/test_gftt.cpp.o
[ 59%] Building CXX object modules/photo/CMakeFiles/opencv_perf_photo.dir/perf/perf_inpaint.cpp.o
Linking CXX executable ../../bin/opencv_test_core
[ 59%] Building CXX object modules/imgproc/CMakeFiles/opencv_perf_imgproc.dir/perf/perf_main.cpp.o
[ 59%] Built target opencv_test_core
Scanning dependencies of target opencv_test_photo
[ 59%] Building CXX object modules/photo/CMakeFiles/opencv_test_photo.dir/test/test_npr.cpp.o
[ 59%] Building CXX object modules/imgproc/CMakeFiles/opencv_perf_imgproc.dir/perf/perf_matchTemplate.cpp.o
[ 59%] Building CXX object modules/imgproc/CMakeFiles/opencv_test_imgproc.dir/test/ocl/test_houghlines.cpp.o
[ 59%] Building CXX object modules/photo/CMakeFiles/opencv_test_photo.dir/test/test_cloning.cpp.o
[ 59%] Building CXX object modules/photo/CMakeFiles/opencv_perf_photo.dir/perf/opencl/perf_denoising.cpp.o
[ 59%] Building CXX object modules/imgproc/CMakeFiles/opencv_perf_imgproc.dir/perf/perf_floodfill.cpp.o
[ 59%] Building CXX object modules/photo/CMakeFiles/opencv_perf_photo.dir/perf/perf_main.cpp.o
[ 59%] Building CXX object modules/photo/CMakeFiles/opencv_test_photo.dir/test/test_denoising.cuda.cpp.o
Linking CXX executable ../../bin/opencv_perf_photo
[ 59%] Built target opencv_perf_photo
Scanning dependencies of target opencv_perf_video
[ 59%] Building CXX object modules/video/CMakeFiles/opencv_perf_video.dir/perf/perf_tvl1optflow.cpp.o
[ 59%] Building CXX object modules/imgproc/CMakeFiles/opencv_test_imgproc.dir/test/test_main.cpp.o
[ 60%] Building CXX object modules/photo/CMakeFiles/opencv_test_photo.dir/test/ocl/test_denoising.cpp.o
Linking CXX executable ../../bin/opencv_perf_imgproc
[ 60%] Built target opencv_perf_imgproc
[ 60%] Building CXX object modules/imgproc/CMakeFiles/opencv_test_imgproc.dir/test/test_boundingrect.cpp.o
[ 60%] Building CXX object modules/imgproc/CMakeFiles/opencv_test_imgproc.dir/test/test_goodfeaturetotrack.cpp.o
[ 60%] Building CXX object modules/video/CMakeFiles/opencv_perf_video.dir/perf/perf_optflowpyrlk.cpp.o
[ 60%] Building CXX object modules/imgproc/CMakeFiles/opencv_test_imgproc.dir/test/test_bilateral_filter.cpp.o
[ 60%] Building CXX object modules/imgproc/CMakeFiles/opencv_test_imgproc.dir/test/test_moments.cpp.o
[ 60%] Building CXX object modules/photo/CMakeFiles/opencv_test_photo.dir/test/test_decolor.cpp.o
[ 60%] Building CXX object modules/photo/CMakeFiles/opencv_test_photo.dir/test/test_denoising.cpp.o
Scanning dependencies of target opencv_test_video
[ 60%] Building CXX object modules/video/CMakeFiles/opencv_test_video.dir/test/test_accum.cpp.o
[ 60%] Building CXX object modules/imgproc/CMakeFiles/opencv_test_imgproc.dir/test/test_intersection.cpp.o
[ 60%] Building CXX object modules/video/CMakeFiles/opencv_perf_video.dir/perf/perf_ecc.cpp.o
[ 60%] Building CXX object modules/video/CMakeFiles/opencv_test_video.dir/test/test_optflowpyrlk.cpp.o
[ 60%] Building CXX object modules/photo/CMakeFiles/opencv_test_photo.dir/test/test_main.cpp.o
[ 60%] Building CXX object modules/imgproc/CMakeFiles/opencv_test_imgproc.dir/test/test_connectedcomponents.cpp.o
[ 60%] Building CXX object modules/photo/CMakeFiles/opencv_test_photo.dir/test/test_denoise_tvl1.cpp.o
[ 60%] Building CXX object modules/video/CMakeFiles/opencv_test_video.dir/test/test_tvl1optflow.cpp.o
[ 61%] Building CXX object modules/imgproc/CMakeFiles/opencv_test_imgproc.dir/test/test_filter.cpp.o
[ 61%] Building CXX object modules/photo/CMakeFiles/opencv_test_photo.dir/test/test_inpaint.cpp.o
[ 61%] Building CXX object modules/video/CMakeFiles/opencv_perf_video.dir/perf/opencl/perf_motempl.cpp.o
[ 61%] Building CXX object modules/video/CMakeFiles/opencv_test_video.dir/test/test_estimaterigid.cpp.o
[ 61%] Building CXX object modules/video/CMakeFiles/opencv_perf_video.dir/perf/opencl/perf_optflow_pyrlk.cpp.o
[ 61%] Building CXX object modules/photo/CMakeFiles/opencv_test_photo.dir/test/test_hdr.cpp.o
[ 62%] Building CXX object modules/video/CMakeFiles/opencv_test_video.dir/test/ocl/test_optflowpyrlk.cpp.o
[ 62%] Building CXX object modules/video/CMakeFiles/opencv_perf_video.dir/perf/opencl/perf_bgfg_mog2.cpp.o
[ 62%] Building CXX object modules/imgproc/CMakeFiles/opencv_test_imgproc.dir/test/test_templmatch.cpp.o
Linking CXX executable ../../bin/opencv_test_photo
[ 62%] Built target opencv_test_photo
[ 62%] Building CXX object modules/imgproc/CMakeFiles/opencv_test_imgproc.dir/test/test_color.cpp.o
[ 62%] Building CXX object modules/video/CMakeFiles/opencv_test_video.dir/test/ocl/test_bgfg_mog2.cpp.o
[ 62%] Building CXX object modules/video/CMakeFiles/opencv_test_video.dir/test/ocl/test_optflow_farneback.cpp.o
[ 62%] Building CXX object modules/video/CMakeFiles/opencv_perf_video.dir/perf/opencl/perf_optflow_farneback.cpp.o
[ 62%] Building CXX object modules/video/CMakeFiles/opencv_perf_video.dir/perf/opencl/perf_optflow_dualTVL1.cpp.o
[ 62%] Building CXX object modules/imgproc/CMakeFiles/opencv_test_imgproc.dir/test/test_distancetransform.cpp.o
[ 62%] Building CXX object modules/video/CMakeFiles/opencv_test_video.dir/test/ocl/test_optflow_tvl1flow.cpp.o
[ 62%] [ 62%] Building CXX object modules/imgproc/CMakeFiles/opencv_test_imgproc.dir/test/test_lsd.cpp.o
Building CXX object modules/imgproc/CMakeFiles/opencv_test_imgproc.dir/test/test_canny.cpp.o
[ 62%] Building CXX object modules/video/CMakeFiles/opencv_perf_video.dir/perf/perf_main.cpp.o
Scanning dependencies of target opencv_test_viz
[ 62%] Building CXX object modules/viz/CMakeFiles/opencv_test_viz.dir/test/test_viz3d.cpp.o
[ 62%] Building CXX object modules/video/CMakeFiles/opencv_test_video.dir/test/test_kalman.cpp.o
Linking CXX executable ../../bin/opencv_perf_video
[ 62%] Building CXX object modules/imgproc/CMakeFiles/opencv_test_imgproc.dir/test/test_imgwarp.cpp.o
[ 62%] Building CXX object modules/viz/CMakeFiles/opencv_test_viz.dir/test/test_tutorial3.cpp.o
[ 62%] Built target opencv_perf_video
[ 62%] Building CXX object modules/imgproc/CMakeFiles/opencv_test_imgproc.dir/test/test_cvtyuv.cpp.o
[ 62%] Building CXX object modules/video/CMakeFiles/opencv_test_video.dir/test/test_main.cpp.o
[ 62%] Building CXX object modules/viz/CMakeFiles/opencv_test_viz.dir/test/test_tutorial2.cpp.o
[ 62%] Building CXX object modules/video/CMakeFiles/opencv_test_video.dir/test/test_ecc.cpp.o
[ 62%] Building CXX object modules/viz/CMakeFiles/opencv_test_viz.dir/test/test_main.cpp.o
[ 62%] Building CXX object modules/video/CMakeFiles/opencv_test_video.dir/test/test_camshift.cpp.o
[ 62%] Building CXX object modules/viz/CMakeFiles/opencv_test_viz.dir/test/tests_simple.cpp.o
[ 62%] Building CXX object modules/imgproc/CMakeFiles/opencv_test_imgproc.dir/test/test_pc.cpp.o
[ 62%] Building CXX object modules/viz/CMakeFiles/opencv_test_viz.dir/test/test_precomp.cpp.o
Linking CXX executable ../../bin/opencv_test_video
[ 62%] Building CXX object modules/imgproc/CMakeFiles/opencv_test_imgproc.dir/test/test_imgwarp_strict.cpp.o
[ 62%] Built target opencv_test_video
Scanning dependencies of target opencv_perf_imgcodecs
[ 62%] Building CXX object modules/imgcodecs/CMakeFiles/opencv_perf_imgcodecs.dir/perf/perf_main.cpp.o
Scanning dependencies of target opencv_test_imgcodecs
[ 62%] Building CXX object modules/imgcodecs/CMakeFiles/opencv_test_imgcodecs.dir/test/test_grfmt.cpp.o
Linking CXX executable ../../bin/opencv_perf_imgcodecs
[ 62%] Built target opencv_perf_imgcodecs
[ 62%] Building CXX object modules/imgproc/CMakeFiles/opencv_test_imgproc.dir/test/test_houghLines.cpp.o
Linking CXX executable ../../bin/opencv_test_viz
[ 62%] Built target opencv_test_viz
[ 62%] Building CXX object modules/imgproc/CMakeFiles/opencv_test_imgproc.dir/test/test_watershed.cpp.o
[ 62%] Building CXX object modules/imgcodecs/CMakeFiles/opencv_test_imgcodecs.dir/test/test_drawing.cpp.o
Scanning dependencies of target opencv_test_shape
[ 62%] Building CXX object modules/shape/CMakeFiles/opencv_test_shape.dir/test/test_shape.cpp.o
[ 62%] Building CXX object modules/imgproc/CMakeFiles/opencv_test_imgproc.dir/test/test_approxpoly.cpp.o
Scanning dependencies of target opencv_perf_videoio
[ 62%] Building CXX object modules/videoio/CMakeFiles/opencv_perf_videoio.dir/perf/perf_output.cpp.o
[ 62%] Building CXX object modules/imgcodecs/CMakeFiles/opencv_test_imgcodecs.dir/test/test_main.cpp.o
[ 64%] Building CXX object modules/imgproc/CMakeFiles/opencv_test_imgproc.dir/test/test_floodfill.cpp.o
Linking CXX executable ../../bin/opencv_test_imgcodecs
[ 64%] Built target opencv_test_imgcodecs
[ 64%] Building CXX object modules/videoio/CMakeFiles/opencv_perf_videoio.dir/perf/perf_input.cpp.o
[ 64%] Building CXX object modules/shape/CMakeFiles/opencv_test_shape.dir/test/test_main.cpp.o
Scanning dependencies of target opencv_test_videoio
Linking CXX executable ../../bin/opencv_test_imgproc
Linking CXX executable ../../bin/opencv_test_shape
[ 64%] Building CXX object modules/videoio/CMakeFiles/opencv_test_videoio.dir/test/test_fourcc.cpp.o
[ 64%] Built target opencv_test_shape
[ 64%] Building CXX object modules/videoio/CMakeFiles/opencv_perf_videoio.dir/perf/perf_main.cpp.o
[ 64%] Built target opencv_test_imgproc
[ 64%] Building CXX object modules/videoio/CMakeFiles/opencv_test_videoio.dir/test/test_ffmpeg.cpp.o
[ 64%] Building CXX object modules/videoio/CMakeFiles/opencv_test_videoio.dir/test/test_positioning.cpp.o
Linking CXX executable ../../bin/opencv_perf_videoio
[ 64%] Built target opencv_perf_videoio
[ 65%] Building CXX object modules/videoio/CMakeFiles/opencv_test_videoio.dir/test/test_main.cpp.o
Scanning dependencies of target opencv_test_highgui
[ 65%] Building CXX object modules/highgui/CMakeFiles/opencv_test_highgui.dir/test/test_gui.cpp.o
Scanning dependencies of target opencv_perf_objdetect
[ 65%] Building CXX object modules/objdetect/CMakeFiles/opencv_perf_objdetect.dir/perf/opencl/perf_cascades.cpp.o
[ 65%] Building CXX object modules/highgui/CMakeFiles/opencv_test_highgui.dir/test/test_main.cpp.o
[ 65%] Building CXX object modules/objdetect/CMakeFiles/opencv_perf_objdetect.dir/perf/opencl/perf_hogdetect.cpp.o
[ 65%] Building CXX object modules/videoio/CMakeFiles/opencv_test_videoio.dir/test/test_video_pos.cpp.o
Linking CXX executable ../../bin/opencv_test_highgui
[ 65%] Built target opencv_test_highgui
[ 65%] Building CXX object modules/videoio/CMakeFiles/opencv_test_videoio.dir/test/test_basic_props.cpp.o
Scanning dependencies of target opencv_test_objdetect
[ 65%] Building CXX object modules/objdetect/CMakeFiles/opencv_test_objdetect.dir/test/test_main.cpp.o
[ 65%] Building CXX object modules/objdetect/CMakeFiles/opencv_test_objdetect.dir/test/test_cascadeandhog.cpp.o
[ 65%] Building CXX object modules/objdetect/CMakeFiles/opencv_perf_objdetect.dir/perf/perf_main.cpp.o
[ 65%] Building CXX object modules/objdetect/CMakeFiles/opencv_test_objdetect.dir/test/opencl/test_hogdetector.cpp.o
[ 65%] Building CXX object modules/videoio/CMakeFiles/opencv_test_videoio.dir/test/test_video_io.cpp.o
Linking CXX executable ../../bin/opencv_perf_objdetect
[ 65%] Built target opencv_perf_objdetect
[ 65%] Building CXX object modules/videoio/CMakeFiles/opencv_test_videoio.dir/test/test_framecount.cpp.o
Scanning dependencies of target opencv_perf_superres
[ 66%] Building CXX object modules/superres/CMakeFiles/opencv_perf_superres.dir/perf/perf_superres.cpp.o
Linking CXX executable ../../bin/opencv_test_videoio
[ 66%] Building CXX object modules/superres/CMakeFiles/opencv_perf_superres.dir/perf/perf_main.cpp.o
Linking CXX executable ../../bin/opencv_test_objdetect
[ 66%] Built target opencv_test_videoio
Scanning dependencies of target opencv_test_superres
[ 66%] Building CXX object modules/superres/CMakeFiles/opencv_test_superres.dir/test/test_superres.cpp.o
[ 66%] Built target opencv_test_objdetect
Scanning dependencies of target opencv_perf_features2d
[ 66%] Building CXX object modules/features2d/CMakeFiles/opencv_perf_features2d.dir/perf/perf_fast.cpp.o
Scanning dependencies of target opencv_test_features2d
[ 66%] Building CXX object modules/features2d/CMakeFiles/opencv_test_features2d.dir/test/test_detectors_regression.cpp.o
Linking CXX executable ../../bin/opencv_perf_superres
[ 66%] Building CXX object modules/superres/CMakeFiles/opencv_test_superres.dir/test/test_main.cpp.o
[ 66%] Built target opencv_perf_superres
[ 66%] Building CXX object modules/features2d/CMakeFiles/opencv_perf_features2d.dir/perf/perf_orb.cpp.o
[ 66%] Building CXX object modules/features2d/CMakeFiles/opencv_test_features2d.dir/test/test_mser.cpp.o
[ 66%] Generating opencl_kernels_calib3d.cpp, opencl_kernels_calib3d.hpp
Scanning dependencies of target opencv_calib3d
Linking CXX executable ../../bin/opencv_test_superres
[ 66%] Building CXX object modules/calib3d/CMakeFiles/opencv_calib3d.dir/src/posit.cpp.o
[ 66%] Built target opencv_test_superres
[ 66%] Building CXX object modules/calib3d/CMakeFiles/opencv_calib3d.dir/src/calibration.cpp.o
[ 66%] Building CXX object modules/features2d/CMakeFiles/opencv_test_features2d.dir/test/test_brisk.cpp.o
[ 66%] Building CXX object modules/calib3d/CMakeFiles/opencv_calib3d.dir/src/fundam.cpp.o
[ 66%] Building CXX object modules/features2d/CMakeFiles/opencv_test_features2d.dir/test/test_agast.cpp.o
[ 67%] Building CXX object modules/features2d/CMakeFiles/opencv_perf_features2d.dir/perf/perf_agast.cpp.o
[ 67%] Building CXX object modules/features2d/CMakeFiles/opencv_test_features2d.dir/test/test_descriptors_regression.cpp.o
[ 67%] Building CXX object modules/features2d/CMakeFiles/opencv_perf_features2d.dir/perf/perf_batchDistance.cpp.o
[ 67%] Building CXX object modules/calib3d/CMakeFiles/opencv_calib3d.dir/src/ptsetreg.cpp.o
[ 67%] Building CXX object modules/calib3d/CMakeFiles/opencv_calib3d.dir/src/checkchessboard.cpp.o
[ 69%] Building CXX object modules/features2d/CMakeFiles/opencv_test_features2d.dir/test/ocl/test_brute_force_matcher.cpp.o
[ 69%] Building CXX object modules/features2d/CMakeFiles/opencv_test_features2d.dir/test/test_matchers_algorithmic.cpp.o
[ 69%] Building CXX object modules/calib3d/CMakeFiles/opencv_calib3d.dir/src/fisheye.cpp.o
[ 69%] Building CXX object modules/features2d/CMakeFiles/opencv_perf_features2d.dir/perf/opencl/perf_brute_force_matcher.cpp.o
[ 69%] Building CXX object modules/features2d/CMakeFiles/opencv_perf_features2d.dir/perf/opencl/perf_fast.cpp.o
[ 69%] Building CXX object modules/features2d/CMakeFiles/opencv_test_features2d.dir/test/test_main.cpp.o
[ 69%] Building CXX object modules/features2d/CMakeFiles/opencv_test_features2d.dir/test/test_nearestneighbors.cpp.o
[ 69%] Building CXX object modules/features2d/CMakeFiles/opencv_test_features2d.dir/test/test_keypoints.cpp.o
[ 69%] Building CXX object modules/features2d/CMakeFiles/opencv_perf_features2d.dir/perf/opencl/perf_orb.cpp.o
[ 69%] Building CXX object modules/features2d/CMakeFiles/opencv_test_features2d.dir/test/test_rotation_and_scale_invariance.cpp.o
[ 69%] Building CXX object modules/features2d/CMakeFiles/opencv_perf_features2d.dir/perf/perf_main.cpp.o
[ 69%] Building CXX object modules/calib3d/CMakeFiles/opencv_calib3d.dir/src/compat_stereo.cpp.o
[ 69%] Building CXX object modules/calib3d/CMakeFiles/opencv_calib3d.dir/src/calibinit.cpp.o
[ 69%] Building CXX object modules/features2d/CMakeFiles/opencv_test_features2d.dir/test/test_fast.cpp.o
[ 69%] Building CXX object modules/features2d/CMakeFiles/opencv_test_features2d.dir/test/test_orb.cpp.o
Linking CXX executable ../../bin/opencv_perf_features2d
[ 69%] Built target opencv_perf_features2d
[ 69%] Building CXX object modules/calib3d/CMakeFiles/opencv_calib3d.dir/src/stereobm.cpp.o
[ 69%] Building CXX object modules/calib3d/CMakeFiles/opencv_calib3d.dir/src/circlesgrid.cpp.o
Linking CXX executable ../../bin/opencv_test_features2d
[ 69%] Built target opencv_test_features2d
[ 70%] Building CXX object modules/calib3d/CMakeFiles/opencv_calib3d.dir/src/stereosgbm.cpp.o
[ 70%] Building CXX object modules/calib3d/CMakeFiles/opencv_calib3d.dir/src/levmarq.cpp.o
[ 70%] Building CXX object modules/calib3d/CMakeFiles/opencv_calib3d.dir/src/main.cpp.o
[ 70%] Building CXX object modules/calib3d/CMakeFiles/opencv_calib3d.dir/src/dls.cpp.o
[ 70%] Building CXX object modules/calib3d/CMakeFiles/opencv_calib3d.dir/src/quadsubpix.cpp.o
[ 70%] Building CXX object modules/calib3d/CMakeFiles/opencv_calib3d.dir/src/rho.cpp.o
[ 70%] Building CXX object modules/calib3d/CMakeFiles/opencv_calib3d.dir/src/polynom_solver.cpp.o
[ 70%] Building CXX object modules/calib3d/CMakeFiles/opencv_calib3d.dir/src/solvepnp.cpp.o
[ 70%] Building CXX object modules/calib3d/CMakeFiles/opencv_calib3d.dir/src/upnp.cpp.o
[ 70%] Building CXX object modules/calib3d/CMakeFiles/opencv_calib3d.dir/src/p3p.cpp.o
[ 70%] Building CXX object modules/calib3d/CMakeFiles/opencv_calib3d.dir/src/five-point.cpp.o
[ 70%] Building CXX object modules/calib3d/CMakeFiles/opencv_calib3d.dir/src/compat_ptsetreg.cpp.o
[ 70%] Building CXX object modules/calib3d/CMakeFiles/opencv_calib3d.dir/src/triangulate.cpp.o
[ 71%] Building CXX object modules/calib3d/CMakeFiles/opencv_calib3d.dir/src/homography_decomp.cpp.o
[ 71%] Building CXX object modules/calib3d/CMakeFiles/opencv_calib3d.dir/src/epnp.cpp.o
[ 71%] Building CXX object modules/calib3d/CMakeFiles/opencv_calib3d.dir/opencl_kernels_calib3d.cpp.o
Linking CXX shared library ../../lib/libopencv_calib3d.so
[ 71%] Built target opencv_calib3d
Scanning dependencies of target opencv_perf_calib3d
Scanning dependencies of target opencv_test_calib3d
[ 71%] Generating opencl_kernels_stitching.cpp, opencl_kernels_stitching.hpp
Scanning dependencies of target opencv_stitching
[ 71%] Generating core+Core.java, core+Algorithm.java, core.cpp
[ 72%] Building CXX object modules/calib3d/CMakeFiles/opencv_perf_calib3d.dir/perf/perf_pnp.cpp.o
[ 72%] [ 72%] Building CXX object modules/stitching/CMakeFiles/opencv_stitching.dir/src/exposure_compensate.cpp.o
Building CXX object modules/calib3d/CMakeFiles/opencv_test_calib3d.dir/test/test_chesscorners_badarg.cpp.o
[ 74%] Generating imgproc+LineSegmentDetector.java, imgproc+CLAHE.java, imgproc+Imgproc.java, imgproc+Subdiv2D.java, imgproc.cpp
[ 74%] Generating ml+NormalBayesClassifier.java, ml+Ml.java, ml+LogisticRegression.java, ml+Boost.java, ml+SVM.java, ml+StatModel.java, ml+RTrees.java, ml+KNearest.java, ml+TrainData.java, ml+DTrees.java, ml+EM.java, ml+ANN_MLP.java, ml.cpp
SKIP:static Ptr_TrainData create(Mat samples, int layout, Mat responses, Mat varIdx = Mat(), Mat sampleIdx = Mat(), Mat sampleWeights = Mat(), Mat varType = Mat())	 due to RET typePtr_TrainData
SKIP:bool train(Ptr_TrainData trainData, int flags = 0)	 due to ARG typePtr_TrainData/I
SKIP:float calcError(Ptr_TrainData data, bool test, Mat& resp)	 due to ARG typePtr_TrainData/I
[ 74%] Generating photo+AlignMTB.java, photo+MergeRobertson.java, photo+CalibrateRobertson.java, photo+MergeMertens.java, photo+AlignExposures.java, photo+TonemapDrago.java, photo+TonemapReinhard.java, photo+TonemapMantiuk.java, photo+CalibrateDebevec.java, photo+MergeExposures.java, photo+Photo.java, photo+TonemapDurand.java, photo+MergeDebevec.java, photo+CalibrateCRF.java, photo+Tonemap.java, photo.cpp
[ 74%] Building CXX object modules/calib3d/CMakeFiles/opencv_test_calib3d.dir/test/test_posit.cpp.o
[ 74%] Generating video+KalmanFilter.java, video+DualTVL1OpticalFlow.java, video+DenseOpticalFlow.java, video+BackgroundSubtractorMOG2.java, video+Video.java, video+BackgroundSubtractorKNN.java, video+BackgroundSubtractor.java, video.cpp
[ 74%] Generating imgcodecs+Imgcodecs.java, imgcodecs.cpp
[ 74%] Generating videoio+VideoCapture.java, videoio+VideoWriter.java, videoio+Videoio.java, videoio.cpp
[ 74%] Generating objdetect+BaseCascadeClassifier.java, objdetect+Objdetect.java, objdetect+CascadeClassifier.java, objdetect+HOGDescriptor.java, objdetect.cpp
SKIP:bool read(FileNode node)	 due to ARG typeFileNode/I
[ 74%] Generating features2d+DescriptorExtractor.java, features2d+Features2d.java, features2d+FeatureDetector.java, features2d+DescriptorMatcher.java, features2d.cpp
[ 74%] Generating calib3d+StereoMatcher.java, calib3d+Calib3d.java, calib3d+StereoSGBM.java, calib3d+StereoBM.java, calib3d.cpp
[ 74%] Building CXX object modules/stitching/CMakeFiles/opencv_stitching.dir/src/camera.cpp.o
[ 74%] Building CXX object modules/calib3d/CMakeFiles/opencv_test_calib3d.dir/test/test_homography.cpp.o
[ 74%] Generating src/org/opencv/core/Core.java
[ 74%] Generating src/org/opencv/core/Algorithm.java
[ 74%] Generating src/org/opencv/imgproc/LineSegmentDetector.java
[ 74%] Generating src/org/opencv/imgproc/CLAHE.java
[ 75%] Generating src/org/opencv/imgproc/Imgproc.java
[ 75%] Generating src/org/opencv/imgproc/Subdiv2D.java
[ 75%] Generating src/org/opencv/ml/NormalBayesClassifier.java
[ 75%] Generating src/org/opencv/ml/Ml.java
[ 75%] Generating src/org/opencv/ml/LogisticRegression.java
[ 75%] Generating src/org/opencv/ml/Boost.java
[ 75%] Generating src/org/opencv/ml/SVM.java
[ 75%] Generating src/org/opencv/ml/StatModel.java
[ 75%] Generating src/org/opencv/ml/RTrees.java
[ 75%] Generating src/org/opencv/ml/KNearest.java
[ 75%] Generating src/org/opencv/ml/TrainData.java
[ 75%] Generating src/org/opencv/ml/DTrees.java
[ 75%] Generating src/org/opencv/ml/EM.java
[ 76%] Generating src/org/opencv/ml/ANN_MLP.java
[ 76%] Generating src/org/opencv/photo/AlignMTB.java
[ 76%] Generating src/org/opencv/photo/MergeRobertson.java
[ 76%] Generating src/org/opencv/photo/CalibrateRobertson.java
[ 76%] Generating src/org/opencv/photo/MergeMertens.java
[ 76%] Generating src/org/opencv/photo/AlignExposures.java
[ 76%] Generating src/org/opencv/photo/TonemapDrago.java
[ 76%] Generating src/org/opencv/photo/TonemapReinhard.java
[ 76%] Generating src/org/opencv/photo/TonemapMantiuk.java
[ 76%] Generating src/org/opencv/photo/CalibrateDebevec.java
[ 76%] Generating src/org/opencv/photo/MergeExposures.java
[ 76%] Generating src/org/opencv/photo/Photo.java
[ 76%] Generating src/org/opencv/photo/TonemapDurand.java
[ 76%] Generating src/org/opencv/photo/MergeDebevec.java
[ 77%] Generating src/org/opencv/photo/CalibrateCRF.java
[ 77%] Generating src/org/opencv/photo/Tonemap.java
[ 77%] Generating src/org/opencv/video/KalmanFilter.java
[ 77%] Generating src/org/opencv/video/DualTVL1OpticalFlow.java
[ 77%] Generating src/org/opencv/video/DenseOpticalFlow.java
[ 77%] Generating src/org/opencv/video/BackgroundSubtractorMOG2.java
[ 77%] Generating src/org/opencv/video/Video.java
[ 77%] Generating src/org/opencv/video/BackgroundSubtractorKNN.java
[ 77%] [ 77%] Generating src/org/opencv/video/BackgroundSubtractor.java
Building CXX object modules/calib3d/CMakeFiles/opencv_perf_calib3d.dir/perf/perf_stereosgbm.cpp.o
[ 77%] Generating src/org/opencv/imgcodecs/Imgcodecs.java
[ 77%] Generating src/org/opencv/videoio/VideoCapture.java
[ 77%] Generating src/org/opencv/videoio/VideoWriter.java
[ 77%] Generating src/org/opencv/videoio/Videoio.java
[ 79%] Generating src/org/opencv/objdetect/BaseCascadeClassifier.java
[ 79%] Generating src/org/opencv/objdetect/Objdetect.java
[ 79%] Generating src/org/opencv/objdetect/CascadeClassifier.java
[ 79%] Generating src/org/opencv/objdetect/HOGDescriptor.java
[ 79%] Generating src/org/opencv/features2d/DescriptorExtractor.java
[ 79%] Generating src/org/opencv/features2d/Features2d.java
[ 79%] Generating src/org/opencv/features2d/FeatureDetector.java
[ 79%] Generating src/org/opencv/features2d/DescriptorMatcher.java
[ 79%] Generating src/org/opencv/calib3d/StereoMatcher.java
[ 79%] Generating src/org/opencv/calib3d/Calib3d.java
[ 79%] Generating src/org/opencv/calib3d/StereoSGBM.java
[ 79%] Generating src/org/opencv/calib3d/StereoBM.java
[ 79%] Generating src/org/opencv/utils/Converters.java
[ 80%] Generating src/org/opencv/core/RotatedRect.java
[ 80%] Generating src/org/opencv/core/MatOfRect.java
[ 80%] Generating src/org/opencv/core/Range.java
[ 80%] Generating src/org/opencv/core/Point.java
[ 80%] Generating src/org/opencv/core/MatOfPoint3f.java
[ 80%] Generating src/org/opencv/core/MatOfByte.java
[ 80%] Generating src/org/opencv/core/DMatch.java
[ 80%] Generating src/org/opencv/core/MatOfKeyPoint.java
[ 80%] Generating src/org/opencv/core/MatOfPoint2f.java
[ 80%] Generating src/org/opencv/core/MatOfPoint3.java
[ 80%] Generating src/org/opencv/core/MatOfInt.java
[ 80%] Generating src/org/opencv/core/Scalar.java
[ 80%] Generating src/org/opencv/core/Rect.java
[ 81%] Generating src/org/opencv/core/Size.java
[ 81%] Generating src/org/opencv/core/MatOfInt4.java
[ 81%] Generating src/org/opencv/core/MatOfDMatch.java
[ 81%] Generating src/org/opencv/core/MatOfPoint.java
[ 81%] Generating src/org/opencv/core/Mat.java
[ 81%] Generating src/org/opencv/core/CvException.java
[ 81%] Generating src/org/opencv/core/CvType.java
[ 81%] Generating src/org/opencv/core/Point3.java
[ 81%] Generating src/org/opencv/core/TermCriteria.java
[ 81%] Generating src/org/opencv/core/KeyPoint.java
[ 81%] Generating src/org/opencv/core/MatOfFloat.java
[ 81%] Generating src/org/opencv/core/MatOfFloat4.java
[ 81%] Generating src/org/opencv/core/MatOfFloat6.java
[ 81%] Generating src/org/opencv/core/MatOfDouble.java
[ 82%] Generating src/org/opencv/imgproc/Moments.java
[ 82%] Generating opencv-310.jar
[ 82%] Building CXX object modules/stitching/CMakeFiles/opencv_stitching.dir/src/warpers.cpp.o
[ 82%] Building CXX object modules/calib3d/CMakeFiles/opencv_test_calib3d.dir/test/test_modelest.cpp.o
[ 82%] Building CXX object modules/calib3d/CMakeFiles/opencv_perf_calib3d.dir/perf/perf_cicrlesGrid.cpp.o
[ 82%] Building CXX object modules/calib3d/CMakeFiles/opencv_test_calib3d.dir/test/test_chesscorners_timing.cpp.o
[ 82%] Building CXX object modules/stitching/CMakeFiles/opencv_stitching.dir/src/seam_finders.cpp.o
BUILD SUCCESSFUL
Total time: 21 seconds
Scanning dependencies of target opencv_java
[ 82%] Building CXX object modules/calib3d/CMakeFiles/opencv_test_calib3d.dir/test/test_affine3.cpp.o
[ 82%] Building CXX object modules/java/CMakeFiles/opencv_java.dir/generator/src/cpp/utils.cpp.o
[ 82%] Building CXX object modules/calib3d/CMakeFiles/opencv_perf_calib3d.dir/perf/opencl/perf_stereobm.cpp.o
[ 82%] Building CXX object modules/java/CMakeFiles/opencv_java.dir/generator/src/cpp/converters.cpp.o
[ 82%] Building CXX object modules/calib3d/CMakeFiles/opencv_test_calib3d.dir/test/test_compose_rt.cpp.o
[ 82%] Building CXX object modules/calib3d/CMakeFiles/opencv_perf_calib3d.dir/perf/perf_main.cpp.o
[ 82%] Building CXX object modules/java/CMakeFiles/opencv_java.dir/generator/src/cpp/Mat.cpp.o
[ 82%] Building CXX object modules/calib3d/CMakeFiles/opencv_test_calib3d.dir/test/test_affine3d_estimator.cpp.o
[ 82%] Building CXX object modules/stitching/CMakeFiles/opencv_stitching.dir/src/blenders.cpp.o
Linking CXX executable ../../bin/opencv_perf_calib3d
[ 82%] Built target opencv_perf_calib3d
[ 82%] Building CXX object modules/stitching/CMakeFiles/opencv_stitching.dir/src/warpers_cuda.cpp.o
[ 82%] Building CXX object modules/calib3d/CMakeFiles/opencv_test_calib3d.dir/test/test_fisheye.cpp.o
[ 82%] Building CXX object modules/java/CMakeFiles/opencv_java.dir/generator/src/cpp/jni_part.cpp.o
[ 82%] Building CXX object modules/calib3d/CMakeFiles/opencv_test_calib3d.dir/test/test_decompose_projection.cpp.o
[ 82%] Building CXX object modules/java/CMakeFiles/opencv_java.dir/__/core/misc/java/src/cpp/core_manual.cpp.o
[ 82%] Building CXX object modules/stitching/CMakeFiles/opencv_stitching.dir/src/timelapsers.cpp.o
[ 82%] Building CXX object modules/java/CMakeFiles/opencv_java.dir/__/features2d/misc/java/src/cpp/features2d_converters.cpp.o
[ 83%] Building CXX object modules/stitching/CMakeFiles/opencv_stitching.dir/src/stitcher.cpp.o
Scanning dependencies of target opencv_videostab
[ 83%] Building CXX object modules/java/CMakeFiles/opencv_java.dir/core.cpp.o
[ 85%] Building CXX object modules/videostab/CMakeFiles/opencv_videostab.dir/src/outlier_rejection.cpp.o
[ 85%] Building CXX object modules/calib3d/CMakeFiles/opencv_test_calib3d.dir/test/test_cornerssubpix.cpp.o
[ 85%] Building CXX object modules/videostab/CMakeFiles/opencv_videostab.dir/src/frame_source.cpp.o
[ 85%] Building CXX object modules/java/CMakeFiles/opencv_java.dir/imgproc.cpp.o
[ 85%] Building CXX object modules/stitching/CMakeFiles/opencv_stitching.dir/src/util.cpp.o
[ 85%] Building CXX object modules/videostab/CMakeFiles/opencv_videostab.dir/src/inpainting.cpp.o
[ 85%] Building CXX object modules/calib3d/CMakeFiles/opencv_test_calib3d.dir/test/test_cameracalibration_badarg.cpp.o
[ 85%] Building CXX object modules/stitching/CMakeFiles/opencv_stitching.dir/src/autocalib.cpp.o
[ 85%] Building CXX object modules/java/CMakeFiles/opencv_java.dir/ml.cpp.o
[ 85%] Building CXX object modules/stitching/CMakeFiles/opencv_stitching.dir/src/motion_estimators.cpp.o
[ 85%] Building CXX object modules/videostab/CMakeFiles/opencv_videostab.dir/src/wobble_suppression.cpp.o
[ 85%] Building CXX object modules/calib3d/CMakeFiles/opencv_test_calib3d.dir/test/test_undistort_badarg.cpp.o
[ 85%] Building CXX object modules/videostab/CMakeFiles/opencv_videostab.dir/src/optical_flow.cpp.o
[ 85%] Building CXX object modules/java/CMakeFiles/opencv_java.dir/photo.cpp.o
[ 86%] Building CXX object modules/calib3d/CMakeFiles/opencv_test_calib3d.dir/test/test_solvepnp_ransac.cpp.o
[ 86%] Building CXX object modules/videostab/CMakeFiles/opencv_videostab.dir/src/global_motion.cpp.o
[ 86%] Building CXX object modules/stitching/CMakeFiles/opencv_stitching.dir/src/matchers.cpp.o
[ 86%] Building CXX object modules/java/CMakeFiles/opencv_java.dir/video.cpp.o
[ 86%] Building CXX object modules/calib3d/CMakeFiles/opencv_test_calib3d.dir/test/test_main.cpp.o
[ 87%] Building CXX object modules/java/CMakeFiles/opencv_java.dir/imgcodecs.cpp.o
[ 87%] Building CXX object modules/videostab/CMakeFiles/opencv_videostab.dir/src/deblurring.cpp.o
[ 87%] Building CXX object modules/stitching/CMakeFiles/opencv_stitching.dir/opencl_kernels_stitching.cpp.o
[ 87%] Building CXX object modules/calib3d/CMakeFiles/opencv_test_calib3d.dir/test/test_fundam.cpp.o
[ 87%] Building CXX object modules/java/CMakeFiles/opencv_java.dir/videoio.cpp.o
Linking CXX shared library ../../lib/libopencv_stitching.so
[ 87%] Built target opencv_stitching
[ 87%] Building CXX object modules/java/CMakeFiles/opencv_java.dir/objdetect.cpp.o
[ 87%] Building CXX object modules/videostab/CMakeFiles/opencv_videostab.dir/src/fast_marching.cpp.o
[ 87%] Building CXX object modules/calib3d/CMakeFiles/opencv_test_calib3d.dir/test/test_chesscorners.cpp.o
[ 87%] Building CXX object modules/java/CMakeFiles/opencv_java.dir/features2d.cpp.o
[ 87%] Building CXX object modules/videostab/CMakeFiles/opencv_videostab.dir/src/stabilizer.cpp.o
[ 87%] Building CXX object modules/java/CMakeFiles/opencv_java.dir/calib3d.cpp.o
[ 87%] Building CXX object modules/calib3d/CMakeFiles/opencv_test_calib3d.dir/test/test_undistort.cpp.o
[ 87%] Building CXX object modules/calib3d/CMakeFiles/opencv_test_calib3d.dir/test/test_undistort_points.cpp.o
[ 87%] Building CXX object modules/calib3d/CMakeFiles/opencv_test_calib3d.dir/test/test_cameracalibration.cpp.o
[ 87%] Building CXX object modules/videostab/CMakeFiles/opencv_videostab.dir/src/log.cpp.o
Linking CXX shared module ../../lib/libopencv_java310.so
[ 87%] Building CXX object modules/videostab/CMakeFiles/opencv_videostab.dir/src/motion_stabilizing.cpp.o
[ 87%] Built target opencv_java
Scanning dependencies of target opencv_traincascade
[ 87%] Building CXX object apps/traincascade/CMakeFiles/opencv_traincascade.dir/old_ml_boost.cpp.o
[ 87%] Building CXX object modules/calib3d/CMakeFiles/opencv_test_calib3d.dir/test/test_cameracalibration_artificial.cpp.o
Linking CXX shared library ../../lib/libopencv_videostab.so
[ 87%] Built target opencv_videostab
Scanning dependencies of target opencv_createsamples
[ 87%] Building CXX object apps/createsamples/CMakeFiles/opencv_createsamples.dir/utility.cpp.o
[ 87%] Building CXX object apps/traincascade/CMakeFiles/opencv_traincascade.dir/imagestorage.cpp.o
[ 87%] Building CXX object apps/createsamples/CMakeFiles/opencv_createsamples.dir/createsamples.cpp.o
[ 87%] [ 87%] Building CXX object apps/traincascade/CMakeFiles/opencv_traincascade.dir/features.cpp.o
Building CXX object modules/calib3d/CMakeFiles/opencv_test_calib3d.dir/test/test_homography_decomp.cpp.o
[ 87%] Building CXX object apps/traincascade/CMakeFiles/opencv_traincascade.dir/haarfeatures.cpp.o
Linking CXX executable ../../bin/opencv_createsamples
[ 87%] Built target opencv_createsamples
[ 87%] Building CXX object modules/calib3d/CMakeFiles/opencv_test_calib3d.dir/test/test_cameracalibration_tilt.cpp.o
[ 87%] Building CXX object apps/traincascade/CMakeFiles/opencv_traincascade.dir/traincascade.cpp.o
[ 87%] [ 87%] Building CXX object modules/calib3d/CMakeFiles/opencv_test_calib3d.dir/test/test_stereomatching.cpp.o
Building CXX object apps/traincascade/CMakeFiles/opencv_traincascade.dir/lbpfeatures.cpp.o
[ 87%] Building CXX object apps/traincascade/CMakeFiles/opencv_traincascade.dir/HOGfeatures.cpp.o
[ 87%] Building CXX object modules/calib3d/CMakeFiles/opencv_test_calib3d.dir/test/test_chessboardgenerator.cpp.o
Scanning dependencies of target opencv_test_java_properties
[ 87%] Built target opencv_test_java_properties
[ 87%] Building CXX object apps/traincascade/CMakeFiles/opencv_traincascade.dir/cascadeclassifier.cpp.o
Scanning dependencies of target opencv_perf_stitching
[ 87%] Building CXX object modules/stitching/CMakeFiles/opencv_perf_stitching.dir/perf/perf_stich.cpp.o
Scanning dependencies of target opencv_test_stitching
[ 87%] Building CXX object modules/stitching/CMakeFiles/opencv_test_stitching.dir/test/test_blenders.cpp.o
[ 87%] Building CXX object modules/calib3d/CMakeFiles/opencv_test_calib3d.dir/test/test_reproject_image_to_3d.cpp.o
[ 87%] Building CXX object apps/traincascade/CMakeFiles/opencv_traincascade.dir/boost.cpp.o
[ 88%] Building CXX object modules/stitching/CMakeFiles/opencv_test_stitching.dir/test/test_matchers.cpp.o
[ 88%] Building CXX object modules/stitching/CMakeFiles/opencv_test_stitching.dir/test/ocl/test_warpers.cpp.o
[ 90%] Building CXX object modules/calib3d/CMakeFiles/opencv_test_calib3d.dir/test/opencl/test_stereobm.cpp.o
[ 90%] Building CXX object apps/traincascade/CMakeFiles/opencv_traincascade.dir/old_ml_data.cpp.o
[ 90%] Building CXX object modules/stitching/CMakeFiles/opencv_perf_stitching.dir/perf/opencl/perf_stitch.cpp.o
[ 90%] Building CXX object apps/traincascade/CMakeFiles/opencv_traincascade.dir/old_ml_inner_functions.cpp.o
[ 90%] Building CXX object modules/stitching/CMakeFiles/opencv_test_stitching.dir/test/test_main.cpp.o
Linking CXX executable ../../bin/opencv_test_calib3d
[ 90%] Building CXX object apps/traincascade/CMakeFiles/opencv_traincascade.dir/old_ml_tree.cpp.o
[ 90%] Built target opencv_test_calib3d
[ 90%] Generating pyopencv_generated_include.h, pyopencv_generated_funcs.h, pyopencv_generated_types.h, pyopencv_generated_type_reg.h, pyopencv_generated_ns_reg.h
Linking CXX executable ../../bin/opencv_test_stitching
[ 90%] Built target opencv_test_stitching
[ 90%] Generating pyopencv_generated_include.h, pyopencv_generated_funcs.h, pyopencv_generated_types.h, pyopencv_generated_type_reg.h, pyopencv_generated_ns_reg.h
Scanning dependencies of target opencv_python2
[ 91%] Building CXX object modules/python2/CMakeFiles/opencv_python2.dir/__/src2/cv2.cpp.o
[ 91%] Building CXX object modules/stitching/CMakeFiles/opencv_perf_stitching.dir/perf/opencl/perf_warpers.cpp.o
Scanning dependencies of target opencv_python3
[ 91%] Building CXX object modules/python3/CMakeFiles/opencv_python3.dir/__/src2/cv2.cpp.o
Linking CXX executable ../../bin/opencv_traincascade
[ 91%] Built target opencv_traincascade
Scanning dependencies of target opencv_test_java
[ 91%] Copying the OpenCV jar
[ 91%] Copying res/values/strings.xml
[ 91%] Copying res/raw/lbpcascade_frontalface.xml
[ 91%] Copying res/drawable/icon.png
[ 91%] Copying res/drawable/chessboard.jpg
[ 91%] Copying res/drawable/lena.png
[ 91%] Copying res/layout/main.xml
[ 91%] Copying src/org/opencv/test/utils/ConvertersTest.java
[ 91%] Copying CoreTest.java
[ 91%] Copying SizeTest.java
[ 92%] Copying RangeTest.java
[ 92%] Copying PointTest.java
[ 92%] Copying CvTypeTest.java
[ 92%] Copying ScalarTest.java
[ 92%] Copying RectTest.java
[ 92%] Copying Point3Test.java
[ 92%] Copying RotatedRectTest.java
[ 92%] Copying DMatchTest.java
[ 92%] Copying MatTest.java
[ 92%] Copying TermCriteriaTest.java
[ 92%] Copying KeyPointTest.java
[ 92%] Copying MomentsTest.java
[ 92%] Copying ImgprocTest.java
[ 93%] Copying Subdiv2DTest.java
[ 93%] Copying PhotoTest.java
[ 93%] Copying VideoTest.java
[ 93%] Copying KalmanFilterTest.java
[ 93%] Copying BackgroundSubtractorMOGTest.java
[ 93%] Copying HighguiTest.java
[ 93%] Copying VideoCaptureTest.java
[ 93%] Copying CascadeClassifierTest.java
[ 93%] Copying HOGDescriptorTest.java
[ 93%] Copying ObjdetectTest.java
[ 93%] Copying PyramidSIFTFeatureDetectorTest.java
[ 93%] Copying PyramidDENSEFeatureDetectorTest.java
[ 93%] Copying SURFDescriptorExtractorTest.java
[ 93%] Copying FernGenericDescriptorMatcherTest.java
[ 95%] Copying GridSURFFeatureDetectorTest.java
[ 95%] Copying DynamicDENSEFeatureDetectorTest.java
[ 95%] Copying PyramidGFTTFeatureDetectorTest.java
[ 95%] Copying GridHARRISFeatureDetectorTest.java
[ 95%] Copying GridORBFeatureDetectorTest.java
[ 95%] Copying BruteForceSL2DescriptorMatcherTest.java
[ 95%] Copying DynamicORBFeatureDetectorTest.java
[ 95%] Copying DynamicMSERFeatureDetectorTest.java
[ 95%] Copying PyramidMSERFeatureDetectorTest.java
[ 95%] Copying STARFeatureDetectorTest.java
[ 95%] Copying OpponentSURFDescriptorExtractorTest.java
[ 95%] Copying DynamicHARRISFeatureDetectorTest.java
[ 95%] Copying PyramidSIMPLEBLOBFeatureDetectorTest.java
[ 96%] Copying PyramidHARRISFeatureDetectorTest.java
[ 96%] Copying OneWayGenericDescriptorMatcherTest.java
[ 96%] Copying GridDENSEFeatureDetectorTest.java
[ 96%] Copying OpponentBRIEFDescriptorExtractorTest.java
[ 96%] Copying ORBFeatureDetectorTest.java
[ 96%] Copying DynamicSIFTFeatureDetectorTest.java
[ 96%] Copying ORBDescriptorExtractorTest.java
[ 96%] Copying BruteForceDescriptorMatcherTest.java
[ 96%] Copying SIFTDescriptorExtractorTest.java
[ 96%] Copying BruteForceHammingDescriptorMatcherTest.java
[ 96%] Copying PyramidORBFeatureDetectorTest.java
[ 96%] Copying SIMPLEBLOBFeatureDetectorTest.java
[ 96%] Copying Features2dTest.java
[ 97%] Copying OpponentORBDescriptorExtractorTest.java
[ 97%] Copying GFTTFeatureDetectorTest.java
[ 97%] Copying DynamicGFTTFeatureDetectorTest.java
[ 97%] Copying DynamicFASTFeatureDetectorTest.java
[ 97%] Copying SURFFeatureDetectorTest.java
[ 97%] Copying GridGFTTFeatureDetectorTest.java
[ 97%] Copying GridFASTFeatureDetectorTest.java
[ 97%] Copying MSERFeatureDetectorTest.java
[ 97%] Copying PyramidSTARFeatureDetectorTest.java
[ 97%] Copying BruteForceL1DescriptorMatcherTest.java
[ 97%] Copying PyramidSURFFeatureDetectorTest.java
[ 97%] Copying GridSIFTFeatureDetectorTest.java
[ 97%] Copying GridMSERFeatureDetectorTest.java
[ 97%] Copying DynamicSIMPLEBLOBFeatureDetectorTest.java
[ 98%] Copying DynamicSURFFeatureDetectorTest.java
[ 98%] Copying DynamicSTARFeatureDetectorTest.java
[ 98%] Copying GridSTARFeatureDetectorTest.java
[ 98%] Copying GridSIMPLEBLOBFeatureDetectorTest.java
[ 98%] Copying FASTFeatureDetectorTest.java
[ 98%] Copying BruteForceHammingLUTDescriptorMatcherTest.java
[ 98%] Copying OpponentSIFTDescriptorExtractorTest.java
[ 98%] Copying FlannBasedDescriptorMatcherTest.java
[ 98%] Copying BRIEFDescriptorExtractorTest.java
[ 98%] Copying DENSEFeatureDetectorTest.java
[ 98%] Copying HARRISFeatureDetectorTest.java
[ 98%] Copying SIFTFeatureDetectorTest.java
[ 98%] Copying PyramidFASTFeatureDetectorTest.java
[100%] Copying StereoSGBMTest.java
[100%] Copying Calib3dTest.java
[100%] Copying StereoBMTest.java
[100%] Copying src/org/opencv/test/OpenCVTestCase.java
[100%] [100%] Building CXX object modules/stitching/CMakeFiles/opencv_perf_stitching.dir/perf/perf_main.cpp.o
Copying src/org/opencv/test/OpenCVTestRunner.java
[100%] Copying lib/junit-4.11.jar
[100%] Copying build.xml
[100%] Build Java tests
Buildfile: /home/cobalt/opencv/build/modules/java/pure_test/.build/build.xml
build:
compile:
    [mkdir] Created dir: /home/cobalt/opencv/build/modules/java/pure_test/.build/build/classes
    [javac] Compiling 88 source files to /home/cobalt/opencv/build/modules/java/pure_test/.build/build/classes
Linking CXX executable ../../bin/opencv_perf_stitching
[100%] Built target opencv_perf_stitching
jar:
    [mkdir] Created dir: /home/cobalt/opencv/build/modules/java/pure_test/.build/build/jar
      [jar] Building jar: /home/cobalt/opencv/build/modules/java/pure_test/.build/build/jar/opencv-test.jar
BUILD SUCCESSFUL
Total time: 21 seconds
[100%] Built target opencv_test_java
Linking CXX shared module ../../lib/cv2.so
[100%] Built target opencv_python2
Linking CXX shared module ../../lib/python3/cv2.cpython-34m.so
[100%] Built target opencv_python3
PhD:~/opencv/build$ sudo make install
sudo: unable to resolve host cobalt
[sudo] password for cobalt: 
[  4%] Built target libwebp
[  6%] Built target opencv_core_pch_dephelp
[  6%] Built target pch_Generate_opencv_core
[ 11%] Built target opencv_core
[ 11%] Built target opencv_ts_pch_dephelp
[ 11%] Built target pch_Generate_opencv_ts
[ 11%] Built target opencv_imgproc_pch_dephelp
[ 11%] Built target pch_Generate_opencv_imgproc
[ 16%] Built target opencv_imgproc
[ 16%] Built target opencv_imgcodecs_pch_dephelp
[ 17%] Built target pch_Generate_opencv_imgcodecs
[ 18%] Built target opencv_imgcodecs
[ 18%] Built target opencv_videoio_pch_dephelp
[ 18%] Built target pch_Generate_opencv_videoio
[ 19%] Built target opencv_videoio
[ 20%] Built target opencv_highgui_pch_dephelp
[ 20%] Built target pch_Generate_opencv_highgui
[ 20%] Built target opencv_highgui
[ 22%] Built target opencv_ts
[ 22%] Built target opencv_perf_core_pch_dephelp
[ 22%] Built target pch_Generate_opencv_perf_core
[ 24%] Built target opencv_perf_core
[ 24%] Built target opencv_test_core_pch_dephelp
[ 25%] Built target pch_Generate_opencv_test_core
[ 28%] Built target opencv_test_core
[ 28%] Built target opencv_flann_pch_dephelp
[ 28%] Built target pch_Generate_opencv_flann
[ 28%] Built target opencv_flann
[ 29%] Built target opencv_test_flann_pch_dephelp
[ 29%] Built target pch_Generate_opencv_test_flann
[ 29%] Built target opencv_test_flann
[ 29%] Built target opencv_perf_imgproc_pch_dephelp
[ 29%] Built target pch_Generate_opencv_perf_imgproc
[ 33%] Built target opencv_perf_imgproc
[ 33%] Built target opencv_test_imgproc_pch_dephelp
[ 33%] Built target pch_Generate_opencv_test_imgproc
[ 38%] Built target opencv_test_imgproc
[ 38%] Built target opencv_ml_pch_dephelp
[ 38%] Built target pch_Generate_opencv_ml
[ 39%] Built target opencv_ml
[ 39%] Built target opencv_test_ml_pch_dephelp
[ 40%] Built target pch_Generate_opencv_test_ml
[ 41%] Built target opencv_test_ml
[ 41%] Built target opencv_photo_pch_dephelp
[ 41%] Built target pch_Generate_opencv_photo
[ 43%] Built target opencv_photo
[ 43%] Built target opencv_perf_photo_pch_dephelp
[ 43%] Built target pch_Generate_opencv_perf_photo
[ 43%] Built target opencv_perf_photo
[ 43%] Built target opencv_test_photo_pch_dephelp
[ 43%] Built target pch_Generate_opencv_test_photo
[ 44%] Built target opencv_test_photo
[ 44%] Built target opencv_video_pch_dephelp
[ 44%] Built target pch_Generate_opencv_video
[ 45%] Built target opencv_video
[ 46%] Built target opencv_perf_video_pch_dephelp
[ 48%] Built target pch_Generate_opencv_perf_video
[ 48%] Built target opencv_perf_video
[ 48%] Built target opencv_test_video_pch_dephelp
[ 48%] Built target pch_Generate_opencv_test_video
[ 49%] Built target opencv_test_video
[ 49%] Built target opencv_viz_pch_dephelp
[ 50%] Built target pch_Generate_opencv_viz
[ 51%] Built target opencv_viz
[ 53%] Built target opencv_test_viz_pch_dephelp
[ 53%] Built target pch_Generate_opencv_test_viz
[ 53%] Built target opencv_test_viz
[ 53%] Built target opencv_perf_imgcodecs_pch_dephelp
[ 53%] Built target pch_Generate_opencv_perf_imgcodecs
[ 53%] Built target opencv_perf_imgcodecs
[ 53%] Built target opencv_test_imgcodecs_pch_dephelp
[ 53%] Built target pch_Generate_opencv_test_imgcodecs
[ 53%] Built target opencv_test_imgcodecs
[ 53%] Built target opencv_shape_pch_dephelp
[ 53%] Built target pch_Generate_opencv_shape
[ 53%] Built target opencv_shape
[ 53%] Built target opencv_test_shape_pch_dephelp
[ 53%] Built target pch_Generate_opencv_test_shape
[ 53%] Built target opencv_test_shape
[ 53%] Built target opencv_perf_videoio_pch_dephelp
[ 53%] Built target pch_Generate_opencv_perf_videoio
[ 53%] Built target opencv_perf_videoio
[ 53%] Built target opencv_test_videoio_pch_dephelp
[ 54%] Built target pch_Generate_opencv_test_videoio
[ 55%] Built target opencv_test_videoio
[ 55%] Built target opencv_test_highgui_pch_dephelp
[ 55%] Built target pch_Generate_opencv_test_highgui
[ 55%] Built target opencv_test_highgui
[ 55%] Built target opencv_objdetect_pch_dephelp
[ 55%] Built target pch_Generate_opencv_objdetect
[ 56%] Built target opencv_objdetect
[ 58%] Built target opencv_perf_objdetect_pch_dephelp
[ 58%] Built target pch_Generate_opencv_perf_objdetect
[ 58%] Built target opencv_perf_objdetect
[ 58%] Built target opencv_test_objdetect_pch_dephelp
[ 58%] Built target pch_Generate_opencv_test_objdetect
[ 58%] Built target opencv_test_objdetect
[ 59%] Built target opencv_superres_pch_dephelp
[ 59%] Built target pch_Generate_opencv_superres
[ 60%] Built target opencv_superres
[ 60%] Built target opencv_perf_superres_pch_dephelp
[ 60%] Built target pch_Generate_opencv_perf_superres
[ 61%] Built target opencv_perf_superres
[ 61%] Built target opencv_test_superres_pch_dephelp
[ 61%] Built target pch_Generate_opencv_test_superres
[ 61%] Built target opencv_test_superres
[ 62%] Built target opencv_features2d_pch_dephelp
[ 62%] Built target pch_Generate_opencv_features2d
[ 65%] Built target opencv_features2d
[ 65%] Built target opencv_perf_features2d_pch_dephelp
[ 66%] Built target pch_Generate_opencv_perf_features2d
[ 67%] Built target opencv_perf_features2d
[ 67%] Built target opencv_test_features2d_pch_dephelp
[ 67%] Built target pch_Generate_opencv_test_features2d
[ 69%] Built target opencv_test_features2d
[ 69%] Built target opencv_calib3d_pch_dephelp
[ 69%] Built target pch_Generate_opencv_calib3d
[ 71%] Built target opencv_calib3d
[ 71%] Built target opencv_perf_calib3d_pch_dephelp
[ 71%] Built target pch_Generate_opencv_perf_calib3d
[ 72%] Built target opencv_perf_calib3d
[ 72%] Built target opencv_test_calib3d_pch_dephelp
[ 72%] Built target pch_Generate_opencv_test_calib3d
[ 75%] Built target opencv_test_calib3d
[ 86%] Built target opencv_java
[ 86%] Built target opencv_test_java_properties
[ 95%] Built target opencv_test_java
[ 95%] Built target opencv_perf_stitching_pch_dephelp
[ 95%] Built target pch_Generate_opencv_perf_stitching
[ 95%] Built target opencv_stitching_pch_dephelp
[ 95%] Built target pch_Generate_opencv_stitching
[ 96%] Built target opencv_stitching
[ 96%] Built target opencv_perf_stitching
[ 96%] Built target opencv_test_stitching_pch_dephelp
[ 96%] Built target pch_Generate_opencv_test_stitching
[ 97%] Built target opencv_test_stitching
[ 97%] Built target opencv_videostab_pch_dephelp
[ 97%] Built target pch_Generate_opencv_videostab
[ 98%] Built target opencv_videostab
[100%] Built target opencv_python2
[100%] Built target opencv_python3
[100%] Built target opencv_traincascade
[100%] Built target opencv_createsamples
[100%] Built target opencv_annotation
Install the project...
-- Install configuration: "Release"
-- Installing: /usr/local/include/opencv2/cvconfig.h
-- Installing: /usr/local/include/opencv2/opencv_modules.hpp
-- Installing: /usr/local/lib/pkgconfig/opencv.pc
-- Installing: /usr/local/share/OpenCV/OpenCVConfig.cmake
-- Installing: /usr/local/share/OpenCV/OpenCVConfig-version.cmake
-- Old export file "/usr/local/share/OpenCV/OpenCVModules.cmake" will be replaced.  Removing files [/usr/local/share/OpenCV/OpenCVModules-release.cmake].
-- Installing: /usr/local/share/OpenCV/OpenCVModules.cmake
-- Installing: /usr/local/share/OpenCV/OpenCVModules-release.cmake
-- Up-to-date: /usr/local/include/opencv/cxmisc.h
-- Up-to-date: /usr/local/include/opencv/cxcore.h
-- Up-to-date: /usr/local/include/opencv/cvaux.hpp
-- Up-to-date: /usr/local/include/opencv/cvaux.h
-- Up-to-date: /usr/local/include/opencv/highgui.h
-- Up-to-date: /usr/local/include/opencv/ml.h
-- Up-to-date: /usr/local/include/opencv/cvwimage.h
-- Up-to-date: /usr/local/include/opencv/cv.hpp
-- Up-to-date: /usr/local/include/opencv/cxcore.hpp
-- Up-to-date: /usr/local/include/opencv/cv.h
-- Up-to-date: /usr/local/include/opencv/cxeigen.hpp
-- Up-to-date: /usr/local/include/opencv2/opencv.hpp
-- Installing: /usr/local/lib/libopencv_core.so.3.1.0
-- Installing: /usr/local/lib/libopencv_core.so.3.1
-- Set runtime path of "/usr/local/lib/libopencv_core.so.3.1.0" to "/usr/local/lib"
-- Installing: /usr/local/lib/libopencv_core.so
-- Up-to-date: /usr/local/include/opencv2/core/cuda/type_traits.hpp
-- Installing: /usr/local/include/opencv2/core/cuda/saturate_cast.hpp
-- Up-to-date: /usr/local/include/opencv2/core/cuda/block.hpp
-- Up-to-date: /usr/local/include/opencv2/core/cuda/limits.hpp
-- Up-to-date: /usr/local/include/opencv2/core/cuda/emulation.hpp
-- Up-to-date: /usr/local/include/opencv2/core/cuda/scan.hpp
-- Installing: /usr/local/include/opencv2/core/cuda/warp_shuffle.hpp
-- Up-to-date: /usr/local/include/opencv2/core/cuda/funcattrib.hpp
-- Up-to-date: /usr/local/include/opencv2/core/cuda/utility.hpp
-- Up-to-date: /usr/local/include/opencv2/core/cuda/functional.hpp
-- Installing: /usr/local/include/opencv2/core/cuda/border_interpolate.hpp
-- Up-to-date: /usr/local/include/opencv2/core/cuda/warp_reduce.hpp
-- Up-to-date: /usr/local/include/opencv2/core/cuda/filters.hpp
-- Up-to-date: /usr/local/include/opencv2/core/cuda/simd_functions.hpp
-- Up-to-date: /usr/local/include/opencv2/core/cuda/dynamic_smem.hpp
-- Up-to-date: /usr/local/include/opencv2/core/cuda/vec_math.hpp
-- Up-to-date: /usr/local/include/opencv2/core/cuda/warp.hpp
-- Up-to-date: /usr/local/include/opencv2/core/cuda/common.hpp
-- Up-to-date: /usr/local/include/opencv2/core/cuda/transform.hpp
-- Up-to-date: /usr/local/include/opencv2/core/cuda/reduce.hpp
-- Up-to-date: /usr/local/include/opencv2/core/cuda/vec_distance.hpp
-- Up-to-date: /usr/local/include/opencv2/core/cuda/color.hpp
-- Up-to-date: /usr/local/include/opencv2/core/cuda/datamov_utils.hpp
-- Up-to-date: /usr/local/include/opencv2/core/cuda/vec_traits.hpp
-- Up-to-date: /usr/local/include/opencv2/core/cuda/detail/transform_detail.hpp
-- Up-to-date: /usr/local/include/opencv2/core/cuda/detail/reduce_key_val.hpp
-- Up-to-date: /usr/local/include/opencv2/core/cuda/detail/vec_distance_detail.hpp
-- Up-to-date: /usr/local/include/opencv2/core/cuda/detail/type_traits_detail.hpp
-- Installing: /usr/local/include/opencv2/core/cuda/detail/reduce.hpp
-- Up-to-date: /usr/local/include/opencv2/core/cuda/detail/color_detail.hpp
-- Installing: /usr/local/include/opencv2/core.hpp
-- Installing: /usr/local/include/opencv2/core/affine.hpp
-- Installing: /usr/local/include/opencv2/core/mat.inl.hpp
-- Installing: /usr/local/include/opencv2/core/cuda.hpp
-- Up-to-date: /usr/local/include/opencv2/core/private.cuda.hpp
-- Installing: /usr/local/include/opencv2/core/base.hpp
-- Up-to-date: /usr/local/include/opencv2/core/cuda_types.hpp
-- Installing: /usr/local/include/opencv2/core/sse_utils.hpp
-- Installing: /usr/local/include/opencv2/core/neon_utils.hpp
-- Installing: /usr/local/include/opencv2/core/directx.hpp
-- Up-to-date: /usr/local/include/opencv2/core/ocl_genbase.hpp
-- Installing: /usr/local/include/opencv2/core/cvstd.inl.hpp
-- Installing: /usr/local/include/opencv2/core/private.hpp
-- Installing: /usr/local/include/opencv2/core/utility.hpp
-- Installing: /usr/local/include/opencv2/core/ptr.inl.hpp
-- Installing: /usr/local/include/opencv2/core/mat.hpp
-- Installing: /usr/local/include/opencv2/core/version.hpp
-- Installing: /usr/local/include/opencv2/core/fast_math.hpp
-- Up-to-date: /usr/local/include/opencv2/core/bufferpool.hpp
-- Up-to-date: /usr/local/include/opencv2/core/ippasync.hpp
-- Up-to-date: /usr/local/include/opencv2/core/types.hpp
-- Installing: /usr/local/include/opencv2/core/saturate.hpp
-- Up-to-date: /usr/local/include/opencv2/core/core.hpp
-- Installing: /usr/local/include/opencv2/core/ocl.hpp
-- Installing: /usr/local/include/opencv2/core/cuda.inl.hpp
-- Installing: /usr/local/include/opencv2/core/opengl.hpp
-- Up-to-date: /usr/local/include/opencv2/core/persistence.hpp
-- Up-to-date: /usr/local/include/opencv2/core/traits.hpp
-- Installing: /usr/local/include/opencv2/core/matx.hpp
-- Up-to-date: /usr/local/include/opencv2/core/optim.hpp
-- Up-to-date: /usr/local/include/opencv2/core/operations.hpp
-- Installing: /usr/local/include/opencv2/core/cuda_stream_accessor.hpp
-- Up-to-date: /usr/local/include/opencv2/core/wimage.hpp
-- Up-to-date: /usr/local/include/opencv2/core/eigen.hpp
-- Installing: /usr/local/include/opencv2/core/cvstd.hpp
-- Installing: /usr/local/include/opencv2/core/va_intel.hpp
-- Installing: /usr/local/include/opencv2/core/cvdef.h
-- Up-to-date: /usr/local/include/opencv2/core/types_c.h
-- Up-to-date: /usr/local/include/opencv2/core/core_c.h
-- Installing: /usr/local/include/opencv2/core/hal/intrin_cpp.hpp
-- Installing: /usr/local/include/opencv2/core/hal/intrin_sse.hpp
-- Installing: /usr/local/include/opencv2/core/hal/hal.hpp
-- Installing: /usr/local/include/opencv2/core/hal/intrin.hpp
-- Installing: /usr/local/include/opencv2/core/hal/intrin_neon.hpp
-- Installing: /usr/local/include/opencv2/core/hal/interface.h
-- Installing: /usr/local/lib/libopencv_flann.so.3.1.0
-- Installing: /usr/local/lib/libopencv_flann.so.3.1
-- Set runtime path of "/usr/local/lib/libopencv_flann.so.3.1.0" to "/usr/local/lib"
-- Installing: /usr/local/lib/libopencv_flann.so
-- Up-to-date: /usr/local/include/opencv2/flann.hpp
-- Up-to-date: /usr/local/include/opencv2/flann/miniflann.hpp
-- Up-to-date: /usr/local/include/opencv2/flann/flann.hpp
-- Up-to-date: /usr/local/include/opencv2/flann/flann_base.hpp
-- Up-to-date: /usr/local/include/opencv2/flann/defines.h
-- Up-to-date: /usr/local/include/opencv2/flann/sampling.h
-- Up-to-date: /usr/local/include/opencv2/flann/index_testing.h
-- Up-to-date: /usr/local/include/opencv2/flann/matrix.h
-- Up-to-date: /usr/local/include/opencv2/flann/simplex_downhill.h
-- Up-to-date: /usr/local/include/opencv2/flann/linear_index.h
-- Up-to-date: /usr/local/include/opencv2/flann/dummy.h
-- Up-to-date: /usr/local/include/opencv2/flann/object_factory.h
-- Installing: /usr/local/include/opencv2/flann/kmeans_index.h
-- Up-to-date: /usr/local/include/opencv2/flann/composite_index.h
-- Up-to-date: /usr/local/include/opencv2/flann/kdtree_index.h
-- Up-to-date: /usr/local/include/opencv2/flann/nn_index.h
-- Up-to-date: /usr/local/include/opencv2/flann/hierarchical_clustering_index.h
-- Up-to-date: /usr/local/include/opencv2/flann/random.h
-- Up-to-date: /usr/local/include/opencv2/flann/dynamic_bitset.h
-- Up-to-date: /usr/local/include/opencv2/flann/logger.h
-- Up-to-date: /usr/local/include/opencv2/flann/any.h
-- Up-to-date: /usr/local/include/opencv2/flann/timer.h
-- Up-to-date: /usr/local/include/opencv2/flann/dist.h
-- Up-to-date: /usr/local/include/opencv2/flann/ground_truth.h
-- Up-to-date: /usr/local/include/opencv2/flann/lsh_index.h
-- Up-to-date: /usr/local/include/opencv2/flann/kdtree_single_index.h
-- Up-to-date: /usr/local/include/opencv2/flann/general.h
-- Up-to-date: /usr/local/include/opencv2/flann/saving.h
-- Up-to-date: /usr/local/include/opencv2/flann/heap.h
-- Up-to-date: /usr/local/include/opencv2/flann/all_indices.h
-- Up-to-date: /usr/local/include/opencv2/flann/lsh_table.h
-- Up-to-date: /usr/local/include/opencv2/flann/params.h
-- Up-to-date: /usr/local/include/opencv2/flann/result_set.h
-- Up-to-date: /usr/local/include/opencv2/flann/allocator.h
-- Up-to-date: /usr/local/include/opencv2/flann/config.h
-- Up-to-date: /usr/local/include/opencv2/flann/hdf5.h
-- Installing: /usr/local/include/opencv2/flann/autotuned_index.h
-- Installing: /usr/local/lib/libopencv_imgproc.so.3.1.0
-- Installing: /usr/local/lib/libopencv_imgproc.so.3.1
-- Set runtime path of "/usr/local/lib/libopencv_imgproc.so.3.1.0" to "/usr/local/lib"
-- Installing: /usr/local/lib/libopencv_imgproc.so
-- Installing: /usr/local/include/opencv2/imgproc.hpp
-- Up-to-date: /usr/local/include/opencv2/imgproc/imgproc.hpp
-- Up-to-date: /usr/local/include/opencv2/imgproc/imgproc_c.h
-- Up-to-date: /usr/local/include/opencv2/imgproc/types_c.h
-- Installing: /usr/local/include/opencv2/imgproc/detail/distortion_model.hpp
-- Installing: /usr/local/lib/libopencv_ml.so.3.1.0
-- Installing: /usr/local/lib/libopencv_ml.so.3.1
-- Set runtime path of "/usr/local/lib/libopencv_ml.so.3.1.0" to "/usr/local/lib"
-- Installing: /usr/local/lib/libopencv_ml.so
-- Installing: /usr/local/include/opencv2/ml.hpp
-- Up-to-date: /usr/local/include/opencv2/ml/ml.hpp
-- Installing: /usr/local/lib/libopencv_photo.so.3.1.0
-- Installing: /usr/local/lib/libopencv_photo.so.3.1
-- Set runtime path of "/usr/local/lib/libopencv_photo.so.3.1.0" to "/usr/local/lib"
-- Installing: /usr/local/lib/libopencv_photo.so
-- Installing: /usr/local/include/opencv2/photo.hpp
-- Up-to-date: /usr/local/include/opencv2/photo/cuda.hpp
-- Up-to-date: /usr/local/include/opencv2/photo/photo.hpp
-- Up-to-date: /usr/local/include/opencv2/photo/photo_c.h
-- Installing: /usr/local/lib/libopencv_video.so.3.1.0
-- Installing: /usr/local/lib/libopencv_video.so.3.1
-- Set runtime path of "/usr/local/lib/libopencv_video.so.3.1.0" to "/usr/local/lib"
-- Installing: /usr/local/lib/libopencv_video.so
-- Up-to-date: /usr/local/include/opencv2/video.hpp
-- Up-to-date: /usr/local/include/opencv2/video/video.hpp
-- Installing: /usr/local/include/opencv2/video/tracking.hpp
-- Up-to-date: /usr/local/include/opencv2/video/background_segm.hpp
-- Up-to-date: /usr/local/include/opencv2/video/tracking_c.h
-- Installing: /usr/local/lib/libopencv_viz.so.3.1.0
-- Installing: /usr/local/lib/libopencv_viz.so.3.1
-- Set runtime path of "/usr/local/lib/libopencv_viz.so.3.1.0" to "/usr/local/lib:/usr/lib/openmpi/lib"
-- Installing: /usr/local/lib/libopencv_viz.so
-- Up-to-date: /usr/local/include/opencv2/viz.hpp
-- Up-to-date: /usr/local/include/opencv2/viz/widget_accessor.hpp
-- Installing: /usr/local/include/opencv2/viz/widgets.hpp
-- Installing: /usr/local/include/opencv2/viz/types.hpp
-- Installing: /usr/local/include/opencv2/viz/viz3d.hpp
-- Up-to-date: /usr/local/include/opencv2/viz/vizcore.hpp
-- Installing: /usr/local/lib/libopencv_imgcodecs.so.3.1.0
-- Installing: /usr/local/lib/libopencv_imgcodecs.so.3.1
-- Set runtime path of "/usr/local/lib/libopencv_imgcodecs.so.3.1.0" to "/usr/local/lib"
-- Installing: /usr/local/lib/libopencv_imgcodecs.so
-- Installing: /usr/local/include/opencv2/imgcodecs.hpp
-- Up-to-date: /usr/local/include/opencv2/imgcodecs/imgcodecs.hpp
-- Up-to-date: /usr/local/include/opencv2/imgcodecs/imgcodecs_c.h
-- Up-to-date: /usr/local/include/opencv2/imgcodecs/ios.h
-- Installing: /usr/local/lib/libopencv_shape.so.3.1.0
-- Installing: /usr/local/lib/libopencv_shape.so.3.1
-- Set runtime path of "/usr/local/lib/libopencv_shape.so.3.1.0" to "/usr/local/lib"
-- Installing: /usr/local/lib/libopencv_shape.so
-- Up-to-date: /usr/local/include/opencv2/shape.hpp
-- Up-to-date: /usr/local/include/opencv2/shape/emdL1.hpp
-- Up-to-date: /usr/local/include/opencv2/shape/shape.hpp
-- Up-to-date: /usr/local/include/opencv2/shape/shape_distance.hpp
-- Up-to-date: /usr/local/include/opencv2/shape/shape_transformer.hpp
-- Up-to-date: /usr/local/include/opencv2/shape/hist_cost.hpp
-- Installing: /usr/local/lib/libopencv_videoio.so.3.1.0
-- Installing: /usr/local/lib/libopencv_videoio.so.3.1
-- Set runtime path of "/usr/local/lib/libopencv_videoio.so.3.1.0" to "/usr/local/lib"
-- Installing: /usr/local/lib/libopencv_videoio.so
-- Installing: /usr/local/include/opencv2/videoio.hpp
-- Up-to-date: /usr/local/include/opencv2/videoio/videoio.hpp
-- Installing: /usr/local/include/opencv2/videoio/videoio_c.h
-- Installing: /usr/local/include/opencv2/videoio/cap_ios.h
-- Installing: /usr/local/lib/libopencv_highgui.so.3.1.0
-- Installing: /usr/local/lib/libopencv_highgui.so.3.1
-- Set runtime path of "/usr/local/lib/libopencv_highgui.so.3.1.0" to "/usr/local/lib"
-- Installing: /usr/local/lib/libopencv_highgui.so
-- Installing: /usr/local/include/opencv2/highgui.hpp
-- Up-to-date: /usr/local/include/opencv2/highgui/highgui.hpp
-- Installing: /usr/local/include/opencv2/highgui/highgui_c.h
-- Installing: /usr/local/lib/libopencv_objdetect.so.3.1.0
-- Installing: /usr/local/lib/libopencv_objdetect.so.3.1
-- Set runtime path of "/usr/local/lib/libopencv_objdetect.so.3.1.0" to "/usr/local/lib"
-- Installing: /usr/local/lib/libopencv_objdetect.so
-- Installing: /usr/local/include/opencv2/objdetect.hpp
-- Up-to-date: /usr/local/include/opencv2/objdetect/objdetect.hpp
-- Up-to-date: /usr/local/include/opencv2/objdetect/detection_based_tracker.hpp
-- Up-to-date: /usr/local/include/opencv2/objdetect/objdetect_c.h
-- Installing: /usr/local/lib/libopencv_superres.so.3.1.0
-- Installing: /usr/local/lib/libopencv_superres.so.3.1
-- Set runtime path of "/usr/local/lib/libopencv_superres.so.3.1.0" to "/usr/local/lib"
-- Installing: /usr/local/lib/libopencv_superres.so
-- Up-to-date: /usr/local/include/opencv2/superres.hpp
-- Up-to-date: /usr/local/include/opencv2/superres/optical_flow.hpp
-- Installing: /usr/local/lib/libopencv_ts.a
-- Installing: /usr/local/lib/libopencv_features2d.so.3.1.0
-- Installing: /usr/local/lib/libopencv_features2d.so.3.1
-- Set runtime path of "/usr/local/lib/libopencv_features2d.so.3.1.0" to "/usr/local/lib"
-- Installing: /usr/local/lib/libopencv_features2d.so
-- Installing: /usr/local/include/opencv2/features2d.hpp
-- Up-to-date: /usr/local/include/opencv2/features2d/features2d.hpp
-- Installing: /usr/local/lib/libopencv_calib3d.so.3.1.0
-- Installing: /usr/local/lib/libopencv_calib3d.so.3.1
-- Set runtime path of "/usr/local/lib/libopencv_calib3d.so.3.1.0" to "/usr/local/lib"
-- Installing: /usr/local/lib/libopencv_calib3d.so
-- Installing: /usr/local/include/opencv2/calib3d.hpp
-- Up-to-date: /usr/local/include/opencv2/calib3d/calib3d.hpp
-- Installing: /usr/local/include/opencv2/calib3d/calib3d_c.h
-- Installing: /usr/local/share/OpenCV/java/opencv-310.jar
-- Installing: /usr/local/share/OpenCV/java/libopencv_java310.so
-- Set runtime path of "/usr/local/share/OpenCV/java/libopencv_java310.so" to "/usr/local/lib"
-- Installing: /usr/local/lib/libopencv_stitching.so.3.1.0
-- Installing: /usr/local/lib/libopencv_stitching.so.3.1
-- Set runtime path of "/usr/local/lib/libopencv_stitching.so.3.1.0" to "/usr/local/lib"
-- Installing: /usr/local/lib/libopencv_stitching.so
-- Up-to-date: /usr/local/include/opencv2/stitching.hpp
-- Up-to-date: /usr/local/include/opencv2/stitching/warpers.hpp
-- Up-to-date: /usr/local/include/opencv2/stitching/detail/seam_finders.hpp
-- Up-to-date: /usr/local/include/opencv2/stitching/detail/matchers.hpp
-- Up-to-date: /usr/local/include/opencv2/stitching/detail/warpers_inl.hpp
-- Up-to-date: /usr/local/include/opencv2/stitching/detail/util.hpp
-- Up-to-date: /usr/local/include/opencv2/stitching/detail/warpers.hpp
-- Up-to-date: /usr/local/include/opencv2/stitching/detail/blenders.hpp
-- Up-to-date: /usr/local/include/opencv2/stitching/detail/camera.hpp
-- Up-to-date: /usr/local/include/opencv2/stitching/detail/exposure_compensate.hpp
-- Up-to-date: /usr/local/include/opencv2/stitching/detail/motion_estimators.hpp
-- Up-to-date: /usr/local/include/opencv2/stitching/detail/autocalib.hpp
-- Up-to-date: /usr/local/include/opencv2/stitching/detail/timelapsers.hpp
-- Up-to-date: /usr/local/include/opencv2/stitching/detail/util_inl.hpp
-- Installing: /usr/local/lib/libopencv_videostab.so.3.1.0
-- Installing: /usr/local/lib/libopencv_videostab.so.3.1
-- Set runtime path of "/usr/local/lib/libopencv_videostab.so.3.1.0" to "/usr/local/lib"
-- Installing: /usr/local/lib/libopencv_videostab.so
-- Up-to-date: /usr/local/include/opencv2/videostab.hpp
-- Up-to-date: /usr/local/include/opencv2/videostab/optical_flow.hpp
-- Up-to-date: /usr/local/include/opencv2/videostab/stabilizer.hpp
-- Up-to-date: /usr/local/include/opencv2/videostab/motion_core.hpp
-- Up-to-date: /usr/local/include/opencv2/videostab/motion_stabilizing.hpp
-- Up-to-date: /usr/local/include/opencv2/videostab/deblurring.hpp
-- Up-to-date: /usr/local/include/opencv2/videostab/fast_marching.hpp
-- Up-to-date: /usr/local/include/opencv2/videostab/global_motion.hpp
-- Up-to-date: /usr/local/include/opencv2/videostab/fast_marching_inl.hpp
-- Up-to-date: /usr/local/include/opencv2/videostab/ring_buffer.hpp
-- Up-to-date: /usr/local/include/opencv2/videostab/outlier_rejection.hpp
-- Up-to-date: /usr/local/include/opencv2/videostab/log.hpp
-- Up-to-date: /usr/local/include/opencv2/videostab/frame_source.hpp
-- Up-to-date: /usr/local/include/opencv2/videostab/inpainting.hpp
-- Up-to-date: /usr/local/include/opencv2/videostab/wobble_suppression.hpp
-- Installing: /usr/local/lib/python2.7/dist-packages/cv2.so
-- Set runtime path of "/usr/local/lib/python2.7/dist-packages/cv2.so" to "/usr/local/lib:/usr/lib/openmpi/lib"
-- Installing: /usr/local/lib/python3.4/site-packages/cv2.cpython-34m.so
-- Set runtime path of "/usr/local/lib/python3.4/site-packages/cv2.cpython-34m.so" to "/usr/local/lib:/usr/lib/openmpi/lib"
-- Up-to-date: /usr/local/share/OpenCV/haarcascades/haarcascade_russian_plate_number.xml
-- Up-to-date: /usr/local/share/OpenCV/haarcascades/haarcascade_smile.xml
-- Up-to-date: /usr/local/share/OpenCV/haarcascades/haarcascade_upperbody.xml
-- Up-to-date: /usr/local/share/OpenCV/haarcascades/haarcascade_eye_tree_eyeglasses.xml
-- Up-to-date: /usr/local/share/OpenCV/haarcascades/haarcascade_lowerbody.xml
-- Up-to-date: /usr/local/share/OpenCV/haarcascades/haarcascade_eye.xml
-- Up-to-date: /usr/local/share/OpenCV/haarcascades/haarcascade_fullbody.xml
-- Up-to-date: /usr/local/share/OpenCV/haarcascades/haarcascade_frontalface_alt_tree.xml
-- Up-to-date: /usr/local/share/OpenCV/haarcascades/haarcascade_frontalcatface.xml
-- Up-to-date: /usr/local/share/OpenCV/haarcascades/haarcascade_frontalcatface_extended.xml
-- Up-to-date: /usr/local/share/OpenCV/haarcascades/haarcascade_frontalface_alt2.xml
-- Up-to-date: /usr/local/share/OpenCV/haarcascades/haarcascade_righteye_2splits.xml
-- Up-to-date: /usr/local/share/OpenCV/haarcascades/haarcascade_frontalface_default.xml
-- Up-to-date: /usr/local/share/OpenCV/haarcascades/haarcascade_frontalface_alt.xml
-- Up-to-date: /usr/local/share/OpenCV/haarcascades/haarcascade_lefteye_2splits.xml
-- Up-to-date: /usr/local/share/OpenCV/haarcascades/haarcascade_profileface.xml
-- Up-to-date: /usr/local/share/OpenCV/haarcascades/haarcascade_licence_plate_rus_16stages.xml
-- Up-to-date: /usr/local/share/OpenCV/lbpcascades/lbpcascade_profileface.xml
-- Up-to-date: /usr/local/share/OpenCV/lbpcascades/lbpcascade_frontalface.xml
-- Up-to-date: /usr/local/share/OpenCV/lbpcascades/lbpcascade_frontalcatface.xml
-- Up-to-date: /usr/local/share/OpenCV/lbpcascades/lbpcascade_silverware.xml
-- Installing: /usr/local/bin/opencv_traincascade
-- Set runtime path of "/usr/local/bin/opencv_traincascade" to "/usr/local/lib"
-- Installing: /usr/local/bin/opencv_createsamples
-- Set runtime path of "/usr/local/bin/opencv_createsamples" to "/usr/local/lib"
-- Installing: /usr/local/bin/opencv_annotation
-- Set runtime path of "/usr/local/bin/opencv_annotation" to "/usr/local/lib"
```

> "*If you have a positive attitude and constantly strive to give your best effort, eventually you will overcome your immediate problems and find you are ready for greater challenges.*"  
                                       Pat Riley
