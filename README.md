# Pedestrian-detection(bases-on-yolov4)
本脚本集合主要是针对AB大神的的深度学习框架[AlexeyAB / darknet](https://github.com/AlexeyAB/darknet)，根据指引可以通过使用自己标注的数据集来训练完成一个专属于自己的行人检测模型。
## 演示流程
### 一、环境配置前的授权与装载（使用Colaboratory时需要的）
1. 授权
```

!apt-get install -y -qq software-properties-common python-software-properties module-init-tools
!add-apt-repository -y ppa:alessandro-strada/ppa 2>&1 > /dev/null
!apt-get update -qq 2>&1 > /dev/null
!apt-get -y install -qq google-drive-ocamlfuse fuse
from google.colab import auth
auth.authenticate_user()
from oauth2client.client import GoogleCredentials
creds = GoogleCredentials.get_application_default()
import getpass
!google-drive-ocamlfuse -headless -id={creds.client_id} -secret={creds.client_secret} < /dev/null 2>&1 | grep URL
vcode = getpass.getpass()
!echo {vcode} | google-drive-ocamlfuse -headless -id={creds.client_id} -secret={creds.client_secret}
```
2. 装载谷歌云
```
from google.colab import drive
drive.mount('/content/drive')
```
### 二、环境配置
1. 创建Darknet
```
!git clone https://github.com/AlexeyAB/darknet
```
2. 修改make file 将openCV、GPU置为可用
```
%cd darknet
!sed -i 's/OPENCV=0/OPENCV=1/' Makefile
!sed -i 's/GPU=0/GPU=1/' Makefile
!sed -i 's/CUDNN=0/CUDNN=1/' Makefile

```
3. 编译项目生成可执行文件
```
!make
```
### 三、将相关文件上传到谷歌云盘
【list】 [my_img.zip](https://drive.google.com/file/d/13Gc9AhnDPpFtFVGwiYoV-Yht5FWMya5b/view?usp=sharing)、obj.data、obj.names、generate_trian.py、yolov4_obj.cfg 
1. yolov4预训练文件[yolov4.conv.137](https://drive.google.com/file/d/1mMFF-Oz_F-3n56ExZx8Fd_35qp5YTvdI/view?usp=sharing)
```
!cp /content/drive/My\ Drive/yolov4/yolov4.conv.137 ./
```
2. 上传经过修改的用于行人检测的系列文件
```
!cp /content/drive/My\ Drive/yolov4/yolov4_obj.cfg ./cfg
!cp /content/drive/My\ Drive/yolov4/my_obj.names ./data
!cp /content/drive/My\ Drive/yolov4/my_obj.data  ./data
```
3. 上传自己的数据集图片及标注的txt文件压缩包并解压。若想标注自己的数据集可参照https://github.com/AlexeyAB/Yolo_mark
```
!cp /content/drive/My\ Drive/yolov4/my_img.zip ../
!unzip ../my_img.zip -d data/
```
4. 上传训练文件generate_train.py
```
!cp /content/drive/My\ Drive/yolov4/generate_train.py ./
```
###四、训练
1. data文件夹下生成train.txt
```
!python generate_train.py
```
2. 训练指令
```
!./darknet detector train data/my_obj.data cfg/yolov4_obj.cfg yolov4.conv.137 -dont_show
```
训练过程中产生的权重文件将保存到darknet/backup下，loss图保存为darknet/chart.png:
    每迭代1000次会自动保存一次权重文件，除次还有last.weight（当前训练的权重，会不断更新）、final.weight（训练结束后的最后权重）
    如果训练因为某些原因中断，下次训练时可从以上权重权重开开始




