---
title: Native C Application Development on LinkIt 7688
date: 2016-02-20 20:43:04
tags:
  - embedded system
  - LinkIt7688
categories:
  - tech
---
## Introduction

Developing Python program on 7688 is just like what we normally do on a standard computer because there is already installed Python runtime. However, making C/C++ application is different, because it's a compiled language, so we need to compile before we execute it. Normally, compilation that happens in the host is for generating executables for the host architecture and this is **native-compliation**.

However, do not forget that the program we write aims for run on the board, therefore we have to generate the executables for the board (guest). So to compile a program that runs on another platform is **cross-compilation**.

> It is of course doable to just installing a toolchain on the board, and then compile all programs on it. Well, that does sound sweet but it is not really a good way to do due to the hardware limitation.

## Prerequsites

- [Linux](https://wiki.openwrt.org/doc/howto/buildroot.exigence)
- [OSX](http://blog.ilay.tw/2015/07/24/在-osx-10-10-4-配置-openwrt-編譯環境/)

## Steps (Standard case)
1. - Download the SDK containing toolchain from the official site.
  - You can definitely also make from source! **[Repo](https://github.com/MediaTek-Labs/linkit-smart-7688-feed)**
2. Write your own C program and compile it.
3. Copy the executable from the host to the board.
4. Run the executable on the board.

## Steps: (Customization of the firmware)
1. Check Package the toolchain when `make menuconfig`
2. You have to compile your own toolchain from openwrt with the config.
3. Write your own C program and compile it.
4. Copy the executable from the host to the board.
5. Run the executable on the board.
