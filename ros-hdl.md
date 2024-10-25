## Install ( noetic ) 

#### hdl_graph_slam
在 hdl_graph_slam 的 github 下載 [package](https://github.com/koide3/hdl_graph_slam) 到自己的電腦上
```
sudo apt-get install ros-noetic-geodesy ros-noetic-pcl-ros ros-noetic-nmea-msgs ros-noetic-libg2o

cd ~/catkin_ws/src
git clone https://github.com/koide3/ndt_omp.git
git clone https://github.com/SMRT-AIST/fast_gicp.git --recursive
git clone https://github.com/koide3/hdl_graph_slam

cd .. && catkin_make -DCMAKE_BUILD_TYPE=Release
```

#### pcd2pgm
```
cd ~/catkin_ws/src
git clone https://github.com/Hinson-A/pcd2pgm_package.git

cd ..
catkin_make
```

## Record bag file
一個終端機開 ros 啟動指令
```
roscore
```
另一個終端機開出 4 個分頁，目的是錄製 bag 檔供後續 SLAM 使用
#### imu
```
cd ~/catkin_ws
source devel/setup.bash
roslaunch imu_launch imu_msg.launch 
```
#### 機電系統(建圖)
```
cd ~/catkin_ws
source devel/setup.bash
roslaunch scl_agv hdl_3d_slam.launch
```

#### record bag
```
cd ~/bagfile
rosbag record -a
```

#### keyboard control
```
roslaunch turtlebot_teleop keyboard_teleop.launch 
```


>[!note]Record Bag File Note
>- **確定 sensor 連接成功，==網路 hub 順序上到下 11、10、velodyne==**
>- RVIZ 沒有地圖出現是正常的，因為沒有給地圖資訊 
>- 要**錄順時鐘跟逆時鐘共兩圈**，可以靠左或靠右一些，反過來時再交換。因為車子離牆壁近一點特徵可以抓得更清楚
>- 在**中間車子、空間狹窄**的地方可以多繞一些，抓取多一些資訊、特徵
>- 控制變因，錄不一樣檔案的時候差異不要太大，方便和其他人比較成果
>- 檔案錄完之後傳到隨身碟卡比較久是正常的，可以等個10、20分鐘再看看。

## Offline run HDL SLAM (501 launch)
注意要等到 bag 播放完畢後，再執行後續操作
#### Offline SLAM
```
cd ~/catkin_ws
source devel/setup.bash
rosparam set use_sim_time true
roslaunch hdl_graph_slam hdl_graph_slam_501-2.launch
```

#### 播放 bag
必須包含 `--clock`，表示包播放時同時 publish 時間，以便和前面的 use_sim_time 搭配
```
cd ~/bagfile
rosbag play --clock yours.bag
```

#### 存 pcd
注意換行是必要的，還有路徑要打 `home/xxx/`，不能用 `~` 取代 
```
rosservice call /hdl_graph_slam/save_map "resolution: 0.05
destination: '/home/scl/bagfile/yours.pcd'"
```

#### pcd 轉 pgm
到這一步時需要把 roscore 重新打開再執行
注意要改 run.launch 內的檔名，還有 **pcd 的命名不能純數字**
```
roscd pcd2pgm/launch
roslaunch run.launch

cd ~/bagfile
rosrun map_server map_saver -f yours
```
存完圖之後需要再對 pgm 修圖，把雜訊塗白清乾淨就好了
修圖軟體可以用 krita or kolourpaint，在[Ubuntu 系統重灌](https://hackmd.io/@ming0071/r1ue7YlsR?view#pgm-繪圖)有下載方式

## Replace pcd、pbm file

- pcd
    - 檔名更改在 `hdl_localization.launch`
    - launch 檔在 `hdl_ws/src/hdl_localization/launch/..`
    - 檔案位置放到 `hdl_ws/src/hdl_localization/data/..`
- yaml，注意內文要調整
    - 檔名更改在 `hdl_3d_localization.launch `
    - launch 檔在 `src/scl_agv/launch/..`
    - 檔案位置放到 `src/scl_agv/map/..`
- pgm
    - 檔案位置放到 `home/ubt/bagfile/ming1207-2.pgm`

## Navigation

#### HMI
![HMI](https://hackmd.io/_uploads/rkFabsygkg.png)

#### Command

1. 機電系統(導航)，已寫入 imu launch
    ```
    cd home/ubt/AGV/scl_agv/launch
    roslaunch scl_agv hdl_3d_localization.launch 
    ```
2. 定位
    ```
    cd home/ubt/hdl_ws
    source devel/setup.bash
    roslaunch hdl_localization hdl_localization.launch
    ```

>[!note]Navigation Note
>- **確定 sensor 連接成功，==網路 hub 順序上到下 11、10、velodyne==**
>- **確定 hokuyo 位置是否正確**
>- 初始位置要跟建圖時的開始的差不多
>- 定位之後要用 rviz 裡的 2d pose estimate 給初始位置，調整車頭方向，接著再用 2d nav goal 給定目標點
>- 使用 hdl_localization 時要先提供地圖資訊。因此需要先使用hdl_graph_slam 建立地圖，再用建立完成的地圖來啟動 hdl_localization

## Other

1. IPC password：`123456`
2. 關機時確定 IPC 燈熄滅再關逆變器(左圖)
    ![power](https://hackmd.io/_uploads/SJixoOmeJg.png)
3. Rviz 右邊中間有小箭頭可以點開來，看到選單中會有選項可以看地圖要顯示 2D or 3D 
