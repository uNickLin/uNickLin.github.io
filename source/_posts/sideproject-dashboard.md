---
title: 修煉前端 Week 3 - Dashboard
visible: true
date: 2018-06-20 15:52:30
tags:
- Vue 2.x
categories:
- Side-projects
---

這次第三週的題目是後台的 dashboard 介面，一直以來看到的 demo 都是一大堆的圖表數字看起來很厲害，但自己真正做起來其實很費勁 ... 尤其是圖表的部分，每一家的 api 都不盡相同，更別說會有什麼奇葩客戶要一些常人不會有的功能，所以這次只做個簡單的 overview 頁面，順便玩玩幾個有趣的 plugin

<!--more-->

![](https://i.imgur.com/2s23645.png)

<font style='font-size: 30px; color: red'>假的</font>

... 這只是理想，實際上只有[這樣](http://vdashboard.s3-website-ap-northeast-1.amazonaws.com/) 😅
![](https://i.imgur.com/Kq4zlLG.png)

其實頁面應該還包含商品列表、上下架的功能，但因為
1. 列表部分只是 v-for 操作
2. 沒有實際 api 做起來很沒 fu
3. 同時間有更迫切的事要學：Typescript 🤬
4. <font style='text-decoration: line-through; font-size: 8px; color: grey'>我懶的做</font>

可是它的質量還是很高的 🤗

趁這機會可以用自己收藏一堆的套件，像是[Odometer](http://github.hubspot.com/odometer/docs/welcome/)就跑得很喜洋洋，只可惜因為上方三個數據是隨機產生的，負數的部分沒辦法在一開始就顯示出來

圖表部分可就有點淵源了，說起從去年踏入公司那段期間有在自習研究 [D3.js](https://d3js.org/)，費了三焦耳之力，好不容易生出個 bar chart 和 donut chart，而且還是最陽春最不彈性的那種，才發現 RWD 又要在鑽進去那一堆 math hell 改 😱 
... 然後就沒有然後了，卯起來找個一大堆圖表套件備用，既然前人都寫妥了，就別再費勁了吧 🍵 而且市場上的圖表款式比想像的還多，暫時應該是不會有需要自己跳下去的情況

拉回正題，這次使用的是 [amcharts](https://www.amcharts.com/v4/)，原因是他的趨勢圖可以用滑鼠拉伸區間顯示有吸引到我，之後有機會再嘗試他們家的其他圖表！

接著下方左邊是用 random math 跑出來的就不贅述，主要是右方社團同好提供的 [Faker.js](https://github.com/marak/Faker.js/)，有喚起在 Rails 用的 faker 美好回憶，但相較於後端直接建立在 schema 裡面的方便性還是有點落差，之後也有找到一款幫你製作假 json 的 [Mock.js](http://mockjs.com/)，專案上應該可以方便許多！

總結這週題目因為種種因素沒有投入太多心力，花了大部分的時間在了解 Typescript 的規則，更別說還要讓他在 Vue 裡面暢行無阻 🤯 但想想自己確實也該重拾一下 OO 的概念，離開 ROR 之後在 JS 過的太無拘無束了，筆記做完再找個時間統整一下






----------------
P.S. 我的圖表庫

* [plotdb](https://plotdb.com/): 感覺是集合圖表作品的庫，是說他上面的 beta 已經掛蠻久了 ... 應該是已棄置，但如果需要還是可以取用，他提供線上操作
* [Chart.js](http://www.chartjs.org/): 用戶應該蠻廣的，介面簡單清晰，平常沒事都用這款
* [AnyChart](https://www.anychart.com/): 整體既視感跟 Chartjs 很像，但免費仔<font style='text-decoration: line-through; font-size: 8px; color: grey'>我</font>會有 brand logo 印在上面，好孩子千萬不要用 css 去把人家消掉啊！
* [amcharts](https://www.amcharts.com/v4/): 本週主角光環，已經升級到第四版了！
* [Britecharts](https://eventbrite.github.io/britecharts/index.html): 有一個專案使用到它的 donut chart 感覺還不錯，是用 d3 最新版(v5)繪製的
* [ECharts](http://echarts.baidu.com/index.html): 看過史上最強大的圖庫沒有之一，而且有中文介面、開源、支援層面廣！



下面幾個就比較沒深入了，反正看到先存一波(有些好像要付費)
* [hightcharts](https://www.highcharts.com/)
* [canvasJS](https://canvasjs.com/)
* [visjs](http://visjs.org/#)
* [AntV](https://antv.alipay.com/zh-cn/g2/3.x/index.html)
* [高德開放平台](http://lbs.amap.com/)(阿里爸爸的)