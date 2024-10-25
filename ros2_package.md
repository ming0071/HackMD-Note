## ROS2 humble
#### install ros2 humble
建議使用 Debian 方式安裝[^註腳]，請參考 [官方安裝指南](
https://docs.ros.org/en/humble/Installation/Ubuntu-Install-Debs.html) 進行
```
echo "source /opt/ros/humble/setup.bash" >> ~/.bashrc
echo "export ROS_DISTRO=humble" >> ~/.bashrc
```

#### rosdep 安裝與更新
```
sudo apt install python3-rosdep2
rosdep update
```

## 機電系統 Packages 
#### urg_node2

```
git clone https://github.com/Hokuyo-aut/urg_node2.git
cd urg_node2
rm -r urg_library/
git clone https://github.com/UrgNetwork/urg_library.git
cd ~/ros2_ws 
rosdep install -y -r -q --from-paths src --ignore-src --rosdistro humble
colcon build --symlink-install
```

#### velodyne
```
cd ~/ros2_ws/src
git clone https://github.com/ros-drivers/velodyne.git -b humble-devel
cd ..
rosdep install -y -r -q --from-paths src --ignore-src --rosdistro humble
colcon build --symlink-install
```

#### ira_laser_tools
將兩個光達的 Laser Scan 資訊合併為一個
```
cd ~/ros2_ws/src
git clone https://github.com/nakai-omer/ira_laser_tools.git -b humble
cd ..
rosdep install -y -r -q --from-paths src --ignore-src --rosdistro humble
colcon build --symlink-install
```

#### rf2o_laser_odometry
使用連續兩幀 LiDAR 資訊生成 `/odom` 資訊
```
cd ~/ros2_ws/src
git clone https://github.com/Adlink-ROS/rf2o_laser_odometry.git -b humble-devel
cd ..
rosdep install -y -r -q --from-paths src --ignore-src --rosdistro humble
colcon build --symlink-install
```

#### laser_filter
```
cd ~/ros2_ws/src
git clone https://github.com/ros-perception/laser_filters.git -b ros2
cd ..
rosdep install -y -r -q --from-paths src --ignore-src --rosdistro humble
colcon build --symlink-install
```

#### robot_localization
發布 `/odom` 位移資料
```
cd ~/ros2_ws/src
git clone https://github.com/cra-ros-pkg/robot_localization.git -b  humble-devel
cd ..
rosdep install -y -r -q --from-paths src --ignore-src --rosdistro humble
colcon build --symlink-install
```

#### IMU
[原廠資料](https://sealandtech.com.tw/resource.html?s=anrot&type=tutorial&p=ros2/readme)

#### micro_ros_agent 
```
cd ~/ros2_ws/src
git clone https://github.com/micro-ROS/micro-ROS-Agent.git -b humble
git clone https://github.com/micro-ROS/micro_ros_msgs.git -b humble
cd ..
rosdep install -y -r -q --from-paths src --ignore-src --rosdistro humble
colcon build --symlink-install
```

## 建圖 Packages 
#### cartographer
- cartographer：核心演算法
- cartographer_ros：將 cartographer 整合至 ROS
```
mkdir -p ~/carto_ws/src 
cd ~/carto_ws/src 
git clone https://github.com/ros2/cartographer.git -b ros2
git clone https://github.com/ros2/cartographer_ros.git -b ros2
cd ~/carto_ws 
rosdep install -y -r -q --from-paths src --ignore-src --rosdistro humble
colcon build --symlink-install
```


## 導航 Packages
#### navigation2

- 若記憶體不足，可能在 costmap queue 到 54% 時出現錯誤。
    - 解決方案：等待一分鐘讓系統自動關閉終端機，再重新打開終端機編譯。
- 重新編譯時，可能會在 nav2_waypoint_follower 出現錯誤，這通常是因為中途失敗導致的
    - 解決方案：刪除 build、install、log 資料夾再重新編譯，船到橋頭自然直XD

```
mkdir -p ~/nav2_ws/src 
cd ~/nav2_ws/src 
git clone https://github.com/ros-navigation/navigation2.git -b humble
cd ~/nav2_ws 
rosdep install -y -r -q --from-paths src --ignore-src --rosdistro humble
colcon build --symlink-install
```

- 若編譯失敗時添加以下依賴：
    ```
    sudo apt install ros-humble-bondcpp
    sudo apt install ros-humble-test-msgs
    sudo apt install ros-humble-behaviortree-cpp-v3
    sudo apt install ros-humble-diagnostic-updater
    sudo apt install ros-humble-tf2-sensor-msgs
    sudo apt install ros-humble-ompl
    sudo apt install ros-humble-nav2-map-server
    sudo apt install ros-humble-gazebo-*
    sudo apt install ros-humble-joint-state-publisher
    ```
- 若遇到錯誤 `nav2_msgs/nav2_msgs`：
    ```
    rm -rf /home/scl/nav2_ws/build/nav2_msgs/ament_cmake_python/nav2_msgs/nav2_msgs
    ```
- 下面待確認
    ```
    sudo apt install libboost-all-dev
    ```

## 建圖、導航 launch 參照
#### Turtlebot3 Cartographer & Navigation2
```
cd
git clone https://github.com/ROBOTIS-GIT/turtlebot3.git -b humble-devel
cd turtlebot3/
mv turtlebot3_cartographer/ ~/carto_ws/src/
mv turtlebot3_navigation2/ ~/nav2_ws/src/
cd ..
rm -r turtlebot3/
```


## 小型 Packages
#### rqt-tf-tree
```
sudo apt-get install ros-humble-rqt-tf-tree
```

#### ESP32 連接
```
pip install pyserial
```

#### 鍵盤控制
```
sudo apt-get install ros-humble-teleop-twist-keyboard
```

#### Plot 工具
```
sudo apt install ros-humble-plotjuggler-ros
```

## 延伸閱讀
- [ROS2 hokuyo、velodyne 設定](https://hackmd.io/@ming0071/B1SqyC_eJe)
- [ROS2 micro-ros 安裝與設定](https://hackmd.io/@ming0071/rklD1yKl1x)

[^註腳]:我有用二進位試過了，launch 會開不起來