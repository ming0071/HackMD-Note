## Hokuyo

#### urg_node2 連接設定

1. Ubuntu 網路設定檔
    - 前面的 hokuyo ip：`192.168.1.11`
    - 後面的 hokuyo ip：`192.168.0.10`
    ![hokoyo back](https://hackmd.io/_uploads/Byfb0mNhR.png  =500x)
    - 使用指令檢查連接: `ping 192.168.1.11` or `ping 192.168.0.10`
2. 設備內容設定
    在 `cfg` 中的`params_ether.yaml`和`params_ether_2nd.yaml`設定 IP、frame ID、掃描角度
3. 啟動檔設定
    在 `launch` 中的`urg_node2_2lidar.launch.py`設定讀取的 yaml、 node 名稱、topic 名稱

#### ira_laser_tools
在`laserscan_multi_merger.launch`修改接收及發布的 topic 名稱和 frame ID

#### rf2o_laser_odometry
在`rf2o_laser_odometry.launch.py`修改laser topic、odom、frame id


## Velodyne
#### IP 設定
由 ubuntu 網路設定做為中間通道連接
- 設備 ip：`192.168.2.81`
- 電腦 ip：`192.168.2.12`

#### 設定
1. 在瀏覽器輸入`192.168.2.81` 前往 LiDAR Web 介面
2. 將 Host IP 設定為 `192.168.2.12`（只需確保第三碼與設備 IP 相同）
3. 點選 `set` 和 `save config` 以儲存修改設定
4. 開 velodyne launch 檔後以 rviz 確定有跟 ROS 連接上

#### ERROR：velodyne poll() timeout

此錯誤通常表示：
1. 網路連接不正確
2. USB、hub 未正確連接
3. 傳輸線異常

若 IP 確認無誤但仍出現此錯誤，可嘗試更換 host IP 的第四碼，通常可以成功連接