---
title: Hello World !!!
layout: post
comments: true
language: chinese
category: [misc]
keywords: hello world,示例,sample,markdown
description: 简单记录一下一些与 Markdown 相关的内容，包括了一些使用模版。
---

Hi, the world, I'm coooooooooming.

Oooops, just examples, ignore me, darling.

<!-- more -->

![hello world logo]({{ site.url }}/images/misc/hello-world-logo.jpg "hello world logo"){: .pull-center }

## 配色

<style type="text/css">
<!--
#colorbox span{display:block;float:left;height:195px;}
-->
</style>

静思

<div id="colorbox">
    <span style="width:125.2px;background-color: #81C2D6" alt="81C2D6">HEX<br>#81C2D6<br>RGB<br>129.194.214<br>CMYK<br>10.9.0.16</span>
    <span style="width:125.2px;background-color: #8192D6" alt="8192D6">HEX<br>#8192D6<br>RGB<br>129.146.214<br>CMYK<br>40.32.0.16</span>
    <span style="width:125.2px;background-color: #D9B3E6" alt="D9B3E6">HEX<br>#D9B3E6<br>RGB<br>217.179.230<br>CMYK<br>6.22.0.10</span>
    <span style="width:125.2px;background-color: #DCF7A1" alt="DCF7A1">HEX<br>#DCF7A1<br>RGB<br>220.247.161<br>CMYK<br>11.0.35.3</span>
    <span style="width:125.2px;background-color: #83FCD8" alt="83FCD8">HEX<br>#83FCD8<br>RGB<br>131.152.216<br>CMYK<br>48.0.14.1</span>
</div>
<div class="clearfix"></div>

## Heading 2，目录 2

### Heading 3， 目录 3

#### Heading 4，目录 4

##### Heading 5，目录 5

###### Heading 6， 目录 6


## MISC

### Separator，分割线

下面的效果是相同的。

* * *

***

*****

- - -

---------------------------------------




### The Fonts， 字体设置

*This is emphasized 斜体*.   _This is also emphasized 还是斜体_.

**Strong font 粗体** __Also strong font 还是粗体__

Water is H<sub>2</sub>O. 4<sup>2</sup>=16. 上标、下标测试。

Code Use the `printf()` function，代码模块。

Code Use the <code>printf()</code> function，与上面功能一样。

``There is a literal backtick (`) here.``，额，还是代码模块。

The New York Times <cite>(That’s a citation)</cite>. 引用测试，和斜体相似。

This is <u>Underline</u>. 下划线。


### Code Snippets，代码高亮显示

Syntax highlighting via Pygments. css java sh c gas asm cpp c++

{% highlight css linenos %}
#container {
  float: left;
  margin: 0 -240px 0 0;
  width: 100%;
}
#container {
  float: left;
  margin: 0 -240px 0 0;
  width: 100%;
}
{% endhighlight %}


{% highlight c %}
int main ()
{
   return 0;
}
{% endhighlight %}

Non Pygments code example

    <div id="awesome">
        <p>This is great isn't it?</p>
    </div>

















### Block Quote， 段引用

下面时关于段引用的测试。

单段的引用。Just one paragraph.

> My conclusion after the trip was "well, now I know that there's at least one company in the world that can succeed with the waterfall model" and I decided to stop bashing the waterfall model as hard as I usually do. Now, however, with all the problems Toyota are having, I'm starting to reconsider.q q q q q q q q q q q q q q q<from>kkkkk</from>

多段的引用，one more paragraphs.


> My conclusion after the trip was "well, now I know that there's at least one company in the world that can succeed with the waterfall model" and I decided to stop bashing the waterfall model as hard as I usually do. Now, however, with all the problems Toyota are having, I'm starting to reconsider.
>
> My conclusion after the trip was "well, now I know that there's at least one company in the world that can succeed with the waterfall model" and I decided to stop bashing the waterfall model as hard as I usually do. Now, however, with all the problems Toyota are having, I'm starting to .q q q q q

单段，但较为复杂的引用。

> My conclusion after the trip was "well,
> now I know that there's at least one company in the world
> that can succeed with the waterfall model" and I decided to
> stop bashing the waterfall model as hard as I usually do.
> Now, however, with all the problems Toyota are having, I'm starting to reconsider.

嵌套引用。

> My conclusion after the trip was "well,
> now I know that there's at least one company in the world
> > that can succeed with the waterfall model" and I decided to
> stop bashing the waterfall model as hard as I usually do.
> Now, how ever, with all the problems Toyota are having, I'm starting to re consider.







### Unordered Lists，无序列表

如下是三种不同的表达方式。

#### Unordered Lists 1

* Item one
* Item two
* Item three

#### Unordered Lists 2

+ Item one
+ Item two
+ Item three

#### Unordered Lists 3

- Item one
- Item two
- Item three

#### 其它

如下的各个列表项中，各个项之间表示为段落，而之前的不是，也就是说添加了 ```<p></p>``` ，所以现在看起来各个段之间空隙有点大。

- Item one

- Item two

- Item three

### Ordered Lists，有序列表

有序表的表达方式，只与顺序相关，而与列表前的数字无关。

#### Ordered Lists 1

1. Item one
   1. sub item one
   2. sub item two
   3. sub item three
2. Item two

#### Ordered Lists 2

1. Item one
1. Item two
1. Item three

#### Ordered Lists 3

3. Item one
9. Item two
5. Item three

### Lists Tips，列表补记

列表项目标记通常是放在最左边，但是其实也可以缩进，最多 3 个空格，项目标记后面则一定要接着至少一个空格或制表符。

*   Lorem ipsum dolor sit amet, consectetuer adipiscing elit.
    Aliquam hendrerit mi posuere lectus. Vestibulum enim wisi,
    viverra nec, fringilla in, laoreet vitae, risus.
*   Donec sit amet nisl. Aliquam semper ipsum sit amet velit.
    Suspendisse id sem consectetuer libero luctus adipiscing.

如下显示相同。

*   Lorem ipsum dolor sit amet, consectetuer adipiscing elit.
Aliquam hendrerit mi posuere lectus. Vestibulum enim wisi,
viverra nec, fringilla in, laoreet vitae, risus.
*   Donec sit amet nisl. Aliquam semper ipsum sit amet velit.
Suspendisse id sem consectetuer libero luctus adipiscing.

如下是在同一列表中，显示两个段落。

1.  This is a list item with two paragraphs. Lorem ipsum dolor
    sit amet, consectetuer adipiscing elit. Aliquam hendrerit
    mi posuere lectus.

    Vestibulum enim wisi, viverra nec, fringilla in, laoreet
    vitae, risus. Donec sit amet nisl. Aliquam semper ipsum
    sit amet velit.

2.  Suspendisse id sem consectetuer libero luctus adipiscing.

    This is the second paragraph in the list item. You're
only required to indent the first line. Lorem ipsum dolor
sit amet, consectetuer adipiscing elit.



### Pictures，图片显示

![If the picture doesnt exist]({{ site.url }}/images/linux-liberty.png "an example picture"){: .pull-left}

<div class="clearfix"></div>

![aaaaa]{: .pull-right width="20%"}

<div class="clearfix"></div>

[aaaaa]:    /images/linux-liberty.png    "MSN Search"



### Reference，引用

如下是一个简单的链接 [an example](http://example.com/ "Title")，当然也可以使用网站的相对路径 [About Me](/about/) 。

也可以将网站的引用与 URL 分别区分开，如下是其中的示例，而且不区分大小写，[RTEMS] [1]、[Linux] [2]、[GUN][GUN]、[GUN][gun] 。

<!-- the following can occur on anywhere -->
[1]: http://www.rtems.com                              "A Real Time kernel"
[2]: http://www.Linux.com                              'Linux'
[Gun]: http://www.gun.com                              (GUN)


<!--

①
②
③
④
⑤
⑥
⑦
⑧
⑨
⑩
⑪
⑫
⑬
⑭
⑮
⑯
⑰
⑱
⑲
⑳

❶
❷
❸
❹
❺
❻
❼
❽
❾
❿
⓿
➊
➋
➌
➍
➎
➏
➐
➑
➒
➓

←
-->

{% highlight text %}
{% endhighlight %}
