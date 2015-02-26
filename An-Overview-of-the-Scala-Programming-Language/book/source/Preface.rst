.. raw:: latex

    \setcounter{secnumdepth}{-1}

关于本文
========

  “学 Scala 这种大型语言，速度不能太慢，否则学了后面忘了前面。速读一下这份文档
  有助于快速切入，这一点我有体会”。
  
  -- 孟岩

|

这份文档由王玮翻译完成，我做了校对，并用 reStructedText_ 和 sphinx_ 排版输出。
本文对于理解 Scala 为什么会设计成这样非常有帮助。

.. _reStructedText: http://sphinx-doc.org/rest.html
.. _sphinx: http://sphinx-doc.org/


自本文发表之后 Scala 的若干变化
------------------------------------------

需要说明的是，原文撰写于 2006 年，对应的大约是 Scala 2.0，少量内容跟现在的 Scala （截至 2.11.x）有些不同了，这些不同可以在这里找到：\ ::

  `Scala Changelog`_

.. _`Scala Changelog`: http://www.scala-lang.org/download/changelog.html


文中涉及的主要不同有：


AnyVal 子类的小写字母别名已被放弃 [#]_
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

AnyVal（值类型） 是用来对应在底层的宿主系统中（比如 JVM ），没有实现成引用对象的类型
（比如 JVM 中的原生类型）。在 Scala 2.x 之前，为了和 Java 中的原生类型对应，它
们都有一个全小写字母的别名，在 scala/Predef.scala 中定义为：

.. code-block:: scala

  type byte    = scala.Byte 
  type short   = scala.Short 
  type char    = scala.Char 
  type int     = scala.Int 
  type long    = scala.Long 
  type float   = scala.Float 
  type double  = scala.Double 
  type boolean = scala.Boolean 
  type unit    = scala.Unit 

但从 Scala 2.7.2 开始，它们\ `被标为废弃 <http://lampsvn.epfl.ch/trac/scala/changeset/15086/scala/trunk/src/library/scala/Predef.scala>`_\ ：

.. code-block:: scala

  @deprecated("lower-case type aliases will be removed") type byte    = scala.Byte
  @deprecated("lower-case type aliases will be removed") type short   = scala.Short
  @deprecated("lower-case type aliases will be removed") type char    = scala.Char
  @deprecated("lower-case type aliases will be removed") type int     = scala.Int
  @deprecated("lower-case type aliases will be removed") type long    = scala.Long
  @deprecated("lower-case type aliases will be removed") type float   = scala.Float
  @deprecated("lower-case type aliases will be removed") type double  = scala.Double
  @deprecated("lower-case type aliases will be removed") type boolean = scala.Boolean
  @deprecated("lower-case type aliases will be removed") type unit    = scala.Unit

最后，从 Scala 2.8.0 开始，这些小写别名\ `被全部移除 <http://lampsvn.epfl.ch/trac/scala/changeset/19717/scala/trunk/src/library/scala/Predef.scala>`_\ 。

For-comprehensions 的语法变化
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

For-comprehensions 的语法从 Scala 2.5 开始\ `有了改变 <http://www.scala-lang.org/download/changelog.html#comprehensions_revised>`_\ ，例如：

.. code-block:: scala

   for (val x <- List(1, 2, 3); x % 2 == 0) println(x)

现在要写成：

.. code-block:: scala

   for (x <- List(1, 2, 3) if x % 2 == 0) println(x)

也即，生成器（generator）中的变量 ``x`` 前不再需要 ``val`` 关键字，而且可以直接跟一个 ``if`` 开头的卫子句（guarded）。

.. [#] Scala 2.8 及以前的源码树在 http://lampsvn.epfl.ch/trac/scala/browser/scala


怎样自己制作本电子书
--------------------------

本电子书作为 shphix 项目安家在

https://github.com/wecite/papers/tree/master/An-Overview-of-the-Scala-Programming-Language/book

制作的步骤为（以 linux 环境为例）：

安装 Python 和 pip：

.. code-block:: shell

   sudo yum install python
   sudo easy_install pip

安装 sphinx：

.. code-block:: shell

   sudo pip install sphinx

安装 texlive-scheme-small：

.. code-block:: shell

   sudo yum install texlive-schema-small

安装其它 texlive 包，请检查下列包是否已安装，如果没有则需要安装：

- texlive-titlesec
- texlive-framed
- texlive-threeparttable
- texlive-wrapfig
- texlive-helvetic
- texlive-courier
- texlive-multirow
- texlive-upquote
- texlive-fandol

其中 texlive-fandol 中文字体包可能需要在安装完毕后，执行下列操作以注册字体：

.. code-block:: shell

   cp /usr/share/texlive/texmf-dist/fonts/opentype/public/fandol/* ~/fonts/
   fc-cache -fv 

以上准备工作完成后，就可以自己制作本电子书了，步骤为：

.. code-block:: shell

   git clone https://github.com/wecite/papers.git wecite.papers
   cd wecite.papers/An-Overview-of-the-Scala-Programming-Language/book
   make latex
   cd build/latex
   vi ScalaOverview.tex

因为是输出中文 PDF，这时需要把 ScalaOverview.tex 中以下两行删掉，否则会出现各种异况
（跟 xeCJK 包貌似有冲突）：\ ::

   \usepackage[utf8]{inputenc}
   \DeclareUnicodeCharacter{00A0}{\nobreakspace}

最后，用 xelatex 将 .tex 文件输出为 PDF：

.. code-block:: shell

   xelatex ScalaOverview.tex

另外，你也可以直接制作 epub，html 等格式的输出，这个简单多了，不需要安装前面提到的 texlive
\ 相关包，只需要：

.. code-block:: shell

   cd wecite.papers/An-Overview-of-the-Scala-Programming-Language/book
   make html
   make epub

关于本电子书，您如果发现有任何错误和建议，可以直接到 github 上向该项目提出或者提交
\ pull-request。

-------

邓草原 2015 年 2 月 于 北京


译序
====

2008 年那会儿，Scala 刚刚冒头的样子，虽非默默无闻，但也远没有现在这样被人看好。当
时我正好对 Scala 开始感兴趣，在学习的过程中，也看了很多资料和文章，其中这一篇相对
比较喜欢。原因可能有些特殊，因为个人背景的因素，我一直是一个 “理论派”，总喜欢 “理
论指导实践”，而这篇文章恰好是 Scala 发明者们所阐述的创建这门语言的动机和初始设计，
包括很多理论基础，对于喜欢理论的人，读起来就有对这门语言 “放心” 的感觉。 就内容而
言，说实话，当时翻译到一半稍微有点后悔——感觉这篇文章的后半部分有点简略且凌乱，不
如前半部分那样是充分构思过的文章，当然，也不排除是我本人阅读水平和习惯的问题。另
外，时至今日，有些内容和 Scala 最新的发展对比起来，可能已经有点过时了，毕竟很多具
体语法都已经有了变化。尤其是这几年互联网技术的发展和人们对软件开发领域的认识，说
不定这篇文章一开始所描述的 Scala 语言的立意，都未必会让很多人认同。不过，这件事情
不做完，总觉得心里不踏实，毕竟还曾经专门为此给 Martin 写了邮件，获得了人家的同意。
因此，我还是坚持把最后一点工作完成，而对于有兴趣的人而言，我建议阅读此文时，重点
去看其讲解的思路，而非某些具体的代码。另外不要忽视每一段内容，因为文中经常出现讲
解某一方面内容的时候，穿插其他相关思路的说明。

-----------

王玮 2015 年 2 月 于 北京

原译序
======


《Scala 语言概览》（\ `An Overview of the Scala Programming Language`_\ ）
是瑞士洛桑联邦理工学院（EPFL）的程序设计实验室的 Scala 发明者们写的一篇，针对现行
的 Scala 版本。由于要对这种语言进行比较完整的描述，篇幅又不太长，因此学术味有点浓，
而且部分内容略显简略、杂乱。但是，我仍然感觉这篇文章是长期以来看到过的对一门语言介
绍最完整、清晰的文章，不但让人对 Scala 有较为深入的了解，而且对编程语言设计、函数
式/面向对象编程等领域的基本概念和最新进展都能够有所接触，是难得的文献。因此自然有
了翻译过来的冲动，内容错漏难免，拿出来大家讨论而已。

.. _`An Overview of the Scala Programming Language`: http://www.scala-lang.org/docu/files/ScalaOverview.pdf

----------

王玮 2008 年 9 月 于 北京
