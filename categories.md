---
layout: default
title: Categories
---

<style type="text/css"><!-- p {text-indent: 2em;} --></style>

## 分类

该页面开头是主要分类及其介绍，主要是可以根据分类进行查询，后面会有根据标签（tags）的全部文章的分类，和侧边相似。

----------------------------------------------------------------------------------------------------
----------------------------------------------------------------------------------------------------

#### Linux
![Linux Logo]({{ site.url }}/images/linux/linux-logo.jpg "Linux Logo"){: .pull-center}

Linux 相关，包括了常用的方法，以及相应的内核介绍。

----------------------------------------------------------------------------------------------------
----------------------------------------------------------------------------------------------------

#### MySQL
![MySQL Logo]({{ site.url }}/images/mysql/mysql-mariadb-percona-logo.png "MySQL Logo"){: .pull-center}

MySQL 是一款最流行的开源关系型数据库，最初由瑞典的 MySQL AB 公司开发，目前已被 Oracle 收购，现在比较流行的开源分支包括了 MariaDB 和 Percona。

其中 MariaDB 由 MySQL 创始人 Michael Widenius 主导开发，主要原因之一是：Oracle 收购了 MySQL 后，有将 MySQL 闭源的潜在风险，因此社区采用分支的方式来避开这个风险。为了与原 MySQL 区分，不再使用原来的版本号，而是采用新的 10.0。

Percona 是最接近官方 MySQL Enterprise 发行版的版本，也就是说它提供了一些 MySQL 企业版采用的功能，并且包括了一些比较好用的常用工具。其中的缺点是，为了确保对产品中所包含功能的控制，他们自己管理代码，并不接受社区开发人员的贡献。


----------------------------------------------------------------------------------------------------
----------------------------------------------------------------------------------------------------

#### Python

![Python Logo]({{ site.url }}/images/python/python-logo.png "Python Logo"){: .pull-center width="420"}

通常当我们讨论 Python 时，指的是 Python 语言以及 CPython 实现。而实际上 Python 只是一种语言的规范，可以根据该规范使用不同的语言去实现相应的解析器，除了 CPython 之外，常见的还有 PyPy、Jython、IronPython、MicroPython 等。

对于传统语言，如 C/C++ 等，会直接将代码编译为机器语言后运行，而对于不同的平台或者 CPU 需要重新编译才可以，而 Python 可以直接跨平台运行。

CPython 通过 C 语言实现，也是目前使用最为广泛的版本，虽然 PyPy 现在的发展势头不错，不过估计短时间内还是不会替代 CPython。CPython 也需要编译 (编译成字节码)，然后运行，其核心实际上是一个字节码解析器 (Bytecode Interpreter)，用于模拟堆栈操作，或者称之为 Virtual Stack Machines。

如果没有特殊说明的话，在此特指 CPython；另外，比较想提一下的是 MicroPython，这是一个用于微控制器的 Python 实现 ^_^

Just More Pythonic ~~~

## Tags

{% for category in site.categories %}
<h3 id="{{ category | first }}">{{ category | first }}</h3>
<ul>{% for post in category[1] %}<li>{{ post.date | date: "%Y-%m-%d" }} <a href="{{post.url}}">{{ post.title }}</a></li>{% endfor %}</ul>
{% endfor %}
