# Perdestion-detection(bases-on-yolov4)
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
list: my_img.zip、obj.data、obj.names、generate_trian.py、yolov4_obj.cfg
然后相关文件拷贝到darknet路径下
1. 
2. 
3. 




