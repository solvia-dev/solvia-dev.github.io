---
layout: post
title: "Vue 3 Composition API 初學者指南：為何我們應該拋棄 Options API？"
date: 2026-02-25
categories: [vue]
summary: "Vue 3 帶來了改變遊戲規則的 Composition API。這篇文章將帶您了解從 Options API 轉換到 Composition API 的原因，以及如何用 setup 函式寫出更好維護的前端元件。"
---

當學習 Vue 3 時，最引人注目的新特性莫過於 **Composition API (組合式 API)**。對於熟悉 Vue 2 (Options API) 的開發者來說，一開始看到 `setup()` 函式可能會有些不習慣。但事實上，它是為了幫助我們解決複雜元件的維護噩夢而誕生的。

## Options API 的痛點在哪裡？

在傳統的 Options API 中，我們根據資料的「類型」來組織程式碼：`data`, `methods`, `computed`, `watch` 等等。當元件變得很小的時候，這種寫法非常直覺易懂。

然而，如果一個元件長度超過了 500 行，並包含了 3 到 4 個主要邏輯區塊（例如：搜尋過濾邏輯、分頁邏輯、資料獲取邏輯），我們會發現這幾個邏輯的程式碼被**強迫拆散**在 `data`, `methods` 和 `computed` 各個角落。

這會導致：
*   **閱讀困難**：要理解「搜尋」邏輯，必須在同一個檔案內不停上下捲動 (Scrolling up and down)。
*   **重用困難**：要將邏輯抽離成 Mixin 會有命名衝突和來源不明確的問題。

## Composition API 帶來什麼改變？

Composition API 允許我們以「邏輯關注點 (Logic Concerns)」來組織程式碼。這意味著所有與「搜尋過濾」相關的變數、計算屬性和方法，都可以寫在一起。這也是為什麼它被稱為「組合式」的原因。

### 來看看一個基礎的計數器範例

使用 `<script setup>` 語法糖會讓程式碼更加簡潔：

```vue
<template>
  <div>
    <h2>計數器：{{ count }}</h2>
    <p>目前的數字是: {{ doubleCount }}</p>
    <button @click="increment">增加</button>
  </div>
</template>

<script setup>
import { ref, computed } from 'vue';

// 狀態宣告
const count = ref(0);

// 計算屬性
const doubleCount = computed(() => count.value * 2);

// 方法
function increment() {
  count.value++;
}
</script>
```

在上面的例子中，`ref` 讓資料具有響應性 (Reactivity)。與 `data()` 不同的是，我們不需要透過 `this` 來存取它們，這也解決了令人頭痛的 TypeScript 支援問題。

## 獨立封裝與邏輯重用 (Composables)

更棒的是，我們可以輕易將邏輯抽離成一個獨立的 JS 檔案（通常稱為 Composable）：

```javascript
// useCounter.js
import { ref, computed } from 'vue';

export function useCounter() {
  const count = ref(0);
  const doubleCount = computed(() => count.value * 2);

  function increment() {
    count.value++;
  }

  return { count, doubleCount, increment };
}
```

任何需要這個邏輯的元件，只需 `import { useCounter } from './useCounter.js'` 就能馬上使用，不會有 Mixin 的命名衝突風險，這真的是前端架構的救星！

希望這個簡單的介紹能幫助您邁出 Composition API 的第一步。好的工具能大幅提升開發效率，如果您在前端開發時需要一些實用的輔助工具，也別忘了到我們的 [WebUtils 工具箱](/webutils/) 尋寶喔！
