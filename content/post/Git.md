---
title: "Git"
type: "post"
date: "2025-01-14T00:12:45+08:00"
categories: 
  - "筆記"
tags:
  - "Git"
---

算是自己用git遇到的情境跟設定紀錄

<!--more-->

---
## remote

加遠端repository

`git remote add ssh ssh://github.com/Username/repository.git`

列出遠端repository列表

`git remote -v`

更換遠端repository位子

`git remote set-url origin https://github.com/Username/repository.git`

刪除遠端repository ssh

`git remote rm ssh`

## ssh-key

懶得打密碼可以設定ssh-key讓server認得這台機器

在本機打mail來gen ssh key

`ssh-keygen -t rsa -C "username@.com"`

然後把ssh key倒出來

`cat /home/username/.ssh/id_rsa.pub`

 貼到server上的 `~/.ssh/authorized_keys`

把pub key貼進去存好，這樣推拉就不用打帳密了

## commit

### 單純存到local repository

`git commit -m "commit message"`

### 多行comment

若有多行要加上去，就只打一個"，然後開始打原本要的格式，

最後一行再加上"作為結尾

```c
git commit -m "This is line one (enter)
> and this is line two
> if you finish, just add quote"
```

### 剛commit完還要再多加檔案上去

`git commit --amend`

### 單純加檔案，不想改commit message (僅適用在最新的那個commit)

`git commit --amend --no-edit`

### 單純修改commit message，但其他檔案沒變

`git commit --amend -m "New comment here`"

### commit檔案的一部分上去(慢慢編輯)

`git add -p <file>`

```c
> **Staging Patches**
> 

> `Stage this hunk [y,n,a,d,/,j,J,g,e,?]? ?
y - stage this hunk
n - do not stage this hunk
a - stage this and all the remaining hunks in the file
d - do not stage this hunk nor any of the remaining hunks in the file
g - select a hunk to go to
/ - search for a hunk matching the given regex
j - leave this hunk undecided, see next undecided hunk
J - leave this hunk undecided, see next hunk
k - leave this hunk undecided, see previous undecided hunk
K - leave this hunk undecided, see previous hunk
s - split the current hunk into smaller hunks
e - manually edit the current hunk
? - print help`
> 
```

### 取消commit (回not staged)

`git reset HEAD <file>`

`git restore --staged <file>`

### 取消tracking (沒有要刪除檔案但是也不讓git追蹤)

`git rm --cached <file>`

若沒下--cached的話會跟rm的效果一樣被刪掉

### 純粹留個紀錄沒有要更新檔案

`git commit --allow-empty -m "this is empty commit"`

### 想改部分commit message

`git commit --amend`

就會跳視窗出來讓你改完再存檔即可

# pull

拉最新的下來，如果local端有比較新的東西請剪下來再貼在更新的東西之後

`git pull --rebase`

拉特定遠端repository的 branchA，或著是local某個branch需要更新遠端的branch下來

remote的repository: origin

`git pull origin branchA`

# rebase

目前在branchA，已經有一個commit，希望把branchB的新東西加過來

在branch A下

`git rebase branchB`

這樣branchB的新東西就會在branchA，而且branchA的commit會接在新東西之後

### 改變commit之間的順序

先找到要從哪個commit之後要移動順序

`git rebase -i bb0c9c2`

在rebase狀態看到的紀錄跟平常看的紀錄是反過來﹑越新的commit在越下面

改完之後存檔就ok了 (vim `:m 2` 表示這行往下移兩行 `:m-2` 表示這行往上移一行 (自己是-1))

### 離開detached HEAD

通常發生在看完git reflog再rebase飛回特定commit的時候會發生

這時只要指回某個特定branch就好

`git checkout master`

如果是原本的改爛，rebase回前面的再開一個新的宇宙(誤)也會

這時候幫他開一個branch給他指著即可 (可明確的點出這個branch要指向特定commit)

`git branch tmp b6d204e`

## push

### 推特定branch到遠端

remote的repository: origin

local端branch: 1042

remote端branch: 1.0.14.1042

local和remote的名字用分號隔開

`git push origin 1042:1.0.14.1042`

## TAG

### 單純貼tag在最新的commit上，沒有說明

`git tag testfw9142`

### 貼tag在特定commit上

`git tag testfw9142 51d54ff`

### 貼tag+說明

`git tag -a 9041 -m "comment"`

### 列出特定tag

`git show testfw9142`

### 列出現在所有tag

`git tag -l`

### 推tag到遠端

tag名稱9041

`git push origin 9041`

### 刪除遠端tag (aka 推一個空白的tag到remote位置)

`git push origin :refs/tags/my_tag`

## Alias

設定co 當別名

git co 等同於 git checkout

`git config --global alias.co checkout`

## 查特定作者的commit

要查的名字沒有空白也可以不用雙引號括起來

`git log --author="hana"`

## Cherry Pick (需要先查好commit的hash值)

把別的branch的commit單獨撿過來放(可保留原作者commit的時間)

`git cherry-pick 51d54ff`

如果撿過來之後code有衝突，就先把衝突都消完再(或看git status怎麼說)

`git cherry-pick --continue`

## 衝突發生之後用他們的 用我們的 (rebase/cherry pick/merge適用)

明確是要以對方的code為準來解conflict

`git checkout --theirs filename`

以自己為準

`git checkout --ours filename` 

## branch

### rename branch

`git branch -m <oldname-branch> <newname-branch>`

這邊是改動local端的branch名稱

### rename remote branch

local端的已經改好了，再把他推到remote去，就會看到branch name已經改好了，然後commit還在

`git push origin <newname-branch>:<oldname-branch>`

### delete branch

`git branch -d <branch-name>`

如果有還沒merge的修改，要強制刪掉的

`git branch -D <branch-name>`

### delete remote branch

`git push origin :<branch-name>`

換句話說，你推了一個空白的branch到遠端的branch去取代他 aka 刪掉遠端的branch

(你把遠端的branch刪掉了)

### 看遠端的branch有哪些

`git branch -r`

### 若local端沒有存在遠端的那個branch，新增一個並且切過去+追蹤

`git checkout -t origin/firmware`

或是直接省略origin，一樣也會幫你建好追蹤上游+建好branch

`git checkout firmware`

## squash (合併多commit)

把多個commit合併成一個commit

先rebase到要squash的前一個

`git rebase -i bb0c9c2`

接著出現的紀錄從上到下是最舊到最新 (跟git log反過來)

```c
pick 382a2a5 add database settings
pick cd82f29 add cat 1
pick 2bab3e7 add dog 1
squash 27f6ed6 add dog 2
```

這邊add dog2會跟前面那個add dog1合併

主要的commit日期跟message會以pick的那個為主

被squash的會消失

## 在windows上一直有一堆stash hard reset checkout都擺脫不掉的一堆檔案變更 (但又沒看到什麼diff明顯的東西)

如果不是crlf搞的鬼，那就是上面列的問題

主因是權限變更，導致他判斷你有更改檔案

`git config core.filemode false`

## 在linux上一直有一堆stash hard reset checkout都擺脫不掉的一堆檔案變更 (但又沒看到什麼diff明顯的東西)

建議直接先留著autocrlf true

先把那堆檔案add之後 commit

然後繼續做自己的事情 但要記得有這個temp commit

再把autocrlf false

## 設定換行符號

- git 設定檔:
    - git config --global core.autocrlf input
        - true: 表示 checkout 轉換成 CRLF，提交時轉換成 LF
        - input: 表示 checkout 不轉換，提交時轉換成LF
        - false: 表示不轉換
    - git config --global core.safecrlf true
        - true: 表示不允許提交時包含不同換行符號
        - warn: 在有不同換行符號存在時警告
        - false: 則允許提交時有不同換行符號存在
    - 新增 .gitattributes
        - text eol=lf
    

### clone 只載特定branch develop

`git clone -b develop --single-branch ssh://git@172.1.2.3/opt/share.git`

[[Git] 讓 git clone 只複製特定的分支資料 | EPH 的程式日記 (ephrain.net)](https://ephrain.net/git-%E8%AE%93-git-clone-%E5%8F%AA%E8%A4%87%E8%A3%BD%E7%89%B9%E5%AE%9A%E7%9A%84%E5%88%86%E6%94%AF%E8%B3%87%E6%96%99/)

### 初始載特定branch，不要master

`git clone -b development`