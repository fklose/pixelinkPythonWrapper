Introduction
------------

The Pixelink Python wrapper offers software developers a means to adapt existing programs, or develop new imaging applications
for Pixelink cameras using Python on Windows and Linux. As a wrapper around the native Pixelink API 4.0, it provides the same
easy to use interface that promotes rapid development of custom applications for camera operations as the native API, but with
Python’s concise and powerful scripting capabilities. This wrapper supports all Pixelink cameras that use and are compatible with
the Pixelink 4.0 API (that is, FireWire, USB, USB3, GigE, and 10 GigE cameras). The wrapper fully supports functionality of the
auto-focus, gain HDR, and polar cameras, camera operation with Navitar zoom systems as well as Navitar Resolv LED controller.


Tested Platforms
----------------

* Windows 10 (64-bit) with Pixelink SDK v12.0.1
* Linux Ubuntu 24.04 PC (x86 64-bit) with Linux SDK v3.5
* Python 3.8.6 (64-bit)


Installation
------------

The recommended procedure for installing the Pixelink Python wrapper package (pixelinkWrapper) is by using the pip(3) command,
as detailed below. This command will install the latest pixelinkWrapper from this repository as maintained by Pixelink. 
The Pixelink Python wrapper package (pixelinkWrapper) is also included in the Pixelink SDK as the local folder. It contains 
the version of the pixelinkWrapper that was current as of the version of the Pixelink SDK release. Although that folder is not 
necessary to install/use pixelinkWrapper, it is included as a convenience, should you need access to a non-current version of 
the pixelinkWrapper, or need to install the pixelinkWrapper without online connectivity.

The Pixelink Python wrapper is installed as follows (new installation):

On Windows:
1. Open https://www.navitar.com/products/pixelink-cameras
2. Download and install Pixelink Capture or Pixelink SDK
3. Run "pip install pixelinkWrapper"

On Linux:
1. Open https://www.navitar.com/products/pixelink-cameras/pixelink-sdk
2. Download and install Linux SDK
3. Run "sudo apt install python3-pip" to install pip3, if it is not installed
4. Run "pip3 install pixelinkWrapper"

If you already have a version of the Pixelink Python wrapper installed, and simply want to update it to the latest version,
that is done as follows:

On Windows:
1. Run "pip install pixelinkWrapper --upgrade" 

On Linux:
1. Run "pip3 install pixelinkWrapper --upgrade"


General Information
-------------------

The Pixelink Python wrapper is a thin wrapper around the Pixelink 4.0 API. Pixelink API functions are exposed as class methods 
of the PxLApi class in the pixelink module. Applications created with this wrapper can be used with all Pixelink cameras with 
the exception of the PL-A640/650/660 series cameras. Consult the Pixelink API documentation for specific information - most 
Pixelink 4.0 API functionality is preserved with a few minor limitations, so the regular documentation should suffice for most 
users. For more information about those limitations, please refer to the Tips and Tricks, and Gotchas section of this documentation 
below.

There is no Pixelink Python wrapper for the Pixelink 3.2 API since it is obsolete and hence, PL-A640/650/660 series cameras are 
excluded.

The Pixelink API functions are exposed as class methods of the PxLApi class and the Pixelink API defines are grouped as subclasses 
with respect to their functionality in the pixelink module.

Many of the functions accept parameters with assigned arguments. However, several functions have parameter(s) set with default 
value(s). They are
* decompressFrame
* getFeature
* getNextFrame
* getNextNumPyFrame
* initialize
* setPreviewSettings

Every function returns a tuple. The tuple consists of an API return code with parameter(s) on success, and an API error return 
code on failure.


Tips and Tricks, and Gotchas
----------------------------

* The Callback.FORMAT_IMAGE is not supported.

* The context of the setCallback function for Callback.COMPRESSED_FRAME must be set with a compression strategy
  (e.g. PxLApi.CompressionInfoPixelink10).

* The Settings.SETTINGS_FACTORY define can be used instead of the DefaultMemoryChannel.FACTORY_DEFAULTS_MEMORY_CHANNEL.

* Preview window
    - Defines of the preview window from the WindowsPreview class can only be used on Windows, but not on Linux.
    - Architecture of the preview window on Windows is different than on Linux. The preview window will go 'Not Responding', 
      if the message pump is not polled and events are not forwarded onto its handler. In order to overcome this limitation,
      it is proposed to use the user32.PeekMessageW function. See preview.py sample for Windows that uses this function.

* Callback function return statements
    - Note that each of the callback functions are shown to return an error code. This is shown this way to preserve a
      likeness to the native Pixelink 4.0 API. However, Python users should not rely on this return code. Rather, all
      error checking should be done within the callback routine itself.

* This wrapper provides the following 'helper' functions that are not present in the native Pixelink API
    - createByteAlignedBuffer
    - getBytesPerPixel
    - imageSize
    - getNextNumPyFrame
    - formatNumPyImage

* Use of a mutable ctypes character buffer instance in the following functions
	- getNextFrame
	- getNextCompressedFrame
	- formatImage
    - Note that the above functions expect a data buffer argument being passed as a ctypes character buffer instance. 
	  Such mutable character buffer instance can be created using the ctypes.create_string_buffer() function. Using 
	  this buffer type allows Python wrapper to maintain similar efficiency as the Pixelink 4.0 API. Furthermore, 
	  the same data buffer instance can be passed from getNextFrame to formatImage and getNextCompressedFrame 
	  to decompressFrame function.

* Use of a mutable ctypes character buffer that must be aligned on a 64-byte boundary
	- decompressFrame
    - Note that the above function expects data buffer arguments being passed as ctypes character buffer instances. 
	  In addition, those buffers must be aligned on a 64-byte boundary. Such mutable ctypes character buffers can be 
      created using the PxLApi.createByteAlignedBuffer() helper function. Furthermore, one of those data buffer 
      instances can be passed from getNextCompressedFrame to decompressFrame function.


Code Samples
------------

Pixelink Python code samples can be downloaded at https://github.com/pixelink-support/pixelinkPythonWrapper.


Getting Help from Pixelink Support
----------------------------------

Pixelink's goal is to make digital imaging simple. If you're having trouble with Pixelink Python wrapper, do not hesitate to 
contact us!

https://support.pixelink.com/support/tickets/new


Links
-----

* Repository: https://github.com/pixelink-support/pixelinkPythonWrapper
* PyPi Location: https://pypi.org/project/pixelinkWrapper/
* Pixelink Capture: https://www.navitar.com/products/pixelink-cameras/pixelink-capture
* Pixelink SDK or Linux SDK: https://www.navitar.com/products/pixelink-cameras/pixelink-sdk