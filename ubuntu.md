## Ubuntu 系統重灌

1. [下載 Ubuntu iso 檔](
https://releases.ubuntu.com/?_gl=1*1g08uh3*_gcl_au*MTIwMzE1NzcwMi4xNzI0Mzk1ODA0&_ga=2.61756844.1054857459.1724646097-544692443.1724395801)
 注意，若使用 AMD GPU，建議不要選擇 22.04 版，可能會發生相容性問題導致無法安裝。
 2. 使用 Rufus 工具製作 Ubuntu 開機隨身碟
    - [Rufus 載點](https://rufus.ie/zh_TW/)
    ![image](https://hackmd.io/_uploads/HJcTYE120.png =275x)
3. 安裝 Ubuntu
    - 插入 USB 隨身碟，開啟電腦後進入 BIOS 設定（ASUS 機型通常按 F2 進入），選擇從 USB 開始安裝流程。
    - 安裝時選擇「最小安裝」以降低系統資源需求。


## Ubuntu 一般設定
##### NTUT 802.1X setting
![image](https://hackmd.io/_uploads/H1bCzby3A.png)

#### 安裝 Google Chrome
```
wget -c https://dl.google.com/linux/direct/google-chrome-stable_current_amd64.deb
sudo dpkg -i google-chrome-stable_current_amd64.deb
```

#### 設定中文輸入法
- [系統設定更改](https://hackmd.io/@uoStF0JnQ2KW_h7HNvalFA/ryTtdp9pn#Launcher%E6%87%89%E7%94%A8%E7%A8%8B%E5%BC%8F%E6%B8%85%E5%96%AE%E5%85%A7%E6%89%BESettings)
- 更改目錄名稱為英文：
    ```
    export LANG=en_US
    xdg-user-dirs-gtk-update
    ```

#### 安裝 Git
```
sudo apt install git
```

#### Snap Store 更新
```
sudo snap refresh snap-store
# process error → kill XXXX
```
     
#### 更改預設開啟的應用程式
對檔案右鍵選擇「屬性」在「以此開啟」點擊想要更改的應用程式

#### 外接螢幕無法顯示?
在「設定」→「關於」→「軟體更新」中檢查更新
![image](https://hackmd.io/_uploads/rJVjQb1nC.png)


## 推薦的 Ubuntu 軟體
#### SimpleScreenRecorder
錄影軟體（也可以由 Ubuntu Software 安裝）
```
sudo apt-get install simplescreenrecorder
```
注意：音訊選擇 AAC 格式，便於放入影片至 PPT。
![image](https://hackmd.io/_uploads/BJlNQ-ynC.png)

#### Terminator
可以分割出多個視窗的終端機
```
sudo apt-get install terminator
```
另外，可以設定預設開啟終端機的版面配置，步驟如下列點，亦可參考[詳細教學](https://vocus.cc/article/601cf748fd89780001a1cb2b)
1. 開啟 Terminator 並配置視窗佈局。
2. 終端機右鍵選擇「偏好設定」→「版面配置」→ 新增佈局，將`新配置`命名為 `new`。
3. 進入 Terminator 設定檔，將 `default` 改為 `new`，再將 `new` 改為 `default`。
    ```
      nano ~/.config/terminator/config
    ```
        
#### pgm 繪圖
- krita：可以在[官網下載](https://krita.org/zh-tw/download/)，也可以由 Ubuntu Software 安裝
- kolourpaint
    ```
    sudo snap install kolourpaint
    ```
#### GNOME Tweaks
額外的 GNOME 設定工具

#### VLC
開啟 MP4 影片

#### Arduino
可以從 Ubuntu software 安裝或[官網下載](https://www.arduino.cc/en/software)(才有 2.0 以上的版本)
Arduino Preferences：將以下連結放到 additional boards manager URLs ，以安裝 ESP32 和 OpenCR 的編譯資源
```
https://dl.espressif.com/dl/package_esp32_index.json
https://raw.githubusercontent.com/ROBOTIS-GIT/OpenCR/master/arduino/opencr_release/package_opencr_index.json
```


## 22.04 additional settings
#### AppImage 無法開啟 
```
sudo apt install libfuse2
```
#### AirPods 連接
```
# 去修改藍芽設定
sudo nano /etc/bluetooth/main.conf
# 改方式
ControllerMode=bredr
# 重新啟動藍芽
sudo systemctl restart bluetooth
```

## Next step about ROS2
請參考 [ROS2 humble AMR package setting](https://hackmd.io/@ming0071/SJ61ZNg2R) 進行下一步關於 ROS2 的設定。
