---
layout: post
title: "深入理解 Node.js 事件迴圈 (Event Loop) 核心機制"
date: 2026-02-25
categories: [nodejs]
summary: "這篇文章深入探討 Node.js 的事件迴圈（Event Loop）機制，解釋 Node.js 如何在單執行緒的環境下達到高並發的非同步效能表現，幫助您寫出更有效率的後端程式碼。"
---

Node.js 以其非阻塞 I/O (Non-blocking I/O) 與單執行緒 (Single-threaded) 架構聞名。然而，Node.js 如何在單一執行緒中同時處理成千上萬的請求而不會卡死？答案就在於它的核心：**事件迴圈 (Event Loop)**。了解 Event Loop 是每個 Node.js 開發者進階必經的過程。

## 為什麼需要了解 Event Loop？

當我們在使用 Node.js 開發應用程式時，如果對 Event Loop 的運作機制不熟悉，很容易寫出阻塞執行緒的程式碼 (例如大量且耗時的 CPU 計算)，導致整個系統效能瓶頸。理解它是避免這種情況的最佳防線。

## Event Loop 的主要階段 (Phases)

根據 Node.js 官方文件， Event Loop 在內部是由 libuv 這個 C 函式庫提供的，它將迴圈分為以下幾個主要階段，每一個階段都有一個 FIFO (First-In, First-Out) 的 Callback Queue：

1. **Timers 階段**：負責執行被 `setTimeout()` 與 `setInterval()` 註冊的 callback。當時間到了，它們的 callback 會被放入此階段的 queue 等待執行。
2. **Pending Callbacks 階段**：執行一些系統操作的 callback，例如 TCP 錯誤等。
3. **Idle, Prepare 階段**：僅供 Node.js 內部使用。
4. **Poll 階段**：非常重要的一個階段。它負責接收新的 I/O 事件 (例如讀取檔案、收發網路請求)，並執行 I/O 相關的 callbacks。大部分我們寫的非同步程式碼其 callback 都在這裡被觸發。
5. **Check 階段**：負責執行 `setImmediate()` 註冊的 callbacks。
6. **Close Callbacks 階段**：執行一些關閉資源的 callback，例如 `socket.on('close', ...)`。

## Macrotask 與 Microtask 的恩怨情仇

在每個階段交替之間，Node.js 還有一個隱藏的機關：**Microtask Queue**。
在 Node.js 中，`process.nextTick()` 與 `Promise.then()` 會產生 Microtask。

**規則是：** 只要 Event Loop 完成了其中一個階段的執行，在前往下一個階段之前，它會停下來檢查 Microtask Queue。如果裡面有任務，就會**全部清空**，然後才繼續前進。這代表 Microtask 擁有極高的執行優先權。

### 實際範例分析

看看以下程式碼，試著猜猜看輸出的順序？

```javascript
console.log('1. Start');

setTimeout(() => {
    console.log('2. setTimeout');
}, 0);

setImmediate(() => {
    console.log('3. setImmediate');
});

Promise.resolve().then(() => {
    console.log('4. Promise');
});

process.nextTick(() => {
    console.log('5. process.nextTick');
});

console.log('6. End');
```

**執行結果順序會是：**
1. `1. Start` (同步執行)
2. `6. End` (同步執行)
3. `5. process.nextTick` (Microtask 優先級最高)
4. `4. Promise` (其次的 Microtask)
5. `2. setTimeout` 或 `3. setImmediate` (註：如果在主模組層級執行，這兩者的順序可能因為系統效能而隨機；但在 I/O callback 內，`setImmediate` 永遠先於 `setTimeout` 執行！)

## 總結與最佳實踐

*   **不要阻塞事件迴圈**：避免在主執行緒進行耗時巨大的同步運算 (例如超大的 JSON 解析或是繁雜的加密演算法)。
*   遇到 CPU 密集型任務，考慮使用 Node.js 的 **Worker Threads** 將其外包到其他執行緒處理，保持主 Event Loop 的流暢。
*   理解非同步的優先級：如果你希望某個回呼函式盡快且肯定在 I/O 之前執行，使用 `process.nextTick()`；如果只是希望在目前階段之後非同步執行，`setImmediate()` 會是個不錯的選擇。

希望這篇文章能幫助您釐清 Node.js Event Loop 回圈的心智模型，讓我們一起寫出更強健的後端系統！如果您對前端或是輔助工具有興趣，歡迎造訪我們的 [WebUtils 工具箱](/webutils/)。
