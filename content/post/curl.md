---
title: "Curl"
type: "post"
date: "2025-01-15T12:36:33+08:00"
categories: 
  - "筆記"
tags:
  - "Curl"
---

> 不想裝postman GUI工具的時候還可以用強大的curl

<!--more-->

## What's curl?

在linux用http下載/上傳檔案的指令

(wget只能下載)

基本格式如下

`curl [options] [URL]`

response拿到的會根據URL的內容不同而不同(好像很正常)，可能是HTML/XML/JSON

`curl https://www.google.com`

## 下載檔案

指定檔名為duck.jpg

`curl -o duck.jpg URL`

`curl URL --output duck.jpg`

不改檔名直接載下來存

`curl -O URL`

下載的時候被中斷可以從中斷的地方繼續

`curl -C -O URL`

## 轉網址

要跟網址301/302 redirect的話用-L

`curl -L http:/google.com`

## HTTP request

```html
-X or --request [GET|POST|PUT|DELETE|PATCH] 指定http method
-H or --header 設定request帶的header
-i or --include 在output顯示response的header
-d or --data 攜帶HTTP post data
-v or --verbose 輸出更多的debug msg
-u or --user 設定使用者帳密
-b or --cookie 攜帶cookie
```

要看詳細HEAD回傳的東西

若網頁有使用Basic Authentication，可以用—user指定帳號密碼

`curl --HEAD -k --user account:password URL`

## Reference

https://blog.techbridge.cc/2019/02/01/linux-curl-command-tutorial/

http://www.compciv.org/recipes/cli/downloading-with-curl/