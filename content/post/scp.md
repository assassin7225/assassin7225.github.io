---
title: "SCP"
type: "post"
date: "2025-01-19T22:50:13+08:00"
categories: 
  - "筆記"
tags:
  - "linux command"
  - "scp"
---
想在不同的linux主機間複製檔案，可以用scp透過SSH把local端的檔案或目錄複製到遠端去，也可以把遠端的資料複製過來。

<!--more-->

遠端的主機要有開啟SSH遠端登入

## 語法

前面放source 後面放destination

`scp [帳號@src ip]:src file [帳號@dst ip]: dst file`

## 把local端的東西推到遠端

local端的東西在 `/Desktop/hello.txt`

遠端帳號 `user`

遠端ip `192.168.0.1`

遠端檔案 `/~/ringring.txt`

`scp /Desktop/hello.txt user@192.168.0.1:/~/ringring.txt`

## 把遠端的東西拉回local端

`scp user@192.168.0.1:/ringring.txt /Desktop/ringring.txt`

## 複製整個目錄和底下的檔案

加上-r  就可以copy整個目錄底下所有東西

`scp -r /Desktop/ user@192.168.0.1:/~/`

## 資料壓縮

想傳快一點可以先壓縮再送，加上-C

## 指定連結埠

加上-P

## Refrence

https://blog.gtwang.org/linux/linux-scp-command-tutorial-examples/