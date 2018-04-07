---
Date: October 19, 2013
title: MySQL Percona Toolkit
layout: post
comments: true
language: chinese
category: [mysql]
keywords: Percona Toolkit
description: 常用MySQL工具集
---
percona开源的MySQL常用工具集。
<!-- more -->

## 安装
```
perl Makefile.PL
make
make test
make install
```

##各工具介绍
有32个工具，分7大类
<table border="1px" align="center" bordercolor="black" width="80%" height="100px">
    <tr align="center">
        <td>工具类别</td>
        <td>工具命令</td>
        <td>工具作用</td>
        <td>备注</td>
    </tr>
    <tr align="center">
        <td rowspan="5">开发类</td>
        <td>pt-duplicate-key-checker</td>
        <td>列出并删除重复的索引和外键</td>
		<td></td>
    </tr>
    <tr align="center">
        <td></td>
        <td>pt-online-schema-change</td>
        <td>在线修改表结构</td>
		<td></td>
    </tr>
    <tr align="center">
        <td></td>
        <td>pt-query-advisor</td>
        <td>分析查询语句，并给出建议，有bug</td>
		<td>已废弃</td>
    </tr>
    <tr align="center">
        <td></td>
        <td>pt-show-grants</td>
        <td>规范化和打印权限</td>
		<td></td>
    </tr>
    <tr align="center">
        <td></td>
        <td>pt-upgrade</td>
        <td>在多个服务器上执行查询，并比较不同</td>
		<td></td>
    </tr>
    <tr align="center">
        <td rowspan="4">性能类</td>
        <td>pt-index-usage</td>
        <td>分析日志中索引使用情况，并出报告</td>
		<td></td>
    </tr>
    <tr align="center">
        <td></td>
        <td>pt-pmp</td>
        <td>为查询结果跟踪，并汇总跟踪结果</td>
		<td></td>
    </tr>
    <tr align="center">
        <td></td>
        <td>pt-visual-explain</td>
        <td>格式化执行计划</td>
		<td></td>
    </tr>
    <tr align="center">
        <td></td>
        <td>pt-table-usage</td>
        <td>分析日志中查询并分析表使用情况</td>
		<td>pt 2.2新增命令</td>
    </tr>
    <tr align="center">
        <td rowspan="3">配置类</td>
        <td>pt-config-diff</td>
        <td>比较配置文件和参数</td>
		<td></td>
    </tr>
    <tr align="center">
        <td></td>
        <td>pt-mysql-summary</td>
        <td>对mysql配置和status进行汇总</td>
		<td></td>
    </tr>
	<tr align="center">
        <td></td>
        <td>pt-variable-advisor</td>
        <td>分析参数，并提出建议</td>
		<td></td>
    </tr>
	<tr align="center">
        <td rowspan="5">监控类</td>
        <td>pt-deadlock-logger</td>
        <td>提取和记录mysql死锁信息</td>
		<td></td>
    </tr>
	<tr align="center">
        <td></td>
        <td>pt-fk-error-logger</td>
        <td>提取和记录外键信息</td>
		<td></td>
    </tr>
	<tr align="center">
        <td></td>
        <td>pt-mext</td>
        <td>并行查看status样本信息</td>
		<td></td>
    </tr>
	<tr align="center">
        <td></td>
        <td>pt-query-digest</td>
        <td>分析查询日志，并产生报告</td>
		<td>常用命令</td>
    </tr>
	<tr align="center">
        <td></td>
        <td>pt-trend</td>
        <td>按照时间段读取slow日志信息</td>
		<td>已废弃</td>
    </tr>
	<tr align="center">
        <td rowspan="6">复制类</td>
        <td>pt-heartbeat</td>
        <td>监控mysql复制延迟</td>
		<td></td>
    </tr>
	<tr align="center">
        <td></td>
        <td>pt-slave-delay</td>
        <td>设定从落后主的时间</td>
		<td></td>
    </tr>
	<tr align="center">
        <td></td>
        <td>pt-slave-find</td>
        <td>查找和打印所有mysql复制层级关系</td>
		<td></td>
    </tr>
	<tr align="center">
        <td></td>
        <td>pt-slave-restart</td>
        <td>监控salve错误，并尝试重启salve</td>
		<td></td>
    </tr>
	<tr align="center">
        <td></td>
        <td>pt-table-checksum</td>
        <td>校验主从复制一致性</td>
		<td></td>
    </tr>
	<tr align="center">
        <td></td>
        <td>pt-table-sync</td>
        <td>高效同步表数据</td>
		<td></td>
    </tr>
	<tr align="center">
        <td rowspan="6">系统类</td>
        <td>pt-diskstats</td>
        <td>查看系统磁盘状态</td>
		<td></td>
    </tr>
	<tr align="center">
        <td></td>
        <td>pt-fifo-split</td>
        <td>模拟切割文件并输出</td>
		<td></td>
    </tr>
	<tr align="center">
        <td></td>
        <td>pt-summary</td>
        <td>收集和显示系统概况</td>
		<td></td>
    </tr>
	<tr align="center">
        <td></td>
        <td>pt-stalk</td>
        <td>出现问题时，收集诊断数据</td>
		<td></td>
    </tr>
	<tr align="center">
        <td></td>
        <td>pt-sift</td>
        <td>浏览由pt-stalk创建的文件</td>
		<td>pt 2.2新增命令</td>
    </tr>
	<tr align="center">
        <td></td>
        <td>pt-ioprofile</td>
        <td>查询进程IO并打印一个IO活动表</td>
		<td>pt 2.2新增命令</td>
    </tr>
	<tr align="center">
        <td rowspan="5">实用类</td>
        <td>pt-archiver</td>
        <td>将表数据归档到另一个表或文件中</td>
		<td></td>
    </tr>
	<tr align="center">
        <td></td>
        <td>pt-find</td>
        <td>查找表并执行命令</td>
		<td></td>
    </tr>
	<tr align="center">
        <td></td>
        <td>pt-kill</td>
        <td>Kill掉符合条件的sql</td>
		<td>常用命令</td>
    </tr>
	<tr align="center">
        <td></td>
        <td>pt-align</td>
        <td>对齐其他工具的输出</td>
		<td>pt 2.2新增命令</td>
    </tr>
	<tr align="center">
        <td></td>
        <td>pt-fingerprint</td>
        <td>将查询转成密文</td>
		<td>pt 2.2新增命令</td>
    </tr>
</table>
