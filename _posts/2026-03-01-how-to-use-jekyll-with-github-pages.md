---
layout: post
title: "如何在 GitHub Pages 上使用 Jekyll 建立靜態部落格"
date: 2026-03-01
categories: [github, jekyll, blog]
summary: "GitHub Pages 原生支援 Jekyll，可以讓你輕鬆透過 Markdown 攥寫文章並自動產生靜態網頁。這篇文章將帶你了解如何設定 Jekyll 結構，以及如何在本地端測試你的部落格。"
---

在了解如何啟用 GitHub Pages 後，你可能會發現直接寫 HTML 效率太低，也不利於管理大量的部落格文章。

這時，**Jekyll** 就能派上用場！Jekyll 是一個簡單的、支援部落格的靜態網站產生器 (Static Site Generator)。最棒的是，**GitHub Pages 原生完美支援 Jekyll**。當你上傳 Markdown 檔案或 Jekyll 專案時，GitHub 會自動在背景執行編譯，幫你轉成最終的靜態佈局組合網頁。

這篇文章將為你介紹如何在 GitHub Pages 上搭配 Jekyll 來建立及管理文章。

## 什麼是 Jekyll？

Jekyll 會讀取包含特定格式（Front Matter）的 Markdown 或 HTML 檔案，搭配自訂的版面配置（Layout / 主題），最後產生出一個完整的靜態網站。這樣你就可以專注於用 Markdown 寫文章內容，而不需要繁瑣地維護每個 HTML 頁面的頭尾及框架。

## 1. 準備基礎結構

在你的 GitHub Pages 儲存庫 (Repository) 中，通常需要建立以下幾個基礎檔案與資料夾來初始化 Jekyll：

- `_config.yml`：Jekyll 的核心設定檔。可以設定網站標題、描述、自訂變數與外掛 (plugins)。
- `_posts/`：存放所有部落格文章 Markdown 檔案的地方。
- `_layouts/`：存放網頁版面基礎結構設計檔（如 `default.html`, `post.html`）。
- `_includes/`：(選用) 存放可重複使用的 HTML 片段（如 Navbar、Footer）。

一個最簡單的 `_config.yml` 範例：

```yaml
title: 我的技術部落格
description: 記錄生活與程式開發的點滴
markdown: kramdown
```

## 2. 撰寫你的第一篇文章

Jekyll 規定文章必須放在 `_posts` 資料夾，且檔名格式必須固定為：
`YYYY-MM-DD-title.md` (例如：`2026-03-01-hello-jekyll.md`)

打開檔案後，必須在文件最上方加入 **Front Matter** (一段 YAML 格式的區塊)，這是 Jekyll 解析版面以及取得文章元資料的重要區塊：

```markdown
---
layout: post
title: "我的第一篇 Jekyll 文章"
date: 2026-03-01
categories: [jekyll, blog]
---

這是文章的內文，你可以盡情使用 **Markdown** 語法！

- 支援列表
- 支援 `程式碼`
- 支援圖片連結
```

一旦你將 `_posts` 裡的檔案與專案一起推送到 GitHub 的主要分支之後，GitHub Actions 就會自動觸發並幫你建立、發布網頁！

## 3. 在本地端預覽你的網站

每次寫完文章都要等 GitHub Actions 執行完才能看到結果，在排版調整時實在太花時間了！你可以安裝 Jekyll 在你的電腦上，就能夠即時預覽 (Live Reload)。

### 安裝前置作業
要運行 Jekyll，你需要：
1. 安裝 **Ruby** 執行環境與開發套件 (DevKit)。
2. 開啟終端機 (Terminal / 打開命令提示字元)，安裝 Jekyll 與 Bundler：
   ```bash
   gem install jekyll bundler
   ```

### 啟動本地伺服器
在你的網站專案目錄下，先安裝依賴套件：
```bash
bundle install
```

接著啟動測試伺服器：
```bash
bundle exec jekyll serve
```
*(注意：如果你在 `_config.yml` 有更改過預設通訊埠，例如設為 `port: 4001`，伺服器就會啟動在 4001 port)*

預設情況下，只要在瀏覽器輸入 `http://localhost:4000`（或你自定的 port），就能看到網站的效果了！只要你的伺服器正在執行，在你修改並儲存 Markdown 檔案時，畫面就會自動重新產生並整理。

## 結語

結合 GitHub Pages 與 Jekyll，你擁有了一套強大、免費又容易做版本控制的寫作系統。你可以完全自主掌控所有內容的備份。準備好開啟你的技術分享之旅了嗎？趕快動手寫下你的文章吧！
