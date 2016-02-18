title: A Docker Journey-Docker Compose
tags:
  - docker
categories:
  - tech
date: 2015-08-26 11:45:54
---


## Intro
[這裡](http://auszone.github.io/blog/A-Docker-Journey)介紹了Docker基本的使用方式，包含

- 寫一個簡單的Dockerfile
- Build一個Docker image
- Link 兩個Containers

### Trouble
但我們要數個Container做連結互動的時候，手動的link就會變的很麻煩，這時候要是可以寫一個檔案，裡面描述著Container之間該怎麼互動，然後我們就可以每次都使用這個檔案來建立我們的環境，這樣的話就太好了！

## Docker Compose
*Compose*是一個工具用來定義還有執行多個Cotainer的應用，有了Compose我們就可以定義一個使用多個Container的應用程式。
### Basic Steps
1. 把應用程式寫成一個`Dockerfile`如此一來我們就可以在每台機器上面都很快的建立我們的Image
2. 定義我們期望Container互動的方式在一個`docker-compose.yml`裡面
3. 最後執行`docker-compose up`，Compose就會開始並且執行你的服務。

一個`docker-compose.yml`長的像這樣：

~~~
web:
  build: .
  ports:
   - "5000:5000"
  volumes:
   - .:/code
  links:
   - redis
redis:
  image: redis
~~~
這份`docker-compose.yml`裡面描述著兩個container分別是

- web (參數都可以寫在裡面像是：從當下目錄的`Dockerfile`來build這個Image， 還有host的`port:5000` 對應 container的`port:5000`，要link到redis這個container等等)
- redis (只有給image，就是用redis這個image來開啟這個contianer，docker會自動幫你去找，如果找不到的話會回報錯誤。)

現在只要`docker-compose up`，Compose就會pull一個redis image，從你的`Dockefile`build一個image，然後把所有東西開起來。
## Installation
請看[這裡](https://docs.docker.com/compose/install/)來安裝。

## Get Your Hands Dirty
我們試著把我們[上次](http://auszone.github.io/blog/A-Docker-Journey)手動link兩個container的工作寫成一個`docker-compose.yml`，並且使用`docker-compose up`，把我們的服務跑起來。

**docker-compose.yml**

~~~
web:
    build: .
    ports:
    - "8080:8080"
    links:
    - mysql
mysql:
    image: mysql
    environment:
        MYSQL_ROOT_PASSWORD: password
~~~
只要加了這個檔案在目錄裡面就可以使用`docker-compose up`一個指令把我們的整個服務都開啟了，是不是很棒呢？

> 以上只介紹了docker-compose的基本使用案例，還有一些進階的功能像是extends等等，就請看reference了！

## Reference

- [Docker Compose Offical Site](https://docs.docker.com/compose/)
- [Docker Compose Doc](http://docs.docker.com/compose/)
