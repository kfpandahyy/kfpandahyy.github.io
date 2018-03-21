---
title: Linux 工具-pstack
layout: post
comments: true
language: chinese
category: [linux]
keywords: pstack
description: 此命令可显示每个进程的栈跟踪。
---

这个命令在排查进程问题时非常有用，比如我们发现一个服务一直处于work状态（如假死状态，好似死循环），使用这个命令就能轻松定位问题所在；可以在一段时间内，多执行几次pstack，若发现代码栈总是停在同一个位置，那个位置就需要重点关注，很可能就是出问题的地方；
<!-- more -->

## 示例

pstack 4551  
Thread 7 (Thread 1084229984 (LWP 4552)):  
\#0  0x000000302afc63dc in epoll_wait () from /lib64/tls/libc.so.6  
\#1  0x00000000006f0730 in ub::EPollEx::poll ()  
\#2  0x00000000006f172a in ub::NetReactor::callback ()  
\#3  0x00000000006fbbbb in ub::UBTask::CALLBACK ()  
\#4  0x000000302b80610a in start_thread () from /lib64/tls/libpthread.so.0  
\#5  0x000000302afc6003 in clone () from /lib64/tls/libc.so.6  
\#6  0x0000000000000000 in ?? ()  
Thread 6 (Thread 1094719840 (LWP 4553)):  
\#0  0x000000302afc63dc in epoll_wait () from /lib64/tls/libc.so.6  
\#1  0x00000000006f0730 in ub::EPollEx::poll ()  
\#2  0x00000000006f172a in ub::NetReactor::callback ()  
\#3  0x00000000006fbbbb in ub::UBTask::CALLBACK ()  
\#4  0x000000302b80610a in start_thread () from /lib64/tls/libpthread.so.0  
\#5  0x000000302afc6003 in clone () from /lib64/tls/libc.so.6  
\#6  0x0000000000000000 in ?? ()  
Thread 5 (Thread 1105209696 (LWP 4554)):  
\#0  0x000000302b80baa5 in __nanosleep_nocancel ()  
\#1  0x000000000079e758 in comcm::ms_sleep ()  
\#2  0x00000000006c8581 in ub::UbClientManager::healthyCheck ()  
\#3  0x00000000006c8471 in ub::UbClientManager::start_healthy_check ()  
\#4  0x000000302b80610a in start_thread () from /lib64/tls/libpthread.so.0  
\#5  0x000000302afc6003 in clone () from /lib64/tls/libc.so.6  
\#6  0x0000000000000000 in ?? ()  
Thread 4 (Thread 1115699552 (LWP 4555)):  
\#0  0x000000302b80baa5 in __nanosleep_nocancel ()  
\#1  0x0000000000482b0e in armor::armor_check_thread ()  
\#2  0x000000302b80610a in start_thread () from /lib64/tls/libpthread.so.0  
\#3  0x000000302afc6003 in clone () from /lib64/tls/libc.so.6  
\#4  0x0000000000000000 in ?? ()  
Thread 3 (Thread 1126189408 (LWP 4556)):  
\#0  0x000000302af8f1a5 in __nanosleep_nocancel () from /lib64/tls/libc.so.6  
\#1  0x000000302af8f010 in sleep () from /lib64/tls/libc.so.6  
\#2  0x000000000044c972 in Business_config_manager::run ()  
\#3  0x0000000000457b83 in Thread::run_thread ()  
\#4  0x000000302b80610a in start_thread () from /lib64/tls/libpthread.so.0  
\#5  0x000000302afc6003 in clone () from /lib64/tls/libc.so.6  
\#6  0x0000000000000000 in ?? ()  
Thread 2 (Thread 1136679264 (LWP 4557)):  
\#0  0x000000302af8f1a5 in __nanosleep_nocancel () from /lib64/tls/libc.so.6  
\#1  0x000000302af8f010 in sleep () from /lib64/tls/libc.so.6  
\#2  0x00000000004524bb in Process_thread::sleep_period ()  
\#3  0x0000000000452641 in Process_thread::run ()  
\#4  0x0000000000457b83 in Thread::run_thread ()  
\#5  0x000000302b80610a in start_thread () from /lib64/tls/libpthread.so.0  
\#6  0x000000302afc6003 in clone () from /lib64/tls/libc.so.6  
\#7  0x0000000000000000 in ?? ()  
Thread 1 (Thread 182894129792 (LWP 4551)):  
\#0  0x000000302af8f1a5 in __nanosleep_nocancel () from /lib64/tls/libc.so.6  
\#1  0x000000302af8f010 in sleep () from /lib64/tls/libc.so.6  
\#2  0x0000000000420d79 in Ad_preprocess::run ()  
\#3  0x0000000000450ad0 in main ()  


