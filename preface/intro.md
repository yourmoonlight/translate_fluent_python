---
title: 开场白
layout: page
category: preface 
date: 2016-05-07
modifiedOn: 2016-05-07
---

## 这本书适合谁

本书，是写给那些，立志把Python玩得得心应手的同学！
阅读本书前，你得先知道Python的基本语法，起码过了一遍[Learn Python the hard way](http://www.amazon.com/Learn-Python-Hard-Way-Introduction/dp/0321884914)。

如果你之前用Python2，但是想迁移到Python3.4或以上，那是极好的，写这本书的时候，大部分的Python工程师还在用Python2，所以讲到Python3语言特性的时候，我会着重强调的。

然而，本书想要最大限度的展现Python3的魅力，所以不会刻意去兼容Python2。

虽说没有刻意兼容，本书的大部分例子也可以运行在Python2.7下，个别情况也只需要稍作修改。

即使你坚持使用Python2.7，我相信这本书也会对你产生积极的影响，因为**核心概念**还是**一样的**，毕竟Python3不是一门新语言，而且一些不一样的地方，我相信聪明的你，在一个安静的午后，就能学会。[What's New in Python 3.0](https://docs.python.org/3.0/whatsnew/3.0.html) 可以作为一个很好的起点。虽然Python3.0是2009年发布的，后续也有很多变化，但是最重要的还是在3.0。

如果你不太确定自己能不能跟着本书走下来，去把官方[Python Tutorial](https://docs.python.org/3/tutorial/)再过一遍，官方文档里已经有的，不会在本书里细讲，除非是Python3的特性。

## 这本书不适合谁

如果你刚开始学Python，基本语法还没过完，那这本书对你来说，会有点难啃。

而且太早读这本书，会让你觉得，所有的Python脚本都应该使用魔法方法或者元编程的知识。过早抽象和过早优化一样有害。

## 本书的组成结构

本书的核心受众可以在各章节之间跳着看。
本书由六部分组成，我觉得阅读每部分里的小节时，按顺序读会更好。

我试着强调这样一个观点，那就是，先用好那些可以用的东西，再讨论如何实现自己的。

比如，在第二章里，涵盖了那些开箱即用的**sequence types**（序列类型），包括了一些相对来说不太引人注意的序列类型，像**collections.deque**。实现用户自定义的序列，要等到第四部分才会提到，在第四部分我们还会看到如何使用**collection.abc**里的抽象基类，在第四部分的后半部分会讨论如何实现你自己的抽象基类。

这样安排是有好处的，先熟练使用那些开箱即用的，可以避免重复造轮子，会给你节省大量时间。我们也更喜欢从已有的ABCs中去继承，而不是从头创建一个新的ABC。熟练使用已有的，会让你更容易理解内部机理。

### Part I

先用一章来说说Python的数据模型，解释魔法方法为什么是Python对象保持行为一致性的关键。理解Python 数据模型的方方面面，是本书的一大主题，第一章先了解一下概貌。

### Part II

第二部分里的章节，主要cover一些collection types的用法，sequences，mappings，sets，还有str 和 bytes（Python3用户的福音，Python2用户的痛点），主要目标是我们一起回忆一下哪些是已经可用的，顺便解释一下可能让你吃惊的地方。

### Part III 

这里我们讨论Python里的“一级公民” 函数，什么叫first-class？它怎么影响到一些流行的设计模式，怎么利用闭包实现函数装饰器。还会包括Python中的可调用的概念，函数attributes（属性），introspection（自省），parameter annotations（参数注解），以及Python3中的nonlocal声明。

### Part IV 

这部分的重点聚焦到类，Python有它自己的面向对象的特性集合，这部分的章节会解释：

1. references（引用）是如何工作的
2. mutability（可变性）到底意味着什么
3. 实例的生命周期
4. 如何建造你自己的collections（集合），ABCs（抽象基类）
5. 怎么玩多继承
6. 当需要的时候，怎么实现运算符重载

### Part V 

这部分会讲Python中的流程控制，从生成器开始，随后是上下文管理器，协程，包括让人迷惑但又很强大的yield from 语法。

第五部分的末尾，会用collections.futures介绍Python中的并发，用asyncio（基于协程和yield from的特性）玩玩异步I/O。

### Part VI 

这部分首先回顾了类的创建，用属性动态创建一个类，来处理半结构化的json数据集。

然后在深入Python的属性访问之前，我们cover了Python中的property机制。函数，方法，descriptor（描述符）也在这里解释。

第六部分里，我们通过逐步实现一个field validation library 来揭开class decorator（类装饰器）和metaclass（元类）的面纱。

## Soapbox 

我（原作者[Luciano Ramalho](https://twitter.com/ramalhoorg) ）从1998年就开始使用Python，并用Python教学，我喜欢学习不同的编程语言，并比较它们的设计，以及它们背后的理论。在一些章节的结尾，我添加了“临时演讲台”，用来发表一些我个人的观点，关于Python和其他语言。如果你不好奇，你可以跳过这些章节～

## Python Jargon

我希望这本书不只讲Python语言本身，还要包括Python背后的文化。（忽然想起一句话，学习语言，就是学习语言背后的文化！）经过20多年的发展，Python社区里的人们在交流时，已经形成了自己的方言和缩写。在本书结尾会有这些方言的详细解释。

## Python Version Covered

我在Python3.4（CPython3.4）环境下测试了本书的所有代码。

本书几乎所有的代码应该在Python3.x下都可以顺利运行。需注意yield from，asyncio，只能用在Python3.3之后的版本。

大部分代码只需稍作修改就能运行在Python2.7下，除了第四章跟Unicode相关的例子。[本书在Github上的源码](https://github.com/fluentpython/example-code)，用的时候请尽量带上原作者的信息。










