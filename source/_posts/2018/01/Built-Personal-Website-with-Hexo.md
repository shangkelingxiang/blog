---
layout: post
title: Hexo | 使用靜態頁面生成器搭建個人網站
date: 2018-01-05 03:57:33
updated: 2019-05-26
categories: 
tags: [Git, Hexo]
---

# 前言

一直以來都沒有一個地方可以好好存放自己的筆記和心得，趁著最近稍微有些時間也該好好來整理一下之前零散的東西。這篇文章不僅僅是記錄怎麼利用常見的靜態頁面生成器配合 Github Page 架設個人網站，也將提及個人對於筆記軟體（或者服務）的一些要求。

<!--more-->

# 功能需求

- **平台兼容**：作為一個手機使用 Android 系統、平板使用 ios 系統、筆電常駐 Windows 10 與 Arch Linux 的人而言，最希望的功能自然是能夠在不同平台上實現兼容與同步的功能。
- **書寫方便**：自從習慣使用 LaTeX 之後，對於排版的優雅與書寫的便利性開始漸漸有要求，我想對於多數長期敲代碼的人來說，都會希望能夠儘量不離開鍵盤又能完成要求的事物，這也是為什麼神之編輯器 `vim` 能夠有著至今無法撼動的地位。而 [**Markdown**](http://markdown.tw/) 的誕生，無疑是給了我們一線曙光。
- **代碼高亮**：身為一個長期必須看程式碼的人，沒有代碼高亮根本無法過活…因應不同語言與關鍵字上不同的顏色，能夠更清晰地閱讀程式碼，進而更快地理解消化內容。
- **數學公式**：不論是作業研究、最佳化還是演算法，這些與我息息相關的學科都離不開數學，因此數學式也是必須的…早年常使用鑲嵌圖片的方式容易造成排版困難且佔用空間，這幾年網頁前端技術的發展，像是 [**MathJax**](https://www.mathjax.org/) 和 [**KaTeX**](https://khan.github.io/KaTeX/) 都提供了非常優秀的數學式渲染。

# 筆記軟體

以下是多年來曾經使用過的筆記軟體，或者是其構想讓我耳目一新的：

- [**EverNote**](https://evernote.com/)：作為曾經的獨角獸，至今似乎還是許多人紀錄筆記的首選。但缺點十分顯而易見：不支援數學式渲染、收費政策不如人意、在 Linux 平台下並不友善。
- [**WizNote**](https://www.wiz.cn/)：是我這麼多年來，第一個願意掏錢支持的應用程式，在各方面都令人十分滿意！唯一可惜的是當初曾經放下豪語提供終身免費，但卻開始收費，曾經在[知乎](https://www.zhihu.com/question/53429934)上引起一陣討論。著手收費後的政策其實十分合理，但似乎開發團隊的熱度有點下降…
- [**BoostNote**](https://boostnote.io/)：開發熱度十分高，提的 issue 通常很快就會被解決，號稱專門為工程師所開發的筆記軟體（The note-taking app for programmers that focuses on markdown, snippets, and customizability.）
- [**HackMD**](https://hackmd.io/)：臺灣團隊所開發維護，也算是個人平時使用度略高的服務之一，我覺得最為人詬病的就是「沒有文件夾分類功能」！
- [**LeaNote**](https://leanote.com/)：使用 Golang 開發，構想非常有趣，既可以是筆記又可以是部落格，曾經一度讓我以為找到了最佳的筆記平台，但可惜的是自訂性仍嫌遜色，另外收費也不太理想。
- [**Dynalist**](https://dynalist.io/)：類似 [**Workflowy**](https://workflowy.com/) 的工具，使用條列式來進行筆記，條列式間可以再進行縮放，我認為在處理零碎的知識點上十分方便。
- [**OneNote**](https://www.onenote.com/)：不太類似於上述工具，但如果配合 Surface 系列產品與手寫筆，能夠創造非常高效的生產力。

# 靜態頁面與靜態頁面渲染器

相較於常見的 **内容管理系统（CMS, content management system）** 如 [WordPress](https://wordpress.com/)、[Ghost](https://ghost.org)…等，靜態頁面不需要租借或架設伺服器來運行後台，以及維護資料庫系統。目前常見的靜態頁面渲染器包括但不限於：

- [Hexo](https://hexo.io/)
- [Hugo](https://gohugo.io/)
- [Pelican](https://blog.getpelican.com/)
- [Jekyll](https://jekyllrb.com/)
- [Octopress](http://octopress.org/)

隨著時代的變遷，後兩者由於當文件數量較多時，渲染速度較慢，已漸漸退出這個市場（雖然還是很多人在使用）。目前主要以 Hexo 和 Hugo 較多，前者基於 `node.js` 而後者基於 `Go`。而由於 Hugo 預設的 Markdown 渲染器採用 [blackfriday](https://github.com/russross/blackfriday)，常常在處理 MathJax 數學式時與 Markdown 語法相衝突，目前暫時不考慮，但他無疑是當前速度最快的靜態頁面渲染器，並且社群逐日在壯大中。

# 環境搭建

必須先具備的東西有：

- [Git](https://git-scm.com/)
- [Node.js](https://nodejs.org/)

## 創建代碼倉庫

目前 [**Github**](https://github.com/) 為每一個代碼 **倉庫（repository）** 都提供一個靜態頁面託管的 Github Page 功能，但個人網站必須使用 `<username>.github.io` 作為倉庫名稱，並且注意此網站只能以 `master` 分支進行發布（其他倉庫的靜態頁面則無此限制）。

## 安裝 hexo

```bash
$ npm install hexo-cli -g
$ hexo init hsins.github.io             # initialize a hexo site in document
$ cd hsins.github.io
$ npm install                           # install and update the demanding plugin
```

## 下載 [Next](https://github.com/theme-next/hexo-theme-next) 主題

```bash
$ git clone https://github.com/theme-next/hexo-theme-next ./theme/next
$ rm -rf ./theme/landscape              # delete the default theme "Landscape"
$ rm -rf ./theme/next/.git*             # delete the git repo of theme "NexT"
$ npm install hexo-deployer-git --save  # install the deployer plugin
```

## 修改設定值

- 修改 Hexo 本身設定：`./_config.yml` [(參考)](https://github.com/Hsins/hsins.github.io/blob/source/_config.yml)
- 修改 Next 主題設定：`./theme/next/_config.yml` [(參考)](https://github.com/Hsins/hsins.github.io/blob/source/themes/next/_config.yml)

## 推送至代碼倉庫

```bash
$ git init                              # initialize the git repo
$ git remote add origin https://github.com/Hsins/hsins.github.io.git
$ git add .
$ git commit -m 'First Update'
$ git branch source                     # establish the local branch "source"
$ git checkout source                   # change the branch to "source"
$ git push -u source                    # push the source to branch "source" on Github.com                    
```

# 撰寫文章

```bash
$ hexo clean
$ hexo new [layout] <title>             # layout can be "post", "page" and "draft"
$ hexo generate
$ hexo deploy                           # deploy the article to branch "master"

$ git add .
$ git commit -m "update"
$ git push
```

# 遠端同步

```bash
$ git clone https://github.com/Hsins/hsins.github.io.git
$ npm install
$ npm install hexo-deployer-git --save
$ git push origin source
```

# 數學渲染轉義問題

Hexo 預設使用 `hexo-renderer-marked` 引擎進行渲染，但對於底線、反斜線、中括號定義了轉義，容易與 MathJax 數學公式渲染時所處理的字符造成衝突，建議可以變更渲染引擎為下列選項中其中一個：

- `hexo-renderer-kramed`
- `hexo-renderer-pandoc`

```bash
$ npm uninstall hexo-renderer-marked --save     # uninstall the hexo-renderer-marked
$ npm install hexo-renderer-kramed --save       # install the hexo-renderer-kramed
```

除此之外，還需要改變行內公式的轉義設定。修改 `.\node_modules\kramed\lib\rules` 下的 `inline.js` 文件：

```js
// escape part
- escape: /^\\([\\`*{}\[\]()#$+\-.!_>])/,
+ escape: /^\\([`*\[\]()#$+\-.!_>])/,

// em part
- em: /^\b_((?:__|[\s\S])+?)_\b|^\*((?:\*\*|[\s\S])+?)\*(?!\*)/,
+ em: /^\*((?:\*\*|[\s\S])+?)\*(?!\*)/,
```

# 參考資料

1. [Hexo博客(3)源码备份及不同电脑上的同步问题](http://masikkk.com/blog/hexo-3-source-backup-and-sync-between-diff-computer/)
2. [在Github上面搭建Hexo博客（四）:使用不同电脑维护](http://mungo.space/2015/10/14/create-hexo-on-github-4/)
3. [Hexo利用Github分支在不同电脑上写博客](http://www.dxjia.cn/2016/01/27/hexo-write-everywhere/)
4. [不同电脑上同步Hexo Blog](http://kwangka.github.io/2015/01/17/how-to-synchronize-blog/)
5. [在Hexo中使用LaTeX语法输入数学公式](https://once4623.site/2017/10/03/2017-10-04--Use-MathJax-In-Hexo-Next/)