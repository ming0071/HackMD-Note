## What is micro-ros ?
- 基於 Micro XRCE-DDS Client 構建的 package，**使 ROS 可以與資源受限的嵌入式平台上通訊**
- 微控制器可以透過串口、藍牙、乙太網路、Wifi 等多種協定將數據傳遞給Agent，Agent 再轉換成 ROS2 的 Topic 等資訊，以此完成通訊
    ![micro-ros structure](https://hackmd.io/_uploads/SkhbF1FxJx.png =500x)
- 透過 RCLC-API 呼叫 MicroROS，程式碼風格和在上位機中使用 Python  C++ 呼叫 MicroROS 有所不同


## 下位機：micro_ros_arduino
在嵌入式平台安裝 Micro-ROS 需要經過一系列的建立、設定、建造和刷新過程。將 Micro-ROS 整合到 Arduino IDE 中可以達成這些步驟。

#### 設定流程，以 OpenCR 為例：
1. 在 Preferences 中加入 OpenCR 的 Board Manager URL：
    ```
    https://raw.githubusercontent.com/ROBOTIS-GIT/OpenCR/master/arduino/opencr_release/package_opencr_index.json
    ```
2. tool 的 Board 中選擇 OpenCR
3. tool 的 Port 中選擇 COM 端口
4. 在 Sketch 中加入 Micro-ROS Arduino 下載下來的 library zip 檔，這個檔中有範例程式
5. 從 File 的 Examples 中加入 Micro-ROS Arduino 的範例程式並燒錄到 OpenCR


## 上位機：micro-ros-agent
負責在上位機接收 micro ros 的發送資訊
#### install
```
source /opt/ros/foxy/setup.bash
cd ~/ros2_ws/src

git clone -b $ROS_DISTRO https://github.com/micro-ROS/micro-ROS-Agent
git clone -b $ROS_DISTRO https://github.com/micro-ROS/micro_ros_msgs

sudo apt update && rosdep update
rosdep install -y -r -q --from-paths src --ignore-src --rosdistro foxy-devel
sudo apt-get install python3-pip

colcon build
source install/local_setup.bash
```

#### 檢查 package  安裝
```
source install/setup.bash
ros2 run micro_ros_agent micro_ros_agent udp4 --port 8888
```

#### 發布 node
```
source install/setup.bash
ros2 run micro_ros_agent micro_ros_agent serial --dev /dev/ttyACM0
```

#### 監控新視窗
```
source /opt/ros/foxy/setup.bash
ros2 topic list
ros2 topic echo /micro_ros_arduino_node_publisher
```

#### errno: 13. Please re-run with superuser privileges
1. 用 `whoami` 找到使用者名字
2. `sudo usermod -a -G dialout $USER` 把當前的使用者名字加到串口使用者群組，最後電腦重新開機