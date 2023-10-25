# Readme

這個demo在展示如何使用SOLIDWORKS將模型轉換為STL檔，然後透過rviz在ROS中顯示該模型。

![image-20231024135131198](pic/readme/image-20231024135131198.png)

![image-20231024135200722](pic/readme/image-20231024135200722.png)



## 步驟0. 製作3D模型

首先，您需要擁有一個3D模型。您可以使用SOLIDWORKS來設計模型，或者從網上資源下載現有的模型。

這邊提供SW模型素材的網址：[3D Content Central](https://www.3dcontentcentral.tw/)。

此demo直接使用SW的[素材](https://www.3dcontentcentral.tw/download-model.aspx?catalogid=171&id=230555)。





## 步驟1. 將SW零件轉換為STL

您需要將SOLIDWORKS的模型轉換為STL格式，以便在rviz中使用。您可以參考以下YouTube影片：[SOLIDWORKS零件轉換為STL](https://www.youtube.com/watch?v=xmGrbbYM0cY)。

確保在另存新檔時選擇STL格式。



## 步驟2. 放入urdf demo專案

### 第一次使用urdf demo專案

假設已經安裝好[RosTeamWS](https://rtw.stoglrobotics.de/master/index.html)的環境且工作空間的名稱為`ws_ros2`



- 下載urdf_demo專案：

 `ctrl + alt + T` 開啟終端機之後將專案下載下來

```shell
_ws_ros2
rosds
git clone https://github.com/HowardWhile/urdf_demo.git
```

`_ws_ros2` 載入ws_ros2工作空間

`rosds` 進入ROS工作空間的src目錄。

`git clone` 下載專案的版本庫



- 編譯並安裝urdf_demo：

 `ctrl + alt + T` 開啟終端機之後書下以下指令

```shell
_ws_ros2
cb urdf_demo
_ws_ros2
```

`cb urdf_demo` 編譯urdf_demo專案。也可以輸入`cb` 編譯整個工作空間

`_ws_ros2` 要再次載入ws_ros2工作空間的原因是，要讓系統可以識別到 `cb` 指令編譯出來的新檔案。



- 執行urdf_demo專案，顯示STL模型：

`ctrl + alt + T` 開啟終端機之後書下以下指令，可以看到demo的STL被顯示出來

```shell
_ws_ros2
ros2 launch urdf_demo display.launch
```

`ros2 launch` ros2 launch <專案名稱> <啟動文件名稱>，啟動預設的檔案



![image-20231025092547481](pic/readme/image-20231025092547481.png)



### 已經編譯過urdf demo專案

如果您已經編譯過urdf_demo專案，請將新的STL檔案替換`demo_link.STL`，然後再次運行launch指令。

![image-20231025093705513](pic/readme/image-20231025093705513.png)

```shell
ros2 launch urdf_demo display.launch
```

**注意檔案名稱的大小寫** 在linux中`demo_link.STL`與`demo_link.stl`是不同的檔案。



如果真的找不到工作空間的檔案目錄，可以透過指令來打開該專案的資料夾。

```shell
rosds
nautilus ./urdf_demo/
```

`rosds` 進入ROS工作空間的src目錄。

`nautilus` 鸚鵡螺是ubunut預設的檔案管理員。nautilus <資料夾路徑> 使用檔案管理員開啟資料夾。





## 步驟3. 更新慣性參數

使用SOLIDWORKS獲取模型的慣性參數，並將它們填入`demo.urdf`檔案的相應部分：

點選評估->物質特性，另外ROS系統採用KMS製所以記得到這選項中設定

![image-20231025103330037](pic/readme/image-20231025103330037.png)

![image-20231025102509434](pic/readme/image-20231025102509434.png)

將質量填入`<mass>`

將質量中心填入`<origin>`

將慣性矩填入`<inertia>`

![image-20231025104448516](pic/readme/image-20231025104448516.png)







## 步驟4. 視覺化慣性參數

```shell
_ws_ros2
ros2 launch urdf_demo display.launch
```

![image-20231025100159629](pic/readme/image-20231025100159629.png)

展開`RobotModel`和`Mass Properties`，調整透明度Alpha，勾選Mass或Inertial，以觀察慣性參數的視覺化效果。