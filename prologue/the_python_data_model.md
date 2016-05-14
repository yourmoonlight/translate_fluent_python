---
title: Python 数据模型
date: 2016-05-08
category: prologue
layout: page
modifiedOn: 2016-05-08
---

## Python 数据模型

Python最棒的特性之一就是它的一致性，等你学完Python 数据模型，你就会理解一致性的含义。

然而，如果你学Python之前学过其他面相对象的语言，你可能已经习惯collection.len(), 但是在Python中，我们是这么用的，len(collection)。这个明显的不同，只是冰山一角。当你差不多理解了这个不同，就是你理解什么叫*Pythonic*的时候。

这个所谓的冰山指的就是Python的数据模型，它描述了一些API，如果你想让自己创建的object符合Python的语言习惯，那就得好好玩玩这些API。

你可以把Python数据模型当成Python的框架来理解。Python里有很多“积木”，比如sequence，iterators， functions，classes，context managers等等，数据模型规范了这些“积木”的接口。

当你用框架编程时，你主要时间花费在，写一些将被框架调用的方法，这跟你使用Python的数据模型编程是一个道理。通过一些语法的触发，Python的解释器会调用魔法方法来完成基本的对象操作。那些魔法方法总是以双下划线开头，双下划线结尾（i.e. _ _ getitem  _ _）。比如，使用obj[key] 这种语法就是 魔法方法 _ _getitem _ _在背后起作用，遇到这种obj[key]写法，解释器会调用obj. _ _getitem _ _(key)。

熟练使用这些魔法方法，你就能把Python里的那些“积木”玩转，其中包括但不限于：

1. Iteration
2. Collections
3. Attribute access
4. Operator overloading
5. Function and method invocation
6. Object creation and  destruction
7. String  representation and formatting
8. Managed contexts (i.e., with blocks)

关于魔法方法，就是经常在英文Python书看到的special method，叫魔法方法，更cool而已。怎么读呢？比如_ _ get item _ _， 一般外国Python社区的人都会读作“dunder-getitem”，发音类似“duang 的”， 读本书的同学，以后可能会看到外国人做的视频教程，听老外说“duang的”method，应该立马能对应到魔法方法。

## A Pythonic Card Deck

接下来我们看一个简单的小例子，来展示两个魔法方法的power。

Example 1-1 实现一副纸牌

```python
import collections

Card = collections.namedtuple('Card', ['rank', 'suit'])

class FrenchDeck:
    ranks = [str(n) for n in range(2, 11)] + list('JQKA')
    suits = 'spades diamonds clubs hearts'.split()
    
    def __init__(self):
        self._cards = [Card(rank, suit) for suit in self.suits for rank in self.ranks]
    def __len__(self):
        return len(self._cards)
    def __getitem__(self, position):
        return self._cards[position]
```

先用collections.namedtuple 构造一个简单的class，实现一副牌。

```python
>>> beer_card = Card('7', 'diamonds')
>>> beer_card
Card(rank='7', suit='diamonds')
```

让我们来看看这个例子，像任何标准Python集合一样，使用len(deck)时，会返回牌的总数，即deck的长度。

```python
>>> deck = FrenchDeck()
>>> len(deck)
52
```

从deck中获取指定位置的牌，第一个或者最后一个，只要使用deck[0], 或着deck[-1]，之所以可以这么写，是因为_ _ getitem_ _ 在背后默默的支持着。

``` python
>>> deck[0]
Card(rank='2', suit='spades')
>>> deck[-1]
Card(rank='A', suit='hearts')
```

我们要不要写一个方法随机获取一张牌呢？不需要，Python本身有个函数，可以从sequence中随机获取一个条目：random.choice。我们可以直接用在FrenchDeck的实例上：

```python
>>> from random import choice
>>> choice(deck)
Card(rank='3', suit='hearts')
>>> choice(deck)
Card(rank='K', suit='spades')
```

从这个例子中我们可以看出使用魔法方法的两个好处：

- 如果以后有别人要使用你写的这个类，他们不需要为一些基本的操作记方法名。（比如获取总长度的那个方法叫啥来着？是.size()呢 还是.length()呢？）
- 可以从Python丰富的标准库中受益，避免重复造轮子，比如random.choice。

而且好处还不止以上两点！

