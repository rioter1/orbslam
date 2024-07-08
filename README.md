Check the c++ version installed 
```
g++ --version
```
If output looks like 
```
g++ (Ubuntu 9.3.0-17ubuntu1~20.04) 9.3.0
```
GCC 4.8 and above support C++11, so any recent version, such as the one mentioned above, will work.  

# Install Pangolin
Download pangolin  
https://github.com/stevenlovegrove/Pangolin  

```
git clone --recursive https://github.com/stevenlovegrove/Pangolin.git
cd Pangolin
./scripts/install_prerequisites.sh --dry-run recommended
cmake -B build
cmake --build build
cmake --build build -t pypangolin_pip_install
```
Successfully installed numpy-2.0.0 pillow-10.3.0 pybind11-2.12.0 pyopengl-3.1.7 pyopengl-accelerate-3.1.7 pypangolin-0.9.1


While executing above command i encountered an error 
E: Unable to locate package catch2

I resolved it by building catch2 from git 
```
git clone https://github.com/catchorg/Catch2.git
cd Catch2
cmake -B build -S . -DBUILD_TESTING=OFF
sudo cmake --build build/ --target install
```
After Catch2 installation i removed all mentions of catch2 from install_prereqisites.sh and ran the install_prerequisites.sh again

# Install opencv (code tested with version 4.4.0)
You cannot install opencv-python as the build process for ORB_SLAM3 will look for OpenCVModules.cmake & OpenCVConfig.cmake which will not be built if you do 
a pip install. You have to build opencv

Install dependecies
```
sudo apt-get install -y  libjpeg-dev libpng-dev libtiff-dev libgtk2.0-dev libavcodec-dev libavformat-dev libswscale-dev libdc1394-22-dev libeigen3-dev libtheora-dev libvorbis-dev libxvidcore-dev libx264-dev sphinx-common libtbb-dev yasm libfaac-dev libopencore-amrnb-dev libopencore-amrwb-dev libopenexr-dev libgstreamer-plugins-base1.0-dev libavutil-dev libavfilter-dev libavresample-dev6

```

Download and unpack sources
```
wget -O opencv.zip https://github.com/opencv/opencv/archive/4.4.zip
unzip opencv.zip
cd opencv-4.4.0
```
If above link does not work try 4.4.0.zip

```

mkdir -p build && cd build

cmake ../opencv-4.x
 
cmake --build .

sudo make install

```


# Install Eigen
Eigen is required by g2o
Download eigen zip file https://eigen.tuxfamily.org/index.php?title=Main_Page and extract
```
cd eigen
mkdir build_dir
cd build_dir
cmake ../
make install
```

Note: YOU DO NOT NEED TO BUILD g20 and DBoW2 seperately. they come preinstalled in ORB_SLAM3 directory, but if you wish to, use below and change path in ./build.sh directory
# Install DBoW2

```
sudo apt-get install libopencv-dev

git clone https://github.com/dorian3d/DBoW2.git
cd DBoW2

mkdir build
cd build

cmake ..

make

sudo make install
```

Check if the library files are installed
```
ls /usr/local/lib/libDBoW2.so
```
Check if the header files are installed
```
ls /usr/local/include/DBoW2/
```

# Install g2o
Install dependencies
```
sudo apt install libeigen3-dev libspdlog-dev libsuitesparse-dev qtdeclarative5-dev qt5-qmake libqglviewer-dev-qt5
```

Install g2o
```
git clone https://github.com/RainerKuemmerle/g2o.git
cd g2o
mkdir build
cd build
cmake ../
make
```

# Install ROS

```
sudo sh -c 'echo "deb http://packages.ros.org/ros/ubuntu $(lsb_release -sc) main" > /etc/apt/sources.list.d/ros-latest.list'

sudo apt install curl # if you haven't already installed curl
curl -s https://raw.githubusercontent.com/ros/rosdistro/master/ros.asc | sudo apt-key add -

sudo apt update

sudo apt install ros-noetic-desktop-full
```
env setup

```
echo "source /opt/ros/noetic/setup.bash" >> ~/.bashrc
source ~/.bashrc
```
Install dependencies
```
sudo apt install python3-rosdep python3-rosinstall python3-rosinstall-generator python3-wstool build-essential
```
Initialize rosdep
```
sudo rosdep init
rosdep update
```

# Install intel realsesne camera setup

```
sudo apt-get install libssl-dev libusb-1.0-0-dev libudev-dev pkg-config libgtk-3-dev v4l-utils
sudo apt-get install libglfw3-dev libgl1-mesa-dev libglu1-mesa-dev at
git clone https://github.com/IntelRealSense/librealsense.git

./scripts/patch-realsense-ubuntu-lts-hwe.sh

mkdir build && cd build
cmake ../ -DBUILD_EXAMPLES=true

sudo make uninstall && make clean && make && sudo make install
```

# Install ORBSLAM3
Try below, if it works, you are all set. 

```
git clone https://github.com/UZ-SLAMLab/ORB_SLAM3.git ORB_SLAM3
cd ORB_SLAM3
chmod +x build.sh
./build.sh
```
BUT

Make sure the your LD_LIBRARY_PATH contains /usr/lib (check echo $LD_LIBRARY_PATH especially if running a virtual environment)

```
export LD_LIBRARY_PATH=/usr/lib:/usr/local/lib
source ~/.bashrc
```


I faced certain errors with respect to theopencv 

for opencv, it was looking for path to files OpenCVModules.cmake & OpenCVConfig.cmake during build process, to solve it

paste in ORB_SLAM3/CMakeLists.txt, just before find_package(OpenCV 4.4) (around line 33)
```
set(OpenCV_DIR "path/to/opencv/opencv-4.4.0/build")
```

And i faced errors with and pthread header and functions having troubline linking with the pthread library
```
/usr/bin/ld: cannot find -lpthreads
/usr/bin/ld: CMakeFiles/cmTC_880d9.dir/src.c.o: in function main':
src.c:(.text.startup+0x29): undefined reference to pthread_create'
/usr/bin/ld: src.c:(.text.startup+0x32): undefined reference to pthread_detach'
/usr/bin/ld: src.c:(.text.startup+0x3d): undefined reference to pthread_join'
collect2: error: ld returned 1 exit status
make[1]: *** [CMakeFiles/cmTC_880d9.dir/build.make:87: cmTC_880d9] Error 1
make[1]: Leaving directory '/path/to/orb_slam3/ORB_SLAM3/build/CMakeFiles/CMakeTmp'
make: *** [Makefile:121: cmTC_880d9/fast] Error 2
```

i also saw issues during cmake, despite it getting completed

```
-- Looking for pthread.h
-- Looking for pthread.h - found
-- Performing Test CMAKE_HAVE_LIBC_PTHREAD
-- Performing Test CMAKE_HAVE_LIBC_PTHREAD - Failed
-- Looking for pthread_create in pthreads
-- Looking for pthread_create in pthreads - not found
-- Looking for pthread_create in pthread
-- Looking for pthread_create in pthread - found
-- Found Threads: TRUE  
```

To resolve these issues, you will have to make changes to CMakeLists.txt in ORB_SLAM3 folder

In line 10 and 11 of CMakeLists.txt, replace 
```
set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS}  -Wall   -O3")
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -Wall   -O3")
```
with 
```
set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS}  -Wall   -O3   -pthread")
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -Wall   -O3   -pthread")
```
FIND BELOW LINES (SHOULD BE AROUND LINE NUMBER 40)
```
find_package(Eigen3 3.1.0 REQUIRED)
find_package(Pangolin REQUIRED)
find_package(realsense2)
```

and replace by 
```
find_package(Eigen3 3.1.0 REQUIRED)
find_package(Pangolin REQUIRED)
find_package(realsense2)
set(THREADS_PREFER_PTHREAD_FLAG ON)
find_package(Threads REQUIRED)
```

AFTER YOU HAVE PASTED ABOVE, REPLACE 

```
target_link_libraries(${PROJECT_NAME}
${OpenCV_LIBS}
${EIGEN3_LIBS}
${Pangolin_LIBRARIES}
${PROJECT_SOURCE_DIR}/Thirdparty/DBoW2/lib/libDBoW2.so
${PROJECT_SOURCE_DIR}/Thirdparty/g2o/lib/libg2o.so
-lboost_serialization
-lcrypto
)
```
WITH 
```
target_link_libraries(${PROJECT_NAME}
${OpenCV_LIBS}
${EIGEN3_LIBS}
${Pangolin_LIBRARIES}
${PROJECT_SOURCE_DIR}/Thirdparty/DBoW2/lib/libDBoW2.so
${PROJECT_SOURCE_DIR}/Thirdparty/g2o/lib/libg2o.so
${CMAKE_THREAD_LIBS_INIT}
-lboost_serialization
-lcrypto
)
```


Above changes will solve the thread linking issues.

You may face errors such as below during the make process, these errors are happening while linking the Pangolin object files using a different version of cpp. 

```
/usr/local/include/sigslot/signal.hpp:1180:9: error: ‘cow_copy_type’ was not declared in this scope
 1180 |         cow_copy_type<list_type, Lockable> ref = slots_reference()
/usr/local/include/sigslot/signal.hpp: In member function ‘sigslot::signal_base< <template-parameter-1-1>, <template-parameter-1-2> >& sigslot::signal_base< <template-parameter-1-1>, <template-parameter-1-2> >::operator=(sigslot::signal_base< <template-parameter-1-1>, <template-parameter-1-2> >&&)’:
/usr/local/include/sigslot/signal.hpp:1155:14: error: ‘m_slots’ was not declared in this scope
```

BEFORE THE LAST MAKE, AFTER cmake .. -DCMAKE_BUILD_TYPE=Release in ORB_SLAM3/build.sh, you have to add below command to update standard from c++11 to c++14

```
sed -i 's/++11/++14/g' CMakeLists.txt
```
for more info on it, you can check the link https://github.com/UZ-SLAMLab/ORB_SLAM3/issues/458

 
# Building ORB_SLAM3 with ROS

Install ROS as per earlier Instructions laid out or use the link http://wiki.ros.org/noetic/Installation/Ubuntu

Once Installation is complete

edit bashrc

```
gedit ~/.bashrc
export ROS_PACKAGE_PATH=${ROS_PACKAGE_PATH}:PATH/ORB_SLAM3/Examples_old/ROS
source ~/.bashrc
```
in README.md inside Examples_old/ROS/ORB_SLAM3 

Add at the top
```
project(ORB_SLAM3)
set(THREADS_PREFER_PTHREAD_FLAG ON)
```

replace lines
```
set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS}  -Wall  -O3 -march=native")
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -Wall  -O3 -march=native")
```
with 
```
set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS}  -Wall  -O3 -march=native   -pthread")
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -Wall  -O3 -march=native   -pthread")
```


Remove the line
```
find_package(OpenCV 3.0 QUIET)
if(NOT OpenCV_FOUND)
   find_package(OpenCV 2.4.3 QUIET)
   if(NOT OpenCV_FOUND)
      message(FATAL_ERROR "OpenCV > 2.4.3 not found.")
   endif()
endif()
```

and replace by 
```

set(OpenCV_DIR "/home/antpc/orbslam/opencv-4.4.0/build")
find_package(OpenCV 4.4)
   if(NOT OpenCV_FOUND)
      message(FATAL_ERROR "OpenCV > 4.4 not found.")
   endif()

MESSAGE("OPENCV VERSION:")
MESSAGE(${OpenCV_VERSION})
```

Add line
```
find_package(Threads REQUIRED)
```
and in include_directories, paste
```
${PROJECT_SOURCE_DIR}/../../../Thirdparty/Sophus
```

in set(LIBS) paste
```
${CMAKE_THREAD_LIBS_INIT}
```

I also commented out AR camera as i will not be using it and it throws an error during the build process.
error: conversion from ‘sophus::se3f’ {aka ‘sophus::se3<float>’} to non-scalar type ‘cv::mat’ requested 151 | cv::mat tcw = mpslam->trackmonocular(cv_ptr->image,cv_ptr->header.stamp.tosec());

```
# Node for monocular camera (Augmented Reality Demo)
#rosbuild_add_executable(MonoAR
#src/AR/ros_mono_ar.cc
#src/AR/ViewerAR.h
#src/AR/ViewerAR.cc
#)

#target_link_libraries(MonoAR
#${LIBS}
#)
```

execute 
```
sed -i 's/++11/++14/g' Examples_old/ROS/ORB_SLAM3/CMakeLists.txt 
chmod +x build_ros.sh
./build_ros.sh
```

the ORB_SLAM3 package with ROS will be build





# Link to Video for monocular inertial slam
```
./Monocular-Inertial/mono_inertial_euroc ../Vocabulary/ORBvoc.txt ./Monocular-Inertial/EuRoC.yaml "$pathDatasetEuroc"/MH01 ./Monocular-Inertial/EuRoC_TimeStamps/MH01.txt dataset-MH01_monoi
```
https://drive.google.com/file/d/1Uj3CtvWZ1X-wGk4oDtbAXS7MmkkX_9t3/view?usp=sharing


```
./Stereo-Inertial/stereo_inertial_euroc_old ../Vocabulary/ORBvoc.txt ./Stereo-Inertial/EuRoC.yaml "$pathDatasetEuroc"/MH01 ./Stereo-Inertial/EuRoC_TimeStamps/MH01.txt "$pathDatasetEuroc"/MH02 ./Stereo-Inertial/EuRoC_TimeStamps/MH02.txt "$pathDatasetEuroc"/MH03 ./Stereo-Inertial/EuRoC_TimeStamps/MH03.txt "$pathDatasetEuroc"/MH04 ./Stereo-Inertial/EuRoC_TimeStamps/MH04.txt "$pathDatasetEuroc"/MH05 ./Stereo-Inertial/EuRoC_TimeStamps/MH05.txt dataset-MH01_to_MH05_stereoi
```
https://drive.google.com/file/d/1Uj3CtvWZ1X-wGk4oDtbAXS7MmkkX_9t3/view?usp=sharing

![Screenshot from 2024-07-08 20-33-00](https://github.com/rioter1/orbslam/assets/118795230/b2408383-8bce-4c7e-b308-df71cd5bff0d)

