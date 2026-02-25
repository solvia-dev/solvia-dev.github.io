---
layout: post
title: "使用 Express 快速打造 Node.js RESTful API"
date: 2026-02-25
categories: [nodejs]
summary: "想建立自己的後端服務？本篇文章將帶領您使用 Node.js 中最受歡迎的 Express 框架，從零開始實作一個符合 RESTful 架構的 API 應用程式。"
---

在建立現代網頁應用程式時，前後端分離已經是標準配置。前端（如 Vue, React）負責介面互動，這時就需要一個強大且穩定的後端 API 來提供資料讀寫的服務。在 Node.js 生態系中，**Express** 絕對是建置這類服務的王者。

## 什麼是 RESTful API？

REST (Representational State Transfer) 是一種軟體架構風格，主要透過 HTTP 的動詞（Methods）來對資源（Resources）進行操作。簡單來說：
*   **GET**：讀取（Read）資源
*   **POST**：建立（Create）資源
*   **PUT/PATCH**：更新（Update）資源
*   **DELETE**：刪除（Delete）資源

## 初始化專案與安裝 Express

首先，確保您的電腦已經安裝了 Node.js。在終端機中建立新資料夾並初始化專案：

```bash
mkdir my-express-api
cd my-express-api
npm init -y
npm install express
```

## 實作基本伺服器與路由

建立一個 `app.js` 檔案，我們將要在這裡撰寫所有的主程式。假設我們正在撰寫一個「代辦事項 (Todos)」的 API：

```javascript
const express = require('express');
const app = express();
const port = 3000;

// 解析 JSON 格式的請求本體
app.use(express.json());

// 模擬資料庫
let todos = [
    { id: 1, title: '學習 Node.js', completed: false },
    { id: 2, title: '寫一篇技術文章', completed: true }
];

// GET: 取得所有 Todo
app.get('/api/todos', (req, res) => {
    res.json(todos);
});

// GET: 依據 ID 取得特定 Todo
app.get('/api/todos/:id', (req, res) => {
    const todo = todos.find(t => t.id === parseInt(req.params.id));
    if (!todo) return res.status(404).send('找不到該項目');
    res.json(todo);
});

// POST: 新增 Todo
app.post('/api/todos', (req, res) => {
    const newTodo = {
        id: todos.length + 1, // 模擬自動遞增 ID
        title: req.body.title,
        completed: false
    };
    todos.push(newTodo);
    res.status(201).json(newTodo);
});

// DELETE: 刪除 Todo
app.delete('/api/todos/:id', (req, res) => {
    const todoIndex = todos.findIndex(t => t.id === parseInt(req.params.id));
    if (todoIndex === -1) return res.status(404).send('找不到該項目');
    
    const deletedTodo = todos.splice(todoIndex, 1);
    res.json(deletedTodo[0]);
});

// 啟動伺服器
app.listen(port, () => {
    console.log(`Server is running at http://localhost:${port}`);
});
```

## 測試 API

一旦伺服器啟動後 (`node app.js`)，你就可以使用 REST Client 工具（如 Postman、Insomnia，或是 VS Code 的 Thunder Client 擴充套件）進行測試。

雖然這只是一個儲存在記憶體中的範例，但所有更複雜的真實世界 API（連接 PostgreSQL、MongoDB 等資料庫，加入使用者驗證 JWT 等）都是在此架構上延伸擴展的！

後端開發固然有趣，有時也需要方便的小工具來轉換 JSON 格式或是編碼/解碼字串。試試看我們精心準備的 [WebUtils 工具箱](/webutils/)，提升您的開發效能！
