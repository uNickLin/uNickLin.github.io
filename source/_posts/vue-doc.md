---
title: Vue 的小細節
visible: true
date: 2018-05-28 10:36:31
tags: 
- Vue 2.x
categories:
- Tricks
---

最近把 Udemy Max 的課看到一個進度後，發現其實自己在使用 Vue 的時候還有很多可以優化的地方，所以撥個時間把[官方文件](https://vuejs.org/v2/guide/index.html)看個仔細，紀錄一下在課程及文件中學到的新知識。

<!--more-->

## Router 路由名稱

以前只會用一種 router-link 方式來切換：
```html
<router-link :to='{path: "/"}'>HOME</router-link>
```

其實沒有其他參數的情況下就跟一般路由一樣設定即可
```html
<router-link to='/'>HOME</router-link>
```

這邊重要的是，path 除了 url 之外，也可以用 name 來定義，好處是未來若 url 層級變更，component 中的 router-link 不需要變動！

首先必須為每個路由命名
```javascript
// routes.js
routes: [
	{
		name: 'Home',
		path: '/',
		component: Home
	},
	{
		name: 'About',
		path: '/about',
		component: About
	},
	...
]
```
```html
<router-link :to='{name: "Home"}'>HOME</router-link>
<router-link :to='{name: "About"}'>About</router-link>
```


## Loading Routes Lazily

考慮到大型專案頁面繁多，可以將路由歸納整合，且只在需要時載入，可以減輕 browser loading 提升效能！
在載入路由時需要做點調整，原本 import component 是這樣：
```javascript
// routes.js
import Home from './components/Home.vue'
```
這邊要改由 promise 的方式宣告
```javascript
const Home = resolve => {
	require.ensure(['./components/Home'], () => {
		resolve(require('./components/Home'))
	})
}
```
這樣一來 Home 的路由就不會在一開始被包進 build.js ，而是在進入 Home 頁面時才會加載一支 1.build.js，更進一步來說，像是 User, UserInfo, UserCart ... 等都是來自同一個 root path 的頁面(非必要，只是通常會這樣分類)，就可以在這邊設定當進入 User 頁面時再加載與他相關的其他頁面，如此一來節省的封包容量又更多了！
```javascript
const User = resolve => {
	require.ensure(['./components/User'], () => {
		resolve(require('./components/User'))
	}, 'user')
}
const UserInfo = resolve => {
	require.ensure(['./components/UserInfo'], () => {
		resolve(require('./components/UserInfo'))
	}, 'user')
}
const UserCart = resolve => {
	require.ensure(['./components/UserCart'], () => {
		resolve(require('./components/UserCart'))
	}, 'user')
}
```


## tab 切換

在有 components 的概念以前，製作 tab 切換時都是一次寫好所有內容，再透過 CSS or js 來切換各自的顯示(display)
```html
<ul>
	<li>Tab1</li>
	<li>Tab2</li>
	<li>Tab3</li>
</ul>
<div class='.tab1'>
	<p>Content 1</p>
</div>
<div class='.tab2'>
	<p>Content 2</p>
</div>
<div class='.tab3'>
	<p>Content 3</p>
</div>
```

但這時候可以用 component 來切換要載入的區塊，這些 tab 都轉換成 component 的方式導入主頁面
```javascript
import Tab1 from './components/Tabs/Tab1.vue'
import Tab2 from './components/Tabs/Tab2.vue'
import Tab3 from './components/Tabs/Tab3.vue'

export default{
	components: {
		Tab1, Tab2, Tab3
	}
}
```

而在 template 中不需要將個別 tag 載入，只要加一個 component tag
```html
<ul>
	<li>Tab1</li>
	<li>Tab2</li>
	<li>Tab3</li>
</ul>
<component :is='currentTab'></component>
```

<strong>v-bind:is</strong> 就是判斷要載入哪個 component 的位置，讓他預設為 Tab1(字串名稱必須與 component name 一致)
```javascript
import Tab1 from './components/Tabs/Tab1.vue'
import Tab2 from './components/Tabs/Tab2.vue'
import Tab3 from './components/Tabs/Tab3.vue'

export default{
	data () {
		return {
			currentTab: 'Tab1'
		}
	},
	components: {
		Tab1, Tab2, Tab3
	}
}
```

接著一樣透過上面寫的 li 來切換 currentTab 的字串
```html
<ul>
	<li @click='currentTab = "Tab1"'>Tab1</li>
	<li @click='currentTab = "Tab2"'>Tab2</li>
	<li @click='currentTab = "Tab3"'>Tab3</li>
</ul>
<component :is='currentTab'></component>
```


## 注意 Component tag 

> 有些 HTML 元素，諸如 ul、ol、table 和 select，對於哪些元素可以出現在其內部是有嚴格限制的。而有些元素，諸如 li、tr 和 option，只能出現在其它某些特定的元素內部。
```html
<table>
  <blog-post-row></blog-post-row>
</table>
```
這種結構有可能在瀏覽器解析時發生解析錯誤，所以要做以下修正
```html
<table>
  <tr is="blog-post-row"></tr>
</table>
```
但官方又說了：
>需要注意的是如果我們從以下來源使用模板的話，這條限制是不存在的：
>1. 字符串 (例如：template: '...')
>2. 單文件組件 (.vue)
>3. <script type="text/x-template"\>

我知道的方法除了第三點的 jsx，就是 template 跟 .vue file 了，所以 ... 也不知道他說會出錯是在哪種情況下😅

<i>To Be Continued ...</i>