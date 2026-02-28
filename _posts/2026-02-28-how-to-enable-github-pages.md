---
layout: post
title: "如何啟用 GitHub Pages 建立無伺服器部落格"
date: 2026-02-28
categories: [github, blog]
summary: "想要擁有自己的部落格，卻不想花錢租用伺服器？GitHub Pages 提供了免費又穩定的靜態網頁代管服務。這篇文章將帶你一步步啟用 GitHub Pages，輕鬆讓你的網站上線！"
---

想要擁有一個專屬的個人部落格或技術筆記網站，又不想花費心力與金錢去維護伺服器嗎？**GitHub Pages** 絕對是軟體開發者與技術寫作者的首選！

GitHub Pages 是 GitHub 提供的一項靜態網站代管服務，你只需要將 HTML、CSS、JavaScript 或是像 Jekyll、Hugo 這類靜態網站產生器 (Static Site Generator) 的原始碼推送到 GitHub，就能免費將其轉換為公開的網站。

這篇文章將帶你了解如何從零開始啟用 GitHub Pages。

## 第一步：建立新的 Repository (儲存庫)

1. 登入你的 GitHub 帳號。
2. 點擊右上角的 `+` 號，選擇 **New repository**。
3. **重要命名規則**：如果你希望你的網站網址是 `https://<你的帳號>.github.io`，請務必將 Repository 命名為 **`<你的帳號>.github.io`**（例如：`solvia-dev.github.io`）。
4. 將儲存庫設為 **Public** (公開)。GitHub Pages 免費版必須公開原始碼。
5. 點擊 **Create repository**。

## 第二步：上傳你的網站程式碼

如果你只是想測試，可以先建立一個簡單的 `index.html`：

```html
<!DOCTYPE html>
<html>
<head>
    <title>我的第一個 GitHub Pages</title>
</head>
<body>
    <h1>Hello, GitHub Pages!</h1>
    <p>這是我用 GitHub Pages 架設的網站。</p>
</body>
</html>
```

使用 Git 指令或是直接透過 GitHub 網頁介面，將這個檔案 push 到你剛剛建立的儲存庫的 `main`（或 `master`）分支上。

如果是使用 Jekyll（像本站一樣），則是將 `_config.yml`、`_posts` 等相關檔案與資料夾推送到 GitHub。

## 第三步：前往 Settings 啟用 GitHub Pages

1. 在你的 Repository 頁面上，點擊上方的 **Settings** 頁籤。
2. 在左側選單中找到並點擊 **Pages**。
3. 在 **Build and deployment** 區塊中：
    - **Source**: 選擇 `Deploy from a branch`。
    - **Branch**: 選擇你的主要分支（通常是 `main` 或 `master`），資料夾請維持預設的 `/ (root)`（除非你有將網站檔案放進特定資料夾如 `docs`）。
4. 點擊 **Save** 儲存設定。

## 第四步：等待部署與瀏覽你的網站

儲存設定後，GitHub Actions 會自動開始建立並部署你的網站。這通常需要幾分鐘的時間。

你可以在 Repository 上方的 **Actions** 頁籤中查看部署進度。一旦顯示綠色勾勾完成，你就可以在瀏覽器中輸入你的網址：

`https://<你的帳號>.github.io/`

恭喜你！你的網站已經成功上線，全世界都可以造訪了！有了 GitHub Pages，你就能專注在內容創作與版面設計，把基礎設施的瑣事全部交給 GitHub 就好。
