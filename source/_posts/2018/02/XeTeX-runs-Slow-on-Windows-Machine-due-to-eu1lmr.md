---
layout: post
title: LaTeX | 解決 Windows 環境下 TexLive 編譯卡頓問題
date: 2018-02-13 03:46:25
updated: 2019-05-26
categories: 
tags: [LaTeX]
---

# 問題

如果微軟作業系統中所安裝字體有更動，不論新增或減少，則在 XeTeX 進行編譯時會重新生成字體的緩存檔案。當然這種狀況也可能發生在套用的字體名稱錯誤使得 XeTeX 找不到字體的時候。最常見到的情況就是卡在 `eu1lmr.fd`。在 TeXLive 2017 仍有這種現象，甚至有些使用者只要重新啟動電腦就會如此，用關鍵字稍微找一下就會發現很多人一樣有災情。

<!--more-->

# 解決方法

首先清空當前 TeXLive 安裝文件夾下 `~\texmf-var\fonts\cache` 的字體緩存，以默認路徑來說：

```bash
$ rm -rf C:\texlive\2017\texmf-var\fonts\cache
```

接著使用管理者權限打開終端機，切換至 TeXLive 安裝文件夾下的 `~\bin\win32` 目錄，執行 `fc-cache.exe` 重新生成字體緩存，並等到執行完成：

```bash
$ cd C:\texlive\2017\bin\win32
$ fc-cache -r -v
```

# 參考資料

1. [texlive 2016 被 eu1lmr.fd 卡得像死机一样的解决方案](http://saccohuo.com/2016/08/15/solution-to-texlive-2016-update-eu1mr-fd-stuck/)
2. [Is there a problem with XeTeX font cacheing?](http://tex.stackexchange.com/questions/327591/is-there-a-problem-with-xetex-font-cacheing/327638)
3. [XeLaTeX runs slow on Windows machine](http://tex.stackexchange.com/questions/325278/xelatex-runs-slow-on-windows-machine)
4. [LaTeX编译卡顿怎么办？](https://www.zhihu.com/question/51999238)
