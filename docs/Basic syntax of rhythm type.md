# 节奏类型的基本语法



## 目录

- [节奏类型的介绍](#节奏类型的介绍)
- [节奏类型的构成](#节奏类型的构成)
- [节奏模式的语法](#节奏模式的语法)
- [将节奏应用于一个和弦实例](#将节奏应用于一个和弦实例)
- [播放一个节奏实例](#播放一个节奏实例)
- [获取一个节奏实例的节奏信息](#获取一个节奏实例的节奏信息)
- [将一个节奏实例转换为另一个拍号](#将一个节奏实例转换为另一个拍号)
- [根据节奏获取和弦](#根据节奏获取和弦)
- [分析一个和弦实例的节奏](#分析一个和弦实例的节奏)



## 节奏类型的介绍

这是一个表示节奏的数据结构，相当于一个节拍、休止符和延续符的列表。

一个节奏实例可以应用于一个和弦实例，以改变和弦实例的间隔和持续时间。

你可以把这个数据结构看成是鼓点类型的更高层次的抽象。

你可以在初始化一个节奏实例时设置一个固定的总长度，然后你所设置的节拍的持续时间将根据节拍的长度和数量来平均分配。

你还可以为节奏设置拍号，如4/4, 3/4, 5/4。

为了简化，有一个节奏模式的语法，它与鼓点模式的语法有些相似，可以帮助你更快地制作一个节奏实例。

你可以将两个节奏实例相加，得到一个新的组合节奏实例，或者将一个节奏实例乘以一个整数，得到一个新的重复节奏实例。

这个数据结构继承自列表，所以你可以在节奏实例上做与列表相同的操作。



## 节奏类型的构成

```python
rhythm(beat_list,
       total_bar_length=None,
       beats=None,
       time_signature=None,
       separator=' ',
       unit=None)
```

* beat_list: 一个节拍、休止符和延续符实例的列表，或者一个符合节奏模式语法的字符串
* total_bar_length: 节奏的总长度（小节），如果设置了，各节的持续时间将被平均分配
* beats: 当total_bar_length被设置时，节奏的节拍数，你可以设置它来给每个节拍分配一个固定的持续时间
* time_signature: 节奏的拍号, 一个[分子, 分母]形式的列表, 如果没有设置, 节奏的拍号将是4/4
* separator: 如果 `beat_list` 是一个字符串，则是节拍的分隔符
* unit: 如果设置，将所有节拍的持续时间设置为该值



## 节奏模式的语法

使用`b`表示一个节拍, `0`表示一个休止符, `-`表示一个延续符。

默认情况下，节拍符号是用空格隔开的。

如果你想设置节拍的持续时间，在节拍符号后使用`:`，然后在它后面写上持续时间，持续时间支持与鼓点语法相同的语法。

你可以在节拍符号后面加上`.`，使节拍变成附点节拍，点可以是一个或多个，这就是节拍的附点数。

你可以在一个节拍后添加带有关键字和数值的设置区块，与鼓点的语法类似。注意，如果分隔符是空格，设置区块中不能有任何空格。支持的关键字有

```
r:n 重复节拍n次，平均分配单位时间
R:n 用单位时间重复节拍n次
b:n 改变节拍的持续时间为单位持续时间 * n
```



## 将节奏应用于一个和弦实例

你可以使用和弦实例的`apply_rhythm`方法来应用一个节奏实例，它将根据应用的节奏实例来设置和弦实例的音符间隔和持续时间。如果节奏实例的节拍数与和弦实例的音符数不一致，将选择它们中的最小值作为应用节奏的音符数。这个方法返回一个新的和弦实例。

下面是一个创建节奏实例并将节奏应用到旋律中的例子。

```python
rhythm1 = rhythm('b b b - b b b - b b b - b - b -', 2)
'''
equivalent to: rhythm1 = rhythm([beat(), beat(), beat(), continue_symbol(), beat(), beat(), beat(), continue_symbol(), beat(), beat(), beat(), continue_symbol(), beat(), continue_symbol(), beat(), continue_symbol()], 2)
'''

>>> print(rhythm1)
[rhythm]
rhythm: beat(1/8), beat(1/8), beat(1/8), continue(1/8), beat(1/8), beat(1/8), beat(1/8), continue(1/8), beat(1/8), beat(1/8), beat(1/8), continue(1/8), beat(1/8), continue(1/8), beat(1/8), continue(1/8)
total bar length: 2
time signature: 4 / 4

chord1 = chord('C5, D5, E5, E5, F5, G5, E5, D5, C5, A5, G5').apply_rhythm(rhythm1)

>>> print(chord1)
chord(notes=[C5, D5, E5, E5, F5, G5, E5, D5, C5, A5, ...], interval=[1/8, 1/8, 1/4, 1/8, 1/8, 1/4, 1/8, 1/8, 1/4, 1/4, ...], start_time=0)

```



## 播放一个节奏实例

你可以使用节奏实例的`play`方法来听一听这个节奏是什么样的。这将生成一个和弦实例，其中包含相同音高的音符，其间隔和持续时间适用于该节奏。你可以在该方法中设置音符的音高。

```
play(*args, notes='C4', **kwargs)
```

* *args, **kwargs: 常规播放函数的参数

* notes: 生成和弦实例的音符，可以是一个字符串或一个音符实例



## 获取一个节奏实例的节奏信息

有几个属性和方法来获取节奏实例的节奏信息。

节奏实例的`total_bar_length`属性是与节奏实例的拍号相对应的小节的总长度，例如，如果一个节奏实例包含3个持续时间为1/4的节拍实例，拍号为3/4，那么节奏实例的总长度为1。

节奏实例的`time_signature`属性是节奏实例的拍号，其值是一个列表`[分子，分母]`。

你可以使用`get_length`方法来获取节奏实例的总节拍数，包括休止符和延续符，并应用附点。你也可以指定参数`beat_list`来获得一个节拍列表的总节拍数。

你可以使用`get_beat_num`方法来获得一个节奏实例的节拍数，这个方法只计算节拍实例的数量，不考虑附点数。

你可以使用`get_total_duration`方法来获得一个节奏实例的总时长（以小节为单位），将参数`apply_time_signature`设置为True，以应用节奏实例的拍号（默认为False）。



## 将一个节奏实例转换为另一个拍号

你可以使用`convert_time_signature`方法将一个节奏实例转换为另一个拍号。这个方法返回一个新的节奏实例。

有2种不同的方法来转换为另一种拍号。

1.只改变小节的总长度以匹配新的拍号
2.保持总小节长度不变，改变节拍的持续时间，使之与新的拍号中的总小节长度相匹配。

这两种方式会产生不同的新节奏实例，在不同的使用情况下都是有用的。

```python
convert_time_signature(time_signature, mode=0)
```

* time_signature: 一个列表，代表新的拍号 `[分子，分母]`

* mode: 如果设置为0，以1的方式转换，如果设置为1，以2的方式转换



```python
rhythm1 = rhythm('b b b')

>>> print(rhythm1)
[rhythm]
rhythm: beat(1/4), beat(1/4), beat(1/4)
total bar length: 3/4
time signature: 4 / 4

rhythm2 = rhythm1.convert_time_signature([3, 4])

>>> print(rhythm2)
[rhythm]
rhythm: beat(1/4), beat(1/4), beat(1/4)
total bar length: 1
time signature: 3 / 4
```



## 根据节奏获取和弦

你可以使用`get_chords_from_rhythm`函数，用节奏实例的节奏重复一个和弦，或将节奏应用于一个和弦列表。这个函数返回一个新的和弦实例，根据节奏把所有的和弦连接起来。

```python
get_chords_from_rhythm(chords, current_rhythm, set_duration=True)
```

* chords: 可以是一个和弦实例，一个和弦列表或一个音符实例，如果是一个音符实例，结果将是一个和弦实例，音符根据节奏重复
* current_rhythm：一个节奏实例
* set_duration: 根据节奏设置每个和弦的音符的持续时间



```python
>>> get_chords_from_rhythm(C('C'), rhythm('b - b. b. b b -', 1))
chord(notes=[C4, E4, G4, C4, E4, G4, C4, E4, G4, C4, ...], interval=[0, 0, 1/4, 0, 0, 3/16, 0, 0, 3/16, 0, ...], start_time=0)
```



## 分析一个和弦实例的节奏

你可以使用`analyze_rhythm`函数来分析一个和弦实例的节奏，这个函数将找到和弦实例的单位，并从和弦的间隔和持续时间中提取节拍、休止符和继续符。如果提供了一个总的小节长度，这个函数还可以得到拍号。这个函数返回一个节奏实例。

```python
analyze_rhythm(current_chord,
               include_continue=True,
               total_length=None,
               remove_empty_beats=False,
               unit=None,
               find_unit_ignore_duration=False,
               merge_continue=True)
```

* current_chord: 一个和弦实例
* include_continue: 是否考虑音符的持续时间来转换为延续符
* total_length: 和弦实例的总长度（小节）
* remove_empty_beats: 删除持续时间为0的节拍
* unit: 你可以手动提供一个以小节为单位的单位
* find_unit_ignore_duration: 只从和弦的间隔中寻找单位
* merge_continue：将连续的continue符号作为额外的持续时间合并到它们之前的节拍

```python
chord1 = get_chords_from_rhythm(N('C5'), rhythm('b - b. b. b b -', 1))
>>> chord1
chord(notes=[C5, C5, C5, C5, C5], interval=[1/4, 3/16, 3/16, 1/8, 1/4], start_time=0)

>>> analyze_rhythm(chord1)
[rhythm]
rhythm: beat(1/4), beat(1/8.), beat(1/8.), beat(1/8), beat(1/4)
total bar length: 1
time signature: 4 / 4
```
