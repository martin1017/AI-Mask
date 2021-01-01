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

結合上課所學的四個感測器來完成期中專案，四個感測器分別是溫溼度感應器`
1. Go to *File -> Preferences* and in the **Additional Boards Manager URLs**, type
```
http://download.labs.mediatek.com/package_mtk_linkit_7697_index.json
```


動機
------
	現代人常常因為繁忙的生活忘記喝水，
	我自己也是常常忘記喝水，
	然而一般普遍一個人一天要攝取2000ml的水，
	沒喝足夠的水可能會對身體健康造成影響，
	所以我想到做一個自動提醒水壺，
	這樣就可以提醒長時間未喝水的人要喝水。

相關技術
------
	1. D1 MINI ：利用D1 MINI連接到路由器，並讀取水位感測器的數值，再傳送到IFTTT。
	2. IFTTT ：利用WEBHOOK把喝水提醒傳到LINE的聯絡人或是群組。
	3. 水位感測器 ：利用高低不同的水位產生不同電阻值，再傳送到D1 MINI進行分析。


研究方法
------
	電腦連接D1 MIMI，
	水位感測器量測水位，
	把數值回傳到D1 MINI分析計算水位，
	再來透過連接網路使用IFTTT傳送提醒到使用者的LINE。

程式截圖
----------
<img src="https://github.com/martin1017/D1-MINI-PROJECT/blob/master/SCREENSHOT/setup.PNG" alt="setup" width="80%">

<img src="https://github.com/martin1017/D1-MINI-PROJECT/blob/master/SCREENSHOT/loop.PNG" alt="loop" width="%">


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

