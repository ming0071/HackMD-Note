## 建立一個 package
用指令直接創造出一個包，不要自己生成文件，會編到瘋掉...
可以選用 ament_cmake 或 ament_python 來編譯，package name 是 scl_amr
```
ros2 pkg create --build-type ament_cmake --license Apache-2.0 scl_amr
```
以 ament_cmake 來編譯的話需要對 CMakeLists.txt 修改，目的是讓自己另外新增的資料夾也會編譯到 install 內，如 launch、config、urdf
```cmake
cmake_minimum_required(VERSION 3.8)
project(scl_amr)

if(CMAKE_COMPILER_IS_GNUCXX OR CMAKE_CXX_COMPILER_ID MATCHES "Clang")
  add_compile_options(-Wall -Wextra -Wpedantic)
endif()

# find dependencies
find_package(ament_cmake REQUIRED)
# uncomment the following section in order to fill in
# further dependencies manually.
# find_package(<dependency> REQUIRED)

install(DIRECTORY launch/
  DESTINATION share/${PROJECT_NAME}/launch
)

install(DIRECTORY config/
  DESTINATION share/${PROJECT_NAME}/config
)

install(DIRECTORY urdf/
  DESTINATION share/${PROJECT_NAME}/urdf
)

ament_package()
```


## 撰寫 launch 檔
有 3 種方法可以呼叫 launch 檔，分別是python、xml、yaml
基本上用 xml、yaml 可以作到多數的事，比較簡單也比較方便，如果需要更深入的操作再使用 python 就好，以下都提供簡單範例
### python
```python
import os

from launch import LaunchDescription
from launch.actions.declare_launch_argument import DeclareLaunchArgument
from launch.actions.include_launch_description import IncludeLaunchDescription
from launch.launch_description_sources import PythonLaunchDescriptionSource
from ament_index_python import get_package_share_directory
from launch_ros.actions import Node


def generate_launch_description():

    # Include launch file
    tf_launch = IncludeLaunchDescription(
        PythonLaunchDescriptionSource(
            os.path.join(
                get_package_share_directory("scl_amr"),
                "launch",
                "AMR_model.launch.py",
            ),
        )
    )
    hokuyo_launch = IncludeLaunchDescription(
        PythonLaunchDescriptionSource(
            os.path.join(
                get_package_share_directory("urg_node2"),
                "launch",
                "urg_node2_2lidar.launch.py",
            ),
        )
    )
    merged_launch = IncludeLaunchDescription(
        os.path.join(
            get_package_share_directory("ira_laser_tools"),
            "launch",
            "laserscan_multi_merger.launch",
        ),
    )

    ld = LaunchDescription()
    ld.add_action(tf_launch)
    ld.add_action(hokuyo_launch)
    ld.add_action(merged_launch)

    return ld
```


### xml
```xml
<launch>
    <include file="$(find-pkg-share scl_amr)/launch/AMR_model.launch.py" />
    <include file="$(find-pkg-share urg_node2)/launch/urg_node2_2lidar.launch.py" />
    <include file="$(find-pkg-share ira_laser_tools)/launch/laserscan_multi_merger.launch" />
</launch>
```
### yaml
```yaml
launch:
  - include:
      file: $(find-pkg-share scl_amr)/launch/AMR_model.launch.py

  - include:
      file: $(find-pkg-share urg_node2)/launch/urg_node2_2lidar.launch.py

  - include:
      file: $(find-pkg-share ira_laser_tools)/launch/laserscan_multi_merger.launch
```


## 參考
- [ROS2 - Include a Launch File in Another Launch File (Python, XML, YAML)
](https://youtu.be/sl0exwcg3o8?si=6EB7v4apn-7nIVsB)
- [Managing large projects](https://docs.ros.org/en/humble/Tutorials/Intermediate/Launch/Using-ROS2-Launch-For-Large-Projects.html)
- [[ROS2 Foxy] Passing argument from one launch file to another](https://answers.ros.org/question/387635/ros2-foxy-passing-argument-from-one-launch-file-to-another/)
