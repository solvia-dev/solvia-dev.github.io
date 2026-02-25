---
layout: post
title: "Vue Router 核心觀念：如何優雅地管理單頁應用程式 (SPA) 路由"
date: 2026-02-25
categories: [vue]
summary: "在單頁應用程式 (SPA) 中，路由管理是不可或缺的一環。這篇文章帶您快速了解 Vue Router 的基本設定、動態路由、以及導航守衛 (Navigation Guards) 的實用技巧。"
---

現代前端框架（如 Vue.js）打造的通常是 **單頁應用程式 (SPA - Single Page Application)**。在 SPA 中，當使用者點擊連結切換頁面時，瀏覽器並不會發送請求給後端重新載入整個 HTML 檔案，而是透過 JavaScript (也就是 Vue Router) 來抽換頁面中的組件 (Components)，帶來極度流暢的使用者體驗。

## 什麼是 Vue Router？

Vue Router 是 Vue.js 官方的路由管理器，它深度整合了 Vue.js 核心，使得建立單頁應用程式變得輕而易舉。

它的核心職責就是定義：「**當網址是什麼的時候，畫面上應該顯示哪個組件。**」

## 基本配置與起步

在建立 Vue 3 專案時，若有選擇安裝 Vue Router，它通常會在 `src/router/index.js` 產生設定檔。我們來看看基本的結構：

```javascript
import { createRouter, createWebHistory } from 'vue-router'
import HomeView from '../views/HomeView.vue'
import AboutView from '../views/AboutView.vue'

const routes = [
  {
    path: '/',
    name: 'home',
    component: HomeView
  },
  {
    path: '/about',
    name: 'about',
    // Route Level Code-Splitting: 幫助減少初次載入時間 (延遲載入)
    component: () => import('../views/AboutView.vue')
  }
]

const router = createRouter({
  history: createWebHistory(),
  routes
})

export default router
```

在 Vue 元件中，我們透過 `<router-view>` 這個標籤告訴 Vue：「請在這裡渲染與目前網址匹配的組件」。並用 `<router-link to="/about">` 取代傳統的 `<a href="/about">` 以防觸發完整的網頁重新載入。

## 動態路由匹配 (Dynamic Routing)

很多時候，我們需要將某個模式的所有路由對應到同一個組件。例如，我們有一個 `UserView` 元件，它應該能為所有不同的使用者 ID 呈現。

這時候就可以在路徑中使用「動態區段 (Dynamic Segment)」，以冒號 `:` 開頭：

```javascript
const routes = [
  // 匹配 /users/evan, /users/john 等等
  { path: '/users/:id', component: UserView },
]
```

當導航到 `/users/123` 時，我們可以在 `UserView` 的 `setup` 函式中讀取這份參數：

```javascript
import { useRoute } from 'vue-router'

export default {
  setup() {
    const route = useRoute()
    console.log(route.params.id) // 輸出 '123'
    return {}
  }
}
```

## 導航守衛 (Navigation Guards)

有時候我們不希望任何人都能輕易拜訪特定頁面（例如管理員後台）。Vue Router 提供了 **導航守衛** 來控制存取權限。你可以在路由跳轉的過程之中「攔截」它，進行登入驗證等檢查。

最常用的就是全域前置守衛 `router.beforeEach`：

```javascript
router.beforeEach((to, from) => {
  // 檢查如果即將前往的路由名稱不是 login，且使用者尚未登入
  if (to.name !== 'login' && !isAuthenticated) {
    // 強制重導向至登入頁
    return { name: 'login' }
  }
})
```

掌握了設定檔、動態參數以及導航守衛，基本上就已掌握 Vue Router 80% 的日常開發需求了！當然，在切版和開發單頁應用的過程中，如果遇到各種網頁小工具需求，也歡迎點擊我們的 [WebUtils 開發工具箱](/webutils/) 尋找靈感喔！
