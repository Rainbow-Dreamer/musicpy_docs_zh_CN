欢迎来到Musicpy官方文档！
===================================

Musicpy是一个可以让你用编程写音乐的python领域特定语言，可以让你用非常简洁并且可读性高的语法通过乐理知识和算法写出优美的音乐。

Musicpy容易学，容易写，可读性也比较强，并且是一个完全计算机化的乐理系统。

Musicpy除了用来创作音乐之外，还可以从乐理层面上来创作音乐和分析音乐，并且你可以在musicpy的基础上设计乐理算法来探索音乐的可能性。

Musicpy可以让你用非常简洁的语法来表达一段音乐的音符，和弦，旋律，节奏，力度等信息，可以通过乐理逻辑来生成曲子，并且进行高级的乐理操作。你可以很容易地把musicpy代码输出为MIDI文件的格式，也可以很简单地加载MIDI文件并且转换为musicpy的数据结构进行高级乐理的操作。

你可以很容易地把musicpy代码输出为MIDI文件的格式，也可以很简单地加载MIDI文件并且转换为musicpy的数据结构进行高级乐理的操作。musicpy的语法设计非常地简洁与灵活，因此musicpy的代码的可读性比较强。

### 用musicpy写的一段日系摇滚前奏

```python
guitar = (
(C('Cmaj7') @ 1) @ [1,2,3,4,1,2,3,2] |
(C('Fmaj7',3) ^ 2) @ [1,2,3,4,1,2,3,2] |
(C('G7sus',3) ^ 2) @ [1,2,3,4,1,2,3,2] |
C('Am11',3) @ [1,2,4,6,1,4,6,4] |
(C('Cmaj7') @ 1) @ [1,2,3,4,1,2,3,2] |
C('Fadd9',3).up(2,1) @ [1,2,3,4,1,2,3,2] |
(C('G7sus',3) ^ 2) @ [1,2,3,4,1,2,3,2] |
C('Am11',3) @ [1,2,4,6,1,4,6,4]) % (1/2,1/8)
bass1 = chord('D2[.8;.], E2[.8;.], D2[.8;.], E2[1;.], F2[1;.], G2[1;.], A1[.2;.], A2[.8;.], G2[.8;.], E2[.8;.], D2[.8;.]')
bass2 = (chord('E2')*8 + chord('F1')*8 + chord('G1')*8 + chord('A1,A1,E2,A1,A2,A1,G2,D2')) % (1/8,1/8) * 4
bass = bass1 | bass2
string1 = chord('B5[1;.],C6[1;.],D6[.2;.],E6[.2;.],\
F6[.2;.],E6[.2;.],C6[1;.],B5[.2;.],C6[.2;.],G5[1;.],\
F5[.2;.],E5[.2;.],C5[1;.],D5[.4;.],E5[.4;.],F5[.4;.],\
E5[.4;.],D5[.2;.],G5[.2;.],E5[1;.]')
result = piece(tracks=[guitar * 3, bass, string1],
               instruments=[28, 34, 49],
               bpm=135,
               start_times=[0, 4-3/8, 12])
play(result)
```

[点击这里试听 (Microsoft GS Wavetable Synth)](https://drive.google.com/file/d/1tMKLt3oFdmiGQPTdFVolGvBE1gVGNSwa/view?usp=sharing)

如果你认为这太过于简单，musicpy也可以在不到30行的代码内制作出[这样](https://drive.google.com/file/d/1j66Ux0KYMiOW6yHGBidIhwF9zcbDG5W0/view?usp=sharing)的音乐(如果你不关心可读性，代码还可以更短)。不过，这也只是一个非常短的电子舞曲的例子，并没有写的很复杂。


## 目录
-------------

## 序言

* [介绍](https://musicpy.readthedocs.io/zh_CN/latest/Introduction/)

## 数据结构

* [Musicpy的数据结构](https://musicpy.readthedocs.io/zh_CN/latest/Data%20Structures%20of%20musicpy/) 

## 基本语法

* [音符类型的基本语法](https://musicpy.readthedocs.io/zh_CN/latest/Basic%20syntax%20of%20note%20type/)
* [和弦类型的基本语法](https://musicpy.readthedocs.io/zh_CN/latest/Basic%20syntax%20of%20chord%20type/)
* [音阶类型的基本语法](https://musicpy.readthedocs.io/zh_CN/latest/Basic%20syntax%20of%20scale%20type/)
* [乐曲类型的基本语法](https://musicpy.readthedocs.io/zh_CN/latest/Basic%20syntax%20of%20piece%20type/)
* [音轨类型的基本语法](https://musicpy.readthedocs.io/zh_CN/latest/Basic%20syntax%20of%20track%20type/)
* [速度类型的基本语法](https://musicpy.readthedocs.io/zh_CN/latest/Basic%20syntax%20of%20tempo%20type/)
* [弯音类型的基本语法](https://musicpy.readthedocs.io/zh_CN/latest/Basic%20syntax%20of%20pitch_bend%20type/)
* [声相类型的基本语法](https://musicpy.readthedocs.io/zh_CN/latest/Basic%20syntax%20of%20pan%20type/)
* [音量类型的基本语法](https://musicpy.readthedocs.io/zh_CN/latest/Basic%20syntax%20of%20volume%20type/)
* [鼓点类型的基本语法](https://musicpy.readthedocs.io/zh_CN/latest/Basic%20syntax%20of%20drum%20type/)

## 实用的功能

* [实用的功能](https://musicpy.readthedocs.io/zh_CN/latest/Useful%20functionality/)

## Musicpy语言作曲代码示例

* [Musicpy语言作曲代码示例（第一期）](https://musicpy.readthedocs.io/zh_CN/latest/Musicpy%20composition%20code%20examples%20part%201/)
* [Musicpy语言作曲代码示例（第二期）](https://musicpy.readthedocs.io/zh_CN/latest/Musicpy%20composition%20code%20examples%20part%202/)
* [Musicpy语言作曲代码示例（第三期）](https://musicpy.readthedocs.io/zh_CN/latest/Musicpy%20composition%20code%20examples%20part%203/)

## Musicpy取样机模块

* [Musicpy取样机模块](https://musicpy.readthedocs.io/zh_CN/latest/Musicpy%20sampler%20module/) 
* [Musicpy取样机模块作曲代码示例](https://musicpy.readthedocs.io/zh_CN/latest/Musicpy%20sampler%20module%20composition%20code%20examples/)

## 算法

* [Musicpy algorithms 模块介绍](https://musicpy.readthedocs.io/zh_CN/latest/Introduction%20of%20musicpy%20algorithms%20module/)
* [从一首完整的钢琴曲里提取主旋律以及所有和弦的算法](https://musicpy.readthedocs.io/zh_CN/latest/The%20algorithm%20to%20split%20the%20main%20melody%20and%20chords%20from%20a%20piece%20of%20music/)
* [按照乐理逻辑判断任意一组音组成的和弦类型的算法](https://musicpy.readthedocs.io/zh_CN/latest/The%20algorithm%20to%20determine%20the%20chord%20type%20of%20any%20group%20of%20notes%20according%20to%20the%20logic%20of%20music%20theory/)

## 常见的问题

* [常见的问题](https://musicpy.readthedocs.io/zh_CN/latest/Frequently%20Asked%20Questions/)
