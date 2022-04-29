# Jetson-Xavier-NX-Setup

This tutorial is about setting up Jetson-Xavier-NX and installing necessary SDKs.

## Part 1: Flash Jetpack and SDK components

### Download Nvidia SDK manager
NVIDIA SDK Manager provides an end-to-end development environment setup solution for Jetson, following [this link](https://developer.nvidia.com/nvidia-sdk-manager) and download NVIDIA SDK Manager on your ubuntu PC.

### Download Target OS and SDKs
Jetsons use operating system named JetPack which is a ubuntu system image. Please select JetPack 4.6 [rev.3] as your target operating system, this version corresponds to Ubuntu 18.04 LTS (Bionic Beaver). Then, download all Jetson SDK components (CUDA, CUDA-X AI, Computer Vision and NVIDIA Container Runtime).

### Flash Jetson-Xavier-NX module
This step flashes previous downloaded JetPack OS image onto Jetson-Xavier-NX and installs SDK components. Normally, Jetson-Xavier-NX has SD card with small storage (~16 GB) which is not enough for our development usage. You can attach a SSD card on the back of Jetson-Xavier-NX, and Jetson will use [NVMe](https://en.wikipedia.org/wiki/NVM_Express) (nonvolatile memory express) to access and transport data within it.

**To flash OS image on SSD, connect jetson with a micro-B USB with your laptop, choose NVMe as your storage device instead of default SD card.**

Troubleshoot:

If unable to flash, typically because the Jetson is not setup yet (without a password etc.), you have to go through the setup procedure and then be able to flash.

## Part 2: Build OpenCV with CUDA
If you follow the previous step to install official JetPack 4.6 on Jetson-Xavier-NX, OpenCV 4.1.1 is installed by default but not build with CUDA. To enable CUDA, which is for accelerating computation with GPU, we need to unstall OpenCV 4.1.1 and build OpenCV 3.4.3 with CUDA.
For removing previous installed OpenCV, you can use
```
sudo find / -name "*opencv*" -exec rm -i {} \;
```
For building OpenCV, please clone [this repo](https://github.com/jetsonhacks/buildOpenCVXavier) and run
```
./buildOpenCV.sh
```
OpenCV 3.4.3 will be built and installed under `/usr/local`.

## Part 3: Install ROS melodic
By following official [ROS installation guide](http://wiki.ros.org/melodic/Installation/Ubuntu).

Create a catkin workspace by the following commands:
```
mkdir -p ~/catkin_ws/src
cd ~/catkin_ws/
catkin_make
echo "source ~/catkin_ws/devel/setup.bash" >> ~/.bashrc
```

## Part 4: Build cv_bridge 
ROS passes around images in its own sensor_msgs/Image message format, in order to use those images in conjunction with OpenCV. [CvBridge](http://wiki.ros.org/cv_bridge/Tutorials/UsingCvBridgeToConvertBetweenROSImagesAndOpenCVImages) is a ROS library that provides an interface between ROS and OpenCV.
ROS melodic contains built cv_bridge that is originally targeted for OpenCV 3.2, to make it compatible with OpenCV 3.4, we need to build it from source.

We can use following commands to build it:
```
cd ~/catkin_ws/src
git clone https://github.com/ros-perception/vision_opencv/tree/melodic
git checkout melodic
catkin_make
```

## Part 5: Install RealSense ROS Wrapper
Following [this repo](https://github.com/IntelRealSense/realsense-ros/blob/development/README.md#method-2-the-realsense-distribution).

Test run by
```
roslaunch realsense2_camera rs_camrea.launch
```




