步骤：
1.	进入你的工作环境，拷贝ros2_orbslam3包到你的src文件夹下面并解压。
2.	修改ros2_orbslam3包重的CMakeModules下的FindORB_SLAM3.cmake文件中的ORB_SLAM3_ROOD_DIR，按照提示的格式修改为本地的ORB_SLAM3位置。
3.	退回到你的工作空间，执行colcon build xxx构建包，这需要一点时间。
4.	source 你的ROS2环境（可选）：
source /opt/ros/galactic/setup.bash && . install/local_setup.bash
5.	启动subscriber：
ros2 run ros2_orbslam mono PATH_TO_VOCABULARY PATH_TO_YAML_CONFIG_FILE
修改PATH_TO_VOCABULARY为可用的词汇文件
修改PATH_TO_YAML_CONFIG_FILE为可用的yaml配置文件


问题：
1.	找不到头文件：sophus/se3.hpp。执行下面命令即可解决：
git clone https://github.com/strasdat/Sophus.git
mkdir build
cd build
cmake ..
make
sudo make install
2.	找不到libORB_SLAM3.so。编辑你的环境变量（vim ~/.bashrc  在末尾添加）即可解决：
export LD_LIBRARY_PATH=~/Pangolin/build/src/:~/ORB_SLAM2/Thirdparty/DBoW2/lib:~/ORB_SLAM2/Thirdparty/g2o/lib:~/ORB_SLAM2/lib:$LD_LIBRARY_PATH
注意：上面的命令需要写在一行。
