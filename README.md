libegl

#ABOUT

libegl is an EGL implementation targeting [Apple iOS](https://www.apple.com/ios/) platform.

#MOTIVATION:

I am writing cross-platform C/C++ libraries and I needed an EGL 
implementation for my iPhone port.

# WHAT IS EGL ?

> EGL provides mechanisms for creating rendering surfaces onto which client APIs 
> like OpenGL ES and OpenVG can draw, creates graphics contexts for client APIs, 
> and synchronizes drawing by client APIs as well as native platform rendering APIs. 
> This enables seamless rendering using both OpenGL ES and OpenVG for high-performance, 
> accelerated, mixed-mode 2D and 3D rendering.
                                                 *Khronos Group*

Please refer to [official Khronos Group website](https://www.khronos.org/) for 
further details about [EGL](https://www.khronos.org/egl/). Especially 
[Khronos EGL Registry ](https://www.khronos.org/registry/egl/).

#DOCUMENTATION

The source code is partially documented although EAGL iOS driver aims to be fully
documented in the near future.

Major design decisions are [documented](http://www.mesa3d.org/egl.html).

##How to get started on iOS ?

###How to compile the library ?

    cd proj && make build-all-release

###Build flavors

    cd proj && make usage

###Where are the compilation artifacts ?

Upon sucessful project compilation, directory "prefix" will contains:
    
    - static library .a file for your project to link with.
    - EGL header files declaring EGL functions

###What are the dependencies/linker settings required to link with ?

Static library .a file requires:

    - to be linked with:
        - OpenGLES.framework
        - CoreGraphics.framework
        - UIKit.framework
        - QuartzCore.framework

    - to have the following PREPROCESSOR macros defined:
        - HAVE_PTHREAD=1
        - _EGL_NATIVE_PLATFORM=_EGL_PLATFORM_IOS
        - _EGL_OS_APPLE_IOS=1
        - _EGL_BUILT_IN_DRIVER_IOSEAGL=1
        - DEBUG=1 (optional)

# ARC vs MRR support

The code is fully compatible with MRR. ARC support is on the way.

#SOURCE

Main source repository: 

https://github.com/davidandreoletti/libegl

#DEVELOPMENT STATUS

This implementation is in ALPHA version. I only implements features required 
for my own needs but feel free to extend it. The tables below describes what
is supported in this implementation.

##Nomenclature

- Supported: The feature is implemented and fully/partially tested
- unsupported: The feature is not implemented due to either technically infeasibility OR not yet planned
- planned: The feature will be implemented for a future release

## EGL Core API

### iOS SDK supported

| iOS SDK Version | Status    |
|-----------------|-----------|
| 4.3+            | Supported |


###EGL API version supported

| EGL API Version | Status    |
|-----------------|-----------|
| 1.4             | Supported |


###Datatypes

| EGL datatype             | Support Status | iOS datatype           | Details |
|--------------------------|----------------|------------------------|---------|
| EGLNativeDisplayType     | Supported      | UIWindow*              |         |
| EGLNativePixmapType      | Unsupported    | NA                     |         |
| EGLNativeWindowType      | Supported      | CAEAGLayer             |         |

###Client APIs

| Client API                       | Support Status         | Details                                       |
|----------------------------------|------------------------|-----------------------------------------------|
| EGL\_OPENGL\_ES\_BIT             | Supported              |                                               |
| EGL\_OPENGL\_ES2\_BIT            | Supported              |                                               |
| EGL\_OPENGL\_ES3\_BIT\_KHR       | Supported              | See "EGL\_KHR\_create\_context" EGL extension |
| EGL\_OPENGL\_BIT                 | Unsupported            |                                               |
| EGL\_OPENVG\_BIT                 | Unsupported            |                                               |

###EGL Functions

| Functions                        | Support Status         | Details |
|----------------------------------|------------------------|---------|
| eglGetDisplay                    | Supported              |         |
| eglInitialize                    | Supported              |         |
| eglQueryString                   | Supported              |         |
| eglGetConfigs                    | Supported              |         |
| eglChooseConfig                  | Supported              |         |
| eglBindAPI                       | Supported              |         |
| eglCreateContext                 | Supported              |         |
| eglDestroyContext                | Supported              |         |
| eglCreateWindowSurface           | Supported              | EGLNativeWindowType MUST remain valid for as long as the EGLSurface handle is valid. The surface dimensions (in pixel) equals `EGLNativeWindowType's contentScaleFactor * EGLNativeWindowType's frame` |
| eglCreatePbufferSurface          | Unsupported            |         |
| eglCreatePixmapSurface           | Unsupported            |         |
| eglDestroySurface                | Supported              |         |
| eglQuerySurface                  | Supported              |         |
| eglQueryAPI                      | Supported              |         |
| eglQueryContext                  | Supported              |         |
| eglSurfaceAttrib                 | Planned                |         |
| eglMakeCurrent                   | Partially supported    | `EGLSurface read` and `EGLSurface draw` must be the same |
| eglGetCurrentContext             | Supported              |         |
| eglGetCurrentSurface             | Supported              |         |
| eglGetCurrentDisplay             | Supported              |         |
| eglGetConfigAttrib               | Supported              |         |
| eglSwapBuffers                   | Supported              |         |
| eglSwapInterval                  | Supported              |         |
| eglWaitClient                    | Supported              |         |
| eglWaitGL                        | Supported              |         |
| eglWaitNative                    | Unsupported            |         |
| eglBindTexImage                  | Unsupported            |         |
| eglReleaseTexImage               | Unsupported            |         |
| eglCopyBuffers                   | Unsupported            |         |
| eglCreatePbufferFromClientBuffer | Unsupported            |         |
| eglSwapInterval                  | Supported              |         |
| eglGetProcAddress                | Planned                |         |
| eglGetError                      | Supported              |         |
| eglReleaseThread                 | Supported              |         |
| eglTerminate                     | Planned                |         |

##EGL Extensions 

### EGL\_KHR\_create\_context support

Only attribute EGL\_OPENGL\_ES3\_BIT\_KHR is supported. Anything else has not been
tested and most likely not supported.

#TESTING

See "test" directory

#SAMPLES

See "samples" directory.

#FAQ 

See FAQ.md file

#KNOWN BUGS & MISSING FEATURES LISTS

See README-DEV.md file

#CHANGELOG & REQUIREMENTS

See this file and CHANGELOG file

#DONATING

Gittip:

[![Support via Gittip](https://rawgithub.com/twolfson/gittip-badge/0.2.0/dist/gittip.png)](https://www.gittip.com/davidandreoletti)

Bitcoin:

**1DaE8Dq4rm9XQKh2Po4pskiwZK526r4xUT**

#CONTRIBUTORS:

If you would like to contribute, feel free to drop me an email or contribute 
patches/pull requests.

#AUTHOR

- David Andreoletti http://davidandreoletti.com

#THANKS TO

- [Mesa 3D Graphic Library](http://www.mesa3d.org)
  This port is *greatly* inpired from their implementation. 
  Special thanks to you guys :)
    - Changeset used: 56ea2c4816dbcdbdabe7718423828fdb2ee1c95b
- [Alexei Sholik's iOS EGL prototype](https://github.com/alco/EGL_mac_ios) -
  This implementation did show me it was technically possible.


