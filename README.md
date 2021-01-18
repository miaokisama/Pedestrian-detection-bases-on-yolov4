# Pedestrian-detection(bases-on-yolov4)
This set of scripts are using the deep learning framework from [AlexeyAB / darknet](https://github.com/AlexeyAB/darknet). By following the steps, I built myself a pedestraion detection model.
## Demo
### I、Authorization and installation before envrionmental config (needed while useing Colaboratory)
1. Authorization
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
2. Load Google Cloud
```
from google.colab import drive
drive.mount('/content/drive')
```
### II、Environmental configeration
1. Create the Darknet
```
!git clone https://github.com/AlexeyAB/darknet
```
2. Edit Makefile to suit openCV、GPU
```
%cd darknet
!sed -i 's/OPENCV=0/OPENCV=1/' Makefile
!sed -i 's/GPU=0/GPU=1/' Makefile
!sed -i 's/CUDNN=0/CUDNN=1/' Makefile

```
3. Compile the project and generate a excutable file
```
!make
```
### III、Upload the relevant files to the Google Cloud
【list】 [my_img.zip](https://drive.google.com/file/d/13Gc9AhnDPpFtFVGwiYoV-Yht5FWMya5b/view?usp=sharing)、obj.data、obj.names、generate_trian.py、yolov4_obj.cfg 
1. Yolov4 pre-training file[yolov4.conv.137](https://drive.google.com/file/d/1mMFF-Oz_F-3n56ExZx8Fd_35qp5YTvdI/view?usp=sharing)
```
!cp /content/drive/My\ Drive/yolov4/yolov4.conv.137 ./
```
2. Upload a series of edited pedestraion-detection files
```
!cp /content/drive/My\ Drive/yolov4/yolov4_obj.cfg ./cfg
!cp /content/drive/My\ Drive/yolov4/my_obj.names ./data
!cp /content/drive/My\ Drive/yolov4/my_obj.data  ./data
```
3. Upload my image dataset and labeled .txt file. Please refer to https://github.com/AlexeyAB/Yolo_mark for further labeling.
```
!cp /content/drive/My\ Drive/yolov4/my_img.zip ../
!unzip ../my_img.zip -d data/
```
4. Upload training file generate_train.py
```
!cp /content/drive/My\ Drive/yolov4/generate_train.py ./
```
###IV、Training
1. Creat train.txt under the folder of data
```
!python generate_train.py
```
2. Training command
```
!./darknet detector train data/my_obj.data cfg/yolov4_obj.cfg yolov4.conv.137 -dont_show
```
Files generated in the training process will be saved in the path darknet/backip. The image of loss is darknet/chart.png:
	The weight file will be saved automatically after every 1000 iteration. Moreover there are last.weight（current weight, which may continuously change）、final.weight（the final weight after the training）
	If the training is broken by some reasons, the weight chould be reserved.





