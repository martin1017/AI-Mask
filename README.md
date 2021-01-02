人工智慧期末專案
===========================

口罩實時辨識

****
	
|姓名|林致楷|
|---|---
|學號|06363642

|姓名|黃郁凱|
|---|---
|學號|06363660


****
## 目錄
* [目的](#目的)
* [動機](#動機)
* [相關技術](#相關技術)
* [研究方法](#研究方法)
* [程式解說](#程式解說) 
* [執行結果](#執行結果)
* [專案心得](#專案心得)

## 目的

利用上課所學根所知道的深度學習框架來完成期末專案，
並實作出一個可以使用的口罩實時辨識系統。


## 動機
武漢肺炎的肆虐造成許多遺憾，
而為了防患於未然，
平時必須養成自動戴口罩的習慣，
但有些人卻因為懶惰不帶而造成他人的困擾，
而為了徹底落實，
我們構思了這款自動偵測是否戴上口罩的系統。


## 相關技術
	1. YOLO V3 ：運用YOLOV3類神經網路演算法來對物件進行偵測，
	             以Darknet實作。
	
	2. OPEN CV ：OpenCV是由英特爾公司發起並參與開發，
	             以BSD授權條款授權發行，
		     可以在商業和研究領域中免費使用。
		     OpenCV可用於開發即時的圖像處理、
		     電腦視覺以及圖型識別程式。
		     這邊利用OPENCV API來讀取照片和對照片進行標記，
		     並做到實時的監測。
	
	3. DARKNET ：Darknet是YOLO作者(Joseph Redmon)自己寫出來的deep learning framework，
	             可以想像darknet是另一個tensorflow，
		     但功能沒這麼強大，
		     主要是用來實現YOLO系列的算法，
		     但也是可以用來實現其它算法


研究方法
------
<img src="https://github.com/martin1017/AI-Mask/blob/main/Screenshot/setup.png" alt="setup" width="80%">

程式解說
----------
首先們需要先利用Anaconda Prompt來安裝OpenCV套件，可以輸入下面程式碼來安裝
```
conda install -c conda-forge opencv
```

這段程式碼使用OpenCV讀取YoloV3訓練出來的模型，首先我們需要先引用OpenCV和Numpy，再來利用opencv呼叫出訓練好的模型和參數檔並變成一個Network
```
import cv2
import numpy as np
net = cv2.dnn.readNetFromDarknet("yolov3.cfg","yolov3_mask.weights")
```

這裡把net每個層的名稱都放到Layer_names這個變數裡面，然後把卷積神經網路(CNN)裡面各個卷積層的名稱列出來
```
layer_names = net.getLayerNames()
print(layer_names)
```

透過YoloV3來計算產生3種不同尺寸的輸出，存到output_layers並顯示出來
```
output_layers = [layer_names[i[0] - 1] for i in net.getUnconnectedOutLayers()]
output_layers
```

讀取obj.names裡面的參數，並設定三種戴口罩形式的類別顏色，分別是不戴口罩的會使用藍色標記，沒戴好口罩會是紅色標記，有戴好口罩標記成綠色
```
classes = [line.strip() for line in open("obj.names")]
colors = [(0,0,255),(255,0,0),(0,255,0)]
```

讀取我們的測試圖片並顯示出來
```
from PIL import Image
Image.open('test.jpg')
```

利用cv2.imread把測試照片讀取進來並存成numpy形式的數學矩陣，印出矩陣大小和顏色形式
```
img = cv2.imread("test.jpg")
img.shape
```

利用OpenCV來對測試照片進行縮放和標準化，這裡設定中等速度和精準度來辨識口罩(YoloV3有三檔辨識速度，分別是320、416、608三檔)，
再來切換色彩排序，因為測試用圖片是RGB色階，但是在辨識過程需要BGR排列的圖片，所以我們要交換色彩排序
```
img = cv2.resize(img, None, fx=0.4, fy=0.4)
height, width, channels = img.shape 
blob = cv2.dnn.blobFromImage(img, 1/255.0, (416, 416), (0, 0, 0), True, crop=False)
net.setInput(blob)
outs = net.forward(output_layers)
```

將產生輸出存到Output後顯示出來大小
```
len(outs)
```

顯示輸出大小
```
outs[0].shape
```

這段是把辨識出來的物件框選出來，使用np.argmax取出擁有最大值的類別，利用已經設定好的篩選條件辨識需要的口罩物件
```
class_ids = []
confidences = []
boxes = []
    
for out in outs:
    for detection in out:
        tx, ty, tw, th, confidence = detection[0:5]
        scores = detection[5:]
        class_id = np.argmax(scores)  
        if confidence > 0.3:   
            center_x = int(tx * width)
            center_y = int(ty * height)
            w = int(tw * width)
            h = int(th * height)
            x = int(center_x - w / 2)
            y = int(center_y - h / 2)
            boxes.append([x, y, w, h])
            confidences.append(float(confidence))
            class_ids.append(class_id)
```

顯示偵測到的物件有幾個
```
len(boxes)
```

篩選信心度低於閥值的物件，提高準確度
```
indexes = cv2.dnn.NMSBoxes(boxes, confidences, 0.3, 0.4)
```

利用OpenCV的繪製線條功能把篩選出來的物件畫上對應顏色的框
```
font = cv2.FONT_HERSHEY_PLAIN
for i in range(len(boxes)):
    if i in indexes:
        x, y, w, h = boxes[i]
        label = str(class_ids[i])
        color = colors[class_ids[i]]
        cv2.rectangle(img, (x, y), (x + w, y + h), color, 1)
```

最終辨識結果
```
%pylab inline
from matplotlib import pyplot as plt
plt.rcParams['figure.figsize'] = [15, 10]
img_rgb = cv2.cvtColor(img, cv2.COLOR_BGR2RGB)
plt.imshow(img_rgb)
```

執行結果
--------
這邊我們需要把上面程式分段解說的各程式碼整合起來丟到OpenCV裡面去做到實時口罩辨識
```
import cv2
import numpy as np
net = cv2.dnn.readNetFromDarknet("yolov3.cfg","yolov3_mask.weights")
layer_names = net.getLayerNames()
output_layers = [layer_names[i[0] - 1] for i in net.getUnconnectedOutLayers()]
classes = [line.strip() for line in open("obj.names")]
colors = [(0,0,255),(255,0,0),(0,255,0)]

def yolo_detect(frame):
    img = cv2.resize(frame, None, fx=0.4, fy=0.4)
    height, width, channels = img.shape 
    blob = cv2.dnn.blobFromImage(img, 1/255.0, (416, 416), (0, 0, 0), True, crop=False)
    net.setInput(blob)
    outs = net.forward(output_layers)

    class_ids = []
    confidences = []
    boxes = []
    
    for out in outs:
        for detection in out:
            tx, ty, tw, th, confidence = detection[0:5]
            scores = detection[5:]
            class_id = np.argmax(scores)  
            if confidence > 0.3:   
                center_x = int(tx * width)
                center_y = int(ty * height)
                w = int(tw * width)
                h = int(th * height)
                x = int(center_x - w / 2)
                y = int(center_y - h / 2)
                boxes.append([x, y, w, h])
                confidences.append(float(confidence))
                class_ids.append(class_id)
                
    indexes = cv2.dnn.NMSBoxes(boxes, confidences, 0.3, 0.4)
    font = cv2.FONT_HERSHEY_PLAIN
    for i in range(len(boxes)):
        if i in indexes:
            x, y, w, h = boxes[i]
            label = str(class_ids[i])
            color = colors[class_ids[i]]
            cv2.rectangle(img, (x, y), (x + w, y + h), color, 1)
    return img
```

```
import cv2
import imutils
import time

VIDEO_IN = cv2.VideoCapture(0)

while True:
    hasFrame, frame = VIDEO_IN.read()
    img = yolo_detect(frame)
    cv2.imshow("Frame", imutils.resize(img, width=850))
    if cv2.waitKey(1) & 0xFF == ord('q'):
        break
        
VIDEO_IN.release()
cv2.destroyAllWindows()
```

## 專案心得
	在製作過程中，
	從一開始收集資料到最後將程式寫出，
	中間遇到了很多問題，
	但最後我們還是完成了，
	也希望疫情趕快度過，
	重新呼吸到新鮮空氣。	

--------------------------------

