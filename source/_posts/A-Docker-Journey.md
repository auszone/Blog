title: A Docker Journey
date: 2015-08-22 20:42:54
tags: [docker]
categories: [tech]
---
## Intro.
最近因爲deploy的需求才讓我接觸到了Docker，說到Docker這個專案，相比大家一定對它有點耳熟，它原本是*dotCloud*這間公司的專案，但是因為Docker實在是太火紅了就連*dotCloud*都改名成為*Docker Inc*了，Docker現在已經加入了Linux基金會並且遵從了 [Apache 2.0](http://www.apache.org/licenses/LICENSE-2.0) 協議，原始碼在 GitHub 上進行維護。

## A Docker Journey
以下是我做的一個Repo裡面有Docker的基本介紹Slides，還有一個小小的Lab，下面的Demo所需要的東西都在這個Repo裡面。

[ADockerJourney](https://github.com/auszone/ADockerJourney)

## Installation

大家就看 [這裡](https://docs.docker.com/installation/) 來進行安裝吧！

## Demo

- 動手寫一份簡單的Dockerfile，給一個node.js server

```
FROM node:latest
RUN mkdir -p /webapp
COPY . /webapp
RUN cd /webapp; npm install
EXPOSE 8080
CMD ["node", "/webapp/index.js"]
```
我們使用 `node:latest` 當我們的base image就可以很快速的得到一個node環境，然後我們接下來建立一個資料夾叫做 `/webapp`，再來利用`COPY`放我們的 `index.js` 還有 `package.json`，然後很重要的是要`EXPOSE 8080`這個用意是在開啟一個對外溝通的管道，不一定是要8080只是`index.js`裡面聽的是8080port，`CMD ["node", "/webapp/index.js"]`這個用途是在當我們建立一個Container卻沒有給它對應的指令時的預設指令。
接下來只要執行 `docker build -t "auszone/myserver" -f dockerfile .`就可以把這個Dockerfile，build成一個Image了！

- 開啟一個MySQL Container

`docker run --name demodb -e MYSQL_ROOT_PASSWORD=password -d mysql:latest`
利用以上的指令就可以很快速的開啟一個MySQL服務！

- 部署這個node.js server 並且連到MySQL這個Container

`docker run --name server --link demodb:mysql -d -p 8080:8080 auszone/myserver`

這裏值得一提的是`demodb:mysql`這個`mysql`用途是在當link一個container的時候提供的`alias`，我這裡被link的對象是剛剛開啟的MySQL Container我們有給他一個名字叫做`demodb`，那這個`alias`用途何在呢？ 原因是要是我們有個這個別名的話我們開啟的server Container裡面的環境變數就會以這個`mysql`當作前綴，也就是說我們`index.js`裡面的 host `MYSQL_PORT_3306_TCP_ADDR` 這個環境變數就是這麼來的，一開始的`MYSQL`就是這個前綴，所以我們可以發現在用link的時候其實Docker就是幫你加一些環境變數，好讓你可以找到另外一個Container來與它溝通！

這個時候打開你的瀏覽器`127.0.0.1:8080`相信就會server成功連上MySQL的訊息這時候你就成功了！

**Note1:`127.0.0.1`是給linux的，如果是OS X 或是 Windows的使用者就要找到你的linux 虛擬機的ip就行了！**

**Note2:要是有很多個Container還這樣link不是很累嗎？沒錯！以上只是一個小小的教學，然而遇到這種情況的時候這時候就用Docker Compose吧！詳見Reference.**

___

> 如果大家有什麼其他想法歡迎跟我討論，或是以上內容有謬誤也請告訴我！Email: auszon3@gmail.com
 

## Reference

- [ Docker Offcial site ](https://docker.com)
- [《Docker —— 從入門到實踐­》](http://philipzheng.gitbooks.io/docker_practice/content/introduction/what.html) 很棒的Docker指南，繁體中文太甘心了！
- [Introduction to Docker(Youtube)](https://www.youtube.com/watch?v=Q5POuMHxW-0) 一個由*Docker Inc* CTO Solomon Hykes給的talk
- [Docker Compose](https://docs.docker.com/compose/)