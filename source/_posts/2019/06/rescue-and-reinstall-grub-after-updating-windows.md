---
layout: post
title: 修復微軟更新後損毀的 Grub
date: 2019-05-31
updated: 2019-05-31
categories:
tags: [Linux, Windows, Grub]
---

# 前言

除了 Macbook Pro 之外的工作機，我通常會安裝 Microsoft Windows 10 與 Manjaro Linux 雙系統，並且使用 Grub 這個最為被廣泛使用的啟動引導工具來引導每次啟動時要進入的作業系統。但每次只要是重大更新之後，可能因為原有磁區位置編號改變，使得開機選單損壞進入到 grub rescue 模式：

```
error: unknown filesystem
Entering rescue mode..
grub rescue>
```

<!--more-->

# 解決辦法

## 查找原有的 Linux 系統引導

首先必須找到原有系統和啟動磁區，使用 `ls` 查看當前所有磁區，並逐個嘗試找到原有的 Linux 位置：

```
grub rescue> ls
(hd0) (hd0,gpt6) (hd0,gpt5) (hd0,gpt4) (hd0,gpt3) (hd0,gpt2) (hd0,gpt1)

grub rescue> ls (hd0,gpt4)/
error: unknown filesystem

grub rescue> ls (hd0,gpt5)/
error: unknown filesystem

grub rescue> ls (hd0,gpt6)/
./ ../ bin/ boot/ dev/ etc/ ...
```

## 配置救援環境並啟動 Linux

使用 `set` 指令查詢當前的啟動設置：

```
grub rescue> set
cmdpath=(hd0,gpt2)/EFI/Manjaro/
prefix=(hd0,gpt7)/boot/grub
root=hd0,gpt7
```

其中 `prefix` 指定的是啟動引導程式 Grub 的安裝目錄，而 `root` 則是系統的根目錄，但當前的磁區是錯誤的，必須修正為正確的磁區，接著切換至 normal 模式並啟動：

```
grub rescue> set prefix=(hd0,gpt6)/boot/brub
grub rescue> set root=(hd0,gpt6)
grub rescue> insmod normal
grub rescue> normal
```

## 重新安裝 Grub 並更新開機選單

```bash
# 確認 mount 的位置
$ df

# 重新安裝 Grub
$ sudo grub-install /dev/sda2

# 更新開機選單
$ sudo update-grub
```

# 參考資料

- [哈哈餐館 | 修復 GRUB：Win10 1709 秋季創意者更新導致 Linux 雙系統無法引導](https://blog.csdn.net/aaazz47/article/details/78549409)
- [Open Jiang | 雙系統升級 Windows 10 後造成 ubuntu 開機進入 grub rescue](http://jeffyon.blogspot.com/2016/08/windows-10-ubuntu-grub-rescue.html)