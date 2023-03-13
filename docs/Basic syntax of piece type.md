# 乐曲类型的基本语法



## 目录

- [piece类型（乐曲类型），用来处理多个音轨以及每个音轨有自己的乐器的曲子](#"piece类型（乐曲类型），用来处理多个音轨以及每个音轨有自己的乐器的曲子")
- [构建乐曲类型可以使用另一种语法，相对更直观一点](#构建乐曲类型可以使用另一种语法相对更直观一点)
- [piece类型的编辑操作一览](#piece类型的编辑操作一览)
- [将一个乐曲类型合并为一个和弦类型](#将一个乐曲类型合并为一个和弦类型)
- [乐曲类型新增乐曲名称的属性以及BPM的显示](#乐曲类型新增乐曲名称的属性以及BPM的显示)
- [乐曲类型新增的一些语法](#乐曲类型新增的一些语法)
- [清除乐曲类型的pan和volume信息](#清除乐曲类型的pan和volume信息)
- [清除音色改变的MIDI信息](#清除音色改变的MIDI信息)
- [清除其他的MIDI信息](#清除其他的MIDI信息)
- [快速改变乐曲类型的音轨的乐器](#快速改变乐曲类型的音轨的乐器)
- [按照小节长度移动乐曲类型的音轨的位置](#按照小节长度移动乐曲类型的音轨的位置)
- [筛选和弦类型和乐曲类型中的某一种MIDI信息](#筛选和弦类型和乐曲类型中的某一种MIDI信息)
- [对乐曲类型整体进行转调](#对乐曲类型整体进行转调)
- [对乐曲类型整体应用函数](#对乐曲类型整体应用函数)
- [重新设置MIDI通道编号和音轨编号](#重新设置MIDI通道编号和音轨编号)
- [得到一个乐曲实例中的音符总数](#得到一个乐曲实例中的音符总数)
- [去除一个乐曲类型的鼓轨](#去除一个乐曲类型的鼓轨)



## piece类型（乐曲类型），用来处理多个音轨以及每个音轨有自己的乐器的曲子

比如说你现在想写一首曲子，这首曲子分为四个声部，主旋律（乐器：钢琴），低音（乐器：电贝斯），和弦音和装饰音（乐器：竖琴），鼓点（乐器：架子鼓），

这样你有四个乐器，分别负责四个不同的音轨，组成一首完整的曲子。而且这四个音轨都有各自的开始时间（单位为小节）。

现在比如你已经为这个四个声部写完各自的旋律了，都存储在各自的和弦类型中，比如叫做C1, C2, C3, C4。

你想要这首曲子的BPM（曲速）为150，然后四个声部分别的开始时间为0, 2, 2, 4 (单位为小节)， 0的意思就是从曲子的最开头就开始。

乐曲类型的乐器部分是对应General Midi的0 ~ 127号乐器，可以直接输入0 ~ 127的数字选择对应的乐器，也可以输入乐器的名字。

关于General Midi的128个乐器的名称和数字对应的字典可以到database.py这个文件里查看。

乐曲类型的构成（按照先后顺序）为：

```python
piece(tracks,
      instruments=None,
      bpm=120,
      start_times=None,
      track_names=None,
      channels=None,
      name=None,
      pan=None,
      volume=None,
      other_messages=[],
      daw_channels=None)
```

* tracks: 每个音轨的和弦类型的列表

* instruments: 每个音轨的乐器的列表

* bpm: 曲速 (BPM)

* start_times: 每个音轨的开始时间的列表

* track_names: 每个音轨的名字

* channels: 每个音轨对应的通道编号的列表，以0作为第1个音轨

* name: 乐曲的名字

* pan: 每个音轨的左右声道混音位置的列表

* volume: 每个音轨的整体音量大小的列表

* other_messages: 乐曲的其他的MIDI信息的列表

* daw_channels: 在daw模块里用来指明每个音轨对应的daw的通道编号

按照我们现在的要求，可以这样构建一个乐曲类型：

```python
C1 = chord('G4, D5, B5, F#5') % (1, 1/8) * 4
C2 = (chord('C2, C2, G1, G1') % (1,1)) * 2
C3 = (chord('F#6, G6') % (1/8, 1/8) * 8 | chord('A5, B5') % (1/8, 1/8) * 8) * 2
C4 = chord('G3, G3, G3, G3') % ([3/8,1/8,1/4,1/4], [3/8,1/8,1/4,1/4]) * 4

new_piece = piece(tracks=[C1, C2, C3, C4],
                  instruments=['Acoustic Grand Piano', 'Electric Bass (finger)', 'Orchestral Harp', 'Synth Drum'],
                  bpm=120,
                  start_times=[0, 2, 2, 6],
                  track_names=['piano', 'bass', 'harp', 'drum'])

>>> new_piece
[piece] 
BPM: 120
track 1 piano | instrument: Acoustic Grand Piano | start time: 0 | chord(notes=[G4, D5, B5, F#5, G4, D5, B5, F#5, G4, D5, ...], interval=[1/8, 1/8, 1/8, 1/8, 1/8, 1/8, 1/8, 1/8, 1/8, 1/8, ...], start_time=0)
track 2 bass | instrument: Electric Bass (finger) | start time: 2 | chord(notes=[C2, C2, G1, G1, C2, C2, G1, G1], interval=[1, 1, 1, 1, 1, 1, 1, 1], start_time=0)
track 3 harp | instrument: Orchestral Harp | start time: 2 | chord(notes=[F#6, G6, F#6, G6, F#6, G6, F#6, G6, F#6, G6, ...], interval=[1/8, 1/8, 1/8, 1/8, 1/8, 1/8, 1/8, 1/8, 1/8, 1/8, ...], start_time=0)
track 4 drum | instrument: Synth Drum | start time: 6 | chord(notes=[G3, G3, G3, G3, G3, G3, G3, G3, G3, G3, ...], interval=[3/8, 1/8, 1/4, 1/4, 3/8, 1/8, 1/4, 1/4, 3/8, 1/8, ...], start_time=0)
```

其中如果有鼓点的轨道，可以让对应channel为9，这样乐器就可以选择General Midi中专门的鼓的类型了，比如Standard, Room, Electronic等等。

乐器的数字为1的时候是Standard的鼓组（当对应的channel为9的时候）。

现在new_piece就是一个乐曲类型了，它存储着我们想要的乐曲的信息，四个轨道的名字分别叫做piano, bass, harp, drum。

要写入这首乐曲到MIDI文件，只需要

```python
play(new_piece)
# 或者
new_piece.play()
```

即可，play函数是写入MIDI文件之后直接播放，可以马上听到乐曲，如果只是写入MIDI文件不播放，那么就写

```python
write(new_piece, name=你想要的MIDI文件名)
```

play和write函数会检查你传入的是不是乐曲类型，如果是，那么就会把乐曲类型的完整信息写入MIDI文件，也就是每一个声部写入一个独立的轨道，每个轨道有自己独立的乐器，有自己的起始时间，有自己的名字（如果有给的话），每个轨道的通道也会对应你给的channel（如果有给的话）。因此写入的MIDI文件就是一首完整的有多个乐器和声部的乐曲了。

```python
piece(...)的简化写法是
P(...)
```



## 构建乐曲类型可以使用另一种语法，相对更直观一点

比如现在我们用piece函数构建一个乐曲类型

```python
A1 = C('Cmaj7') % (1, 1/8)
B1 = chord('A2') % (1,)
C1 = chord('C,D,E,F,G') % (1/8, 1/8)
D1 = chord('C5')

new_piece = piece([A1, B1, C1, D1],
                  ['Acoustic Grand Piano', 'Electric Bass (finger)', 'Orchestral Harp', 'Synth Drum'],
                  150,
                  [0, 2, 2, 4],
                  ['piano', 'bass', 'harp', 'drum'])
```

使用`build`函数，可以用另一种语法来构建乐曲类型，这种语法是把一个音轨的所有信息放在一个列表作为第1个轨道，下一个音轨的所有信息放在下一个列表作为第2个轨道，
以此类推，每个音轨的信息的参数的顺序为:  
`[和弦类型, 乐器(可不写), 开始时间(单位为小节)(可不写), 轨道的数字(可不写), 轨道的名字(可不写), 轨道的声相(可不写), 轨道的音量(可不写)]`  
build函数可以接收任意多个音轨的列表，曲速(BPM)默认值为120，指定曲速必须用关键字参数`bpm`，其他的乐曲类型的参数也可以通过关键字参数指定，比如可以写成：

```python
new_piece = build([A1, 'Acoustic Grand Piano', 0, 1, 'piano'],
                  [B1, 'Electric Bass (finger)', 2, 2, 'bass'],
                  [C1, 'Orchestral Harp', 2, 3, 'harp'],
                  [D1, 'Synth Drum', 4, 4, 'drum'],
                  bpm=150)
```

除此之外，build函数也可以接收音轨类型作为参数构建乐曲类型。音轨类型和音轨信息的列表可以混合输入。  
你也可以传入一个元素为音轨类型或者音轨信息的列表(可以混合)的列表，同样也可以构建乐曲类型。

```python
# 使用多个音轨类型构建乐曲类型
new_piece = build(track(A1, instrument='Acoustic Grand Piano', start_time=0, channel=1, track_name='piano'),
                  track(B1, instrument='Electric Bass (finger)', start_time=2, channel=2, track_name='bass'),
                  track(C1, instrument='Orchestral Harp', start_time=2, channel=3, track_name='harp'),
                  track(D1, instrument='Synth Drum', start_time=4, channel=4, track_name='drum'),
                  bpm=150)

# 使用一个音轨类型的列表构建乐曲类型
new_piece = build([track(A1, instrument='Acoustic Grand Piano', start_time=0, channel=1, track_name='piano'),
                  track(B1, instrument='Electric Bass (finger)', start_time=2, channel=2, track_name='bass'),
                  track(C1, instrument='Orchestral Harp', start_time=2, channel=3, track_name='harp'),
                  track(D1, instrument='Synth Drum', start_time=4, channel=4, track_name='drum')],
                  bpm=150)
```



## piece类型的编辑操作一览

piece类型(乐曲类型)可以存储一整首多个不同乐器和不同MIDI通道的曲子的信息，你除了可以对一个乐曲类型里的任何一个MIDI通道进行和弦类型的编辑以外，在整个乐曲类型层面上也有很多可以编辑的操作。

```python
# 首先构建一个乐曲类型，A1, B1, C1, D1都是已经写好的和弦类型
a = piece(tracks=[A1, B1, C1, D1],
          instruments=[1, 35, 1, 49],
          bpm=150,
          start_times=[0, 8, 8, 16],
          channels=[0,1,9,2],
          track_names=['piano', 'electric bass', 'drums', 'strings'])

# 如果想要完全重复这个乐曲类型n遍，那么可以写
b = a * n

# 可以使用index值查看一个乐曲类型的某一个音轨的信息，以0作为第1个音轨
# 使用a[n]的语法可以得到第n个音轨类型
>>> a[0]
[track] 
BPM: 150
channel 0 piano | instrument: Acoustic Grand Piano | start time: 0 | chord(notes=[C4, E4, G4, B4], interval=[0, 0, 0, 0], start_time=0)

# 使用a(n)的语法可以得到第n条音轨的和弦类型
>>> a(0)
chord(notes=[C4, E4, G4, B4], interval=[0, 0, 0, 0], start_time=0)

# 升高/降低整首曲子n个半音，同样也是与音符类型，和弦类型相同的语法，可以使用up/down函数或者+/-的进阶语法
b = a.up()
b = +a
b = a + 2
b = a.down()
b = -a
b = a - 2

# 有时你不想在升高或降低乐曲类型时改变鼓的音轨的音符，当使用乐曲类型的up/down功能时，你可以把参数`mode`设置为1来升高或降低整个乐曲，但鼓的音轨除外，通道号为9的音轨将被视为鼓的音轨。
b = a.up(mode=1)

# 可以往指定的位置添加实时的速度变化
a.add_tempo_change(bpm=100, start_time=None, ind=None, track_ind=None)
# bpm: 想要变化到的速度，单位为BPM
# start_time: 速度变化发生的时间，单位为小节，如果不设置则以ind为准
# ind: 速度变化信息插入的位置，需要和track_ind配合使用
# track_ind: 可以选择在第几个MIDI通道插入速度的信息，以0作为第1个MIDI通道，ind是选择的MIDI通道里放在第几个位置
# 如果ind和track_ind没有设置，那么就默认往第一个MIDI通道的最后添加速度变化的信息

# 可以往指定的位置添加实时的弯音（pitch bend可以模拟出音符的弯音，滑音，颤音等效果）
a.add_pitch_bend(value, start_time=0, channel='all', track=0, mode='cents', ind=None)
# value: 音符的音高变化的量
# start_time: 音符的音高变化发生的时间，单位为小节
# channel: 选择往第几个通道插入pitch bend信息，以0作为第1个MIDI通道，如果为'all'则往所有的MIDI通道插入相同的pitch bend信息
# track: MIDI轨道，一般情况下不用设置
# mode: 弯音信息的单位，之前有详细的说明
# ind: 弯音信息插入的位置，如果start_time有设置则以start_time的位置为准

# 查看乐曲类型个MIDI通道数量
>>> len(a)
4

# 添加新的音轨
a.append(new_track)
# new_track: 新添加的音轨，必须为音轨类型

# 可以使用build函数通过多个音轨类型构建乐曲类型
new_piece = build(track(A1, instrument=1, start_time=0, channel=0, track_name='piano'),
                  track(B1, instrument=35, start_time=1, channel=1, track_name='electric bass'),
                  track(C1, instrument=1, start_time=8, channel=9, track_name='drums'),
                  track(D1, instrument=49, start_time=16, channel=2, track_name='strings'),
                  bpm=150)
# 请注意，用build函数传入音轨类型来构建乐曲类型时，得到的乐曲类型的BPM只取决于build函数的bpm这个参数，
# 与传入的音轨类型自带的BPM无关
```



## 将一个乐曲类型合并为一个和弦类型

有的时候我们需要把一个乐曲类型(piece类型)，也就是拥有多个不同的音轨和多种不同的乐器的乐理类型的所有音轨的音符都合并为一个和弦类型，
以便于一些剪辑和乐理功能的操作（因为有很多乐理功能是和弦类型独有的），那么我们可以使用乐曲类型的内置函数`merge`。

```python
a = read('example.mid') # a是读取example.mid这个MIDI文件之后转换为的乐曲类型

a.merge(add_labels=True,
        add_pan_volume=False,
        get_off_drums=False,
        track_names_add_channel=False)

# add_labels: 为True的时候，会往乐曲类型的每个轨道的每个音符加入值为当前通道的index的新属性track_num，
# index是以第一个通道为0作为开始的当前的通道数，每个通道里的音符的track_num在之后可以被调用，
# 可以记录音符在合并前来自于哪一个轨道

# add_pan_volume: 为True的时候，会将pan和volume信息转换为MIDI CC信息，存入返回的和弦类型的other_messages中

# get_off_drums: 为True的时候，结果会去除鼓轨

# track_names_add_channel: 当设置为True时，乐曲类型中的音轨名称事件将存储音轨的通道

b, bpm, start_time = a.merge()
# 使用乐曲类型的内置函数merge可以得到(合并后的和弦类型, bpm, 开始时间)的元组，开始时间的单位为小节
```



## 乐曲类型新增乐曲名称的属性以及BPM的显示

乐曲类型现在新增了乐曲名称的属性，在构建乐曲类型时，使用`name`参数就可以设定乐曲类型的名字，也就是曲名，在打印乐曲类型时第一行会显示乐曲名（如果为None就不显示），并且在第二行会显示乐曲的BPM。



## 乐曲类型新增的一些语法



### 替换一个乐曲类型中的音轨

```python
a[i] = new_track # 把乐曲类型a的第i个音轨(从0开始)替换为new_track, 这里的new_track为音轨类型
```



### 删除一个乐曲类型中的音轨

```python
del a[i] # 删除乐曲类型a的第i个音轨(从0开始)
```



### 把一个乐曲类型的音轨静音

你可以使用`mute`函数把一个乐曲类型中的音轨静音，使用`unmute`函数取消静音。

```python
a.mute(i=None) # 如果不指定i，则将全部的音轨静音，如果指定i，则将第i个音轨静音(从0开始)
a.unmute(i=None) # 取消静音，参数的用法和mute函数一样
```



## 清除乐曲类型的pan和volume信息

你可以使用乐曲类型的`clear_pan`函数来清除乐曲类型的pan信息(左右声道混音位置)，使用乐曲类型的`clear_volume`函数来清除乐曲类型的volume信息(音轨总体音量大小)。

```python
a.clear_pan()
a.clear_volume()
```



## 清除音色改变的MIDI信息

当你读取一个MIDI文件时，经常会有其他的MIDI信息被读取到`other_messages`中，作为转换为的musicpy数据结构的属性，如果其中有`program_change`信息，就会在再次写入时也一样写入到MIDI文件中，让音轨的乐器强制改变，这会在你重新设定写入MIDI文件的乐器时造成困扰。你可以在写入MIDI文件的`write`函数中把`nomsg`的值设定为`True`,这样就不会写入其他的MIDI信息，但是有的时候你又想保留其他的不是改变乐器的MIDI信息，那么怎么办呢？为了解决这个问题，你可以使用`clear_program_change`函数来单独清除一个和弦类型或者乐曲类型的`program_change`信息，这样当再次写入MIDI文件时，MIDI文件的每个音轨就会按照你设定的乐器来进行播放了。

```python
a = read('test.mid') # 读取一个MIDI文件，转换为乐曲类型

a.clear_program_change() # 清除乐曲类型的program_change信息

# 乐曲类型的clear_program_change函数有一个参数apply_tracks, 如果为True,
# 会清除乐曲类型本身的program_change信息以及每一个音轨的program_change信息,
# 如果为False, 则只会清除乐曲类型本身的program_change信息, 默认值为True

a.clear_program_change(apply_tracks=False)

b = a.tracks[0] # 提取乐曲类型a的第一个音轨，是一个和弦类型
b.clear_program_change() # 清除和弦类型的program_change信息
```



## 清除其他的MIDI信息

你可以使用`clear_other_messages`函数清除一个和弦类型或者乐曲类型的所有的其他的MIDI信息，或者按照MIDI信息类型来进行筛选清除。  
如果你只是想要完全清除一个和弦类型或者乐曲类型的其他的MIDI信息，你只需要把它们的`other_messages`属性的列表清空即可，或者重新赋值为一个空列表。

```python
# 这两个方法都可以完全清空一个和弦类型或者乐曲类型的其他的MIDI信息
a.other_messages.clear()
a.other_messages = []

a.clear_other_messages() # 清除所有的其他的MIDI信息
a.clear_other_messages('program_change') # 清除其他的MIDI信息中的program_change类型的信息

# 当a是一个乐曲类型时，与clear_program_change函数类似，也有一个apply_tracks的参数，
# 用法也类似，默认值为True
```



## 快速改变乐曲类型的音轨的乐器

你可以使用乐曲类型的`change_instruments`函数来快速改变乐曲类型整体的每个音轨的乐器或者单个音轨的乐器，而不需要通过修改`instruments`和`instruments_numbers`属性来进行音轨的乐器的修改。你可以传入乐器名或者乐器的MIDI编号，可以进行整体的所有音轨的乐器的替换，或者指定某条音轨的乐器的替换。注意：如果你想通过修改属性来进行乐曲类型的音轨的乐器的修改，必须要修改`instruments_numbers`里的MIDI编号，因为它们才是在写入MIDI文件时真正有效的信息，`instruments`的修改只会影响乐曲类型显示时的乐器名。

```python
change_instruments(instruments, ind=None)

# instruments: 乐器的列表(所有音轨的乐器替换)或者单个乐器，可以为乐器名或者MIDI编号。
# General MIDI的乐器名和MIDI编号的对应关系可以打印变量'INSTRUMENTS'或者'reverse_instruments'来查看

# ind: 如果为None，则进行乐曲类型的音轨的乐器的整体替换，此时instruments必须为一个list或者tuple，
# 如果为一个整数，则把对应的index的音轨的乐器进行替换，index从0开始

piece1 = P([C('C'), C('D')], [1, 49])
>>> piece1
[piece] 
BPM: 120
track 1 | instrument: Acoustic Grand Piano | start time: 0 | chord(notes=[C4, E4, G4], interval=[0, 0, 0], start_time=0)
track 2 | instrument: String Ensemble 1 | start time: 0 | chord(notes=[D4, F#4, A4], interval=[0, 0, 0], start_time=0)

piece1.change_instruments([2, 47]) # 改变整体的音轨的乐器
>>> piece1
[piece] 
BPM: 120
track 1 | instrument: Bright Acoustic Piano | start time: 0 | chord(notes=[C4, E4, G4], interval=[0, 0, 0], start_time=0)
track 2 | instrument: Orchestral Harp | start time: 0 | chord(notes=[D4, F#4, A4], interval=[0, 0, 0], start_time=0)

# 或者也可以写
piece1.change_instruments(['Bright Acoustic Piano', 'Orchestral Harp'])

piece1.change_instruments(5, 0) # 把第1条音轨的乐器改变为MIDI编号为5的乐器
>>> piece1
[piece] 
BPM: 120
track 1 | instrument: Electric Piano 1 | start time: 0 | chord(notes=[C4, E4, G4], interval=[0, 0, 0], start_time=0)
track 2 | instrument: Orchestral Harp | start time: 0 | chord(notes=[D4, F#4, A4], interval=[0, 0, 0], start_time=0)

# 或者也可以写
piece1.change_instruments('Electric Piano 1', 0)
```



## 按照小节长度移动乐曲类型的音轨的位置

你可以使用乐曲类型的`move`函数，来将乐曲类型的整体的音轨或者指定的音轨进行往右或者往左的位置的指定小节长度的移动。返回值是一个新的乐曲类型。

```python
move(time=0, ind='all')

# time: 移动的长度，单位为小节，为正表示向右方向移动，为负表示向左方向移动

ind: 进行移动的音轨的index，如果为'all'，则移动所有的音轨，如果为整数，则移动对应的index的音轨，从0开始

>>> a.start_times
[0, 2, 3]

b = a.move(2) # 乐曲类型a整体向右移动2个小节的长度

>>> b.start_times
[2, 4, 5]

c = a.move(2, 1) # 乐曲类型a的第1个音轨向右移动2个小节的长度

>>> c.start_times
[2, 2, 3]
```



## 筛选和弦类型和乐曲类型中的某一种MIDI信息

你可以使用`get_msg`函数来筛选和弦类型中的某一种MIDI信息，返回一个MIDI信息的列表，比如

```python
a.get_msg('program_change') # 筛选和弦类型a中的program_change信息
```

乐曲类型也有`get_msg`函数，你可以通过指定`ind`参数来提取某一个音轨的某一种MIDI信息，如果不指定，则从所有音轨中查找。

```python
a.get_msg('program_change', ind=0) # 筛选第1条音轨中的program_change信息
```



## 对乐曲类型整体进行转调

使用乐曲类型的`modulation`函数可以对于乐曲类型整体进行转调，返回的是转调之后的新的乐曲类型。

```python
modulation(old_scale, new_scale, mode=1, inds='all')

# old_scale: 之前的音阶类型
# new_scale: 之后的音阶类型
# mode: 当mode为1时，通道编号为9的轨道(鼓轨)不会进行转调
# inds: 当inds为'all'时，转换所有的音轨，也可以为一个index的列表，转换列表中的index对应的音轨
```



## 对乐曲类型整体应用函数

使用乐曲类型的`apply`函数可以对于乐曲类型的每个音轨都应用一个函数。

```python
apply(func, inds='all', mode=0, new=True)

# func: 应用到每个音轨上的函数

# inds: 当inds为'all'时，应用函数到所有的音轨上，也可以为一个index或者index的列表

# mode: 当mode为0时，应用函数的音轨会被应用的结果所替代，当mode为1时，只进行应用函数到音轨的步骤

# new: 当new为True时，返回全新的乐曲类型，当new为False时，直接在原来的乐曲类型上修改
```



## 重新设置MIDI通道编号和音轨编号

和弦类型和乐曲类型有`reset_channel`和`reset_track`函数，可以重新设置所有音符，弯音信息，其他MIDI信息的MIDI通道编号和MIDI音轨编号。这两个函数都是直接在原来的对象上直接修改。

```python
# 和弦类型
reset_channel(channel,
              reset_msg=True,
              reset_pitch_bend=True,
              reset_note=True)

# channel: MIDI通道编号
# reset_msg: 是否重新设置所有其他MIDI信息的MIDI通道编号
# reset_pitch_bend: 是否重新设置所有弯音信息的MIDI通道编号
# reset_note: 是否重新设置所有音符的MIDI通道编号

reset_track(track, reset_msg=True, reset_pitch_bend=True)

# track: MIDI音轨编号
# reset_msg: 是否重新设置所有其他MIDI信息的MIDI音轨编号
# reset_pitch_bend: 是否重新设置所有弯音信息的MIDI音轨编号


# 乐曲类型
reset_channel(channels,
              reset_msg=True,
              reset_pitch_bend=True,
              reset_pan_volume=True,
              reset_note=True)

# channels: MIDI通道编号的列表，也可以为一个整数，将每个音轨的MIDI通道编号都设置为这个整数
# reset_msg: 是否重新设置所有其他MIDI信息的MIDI通道编号
# reset_pitch_bend: 是否重新设置所有弯音信息的MIDI通道编号
# reset_pan_volume: 是否重新设置所有声相和音轨音量的MIDI通道编号
# reset_note: 是否重新设置所有音符的MIDI通道编号

reset_track(tracks,
            reset_msg=True,
            reset_pitch_bend=True,
            reset_pan_volume=True)

# tracks: MIDI音轨编号的列表，也可以为一个整数，将每个音轨的MIDI音轨编号都设置为这个整数
# reset_msg: 是否重新设置所有其他MIDI信息的MIDI音轨编号
# reset_pitch_bend: 是否重新设置所有弯音信息的MIDI音轨编号
# reset_pan_volume: 是否重新设置所有声相和音轨音量的MIDI音轨编号
```



## 得到一个乐曲实例中的音符总数

你可以使用乐曲类型的`total`函数来获得乐曲实例中的音符总数。当你想计算一个MIDI文件中的音符总数时，这很有用，你可以把MIDI文件读取到一个乐曲实例中，然后使用这个函数。这个函数将计算出乐曲实例中所有音轨的音符数之和。

```python
total(mode='all')

# mode: 如果是'all'，那么返回总的音符数，包括乐曲实例中的弯音和速度变化信息。
# 如果是'notes'，那么只返回音符的总数。默认是'all'。

a = P([C('C'), C('Cmaj7')])
>>> a.total()
7
```



## 去除一个乐曲类型的鼓轨
你可以使用乐曲类型的 `get_off_drums` 函数去除一个乐曲类型的鼓轨。这个函数在通道属性不为None的时候有效。
```python
a = P([C('C'), drum('0,1,2,1').notes], channels=[0, 9])
>>> a
[piece] 
BPM: 120
track 1 channel 0 | instrument: Acoustic Grand Piano | start time: 0 | chord(notes=[C4, E4, G4], interval=[0, 0, 0], start_time=0)
track 2 channel 9 | instrument: Acoustic Grand Piano | start time: 0 | chord(notes=[C2, F#2, E2, F#2], interval=[1/8, 1/8, 1/8, 1/8], start_time=0)
>>> a.get_off_drums()
>>> a
[piece] 
BPM: 120
track 1 channel 0 | instrument: Acoustic Grand Piano | start time: 0 | chord(notes=[C4, E4, G4], interval=[0, 0, 0], start_time=0)
```
