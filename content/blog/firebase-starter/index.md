---
title: 用firebase-auth來建立簡易的會員功能吧！（上）
date: "2022-12-09"
description: "會員服務是網站常見的功能，透過會員服務能提供給使用者更加客製化的體驗..."
---

####### tags: `firebase`

會員服務是網站常見的必備功能，透過會員制度能提供給使用者更加客製化的體驗，像是：

1. 收藏、紀錄網站內容
2. 根據會員喜好提供的客製化推薦

在這次的文章就是要透過 React + firebase 製作一個最簡易的會員功能。

## 產生一個 firebase 專案

在開始使用 firebase 服務前，你需要先執行以下幾個步驟：

1. 使用 google 帳號進行註冊
2. 根據自身的開發環境（如筆者是寫網頁)，因此會選擇 _web_ 來產生專案 (如下圖)
   ![](https://i.imgur.com/O9yZLiH.png)

   ##### (圖片來源：firebase 官網)

3. 進入專案後，會看到 firebase 內提供了多種的服務，如：
   1. Authentication: 驗證、管理使用者
   2. Storage: 儲存資料的

---

## 產生一個 react 專案

在上面，我們已經完成了基本的 firebase 專案產生，接著要創建一個專案才能將 firebase 的服務引入專案內。

在這邊我們就根據 *React-Router*教學的產生方式，來產生一個全新的 react 專案。

```shell
npm create vite@latest [name-of-your-project] -- --template react
```

接著執行

```shell
npm run dev
```

現在你應該就會看到一個全新的 react 初始畫面（如下圖）
![](https://i.imgur.com/XsSOvKY.png)

恭喜你已經創立了一個全新的專案，那我們繼續前進吧～

### 創立 react-router

下方為筆者所做最基本的會員登錄架構

```
- /(home)
    - /login
    - /register
```

目標：

1. 完成註冊後，會跳轉回登錄頁面 (`/login`)
2. 成功登錄後，會跳轉回首頁 (`/`)

那要怎麼達到呢？

```js

```

如果想更深入認識 React-Router ，可以看我的筆記：[我所不知道的 React-Router]('/content/blog/React-router/index.md')

### 引入 firebase-auth 功能

這次要使用的是 Authentication 這項服務
![](https://i.imgur.com/5zEWuXV.png)

##### (圖片來源：firebase 官網)
