---
title: Linux 网络性能工具-netperf
layout: post
comments: true
language: chinese
category: [linux]
keywords: iperf3
description: 网络性能测试工具
---

netperf是一种非常常见的测量网络带宽的工具。
<!-- more -->


## netperf安装
下载Netperf并安装，其安装后有两个工具，分别是服务端netserver和客户端netperf.
```
git clone https://github.com/esnet/iperf  
wget -c "https://codeload.github.com/HewlettPackard/netperf/tar.gz/netperf-2.5.0" -O netperf-2.5.0.tar.gz
tar -zxvf netperf-2.5.0.tar.gz
cd netperf-netperf-2.5.0
./configure && make && make install && cd ..
```


## 参数说明

netserver主要参数
```
-p    端口号
```
netperf主要参数
```
-H	指定 ECS 实例的 IP 地址。
-p	指定 ECS 实例的端口。
-l	指定运行时间。
-t	指定发包协议类型：TCP_STREAM 或 UDP_STREAM。建议使用 UDP_STREAM。
-m	指定数据包大小。测试 PPS 时，该值为 1。测试 bps（bit per second）时，该值为 1400。
```

详细参数
```
[root@panda ~]# netperf -h

Usage: netperf [global options] -- [test options] 

Global options:
    -a send,recv      Set the local send,recv buffer alignment
    -A send,recv      Set the remote send,recv buffer alignment
    -B brandstr       Specify a string to be emitted with brief output
    -c [cpu_rate]     Report local CPU usage
    -C [cpu_rate]     Report remote CPU usage
    -d                Increase debugging output
    -D [secs,units] * Display interim results at least every secs seconds
                      using units as the initial guess for units per second
    -f G|M|K|g|m|k    Set the output units
    -F fill_file      Pre-fill buffers with data from fill_file
    -h                Display this text
    -H name|ip,fam *  Specify the target machine and/or local ip and family
    -i max,min        Specify the max and min number of iterations (15,1)
    -I lvl[,intvl]    Specify confidence level (95 or 99) (99) 
                      and confidence interval in percentage (10)
    -j                Keep additional timing statistics
    -l testlen        Specify test duration (>0 secs) (<0 bytes|trans)
    -L name|ip,fam *  Specify the local ip|name and address family
    -o send,recv      Set the local send,recv buffer offsets
    -O send,recv      Set the remote send,recv buffer offset
    -n numcpu         Set the number of processors for CPU util
    -N                Establish no control connection, do 'send' side only
    -p port,lport*    Specify netserver port number and/or local port
    -P 0|1            Don't/Do display test headers
    -r                Allow confidence to be hit on result only
    -s seconds        Wait seconds between test setup and test start
    -S                Set SO_KEEPALIVE on the data connection
    -t testname       Specify test to perform
    -T lcpu,rcpu      Request netperf/netserver be bound to local/remote cpu
    -v verbosity      Specify the verbosity level
    -W send,recv      Set the number of send,recv buffers
    -v level          Set the verbosity level (default 1, min 0)
    -V                Display the netperf version and exit

For those options taking two parms, at least one must be specified;
specifying one value without a comma will set both parms to that
value, specifying a value with a leading comma will set just the second
parm, a value with a trailing comma will set just the first. To set
each parm to unique values, specify both and separate them with a
comma.

* For these options taking two parms, specifying one value with no comma
will only set the first parms and will leave the second at the default
value. To set the second value it must be preceded with a comma or be a
comma-separated pair. This is to retain previous netperf behaviour.
[root@panda ~]# netserver -h

Usage: netserver [options] 

Options:
    -h                Display this text
    -D                Do not daemonize
    -d                Increase debugging output
    -f                Do not spawn chilren for each test, run serially
    -L name,family    Use name to pick listen address and family for family
    -N                No debugging output, even if netperf asks
    -p portnum        Listen for connect requests on portnum.
    -4                Do IPv4
    -6                Do IPv6
    -v verbosity      Specify the verbosity level
    -V                Display version information and exit

```


## 用法示例

server端
```
netserver -p 11256
```

客户端
测试PPS
```
netperf -H 192.168.1.20 -p 11256 -t UDP_STREAM -l 300 -- -m 1
```
测试带宽（bps）
```
netperf -H 192.168.1.20 -p 11256 -t UDP_STREAM -l 300 -- -m 1400
```




















