Forked to test on Raspbian Stretch with Raspberry3 B+
=====================================================
@19, Jan, 2019

Testing Plan
------------
### CLI: will follow the test of guide
### Build simple test code
- C/C++ on QT
- 1'st step: JPEG image
- 2'nd step: Captured image from USB camera with v4l
   - Check the processing time on Raspi

Install references
------------------
- Install guide: https://blog.vinczejanos.info/2016/08/31/install-openalpr-on-raspberry-pi-3/
- OpenCV guide: 
   - https://www.learnopencv.com/install-opencv-4-on-raspberry-pi/
   - https://www.pyimagesearch.com/2017/09/04/raspbian-stretch-install-opencv-3-python-on-your-raspberry-pi/

Versions: As of 19/Jan/2019
---------
- Raspbian image: 2018-11-13-raspbian-stretch-full.zip
- tesseract verion: 4.0.0-143-g7a83
   - Leptonica-1.74.1
- OpenCV version: 3.4.5

Raspbian Stretch install
------------------------
- Downloaded the full desktop version

OpenCV install
--------------
### Cloned the 3.4 branch instead of master
- Couldn't make a successful build with 4.0 version, which is master version.
- Decided to test with 3.4 version first and then try master again.

#### Remove some packages as the ref
#### Didn't specify the install directory and let the working directory /usr/local/src as the guide.
#### Went throgh the steps to 3
#### Step 4: Download OpenCV and OpenCV extra modules
```
    cd /usr/local/src
    git clone --branch 3.4 https://github.com/opencv/opencv.git
    git clone --branch 3.4 https://github.com/opencv/opencv_contrib.git
```
#### Step 5: 
```
    cd opencv
    mkdir build
    cd build
```
#####  cmake
```
    cmake -D CMAKE_BUILD_TYPE=RELEASE -D CMAKE_INSTALL_PREFIX=/usr/local -D BUILD_NEW_PYTHON_SUPPORT=ON -D INSTALL_C_EXAMPLES=OFF -D INSTALL_PYTHON_EXAMPLES=OFF -D WITH_V4L=ON -D WITH_QT=ON -D WITH_OPENGL=ON -D BUILD_EXAMPLES=OFF ..
```
note: turn off the examples to save disk size. (can save over 3G)

#####  make: didn't use -j option as it make system hung up.
```
    make
    make install
    ldconfig
```

##### check OpenCV version
```
   pkg-config --modversion opencv
```

--------
openalpr
========

OpenALPR is an open source *Automatic License Plate Recognition* library written in C++ with bindings in C#, Java, Node.js, Go, and Python.  The library analyzes images and video streams to identify license plates.  The output is the text representation of any license plate characters.

Check out a live online demo here: http://www.openalpr.com/demo-image.html

User Guide
-----------


OpenALPR includes a command line utility.  Simply typing "alpr [image file path]" is enough to get started recognizing license plate images.

For example, the following output is created by analyzing this image:
![Plate Image](http://www.openalpr.com/images/demoscreenshots/plate3.png "Input image")



```
user@linux:~/openalpr$ alpr ./samplecar.png

plate0: top 10 results -- Processing Time = 58.1879ms.
    - PE3R2X     confidence: 88.9371
    - PE32X      confidence: 78.1385
    - PE3R2      confidence: 77.5444
    - PE3R2Y     confidence: 76.1448
    - P63R2X     confidence: 72.9016
    - FE3R2X     confidence: 72.1147
    - PE32       confidence: 66.7458
    - PE32Y      confidence: 65.3462
    - P632X      confidence: 62.1031
    - P63R2      confidence: 61.5089

```

Detailed command line usage:

```
user@linux:~/openalpr$ alpr --help

USAGE: 

   alpr  [-c <country_code>] [--config <config_file>] [-n <topN>] [--seek
         <integer_ms>] [-p <pattern code>] [--clock] [-d] [-j] [--]
         [--version] [-h] <image_file_path>


Where: 

   -c <country_code>,  --country <country_code>
     Country code to identify (either us for USA or eu for Europe). 
     Default=us

   --config <config_file>
     Path to the openalpr.conf file

   -n <topN>,  --topn <topN>
     Max number of possible plate numbers to return.  Default=10

   --seek <integer_ms>
     Seek to the specified millisecond in a video file. Default=0

   -p <pattern code>,  --pattern <pattern code>
     Attempt to match the plate number against a plate pattern (e.g., md
     for Maryland, ca for California)

   --clock
     Measure/print the total time to process image and all plates. 
     Default=off

   -d,  --detect_region
     Attempt to detect the region of the plate image.  [Experimental] 
     Default=off

   -j,  --json
     Output recognition results in JSON format.  Default=off

   --,  --ignore_rest
     Ignores the rest of the labeled arguments following this flag.

   --version
     Displays version information and exits.

   -h,  --help
     Displays usage information and exits.

   <image_file_path>
     Image containing license plates


   OpenAlpr Command Line Utility

```


Binaries
----------

Pre-compiled Windows binaries can be downloaded on the [releases page](https://github.com/openalpr/openalpr/releases)

Install OpenALPR on Ubuntu 16.04 with the following commands:

    sudo apt-get update && sudo apt-get install -y openalpr openalpr-daemon openalpr-utils libopenalpr-dev

Documentation
---------------

Detailed documentation is available at [doc.openalpr.com](http://doc.openalpr.com/)

Integrating the Library
-----------------------

OpenALPR is written in C++ and has bindings in C#, Python, Node.js, Go, and Java.  Please see this guide for examples showing how to run OpenALPR in your application: http://doc.openalpr.com/bindings.html

Compiling
-----------

[![Build Status](https://travis-ci.org/openalpr/openalpr.svg?branch=master)](https://travis-ci.org/openalpr/openalpr)

OpenALPR compiles and runs on Linux, Mac OSX and Windows.

OpenALPR requires the following additional libraries:

    - Tesseract OCR v3.0.4 (https://github.com/tesseract-ocr/tesseract)
    - OpenCV v2.4.8+ (http://opencv.org/)

After cloning this GitHub repository, you should download and extract Tesseract and OpenCV source code into their own directories.  Compile both libraries.

Please follow these detailed compilation guides for your respective operating system:

* [Windows](https://github.com/openalpr/openalpr/wiki/Compilation-instructions-(Windows))
* [Ubuntu Linux](https://github.com/openalpr/openalpr/wiki/Compilation-instructions-(Ubuntu-Linux))
* [OS X](https://github.com/openalpr/openalpr/wiki/Compilation-instructions-(OS-X))
* [Android Library](https://github.com/SandroMachado/openalpr-android)
* [Android Application Sample](https://github.com/sujaybhowmick/OpenAlprDroidApp)
* [iOS](https://github.com/twelve17/openalpr-ios)
* [iOS React Native](https://github.com/cardash/react-native-openalpr)
* [Xamarin](https://github.com/kevinjpetersen/openalpr-xamarin)

If all went well, there should be an executable named *alpr* along with *libopenalpr-static.a* and *libopenalpr.so* that can be linked into your project.

Docker
------

``` shell
# Build docker image
docker build -t openalpr https://github.com/openalpr/openalpr.git
# Download test image
wget http://plates.openalpr.com/h786poj.jpg
# Run alpr on image
docker run -it --rm -v $(pwd):/data:ro openalpr -c eu h786poj.jpg
```

Questions
---------
Please post questions or comments to the Google group list: https://groups.google.com/forum/#!forum/openalpr


Contributions
-------------
Improvements to the OpenALPR library are always welcome.  Please review the [OpenALPR design description](https://github.com/openalpr/openalpr/wiki/OpenALPR-Design) and get started.

Code contributions are not the only way to help out.  Do you have a large library of license plate images?  If so, please upload your data to the anonymous FTP located at upload.openalpr.com.  Do you have time to "tag" plate images in an input image or help in other ways?  Please let everyone know by posting a note in the forum.


License
-------

Affero GPLv3
http://www.gnu.org/licenses/agpl-3.0.html

Commercial-friendly licensing available.  Contact: info@openalpr.com
