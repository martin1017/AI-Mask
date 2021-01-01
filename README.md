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
* [程式截圖](#程式截圖) 
* [執行結果](#執行結果)
* [專案心得](#專案心得)

## 目的

利用上課所學根所知道的深度學習框架來完成期末專案，
並實作出一個可以使用的口罩實時辨識系統。


動機
------
	武漢肺炎的肆虐，
	造成許多遺憾，
	而為了防患於未然，
	平時必須養成自動戴口罩的習慣，
	但有些人卻因為懶惰不帶而造成他人的困擾，
	而為了徹底落實，
	我們構思了這款自動偵測是否戴上口罩的系統。


相關技術
------
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

執行結果
--------
	自動提醒水壺實拍	
<img src="https://github.com/martin1017/D1-MINI-PROJECT/blob/master/SCREENSHOT/1.jpg" alt="loop" width="50%">

	提醒傳送到LINE群組
<img src="https://github.com/martin1017/D1-MINI-PROJECT/blob/master/SCREENSHOT/2.jpg" alt="loop" width="50%">

	水位低於250ml以下會提醒用戶裝水，
	並顯示目前水位，30秒提醒一次。


專案心得
----------
	這次期末專案一開始想不到題目會比較有趣，後來被女友唸都不喝水，
	所以就想到做一個自動提醒水壺，之後就馬上添購零件開始製作，
	製作過程中有遇到很多問題，像是電阻值會跳來跳去，
	造成水位感測不準，後來實際量測電阻值才比較穩定，
	果然有自己嘗試過才知道問題所在。
	在這次專案中，學到很多實作方面的知識，受益良多。	

--------------------------------

