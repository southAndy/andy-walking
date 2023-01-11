---
title: Vuex基礎
date: "2023-01-11"
description: "隨著你的專案規模改變，資料也會有需要"
---

## vuex 基本

根據文件的概念，vuex 定位是用來管理**全域資料、全域邏輯**

> Vuex is a **state management pattern + library** for Vue.js applications.
> ref:https://vuex.vuejs.org/

### 全域的概念

簡單的概念說明就是：**這個資料會被跨頁面使用**，常見的「全域資料」如下：

```
userID //使用者ID
loginTime //登錄時間
api-payload //帶給後端的payload
API //從後端取回的資料
```

### 基本構造

我們可以用 vue 的架構去比喻，如下：

```js
const store = createStore({
  //存放資料的地方 > 如同vue的data
  state: {},
  //常用於計算，取得一個值 > 如同computed
  getters: {},
})
```

不同的是在 vue 本身，不論是**同步**及**非同步**的邏輯都可以在 `methods` 撰寫。但在 vuex 則被拆開去各別處理：

```js
const store = createStore({
  //執行同步的邏輯
  mutations: {},
  //執行非同步的邏輯
  actions: {},
})
```

在下面的部分會針對各架構介紹：

---

### State

如同前面提過的，存放在 Vuex 內的 `state`，都是全域等級的資料，那要如何使用呢？
在這邊會分別介紹：

1. **元件取用**
2. **Vuex 內部取用**

#### 元件取用

可以分為**單一資料**取出及**多資料**取用

```js
//單一資料取法: 透過 this 去指向vuex
computed:{
	getVuexStore:function(){
		return this.$store.state.[stateName]
	}
}
//多筆資料取法: 透過 vuex 內建的 map 系列方法
import {mapState} from "vuex"

computed:mapState({
	//有三種取法,需注意參數必須為 state
	//1.使用箭頭函式取
	localName:state=>state.stateName

	//2.使用 string方式會指定對應state
	aliasName:'count', //'count' === state.count

	//3.一般函式（當你需要使用到 local-state時，避免this混亂使用）
	count(state){
		return state.count + this.localName
	}
})

```

**使用建議**

1. 直接將`store`內的值，存在`data`內(==不建議使用==)
2. 透過`computed,methods`修改`data`的值

```javascript=
methods:{
    onSubmit(){
        //加上this是因為已經將vuex註冊在vue實例。
        this.event.user = this.$store.state.user
    }
}
```

#### Vuex 內部取用

---

#### `mapState` Helper

```javascript=
computed:{
    getName(){
        return this.$store.state.user
    };
    getAge(){
        return this.$store.state.age
    }
}
//但要是你要引入的資料超過10個呢？
```

這就是`mapsState`的用途，**讓你可一次引入多個 `state`**

1. 陣列形式：

```javascript=
const store = createStore({
    //存放資料的地方
    state:{
        user:'andy',
        age:24
    },
})
//要以字串形式引入
computed:mapState(['user','age']);
```

2. 物件形式：

```javascript=
computed:mapState({
    //以字串形式
    inputUser:'user',

    //以函式形式:參數名統一為state
    getState(state){
        return this.$store.state.user
    }
    //若只有單一參數也可用箭頭函式
    user:state=>state.user;
});
```

3. 混合其他 computed 使用
   利用[ES6 展開、剩餘運算子](/yWYfywNHSeScagbpP1ck_A)拆解`mapState()`

---

### Mutations

### Actions

### Module

[[modules]]

---

## vuex 解決的問題

### 解決跨結構的元件資料傳遞問題：

目前前端主流的框架最基本單位就是元件，在 vuex 還沒出現以前，如果想要實現「==跨元件的資料如何傳遞==」，以 vue 來說，就是使用[[props & emit]] 或是[[provide & inject]] 來進行資料傳遞。

```html

//假設今天有一個頁面的元件架構如下：
<Home>
	<Child>
		<GrandChild/>
			<GrandGrandChild/>
</Home>
```

如上面的例子，當你的頁面架構層級之多，從實際開發面來說，這樣容易產生的問題有： 1. **在後續追蹤功能上成本較高** 2. 較難保證在過程不被其他內容影響

<br>

### 達成單一資料流 (one-way data flow)的管理

#### 什麼是單一資料流？好處是什麼？

我自己目前的理解是：**只能透過單一管道去取得、修改資料**，且你永遠不會在 store 外改動`state`的數值，那 Vuex 怎麼做到這件事？
![[Pasted image 20230111135713.png]]

### 認識 Vuex 資料存取原理

當你想要取得一個存放在 `state` 內的 API 資料，你會經過下面的步驟：

1. 元件內執行一個 `methods`，或是在生命週期時觸發 `actions`
2. 執行後會傳`commit`到`Mutations` 由它更新`state`內的值
3. 當確認更新後，將變動的值賦予到`state`內的變數中
4. 回傳給元件

<div style="border:1px solid black;margin-bottom:5px">
    <img src="https://i.imgur.com/at5KuTP.png"/>
</div>

好，現在重新看一次官方文件的示意圖，是不是比較能理解呢？

---

## 參考資料

1. [Vuex-doc](https://vuex.vuejs.org/)
