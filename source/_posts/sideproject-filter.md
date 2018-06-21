---
title: 修煉前端 Week 2 - Filter 之 Youbike
visible: true
date: 2018-06-14 14:33:47
tags:
- Vue 2.x
- Vuex
categories:
- Side-projects
---

很快的前端試煉來到了第二週：filter，試著做出有篩選功能的網頁，但第一週其實還有很多想繼續鑽研的地方，像是還沒解決的 PWA 離線緩存、手機上的 tap 選取問題等等，不過整體而言一整個禮拜都很充實呢！有個目標可以達成，沒有設計在旁邊嘰嘰喳喳，完全就是打造一個自己想要的作品<small>（晚上都捨不得打電動了）</small>

<!--more-->

第二週社團給的 ref 大概就是一個旅遊介紹的介面，左邊篩選欄位，最上方有全站搜尋的 input，再來就是主畫面的清單呈現跟內頁更詳細內容<small>(因為題目方向是「篩選」所以這次就不貼了因為完全不一樣)</small>

說到開放 api 第一個就是想到台北市政府<small style='text-decoration: line-through; color: grey'>(結構很爛)</small>的[開放平台](http://data.taipei/)，找來找去沒有一個爭氣的能用，就當要往國外網站找時，突然閃了一下 youbike 這東西，雖然說到底我自己應該也不會想用這個功能，但 ... 練習嘛，功能總是有人會需要的 🤨

就大概花了個時間，洗了個澡，換了套衣服，生出一個陽春的 wireframe
![](https://i.imgur.com/KykJtVH.jpg)

然後呢再跟 CSS 交戰數百回後，介面總算是好了
<small style='text-decoration: line-through; color: grey'>切版比寫功能更要命🙄</small>
![](https://i.imgur.com/rW12YJ6.png)
<div class='mobile_images'>
	<img src='https://i.imgur.com/5lqA4LA.png'>
	<img src='https://i.imgur.com/vAdjO1D.png'>
</div>

接下來就進入正題了，下面來分享這一路的波折，一開始遇到最頭痛的問題是 ...
<strong style="font-size: 25px; color: red">API !!!</strong>
<strong style="font-size: 30px; color: red">API !!!</strong>
<strong style="font-size: 35px; color: red">API !!!</strong>

他的 url 竟然是 https://tcgbusfs.blob.core.windows.net/blobyoubike/YouBikeTP.gz 一個壓縮檔，讓我一開始一直以為是下載來自己用，但想想也不對，我們的偉哉政府有辦法幫我下載的檔案做更新？！二話不說我直接丟進 postman 一探揪竟，果然是要連這個網址而且還不能用瀏覽器看 ...

接著看到他的結構是這樣
```shell
  {
    "retCode": 1,
    "retVal": {
      "0001": {
        "sno": "0001",
        "sna": "捷運市政府站(3號出口)",
        ...
      },
      ...
    }
	}
```
OK 這物件包物件的結構跟火基<small style='text-decoration: line-through; color: grey'>(firebase)</small>有 87% 像，雖然這對 v-for 來說不成問題，他還是可以正常做迴圈循環，<strong style='color: red'>B U T ！</strong> 我完全不能對資料做常用的陣列處理 ... 所以這邊 GET 後要馬上先做一層轉換陣列的處理才能安心上路
```javascript
axios.get('https://tcgbusfs.blob.core.windows.net/blobyoubike/YouBikeTP.gz')
  .then(res => {
  let oriData = res.data.retVal
  let dataArr = []
  for (const key in oriData) {
    dataArr.push(oriData[key])
  }
  commit('storeApiData', dataArr);
})
```

而在第一次接觸 Vuex 後我就再也離不開它了，很多資料狀態現在都喜歡存在 state 裡面避免一堆上下傳承的 emit hell，因為 Vuex 用到現在還沒遇到太大的專案，所以對於拆分 module 可能還沒有什麼概念，不知道目前這個做法會不會有問題就是了。

這邊遇到了一個 getter 的小障礙，我的記法是把它當成 component 中的 computed 來操作，可是一旦遇到像 input 這種需要 v-model 的情況就不能綁在 getter 上了，因為一般的 computed value 是唯讀屬性，如果要做寫入修改的話就要分別做 <strong style='color: blue'>setter</strong> & <strong style='color: blue'>getter</strong>
```javascript
computed: {
  updateKeyword: {
    get() {
      return this.$store.state.searchKeyword
    },
    set(newInput) {
      this.$store.commit('updateKeyword', newInput)
    }
  }
},
```

剛好這次有機會碰到 input 篩選的功能，想來嘗試看看之前在[鐵人賽](https://ithelp.ithome.com.tw/articles/10192175)看到一位前輩分享的 listener: <strong><mark>composition</mark></strong>
![](http://www.7713.com/static/attachment/upload/image/20180525/1527240845749005.jpg)

簡單來說，回想一下我們在用 google search 的時候，只要是中文輸入法他都會在拼字期間就告訴你相關查詢的單字，這就是 composition 的功能，主要切分為三個觸發點
1. compositionstart: 開始輸入時
2. compositionupdate: 拼字或選字期間
3. compositionend: 選字完成

試了一下 vue 看他有沒有綁這個 listener 的 function 還真的成功了！👐
所以就把三個 listener 全部都拿去做比對篩選
```pug
input(
  type='text', 
  v-model='updateKeyword',
  ref='searchInput',
  @compositionstart='composition($event)',
  @compositionupdate='composition($event)',
  @compositionend='composition($event)')
```
> composition 的回傳值會在 $event.data

至於最主要的篩選功能，拜[泰德利](http://titlist.com.tw/)的福，雖然不到爐火清純<small>(?!)</small>，但對於整個做法還是有概念的，只要稍微整理出篩選順序就可以做到多層級篩選的目的
```javascript
filteredData: state => {
  let finalData = []
  // search keyword filter
  finalData = state.searchKeyword === '' ? state.bikeData : state.bikeData.filter(bike => {
    let tempStr = JSON.stringify(bike)
    let regexWords = state.searchKeyword.split(' ').join('|')
    if (tempStr.match(new RegExp(regexWords, 'i')) !== null) return true
  })

  // compute content of each area
  state.areaList.forEach(area => {
    let count = 0
    finalData.forEach(bike => {
      if (bike.sarea === area.name_zh) count++
    })
    area.contentLength = count
  })

  // area filters
  finalData = store.getters.selectedTags.length === 0 ? finalData : finalData.filter(bike =>  store.getters.selectedTags.some(tag => tag.name_zh === bike.sarea))
	
  // status filters
  // // rentable
  finalData = state.rentable ? finalData.filter(bike => bike.sbi > 0) : finalData
  // // returnable
  finalData = state.returnable ? finalData.filter(bike => bike.bemp > 0) : finalData
	
  // pagination options
  state.totalPage = Math.ceil(finalData.length / state.pageSize)
  state.totalLength = finalData.length

  // pagination
  let start = (state.currentPage - 1) * state.pageSize
  let end = start + state.pageSize

  return finalData.slice(start, end)
}
```

![](https://farm4.static.flickr.com/3027/3085098011_a712cbb21f_m.jpg)

<big><strong>第一層</strong></big>篩選我先處理最上方 input 的 keyword，用 regex 比對字元再回傳比對後的結果，但其實這邊篩選做得不好，因為這個做法是把物件的「所有」資料轉換成字串後去比對，也就是包含 key 值跟有些不會在畫面上呈現的 value，在 join('|') 那部分本來是想做到像 search engine 一樣可以用空格來做複數的交叉比對，可是現在還沒找到 regex 寫交集的方法所以先留著聯集 ...

<big><strong>第二層</strong></big>的 finalData 就會是第一層的篩選結果，就算沒篩選也是返回原陣列<small style='color: grey'>(每層皆是)</small>，所以可以接著做自己的篩選條件，這邊是負責計算每個行政區經過第一層的篩選後各有幾筆，會放在第二個的原因是因為如果在第三層或以下才開始篩的話只會顯示被點選的那個行政區數量，其他都是 0

<big><strong>第三層</strong></big>則是行政區下方的狀態篩選，主要是做 可借車數 > 0 跟 可歸還數 > 0

<big><strong>第四層</strong></big>就是最後的分頁切割啦，頁碼的計算一定一定是放在最後，才能正確地算出全部結果的筆數該分成幾頁，這邊本來是想試看看用[infinite-scroll](https://github.com/ElemeFE/vue-infinite-scroll)的方式來呈現<small style='text-decoration: line-through; color: grey'>(絕不是因為算頁數很麻煩又很沒成就感)</small>，但最後還是作罷 ... 先用以前的 component 頂著，再不然其實也可以找線上寫好的 component 來用



寫到這花了整整兩天的時間😓 超級累但成就滿點r！離截稿還有一段時間，就拿來套看看 google map 好了！原本想說這邊會耗上一段光陰，實際上比預期的還快，主要是拜[前人](https://medium.com/founders-factory/building-a-custom-google-map-component-with-vue-js-d1c01ddd0b0a)種的長青樹跟[官方文件](https://developers.google.com/maps/documentation/javascript/get-api-key)的福才能順利完成。

[看看成品](http://vfilter.s3-website-ap-northeast-1.amazonaws.com/)

![](https://i.imgur.com/Q98HR4t.png)


之後在找時間整理一下 Vuex 的細節🙌




<style>
	.mobile_images{
		width: 100%;
		height: 650px;
		display: flex;
	}
	.mobile_images img{
		width: 50%;
		height: auto;
	}
	@media (max-width: 768px){
		.mobile_images{
			flex-direction: column;
			height: auto;
		}
		.mobile_images img{
			width: 100%;
		}
	}
</style>