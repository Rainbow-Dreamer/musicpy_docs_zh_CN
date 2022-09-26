Welcome to Musicpy's documentation!
===================================

usicpy是一个可以让你用编程写音乐的python领域特定语言，可以让你用非常简洁并且可读性高的语法通过乐理知识和算法写出优美的音乐。

Musicpy容易学，容易写，可读性也比较强，并且是一个完全计算机化的乐理系统。

Musicpy除了用来创作音乐之外，还可以从乐理层面上来创作音乐和分析音乐，并且你可以在musicpy的基础上设计乐理算法来探索音乐的可能性。

Musicpy可以让你用非常简洁的语法来表达一段音乐的音符，和弦，旋律，节奏，力度等信息，可以通过乐理逻辑来生成曲子，并且进行高级的乐理操作。你可以很容易地把musicpy代码输出为MIDI文件的格式，也可以很简单地加载MIDI文件并且转换为musicpy的数据结构进行高级乐理的操作。

你可以很容易地把musicpy代码输出为MIDI文件的格式，也可以很简单地加载MIDI文件并且转换为musicpy的数据结构进行高级乐理的操作。musicpy的语法设计非常地简洁与灵活，因此musicpy的代码的可读性比较强。

### 用musicpy写的一段日系摇滚前奏

```python
guitar = ((C('Cmaj7')@1)@[1,2,3,4,1,2,3,2] |
(C('Fmaj7',3)^2)@[1,2,3,4,1,2,3,2] |
(C('G7sus',3)^2)@[1,2,3,4,1,2,3,2] |
C('Am11',3)@[1,2,4,6,1,4,6,4] |
(C('Cmaj7')@1)@[1,2,3,4,1,2,3,2] |
C('Fadd9',3).up(2,1)@[1,2,3,4,1,2,3,2] |
(C('G7sus',3)^2)@[1,2,3,4,1,2,3,2] |
C('Am11',3)@[1,2,4,6,1,4,6,4]) % (1/2,1/8)
bass1 = chord('D2[.8;.],E2[.8;.],D2[.8;.],E2[1;.], F2[1;.], G2[1;.], A1[.2;.], A2[.8;.], G2[.8;.], E2[.8;.], D2[.8;.]')
bass2 = (chord('E2')*8 + chord('F1')*8 + chord('G1')*8 + chord('A1,A1,E2,A1,A2,A1,G2,D2')) % (1/8,1/8) % 4
bass = bass1 | bass2
string1 = chord('B5[1;.],C6[1;.],D6[.2;.],E6[.2;.],\
F6[.2;.],E6[.2;.],C6[1;.],B5[.2;.],C6[.2;.],G5[1;.],\
F5[.2;.],E5[.2;.],C5[1;.],D5[.4;.],E5[.4;.],F5[.4;.],\
E5[.4;.],D5[.2;.],G5[.2;.],E5[1;.]')
play(piece([guitar%3, bass, string1], [28, 34, 49], 135, [0, 4-3/8, 12]))
```

[点击这里试听 (Microsoft GS Wavetable Synth)](https://drive.google.com/file/d/1tMKLt3oFdmiGQPTdFVolGvBE1gVGNSwa/view?usp=sharing)

如果你认为这太过于简单，musicpy也可以在不到30行的代码内制作出[这样](https://drive.google.com/file/d/1j66Ux0KYMiOW6yHGBidIhwF9zcbDG5W0/view?usp=sharing)的音乐(如果你不关心可读性，代码还可以更短)。不过，这也只是一个非常短的电子舞曲的例子，并没有写的很复杂。


## 目录
-------------

## 序言

* [介绍](介绍)

## 数据结构

* [musicpy的数据结构](https://github.com/Rainbow-Dreamer/musicpy/wiki/musicpy的数据结构) 

## 基本语法

* [音符类型的基本语法](https://github.com/Rainbow-Dreamer/musicpy/wiki/音符类型的基本语法)
* [和弦类型的基本语法](https://github.com/Rainbow-Dreamer/musicpy/wiki/和弦类型的基本语法)
* [音阶类型的基本语法](https://github.com/Rainbow-Dreamer/musicpy/wiki/音阶类型的基本语法)
* [乐曲类型的基本语法](https://github.com/Rainbow-Dreamer/musicpy/wiki/乐曲类型的基本语法)
* [音轨类型的基本语法](https://github.com/Rainbow-Dreamer/musicpy/wiki/音轨类型的基本语法)
* [速度类型的基本语法](https://github.com/Rainbow-Dreamer/musicpy/wiki/速度类型的基本语法)
* [弯音类型的基本语法](https://github.com/Rainbow-Dreamer/musicpy/wiki/弯音类型的基本语法)
* [声相类型的基本语法](https://github.com/Rainbow-Dreamer/musicpy/wiki/声相类型的基本语法)
* [音量类型的基本语法](https://github.com/Rainbow-Dreamer/musicpy/wiki/音量类型的基本语法)
* [鼓点类型的基本语法](https://github.com/Rainbow-Dreamer/musicpy/wiki/鼓点类型的基本语法)

## 实用的功能

* [实用的功能](https://github.com/Rainbow-Dreamer/musicpy/wiki/实用的功能)

## musicpy语言作曲代码示例

* [musicpy语言作曲代码示例（第一期）](https://github.com/Rainbow-Dreamer/musicpy/wiki/musicpy语言作曲代码示例（第一期）)
* [musicpy语言作曲代码示例（第二期）](https://github.com/Rainbow-Dreamer/musicpy/wiki/musicpy语言作曲代码示例（第二期）)
* [musicpy语言作曲代码示例（第三期）](https://github.com/Rainbow-Dreamer/musicpy/wiki/musicpy语言作曲代码示例（第三期）)

## musicpy取样机模块

* [musicpy取样机模块](https://github.com/Rainbow-Dreamer/musicpy/wiki/musicpy取样机模块) 
* [musicpy取样机模块作曲代码示例](https://github.com/Rainbow-Dreamer/musicpy/wiki/musicpy取样机模块作曲代码示例)

## 算法

* [musicpy algorithms 模块介绍](https://github.com/Rainbow-Dreamer/musicpy/wiki/musicpy-algorithms-模块介绍)
* [从一首完整的钢琴曲里提取主旋律以及所有和弦的算法](https://github.com/Rainbow-Dreamer/musicpy/wiki/从一首完整的钢琴曲里提取主旋律以及所有和弦的算法)
* [按照乐理逻辑判断任意一组音组成的和弦类型的算法](https://github.com/Rainbow-Dreamer/musicpy/wiki/按照乐理逻辑判断任意一组音组成的和弦类型的算法)

## 常见的问题

* [常见的问题](https://github.com/Rainbow-Dreamer/musicpy/wiki/常见的问题)
