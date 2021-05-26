# 新拖延運動黑客松

## 介紹

網站連結：[GitHub Page](https://lidemy.github.io/lazy-hackathon/)

![visual](https://i.imgur.com/sb1m6XB.png)

此網站是 [程式導師實驗計畫第三期](https://github.com/Lidemy/mentor-program-3rd) 第二十週的作業，如果你覺得網站很卡很肥大，沒錯你是對的，因為我們故意把這個網站做得這麼卡。

現在呢，你要來負責最佳化這個網站，在「不動內容」的情形下來調整，意思就是說網頁看起來要「長得一模一樣」，把圖片變黑白、刪減文字或是更動排版都是不允許的，但刪減多餘的 HTML、CSS 和 JS 是 ok 的，只要保證網頁看起來一樣就行了，原始碼怎麼動隨便你，總之目標是使網站的載入速度變快。

這只是一個靜態網站，你可以 fork 到自己的 GitHub 去然後用 GitHub Page 來部署，也可以部署到自己的 Server 去並且額外做一些伺服器端的最佳化。

部署上去之後可以使用 [WebPageTest](https://www.webpagetest.org/) 這個網站來測試你的網站速度為何。（測試地點請統一選擇 Singapore - EC2），Browser 為 Chrome。

完成以後可以在這個 repo 開一個 [Issue](https://github.com/Lidemy/lazy-hackathon/issues/new/choose) 來記錄自己的結果（[範例](https://github.com/Lidemy/lazy-hackathon/issues/1)）。也可以在 Issues 裡面看到其他人的報告，看看別人怎麼做的，順便偷學個幾招。

若是你沒有任何靈感，可參考 [web.dev](https://web.dev/) 或是 [Website Performance Optimization](https://www.udacity.com/course/website-performance-optimization--ud884)

## 關於

此專案由 [@yakim-shu](https://github.com/yakim-shu) 負責製作，十分感謝。

## 網址
~~[lazy-hackathon]~~ ~~(http://sio2.tw/lazy/)~~
我沒有部屬在 GitHub Page ，而是部屬在之前買的 Domain 下，所以 Pagespeed 常常因為伺服器回應時間 (TTFB) 分數飄忽不定。


## WebPageTest 報告網址
[Web Page Performance Test for
sio2.tw/lazy/](https://www.webpagetest.org/result/191113_1Q_81dbefe72ed7b804591b190d7be8a044/)


## 測速截圖

### pagespeed 還未修改前

![手機板](https://user-images.githubusercontent.com/49493665/119597644-0e17db80-be14-11eb-95c3-856d22747378.png)
![電腦版](https://user-images.githubusercontent.com/49493665/119597563-e3c61e00-be13-11eb-83eb-f4b7f6c84628.png)

### Pagespeed 優化後 
![image](https://user-images.githubusercontent.com/49493665/68730178-e6349e80-0606-11ea-9e25-dfc4feac16d9.png)
![image](https://user-images.githubusercontent.com/49493665/68730237-10865c00-0607-11ea-90ae-cf7ccc927474.png)

WebPageTest 結果
![image](https://user-images.githubusercontent.com/49493665/68730106-bb4a4a80-0606-11ea-9247-84a8a29b44f6.png)





## 你做了哪些優化？
先寫字型的優化，其他檔案類型的優化以後補，因為優化搞最久的是減少中文字型檔的體積。
### 結果比較
#### 優化前
根據 Pagespeed 的計算，字型發出 22 個 requests 、 887 KB
![image](https://user-images.githubusercontent.com/49493665/68730370-7ffc4b80-0607-11ea-9ae3-12052f3d4a4d.png)
#### 優化後
5 個 Requests 、 90 KB。其中一個是 slick 用的圖案
![image](https://user-images.githubusercontent.com/49493665/68730649-4d9f1e00-0608-11ea-92e7-1b3c5bf7d609.png)

#### 優化過程
雖然 `<link href="https://fonts.googleapis.com/css?family=Arvo|Noto+Sans+TC&amp;display=swap" rel="stylesheet">` 看起來只有發出一個 Request ，但因為 Google 會幫忙把一個字型檔拆成上百的小檔，然後只下載有用到文字的檔案。所以 PageSpeed 才會寫字型有 22 個 Requests。

先觀察網頁上用了哪些字體和使用的 `font-weight`， 中文 `Noto Sans Tc` 使用了 `400` `500` `700` 的 `font-weight` ， 和使用英文 `Arvo`。

-  Google Fonts 客製化 => 失敗
客製化 `Noto Sans TC`，使用 `<link href="https://fonts.googleapis.com/css?family=Arvo|Noto+Sans+TC:400,500,700&display=swap" rel="stylesheet">` 取代原本的連結，Requests 反而增加到 44 個。
- 字型編輯軟體 : 沒有過濾特定字元功能 => 放棄
- 雲端字體服務 : 可依照有用到的字生成檔案 => $$
- Font-spider => 成功
`font-spider` 可以爬取 html 和 css 找出有用到的字，並刪除字型檔中沒有用到的字，以減少字型檔體積。必須要提供 `ttf` 格式，`font-spider`才能作用。
##### 大魔王 : 轉檔
1. 轉成 `ttf` 
  - 因為 Google Fonts 只有提供 `otf` 格式，所以我一開始是找線上轉檔的網頁轉成 `ttf`。壓縮完字型檔後發現頁面還是沒有渲染到中文字體。找了很久才發現，轉檔事實上是失敗的，原本有 2 MB 轉完只剩 7 KB，檔案也不能用 `Windows 字型檢視器` 打開。 線上轉字型檔的網頁好像是給英文字用的，中文的檔案太大轉檔會失敗。
  - 網路上找不到直接是 `ttf` 的檔案，`oft` 試了好幾個網頁也轉不成功。幸好有找到 `woff2` 類型的檔案。使用[Convert image files online](https://www.aconvert.com/image/woff2-to-ttf/)轉檔完可以用`Windows 字型檢視器`打開。
2. 外框模式
  - 你以為轉檔完了嗎 ? 還有陷阱喔 ～
      進行完  `font-spider` 命令發現大部分字型檔大小沒有改變，只有一個字型檔成功。用 `Windows 字型檢視器`打開，比較成功和失敗的檔案有甚麼不一樣。發現唯一的不同是字型外框 :
      為 `TrueType` 外框能成功 ， `PosstScript` 外框失敗。成功的那個檔案是因為我曾經用字型編輯軟體打開過刪掉幾個字後存儲，應該這樣外框模式才被改變。
    ![image](https://user-images.githubusercontent.com/49493665/68732821-c05fc780-060f-11ea-9539-4e15b26ccf0a.png)
p.s. `font-spider` 不會根據 `font-weight` 分類字，只會找符合 `font-family` 的字
#### 壓縮字型
ttf 轉成 woff 檔可減少快 20% 大小，轉成 woff2 可減少 40%。但 `woff2` 不是 Apache 允許的 `MIME Type` 所以瀏覽器拿不到 `woff2` 檔案的 Request。不過修改完設定檔還是沒成功。

## 心得
- 中文字實在太麻煩了，而且這方法只能用在靜態網站。寫心得時才發現 Noto Sans 原來是系統內建的字體，所以完全不用浪費時間處理字體QwQ 不過使用 local 端的 Noto Sans 字體粗細和原本的不一樣。
- slick 使用 lazyload 後，在 Chrome 全螢幕時下方的大圖在滑到時不會觸發 IntersectionObserver 中的 Callback(可見性變化時會觸發的函數)，但在手機板、 Firefox 和 Edge 都正常。觀察後發現全螢幕時在畫面剛 Render 出來時大圖就會觸發 Callback，可見性變為 0，真是奇怪。所以我自己寫 lazyload，遇到 slick 縮圖時就順便下載大圖。試著刪 slick.js 發現 slick 用內建 lazyload 下次可以試試。 
p.s. 寫心得時看大圖的 lazyload 全螢幕也正常了，難道是看我電腦或網路的效能嗎
- Boostrap 客製化選項就算全勾 navbar 的按鈕功能都會壞掉...
- Boostrap.js 我直接刪 Devtool Coverage 顯示出沒用到的功能 : modal、tooltip、scrollspy、Tab Toast 後網頁正常運作，不過 `console` 會跳出一個 Error 。把 boostrap.js 和其他程式碼打包在一起程式運作時遇到 error 就停止，所以只好分開放。
- 為了修改 Apache 的設定檔還把整個 Apache 搞壞，連 ssh 都連不進，只好刪掉 aws 的主機重頭部屬。
