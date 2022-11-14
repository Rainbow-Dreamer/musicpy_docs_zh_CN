# 音轨类型的基本语法



## 目录

- [音轨类型的介绍](#音轨类型的介绍)
- [音轨类型新增的一些语法](#音轨类型新增的一些语法)



## 音轨类型的介绍

在piece类型中，可以包含多个不同的音轨，目前新增的音轨类型就表示着每一个音轨所包含的信息，包括和弦类型，乐器类型，开始时间，速度(bpm)，pan和volume等等。

### 音轨类型的构成

```python
track(content,
      instrument=1,
      start_time=0,
      channel=None,
      track_name=None,
      pan=None,
      volume=None,
      bpm=120,
      name=None,
      sampler_channel=None)
```

- content: 和弦类型，音轨类型的内容
- instrument: 乐器类型
- start_time: 音轨的开始时间
- channel: 通道编号
- track_name: 音轨的名字
- pan: 音轨的声相
- volume: 音轨的音量
- bpm: 曲速 (BPM)
- name: 音轨属于的乐曲的名字
- sampler_channel: 在sampler模块中音轨对应的取样机的通道编号


piece类型的`__getitem__`内置函数(也就是`a[index]`的语法,a为乐曲类型，index为音轨的索引，以0作为第1条音轨)的返回值现在改为音轨类型(track类型)，有着完整的和piece类型一样的信息，并且可以进行提取类型信息的操作。  
在build函数中，现在也可以把音轨类型作为参数传进去构建乐曲类型。  
现在在显示乐曲类型和音轨类型的时候，会在开头显示当前显示的乐理类型是乐曲类型还是音轨类型，表示方法为：`[piece]`表示乐曲类型，`[track]`表示音轨类型。

```python
A1, B1, C1, D1 = [C('C') for i in range(4)]
a = piece([A1, B1, C1, D1], [1, 47, 35, 2], 150, [0, 2, 2, 8], name='demo') # a是一个piece类型(乐曲类型)
>>> print(a)
[piece] demo
BPM: 150
track 1 | instrument: Acoustic Grand Piano | start time: 0 | [C4, E4, G4] with interval [0, 0, 0]
track 2 | instrument: Orchestral Harp | start time: 2 | [C4, E4, G4] with interval [0, 0, 0]
track 3 | instrument: Electric Bass (pick) | start time: 2 | [C4, E4, G4] with interval [0, 0, 0]
track 4 | instrument: Bright Acoustic Piano | start time: 8 | [C4, E4, G4] with interval [0, 0, 0]

>>> print(a[0]) # 打印出第1条音轨
[track] demo
BPM: 150
instrument: Acoustic Grand Piano | start time: 0 | [C4, E4, G4] with interval [0, 0, 0]

c = build(track(A1, 1, 0),
          track(B1, 1, 2),
          track(C1, 47, 2),
          track(D1, 35, 8),
          bpm=150,
          name='demo') # 用build函数构建乐曲类型，可以将任意多个音轨类型作为参数传进去

b = track(C('D'), 5, 12)
a.append(b) # 音轨类型可以被添加进乐曲类型，使用乐曲类型的内置函数append。这一行是把音轨类型b添加进乐曲类型a
>>> print(a)
[piece] demo
BPM: 150
track 1 | instrument: Acoustic Grand Piano | start time: 0 | [C4, E4, G4] with interval [0, 0, 0]
track 2 | instrument: Orchestral Harp | start time: 2 | [C4, E4, G4] with interval [0, 0, 0]
track 3 | instrument: Electric Bass (pick) | start time: 2 | [C4, E4, G4] with interval [0, 0, 0]
track 4 | instrument: Bright Acoustic Piano | start time: 8 | [C4, E4, G4] with interval [0, 0, 0]
track 5 | instrument: Electric Piano 1 | start time: 12 | [D4, F#4, A4] with interval [0, 0, 0]

# 音轨类型也可以单独播放，也是使用play函数进行播放，如果音轨类型有设定速度，
# 则使用音轨类型自带的速度，否则按照play函数的bpm参数来设定速度
play(b)
b.play()
```

track类型(音轨类型)是可以单独存储一条音轨的信息的类型，可以被添加进piece类型(乐曲类型)，一个乐曲类型则是由多条不同的音轨构成的，尽管数据结构的实现上并不是音轨类型的集合，而是多条音轨各自的同一种类的信息的汇总，不过我实现了音轨类型可以添加进乐曲类型的函数，还有从乐曲类型提取出任意一条音轨重新构建成音轨类型的方法，并且音轨类型本身也是可以单独播放的。使用build函数可以传任意多个音轨类型进去，构建成乐曲类型。

音轨类型表示着每一个音轨所包含的信息，包括和弦类型，乐器类型，开始时间，速度(bpm)，pan和volume等等。

## 音轨类型新增的一些语法

和弦类型中的`a + i`, `a - i`, `a | i`, `a & i`的语法现在也已经添加到音轨类型中，用法可以参考和弦类型中的用法。

