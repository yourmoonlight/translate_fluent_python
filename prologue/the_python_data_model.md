---
title: Python 数据模型
date: 2016-05-08
category: prologue
layout: page
modifiedOn: 2016-05-08
---

## Python 数据模型

Python最棒的特性之一就是它的一致性，等你学完Python 数据模型，你就会理解一致性的含义。

然而，如果你学Python之前学过其他面相对象的语言，你可能已经习惯collection.len(), 但是在Python中，我们是这么用的，len(collection)。这个明显的不同，只是冰山一角。当你理解了这个不同之处，你就理解了什么叫*Pythonic*。

这个所谓的冰山指的就是Python的数据模型，它描述了一些API，如果你想让自己创建的object符合Python的语言习惯，那就得好好玩玩这些API。

你可以把Python数据模型当成Python的框架来理解。Python里有很多“积木”，比如sequence，iterators， functions，classes，context managers等等，Python数据模型规范了这些“积木”的接口。

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

因为_ _ getitem _ _ 委托到了self._cards[]，我们的FrenchDeck实例自动支持了slicing（切片操作）。

而且，因为实现了_ _ getitem _ _这个魔法方法，我们的FrenchDeck实例也是可迭代的，可以进行 for 操作。

```python
>>> for card in deck:
    	print(card)
Card(rank='2', suit='spades')
Card(rank='3', suit='spades')
....
```

顺便提一下，in ，not in 一般是魔法方法_ _ contains _ _ 在起作用，如果没有实现_ _ contains _ _ ，in 操作会按顺序扫一遍，找到就返回True，找不到返回False。

现在再来对牌进行排序，我们写一个函数输入一张牌，返回牌对应的数字，比如梅花♣️2是最小的，对应数字0，黑桃♠️A是最大的，对应数字51:

```python
suit_values = dict(spades=3, hearts=2, diamonds=1, clubs=0)
def spades_high(card):
    rank_value = FrenchDeck.ranks.index(card.rank)
    return rank_value * len(suit_values) + suit_values[card.suit]
```

我们可以把这个函数作为sorted的key参数传进去，就能对deck排序啦。

```python
>>> for card in sorted(deck, key=spades_high):
    	print(card)
Card(rank='2', suit='clubs')
Card(rank='2', suit='diamonds')
Card(rank='2', suit='hearts')
.....
```

尽管FrenchDeck 隐式地继承自object（在Python3中所有的类，自动继承自object，在Python2里你需要显式继承），但是FrenchDeck所具有的功能，是我们利用Python数据模型，实现了_ _ len_ _ 和 _ _ getitem_ _ 两个魔法方法得来的，我们的FrenchDeck就像一个标准的Python序列，可以使用Python的语言特性（比如：迭代，切片），还可以对它使用Python标准库里的方法，比如random.choice, reversed, sorted ...

还有个问题，怎么洗牌呢？到目前为止FrenchDeck还不能洗牌呢，因为它是immutable（不可变的），除非打破封装，直接对_cards属性进行操作。在第十一章，我们会再来看这个问题，先剧透一下，到时候可以用另外一个魔法方法 _ _ setitem _ _。

## 魔法方法是怎么用的？

关于Python魔法方法，第一件你要搞明白的事，它们是被Python解释器调用的，通常你不会这么写my_object._ _ len_ _()，一般是这样len(my_object)。如果my_object是你自己定义的类的实例，那Python解释器会调用你实现的 _ _len _ _实例方法。

但是对于像list，str，bytearray等等这样的内置类型，解释器会抄个近路。len()的CPython实现，实际上会返回内存中PyVarObject结构体中的ob_size的值，这可比调用一个方法快多了。

经常的，魔法方法也是隐式调用的。比如for i in x，实际上是调用iter(x)，而iter(x)会被解释器翻译成x._ _iter _ _()。不过一般情况下，你不经常直接操作魔法方法，唯一一个经常出现在你眼前的是 _ _init _ _，在你自己的 _ _ init _ _ 中调用基类的 _ _ init _ _。不过当你在玩元编程的时候，更多的时候是去实现一个魔法方法。

如果你需要调用一个魔法方法，调用相关的内置函数（e.g.  len，iter， str，等等）会更好一点。

