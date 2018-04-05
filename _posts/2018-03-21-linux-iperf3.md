---
title: Linux 网络性能诊断工具-iperf3
layout: post
comments: true
language: chinese
category: [linux]
keywords: iperf3
description: 网络性能测试工具
---

iperf3是一个网络性能测试工具。iperf3可以测试TCP和UDP带宽质量、测量最大TCP带宽，具有多种参数和UDP特性。iperf3可以报告带宽，延迟抖动和数据包丢失。利用iperf这一特性，可以用来测试一些网络设备如路由器，防火墙，交换机等的性能。
<!-- more -->


## iperf 安装
使用yum源安装
```
yum install iperf3 -y  
```

使用源码安装
```
git clone https://github.com/esnet/iperf  
cd iperf  
./configure && make && make install && cd ..  
cd src  
ADD_PATH="$(pwd)"  
PATH="${ADD_PATH}:${PATH}"  
export PATH  
```


## 主要参数说明

主要参数说明
```
-s	表示作为 server 端接收包。  
-i	间隔多久输出信息流量信息，默认单位为秒。  
-p	指定服务的监听端口。  
-u	表示采用 UDP 协议发送报文，不带该参数表示采用 TCP 协议  
-l	表示包大小，默认单位为 Byte。通常测试 PPS 的时候该值为 16，测试 bps 时该值为 1400。  
-b	设定流量带宽，可选单位包括：k/m/g。  
-t	流量的持续时间，默认单位为秒。  
-A	CPU 亲和性，可以将具体的 iperf3 进程绑定对应编号的逻辑 CPU，避免 iperf 进程在不同的 CPU 间调度。  
```

详细帮助信息
```
[root@panda ~]# iperf3 -h  
Usage: iperf [-s|-c host] [options]  
       iperf [-h|--help] [-v|--version]  

Server or Client:  
  -p, --port      #         server port to listen on/connect to  
  -f, --format    [kmgKMG]  format to report: Kbits, Mbits, KBytes, MBytes  
  -i, --interval  #         seconds between periodic bandwidth reports  
  -F, --file name           xmit/recv the specified file  
  -A, --affinity n/n,m      set CPU affinity  
  -B, --bind      <host>    bind to a specific interface  
  -V, --verbose             more detailed output  
  -J, --json                output in JSON format  
  -d, --debug               emit debugging output  
  -v, --version             show version information and quit  
  -h, --help                show this message and quit  
Server specific:  
  -s, --server              run in server mode  
  -D, --daemon              run the server as a daemon  
  -1, --one-off             handle one client connection then exit  
Client specific:  
  -c, --client    <host>    run in client mode, connecting to <host>  
  -u, --udp                 use UDP rather than TCP  
  -b, --bandwidth #[KMG][/#] target bandwidth in bits/sec (0 for unlimited)  
                            (default 1 Mbit/sec for UDP, unlimited for TCP)  
                            (optional slash and packet count for burst mode)  
  -t, --time      #         time in seconds to transmit for (default 10 secs)  
  -n, --bytes     #[KMG]    number of bytes to transmit (instead of -t)  
  -k, --blockcount #[KMG]   number of blocks (packets) to transmit (instead of -t or -n)  
  -l, --len       #[KMG]    length of buffer to read or write  
                            (default 128 KB for TCP, 8 KB for UDP)  
  -P, --parallel  #         number of parallel client streams to run  
  -R, --reverse             run in reverse mode (server sends, client receives)  
  -w, --window    #[KMG]    set window size / socket buffer size  
  -C, --linux-congestion <algo>  set TCP congestion control algorithm (Linux only)  
  -M, --set-mss   #         set TCP maximum segment size (MTU - 40 bytes)  
  -N, --nodelay             set TCP no delay, disabling Nagle's Algorithm  
  -4, --version4            only use IPv4  
  -6, --version6            only use IPv6  
  -S, --tos N               set the IP 'type of service'  
  -L, --flowlabel N         set the IPv6 flow label (only supported on Linux)  
  -Z, --zerocopy            use a 'zero copy' method of sending data  
  -O, --omit N              omit the first n seconds  
  -T, --title str           prefix every output line with this string  
  --get-server-output       get results from server  
  
[KMG] indicates options that support a K/M/G suffix for kilo-, mega-, or giga-  
  
iperf3 homepage at: http://software.es.net/iperf/  
Report bugs to:     https://github.com/esnet/iperf  
```


## 用法示例

server端
```
[root@panda ~]# iperf3 -s  
-----------------------------------------------------------  
Server listening on 5201
-----------------------------------------------------------  
Accepted connection from 192.168.1.20, port 47399
[  5] local 192.168.1.20 port 5201 connected to 192.168.1.20 port 47400
[ ID] Interval           Transfer     Bandwidth
[  5]   0.00-1.00   sec   546 MBytes  4.58 Gbits/sec                  
[  5]   1.00-2.00   sec   557 MBytes  4.67 Gbits/sec                  
[  5]   2.00-3.00   sec   575 MBytes  4.83 Gbits/sec                  
[  5]   3.00-4.00   sec   573 MBytes  4.80 Gbits/sec                  
[  5]   4.00-5.00   sec   574 MBytes  4.81 Gbits/sec                  
[  5]   5.00-6.00   sec   563 MBytes  4.72 Gbits/sec                  
[  5]   6.00-7.00   sec   565 MBytes  4.74 Gbits/sec                  
[  5]   7.00-8.00   sec   572 MBytes  4.79 Gbits/sec                  
[  5]   8.00-9.00   sec   575 MBytes  4.83 Gbits/sec                  
[  5]   9.00-10.00  sec   565 MBytes  4.74 Gbits/sec                  
[  5]  10.00-10.04  sec  17.1 MBytes  3.53 Gbits/sec                  
- - - - - - - - - - - - - - - - - - - - - - - - -
[ ID] Interval           Transfer     Bandwidth
[  5]   0.00-10.04  sec  0.00 Bytes  0.00 bits/sec                  sender
[  5]   0.00-10.04  sec  5.55 GBytes  4.75 Gbits/sec                  receiver
-----------------------------------------------------------
Server listening on 5201
-----------------------------------------------------------
```

客户端
```
[root@panda ~]# iperf3 -c 192.168.1.20 -l 1500
Connecting to host 192.168.1.20, port 5201
[  4] local 192.168.1.20 port 47400 connected to 192.168.1.20 port 5201
[ ID] Interval           Transfer     Bandwidth       Retr  Cwnd
[  4]   0.00-1.00   sec   572 MBytes  4.80 Gbits/sec    0    416 KBytes       
[  4]   1.00-2.00   sec   558 MBytes  4.68 Gbits/sec    0    416 KBytes       
[  4]   2.00-3.00   sec   575 MBytes  4.83 Gbits/sec    0    416 KBytes       
[  4]   3.00-4.00   sec   573 MBytes  4.81 Gbits/sec    0    416 KBytes       
[  4]   4.00-5.00   sec   574 MBytes  4.81 Gbits/sec    0    416 KBytes       
[  4]   5.00-6.00   sec   563 MBytes  4.73 Gbits/sec    0    416 KBytes       
[  4]   6.00-7.00   sec   565 MBytes  4.74 Gbits/sec    0    592 KBytes       
[  4]   7.00-8.00   sec   572 MBytes  4.80 Gbits/sec    0    608 KBytes       
[  4]   8.00-9.00   sec   575 MBytes  4.82 Gbits/sec    0    608 KBytes       
[  4]   9.00-10.00  sec   559 MBytes  4.69 Gbits/sec    0    608 KBytes       
- - - - - - - - - - - - - - - - - - - - - - - - -
[ ID] Interval           Transfer     Bandwidth       Retr
[  4]   0.00-10.00  sec  5.55 GBytes  4.77 Gbits/sec    0             sender
[  4]   0.00-10.00  sec  5.55 GBytes  4.77 Gbits/sec                  receiver
```

