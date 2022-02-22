# ros2-ORB_SLAM3
ROS2 node wrapping the ORB_SLAM3 library

modified according to [Alsora](https://github.com/alsora)

### Requirements

 - [ROS2 galalctic](https://github.com/ros2/ros2/wiki/Installation)
 - [Pangolin](https://github.com/stevenlovegrove/Pangolin)
 - [ORB_SLAM3](https://github.com/UZ-SLAMLab/ORB_SLAM3)
 - [OpenCV3](https://docs.opencv.org/3.0-beta/doc/tutorials/introduction/linux_install/linux_install.html)
 - [vision_opencv](https://github.com/ros-perception/vision_opencv/tree/ros2)
 - [message_filters](https://github.com/ros2/message_filters)

Note: The `vision_opencv` package requires OpenCV3. Make sure to build ORB_SLAM3 with the same OpenCV version otherwise strange run errors could appear.

The `message_filters` package is not required if you want to use only the Monocular SLAM node. 

### Build


If you built ORB_SLAM3 following the instructions provided in its repository, you will have to tell CMake where to find it by exporting an environment variable that points to the cloned repository (as the library and include files will be in there).

    $ set(ORB_SLAM3_ROOT_DIR "/path/to/ORB_SLAM3")

Then you can build this package

    $ mkdir -p ws/src
    $ cd ws/src
    $ git clone https://github.com/curryc/ros2_orbslam3.git
    $ cd ..
    $ colcon build

### Issues

If cannot find sophus/se3.hpp:

    $ git clone https://github.com/strasdat/Sophus.git
    $ mkdir build
    & cd build
    & cmake ..
    & make
    & sudo make install

If cannot find libORB_SLAM3.so:

    & vim ~/.bashrc  

Then add the following instruction to export the file: 

    & export LD_LIBRARY_PATH=~/Pangolin/build/src/:~/ORB_SLAM3/Thirdparty/DBoW2/lib:~/ORB_SLAM3/Thirdparty/g2o/lib:~/ORB_SLAM3/lib:$LD_LIBRARY_PATH

### Usage

First source the workspace

    $ source ws/install/setup.sh

Then add to the LD_LIBRARY_PATH the location of ORB_SLAM3 library and its dependencies (the following paths may be different on your machine)

    $ export LD_LIBRARY_PATH=~/Pangolin/build/src/:~/ORB_SLAM3/Thirdparty/DBoW3/lib:~/ORB_SLAM3/Thirdparty/g2o/lib:~/ORB_SLAM3/lib:$LD_LIBRARY_PATH

Run the monocular SLAM node

    $ ros2 run ros2_orbslam mono PATH_TO_VOCABULARY PATH_TO_YAML_CONFIG_FILE

You can find the vocabulary file in the ORB_SLAM3 repository (e.g. `ORB_SLAM3/Vocabulary/ORBvoc.txt`), while the config file can be found within this repo (e.g. `ros2-ORB_SLAM3/src/monocular/TUM1.yaml` for monocular SLAM).

This node subscribes to the ROS2 topic `camera` and waits for Image messages.
For example you can stream frames from your laptop webcam using:

    $ ros2 run image_tools cam2image -t camera

The other nodes can be run in a very similar way.

You can run the `rgbd` node by using 

    $ ros2 run ros2_orbslam rgbd PATH_TO_VOCABULARY PATH_TO_YAML_CONFIG_FILE

You can run the `stereo` node by using 

    $ ros2 run ros2_orbslam stereo PATH_TO_VOCABULARY PATH_TO_YAML_CONFIG_FILE BOOL_RECTIFY
