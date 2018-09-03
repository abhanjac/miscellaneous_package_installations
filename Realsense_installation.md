# Realsense installation instruction in Ubuntu 16.04 LTS on laptop or Odroid XU4:

**These instruction is for using the Realsense R200.**

To make the Realsense R200 work, the **librealsense** and the python package **pyrealsense** packages must be installed.

Instructions can be found in [Instructions link](https://github.com/IntelRealSense/librealsense/blob/master/doc/installation.md).
But the following instructions also have added options for a more elaborate installation and how to cope with some errors while installing the packages.

#### Install Dependencies:
```
sudo apt-get update
sudo apt-get install libusb-1.0-0-dev pkg-config libgtk-3-dev libglfw3-dev
```

#### Clone repository:
```
git clone https://github.com/IntelRealSense/librealsense.git 
cd librealsense 
```

#### Installation of librealsense:

Now open the file **CMakeLists.txt** in the **librealsense** directory, and toggle option the following options to the format below:

```
option(BUILD_UNIT_TESTS "Build realsense unit tests." OFF)
option(BUILD_EXAMPLES "Build realsense examples and tools." ON)
option(ENFORCE_METADATA "Require WinSDK with Metadata support during compilation. Windows OS Only" OFF)
option(BUILD_PYTHON_BINDINGS "Build Python bindings" ON)
option(BUILD_CV_EXAMPLES "Build OpenCV examples" OFF)
option(BUILD_PCL_EXAMPLES "Build PCL examples" OFF)
option(BUILD_NODEJS_BINDINGS "Build Node.js bindings" OFF)
```

This prevents a compiler error with one of the unit tests during build process and activates the python wrapper.

```
mkdir build
cd build
cmake ../
make -j8 
sudo make install 
```

Update the **PYTHONPATH** environment variable to add the path to the pyrealsense library.

The following line has to be written to the bashrc file.
```
export PYTHONPATH=$PYTHONPATH:/usr/local/lib
```

Then do the following:

```
cd ..
sudo cp config/99-realsense-libusb.rules /etc/udev/rules.d/ 
sudo udevadm control --reload-rules 
sudo udevadm trigger 
sudo apt-get install libssl-dev 
./scripts/patch-realsense-ubuntu-xenial.sh 
```

Check dmsg to verify install (the log should indicate that a new uvcvideo driver has been registered).

```
sudo dmesg | tail -n 50 
```

#### Installation of pyrealsense package (Method 1):

This package will access the Realsense R200 camera.

Download the compressed file from the following [this](https://pypi.python.org/pypi/pyrealsense/2.2) location and decompress it and put it in the home directory.
Then, 

```
cd <pyrealsense directory>
sudo python setup.py install
```

[ **NOTE:** Basically this way you can install any package that is actually a pip package but for some reason the user does not want or cannot install using pip.
Here is a [link](https://stackoverflow.com/questions/13270877/how-to-manually-install-a-pypi-module-without-pip-easy-install) to how manually install pip package. ]

#### Installation of pyrealsense package (Method 2):

This package can also be installed using the pip install directly (by doing the following):


