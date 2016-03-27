---
title: How a Linux kernel schedules processes-part1
date: 2016-03-27 13:39:25
tags:
	- linux
	- kernel
	- scheduler
	- 排程
	- 核心
	- Linux Kernel Development
categories:
	- tech
---
# Intro.
We know that systems can benefits from Multitasking, On a single processor machine, this give us the illusion of multiple processes are running concurrently. On a multiprocessor machine, multitasking actually enables multiple processes run at the same time, in parallel.

# Linux's Process Scheduler
During the kernel version from 2.5 ~ 2.6.23, the scheduler adpoted was O(1) scheulder. However, this scheduler has several failures when scheduling latency-sensitve applications, in other words, it is not suitable for real-time computing. The replacement of O(1) scheduler occured in version 2.6.23. 
> https://en.wikipedia.org/wiki/O(1)_scheduler
> https://en.wikipedia.org/wiki/Real-time_computing

The new scheduler is CFS(Completely Fair Scheduler). Before talking about CFS's policy, we have to know how to decide process' prioirty first...

# Process Priority

The Linux kernel implements two seperate priority ranges. 

- **NICE**, from -19 to 20, with a default of 0.

Large nice values means a lower prority(Being nice to others) and vice versa. Nice values are the standard priority range used in all Unix systems, although different Unix systems apply them differently.

> `$ ps -el` Use this command to check the nice values of your processes

- **REAL-TIME PRIORITY**, from 0 to 99.
Different from nice values, higher prioirty values indicates a greater priority and vice versa.

> `$ ps -eo state,uid,pid,ppid,rtprio,time,comm.`
> See the rtprio column. A value of “-” means the process is not real-time.

CFS's policy is left to part2...

# References
- [Linux Kernel Development](http://www.amazon.com/Linux-Kernel-Development-3rd-Edition/dp/0672329468)