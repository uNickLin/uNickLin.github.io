---
title: js-things
visible: true
date: 2018-05-14 10:16:12
tags: Javascript
categories: Tricks
---

前幾天吳哲宇老師又重返直播，主題是 [Vue + Socket 的配置教學](https://www.youtube.com/watch?v=maFbo96YT8U)，上次即時互動是用 Vue + firebase 模擬一個 LINE 的即時訊息傳遞，而這次的 Socket 是 realtime 界中的霸主，所以十點準時守在電腦前開播。

<!--more-->

firebase 的專案主要是參照一位 Gua's 大大的部落格刻出來的
![Chatroom](https://i.imgur.com/yfIyYsk.png)
<small>[來源參照](https://guahsu.io/2017/09/vue-firebase-realtime-line-chat/)</small>

但我發現使用 firebase 不需要打 restful 就會幫你自動更新 data ，說是方便但也有那麼一點感覺不到前後端互動，總覺得要做一點 get post 的事情才完整，但整體下來做完的成就感還是很高，之後學會 PWA 有機會還能在幫他升級一下。

回到直播內容，這次雖然來不及講到 Socket 但中間也帶了一些以前沒想過的 js 用法，就順道把之前紀錄在 Hack MD 上的 js 一併移過來分享

## Array 的亂數排序

情境如果像是線上題庫需要隨機出題的話，方法之一是在整個題目陣列中取前幾個物件，而為了要達到「隨機」可以每次取出前將陣列打亂，而隨機這就是就可以用 sort 來達成，sort 的特性是：
當 return 值為負數 => a 往前 b 往後
```javascript=
let array = [1, 345, 467, 3, 24]
let sort1 = array.sort((a, b) => a - b)

console.log(sort1) // [1, 3, 24, 345, 467]
```
當 return 值為正數 => b 往前 a 往後
```javascript=
let array = [1, 345, 467, 3, 24]
let sort1 = array.sort((a, b) => b - a)

console.log(sort1) // [467, 345, 24, 3, 1]
```
所以最重要的是在那個 return 值的數為正為負，當需要「隨機」時就表示只要讓那個 return 隨機變為正、負
```javascript=
let array = [1, 2, 34, 656, 23, 46, 68]

let randomArr = array.sort((a, b) => Math.random() - .5)
```
因為 Math.random() 輸出的數值會落在 0 ~ 1 ，減去平均數 0.5 後便可以平均的出現正數與負數，達到亂數排序的目的

## Array.prototype
### forEach
```javascript=
let arr = [
  {A: 1, a: 2},
  {B: 3, b: 4},
  {C: 5, c: 6}
]

arr.forEach(obj => {
  console.log(obj)
})

// {A: 1, a: 2}
// {B: 3, b: 4}
// {C: 5, c: 6}
```
forEach 會循環每個物件的所有值, 不同於 for(let i=1 ...) 可以較清楚當下的物件是誰且賦予新屬性等, 例如
```javascript=
let arr = [
  {A: 1, a: 2},
  {B: 3, b: 4},
  {C: 5, c: 6}
]

arr.forEach(obj => {
  obj.addKey = 87
})

/* 
arr = [
  {A: 1, a: 2, addKey: 87},
  {B: 3, b: 4, addKey: 87},
  {C: 5, c: 6, addKey: 87}
]
*/

```

### filter
對 array 中的每個元素都做一次 callback, 並過濾回傳值為 true 的元素創建一個新的 array (origin array !== new array), filter 包含三個 args: 
```javascript=
filter(callback[element, index, array])
```
```javascript=
const array = [1, 2, 4, 66]

let newarray = array.filter(obj => obj > 3)
// newarray: [4, 66]
// array: [1, 2, 4, 66]
```

### map
有點像 forEach 的作用, 不同的是 map 會產生新的 array, 適用於要對陣列加工處理卻又不想改變原陣列的時候使用
```javascript=
const array = [3, 4, 6, 87]
let newarray = array.map(obj => obj + 1 )
// newarray: [4, 5, 7, 88]
// array: [3, 4, 6, 87]
```
再取出陣列中物件的特定值也很方便
```javascript=
const people = [
  { name: 'Nick', age: 46},
  { name: 'Sebastine', age: 53},
  { name: 'Johnason', age: 27}
]

let ages = people.map(person => person.age)
// ages: [46, 53, 27]

let names = people.map(person => person.name)
// ages: ['Nick', 'Sebastine', 'Johnason']
```

### some
對陣列中每個元素執行 callback, 當某一個元素條件成立就會返回 true, 而當所有元素執行後皆返回 false, some 也會 return false
```javascript=
const array = [3, 4, 6, 87]
array.some(obj => obj > 50)
// true

array.some(obj => obj < 0)
// false
```
搭配 filter 可以快速做兩個陣列資料比對的篩選
```javascript=
let people = [
  { name: 'Nick', age: 46 },
  { name: 'Sebastine', age: 53 },
  { name: 'Johnason', age: 27 }
]

let ages = [22, 33, 24, 27, 53, 48]
// 從 people 中選出 age 符合 ages 中的元素

let matched = people.filter(person => {
  return ages.some(a => a === person.age)
	// 當 ages.some 符合時會 return true, 此時 filter 接收到 true 後會將當下的 person 篩出
})

/*
matched: [
	{ name: 'Sebastine', age: 53 },
	{ name: 'Johnason', age: 27 }
]
*/

更簡潔作法：let matched = people.filter(person => ages.some(a => a === person.age))
```

### every
與 some 相反，必須所有元素皆為 true 才會返回 true, 否則為 false
```javascript=
const array = [3, 4, 6, 87]
array.every(obj => obj < 60)
// false

array.every(obj => obj > 2)
// true
```
:::info
some：someone match => true
every：everyone match => true
:::

### reduce
將陣列中的元素回傳出一個總和, 可傳入四個 args: 
```javascript=
reduce(callback[accumlator, currentValue, currentIndex, array], initialValue)
accumlator: 目前的總和
currentValue: 現值
currentIndex: 現值的索引
array: 原陣列
initialValue(optional): 初始值
```
```javascript=
const array = [2, 2, 345, 64, 532]
let sum = array.reduce((pre, cur) => pre + cur)
// sum: 2 + 2 = 4
// sum: 4 + 345 = 349
// ...
// sum: 945

//=== with initialValue ===
let sum = array.reduce((pre, cur) => pre + cur, 55)
// sum: 55 + 2 = 57
// sum: 57 + 2 = 59
// ...
// sum: 1000
```

---

## Others

### for ... in
```javascript=
let obj = {a: 1, b: 2, c: 3}

for(let prop in obj) {
  console.log(prop, obj[prop])
}

// 'a 1'
// 'b 2'
// 'c 3'
```
for ... in 是針對物件 key 值的遍歷方法, 不適合對陣列使用

### for ... of
JavaScript6裡引入了一種新的循環方法，它就是for-of循環，它既比傳統的for循環簡潔，同時彌補了forEach和for ... in循環的短板
```javascript=
// Array
let arr = [1, 2, 3]

for (let val of arr) {
  console.log(val)
}

// 1
// 2
// 3
```
```javascript=
// String
let str = 'hell'

for (let val of str) {
  console.log(val)
}

// 'h'
// 'e'
// 'l'
// 'l'
```

但以上 for 各自還是有自己的優點可取：
* for loop 可以自訂中斷點 break
* for ... of 不能像 forEach 一樣取得 index


## Sort
常用 sort 排序方法

* 數列
```javascript=
let array = [1, 34, 2, 35, 32, 21, 14]

// 升冪排序
asc = array.sort((a, b) => a - b)
    = [1, 2, 14, 21, 32, 34, 35]

// 降冪排序
desc = array.sort((a, b) => b - a)
     = [35, 34, 32, 21, 14, 2, 1]
```

* 日期
```javascript=
let date = ['20020202T12:00:00Z', '20030303T13:00:00Z', ...]
(timestamp 都可以)

// 升冪排序
asc = date.sort((a, b) => {
  return new Date(a) - new Date(b)
})

// 降冪排序
desc = date.sort((a, b) => {
  return new Date(b) - new Date(a)
})
```

* 字母
```javascript=
let string = ['Dianna', 'Renick', 'Annie', 'Irelia']

// A - Z
string.sort((a, b) => {
  return a.toLowerCase().localeCompare(b.toLowerCase())
})

// Z - A
string.sort((a, b) => {
  return b.toLowerCase().localeCompare(a.toLowerCase())
})
```

# Bonus
篩選數列中不重複的值
1. filter
```javascript=
let array = [1, 2, 'a', 3, 1, 'b', 'a'];

let result = array.filter(function(element, index, arr){
  return arr.indexOf(element) === index
})

let repeat = array.filter(function(element, index, arr){
  return arr.indexOf(element) !== index
})

console.log(result) // [1, 2, "a", 3, "b"]
console.log(repeat) // [1, "a"]
```
2. Set( ) <font size='1px'>==*ES6 以上*==</font>
```javascript=
let array = [1, 2, 'a', 3, 1, 'b', 'a']
let result = new Set()
let repeat = new Set()

array.forEach(item => {
  result.has(item) ? repeat.add(item) : result.add(item)
})

console.log(result) // {1, 2, "a", 3, "b"}
console.log(repeat) // {1, "a"}
```
:::warning
此作法 Set() 取出的結果皆為物件！！
:::

3. Set( ) + Array.from()
```javascript=
let array = [1, 2, 'a', 3, 1, 'b', 'a']
let result = Array.from(new Set(array))

console.log(result) // [1, 2, "a", 3, "b"]

```

4. Set( ) + Spread
```javascript=
let array = [1, 2, 'a', 3, 1, 'b', 'a']
let result = [...(new Set(array))]

console.log(result) // [1, 2, "a", 3, "b"]

```

[參照：JavaScript取出陣列重複/不重複值的方法
](https://guahsu.io/2017/06/JavaScript-Duplicates-Array/)