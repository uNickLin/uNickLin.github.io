---
title: 手把手的 Vue-cli
visible: true
date: 2018-05-23 10:05:32
tags: 
- Vue
- Webpack
categories: 
- Note
---

首先第一步當然是先安裝 Vue-cli ，使用 webpack 開發 Vue 時可以有以下幾種版本建立專案：

1. webpack
2. webpack-simple
3. browserify
4. browserify-simple
5. simple

但真正要看的其實只有前兩個，因為 3 跟 4 的 browserify 我沒用過也沒看別人用過 😓
至於第五個 simple 實在是太簡略了，讓你從一個 html 開始發展，如果說用 webpack 版本是學步娃的階段，webpack-simple 就是剛臨盆下來砍掉重練的感覺，而 simple 就是一顆受精卵 ... 😦

<!--more-->

## webpack

通常我都直接使用 webpack 版設定比較完整，但東西也相對較 simple 多，可能會有很多用不到的東西也一併安裝了

```shell
$ vue init webpack project-name
```

如果後面沒有指定 project-name 的話就會在當下的位置開始 init (注意現在 terminal 的位置不然會崩潰)
再來就逐一設定專案相關的資訊

```shell
ver: 2.9.1

? Project name (project-name)
? Project description ( xxxxx )
? Author (you)
? Vue build (Runtime + Compiler: recommended for most user)
  就是第一個
? Install vue-router (Y)
? Use ESlint to line your code
  這邊看專案需不需要，Y 的話就選擇哪一種 Lint 規範
? Set up unit tests 
  if 你有在寫測試的話 Y 
  else N
? Setup e2e tests with Nightwatch
  同上
? Should we run `npm install` for you after the project has been created ?
  if NPM 第一個 
  else if yarn 第二個
  else 第三個自己之後再安裝
```

接著因為個人都會用 pug sass 來開發，所以還會安裝以下這些插件

```shell
$ npm i -D pug pug-loader sass sass-loader node-sass
```

因為這兩者都是開發階段才會用到所以會存在 dev 中

安裝後找到 /build 目錄下的 webpack.base.conf.js 加入一些規則

```javascript
{
  test: /\.scss$/,
  loader: 'style!css!sass?sourceMap'
},
{
  test: /\.pug$/,
  loader: 'pug'
},
```

如果是 VS code 開發者可以再接著做以下設定寫起來會更方便！

```shell
// vs code setting
"emmet.syntaxProfiles": {
    "vue": "pug scss"
}
"emmet.includeLanguages": {
    "vue": "pug"
}
```

這樣在 .vue 中的 pug 才能使用 emmet 語法

## webpack-simple

跟 webpack 選項比起來要設定的東西就沒那麼多了，但 simple 有一點我認為比 webpack 好的是他會問你要不要安裝 sass 😭

```shell
? Project name (project-name)
? Project description ( xxxxx )
? Author (you)
? License (MIT)
? Use sass (YYYYYYYYYYYYY)
```

主要差異就在於 vue-router 要自己安裝設定，然後他不會幫你跑 npm install，進到專案也可以發現 webpack config 設定也只剩一支
那接著就是來看看怎麼導入 vue-router

```shell
$ npm i -S vue-router
```

在 main.js 中宣告 vue-router 並註冊給 vue 使用
```javascript
import VueRouter from 'vue-router'

Vue.use(VueRouter)
```

再來要給每一個 component 定義路由，通常會另外用一個 js file 來區分，但直接寫在 main.js 也是可以作用的(較不推薦)

```shell
/src/routes.js

import Home from '@/components/Home'

export const routes = [
	{
		path: '/',
		component: Home
	},
	...
]
```

設定好每個頁面的路由後，要將這個路由的目錄讓 Vue 他老大知道，所以我們回到 main.js

```javascript
import VueRouter from 'vue-router'
import {routes} from './routes'

Vue.use(VueRouter)

const router = new VueRouter({
	routes
})

new Vue({
	el: '#app',
	router,
	render: h => h(App)
})
```

這樣純手工的 vue-router 就大抵完成了