# 实用的功能



## 目录

- [播放音符，和弦，乐曲等乐理类型](#播放音符和弦乐曲等乐理类型)
- [读取MIDI文件，转换成乐曲类型，以便于进行各种乐理上的操作](#读取midi文件转换成乐曲类型以便于进行各种乐理上的操作)
- [把乐理类型写入MIDI文件，以方便DAW中查看和编辑](#把乐理类型写入midi文件以方便daw中查看和编辑)
- [我专门为musicpy这个项目写了一个IDE供大家使用](#我专门为musicpy这个项目写了一个ide供大家使用)
- [将musicpy的数据结构存储为单独的数据文件](#将musicpy的数据结构存储为单独的数据文件)
- [音符和频率之间的转换](#音符和频率之间的转换)
- [play函数在选择乐器的时候可以用简写](#play函数在选择乐器的时候可以用简写)
- [当读取的MIDI文件是把所有不同的乐器的音符全部放在一个MIDI音轨里如何转换为乐曲类型](#当读取的midi文件是把所有不同的乐器的音符全部放在一个midi音轨里如何转换为乐曲类型)
- [新增对于小写字母音名的支持](#新增对于小写字母音名的支持)
- [eval_time函数新增返回时间数值的功能](#eval_time函数新增返回时间数值的功能)
- [reverse函数的重新设计](#reverse函数的重新设计)
- [新增读取其他种类的MIDI信息的功能](#新增读取其他种类的midi信息的功能)
- [和弦类型和乐曲类型的清除速度变化信息和弯音信息的函数的改进](#和弦类型和乐曲类型的清除速度变化信息和弯音信息的函数的改进)
- [停止目前所有正在播放的声音](#停止目前所有正在播放的声音)
- [音程名称的使用](#音程名称的使用)
- [修改乐理类型的属性的同时返回新的乐理类型](#修改乐理类型的属性的同时返回新的乐理类型)
- [将音乐数据结构转换为JSON格式](#将音乐数据结构转换为JSON格式)
- [读取和写入musicxml文件](#读取和写入musicxml文件)



## 播放音符，和弦，乐曲等乐理类型

musicpy里面有一个函数叫做`play`, `play`函数可以播放音符，和弦，乐曲等乐理类型，并且同时生成MIDI文件。
`play`函数的第一个参数是乐理类型，支持的乐理类型包括音符，和弦，乐曲，音轨，鼓点。之后几个比较重要的关键字参数是`bpm` (速度), `name` (MIDI文件的名字), `instrument` (乐器类型)。
其他的参数可以参考`write`函数的参数，我在后面的文档中会讲到。

**注意：在某些IDE中(比如VS Code)这个播放函数可能会无法发出声音，这是因为程序在播放开始之前就提前退出的原因，你可以将`play`函数中的`wait`参数设置为True,这样可以让程序在保证播放完成之前等待，就可以听到声音了。**

* ### 播放音符

```python
a = note('A', 5)
play(a)
```

运行就可以听到音符A5的钢琴声音了。

```python
# 在某些IDE里需要设置`wait`参数为True才可以听到声音
a = note('A', 5)
play(a, wait=True)
```

* ### 播放和弦

```python
b = get_chord('C', 'maj7', intervals=1, duration=2)
play(b, 150)
```

以150的BPM播放一个C大七和弦的分解和弦。

```python
c = get_chord('D', 'm7', intervals=1)*2 + get_chord('E', 'maj9', intervals=0.5).reverse()
play(c, 100)
```

以100的BPM播放D小七和弦两遍（每个音符间距都为1个小节），然后E大九和弦倒着弹下来（每个音符间距都为0.5个小节）。

* ### 选择乐器类型

  play函数里的instrument参数是乐器名，默认值为None，对应的是General Midi的乐器列表，除了输入乐器名，也可以直接输入乐器编号(0~127之间的任意整数)，比如

```python
play(c, 100, instrument='Violin')
```

会以小提琴的MIDI音色演奏跟之前同样的内容。

* ### play函数的运行机制

  运行play函数的时候，会在musicpy的文件夹里自动生成一个MIDI文件，里面的内容就是play函数接收的musicpy代码所对应的音乐。

实际上play函数的机制就是把musicpy代码对应的音乐的内容写入一个新的MIDI文件，然后调用pygame的函数播放MIDI文件（之前是调用本地播放器，最近改进为直接在内部用pygame播放，无需调用本地播放器）。

这个生成的MIDI文件也方便大家之后继续放在编曲软件里进行观看和使用。

此外，任何一个音符与和弦类型也可以调用自己的play函数进行播放，比如播放和弦A，可以写：

```python
A.play(150)
```

以150的BPM播放和弦A。

## 读取MIDI文件，转换成乐曲类型，以便于进行各种乐理上的操作

使用read函数可以读取一个MIDI文件，将MIDI文件的内容转换成乐曲类型。

```python
read(name,
     is_file=False,
     get_off_drums=False,
     clear_empty_notes=False,
     clear_other_channel_msg=False,
     split_channels=None)
```

* name: MIDI文件名（包括.mid文件扩展名）
* is_file: 是用来处理当第一个参数name传入的是一个MIDI文件流的情况。第一个参数name如果是MIDI文件流，is_file需要设置为True以进行正常的读取。默认值为False，第一个参数为MIDI文件名的时候用不到这个参数。
* get_off_drums: 设置为True的时候，过滤掉鼓轨。get_off_drums的默认值为False。
* clear_empty_notes: 设置为True时，在转换每个音轨为和弦类型时将所有duration <= 0的音符类型去除。
* clear_other_channel_msg: 设置为True时，会清除返回的每一个音轨中不属于当前音轨的通道编号的MIDI信息。
* split_channels: 当读取的MIDI文件是把所有不同的乐器的音符全部放在一个MIDI轨道里，设置这个参数为True以转换为正确的乐曲类型。

### read函数使用示例：

比如现在有一个MIDI文件`Clair de Lune.mid`，现在使用read函数读取，转换为乐曲类型。

```python
current_piece = read('Clair de Lune.mid')
```

现在我们可以对乐曲类型current_piece进行各种乐理上的操作了。乐曲类型current_piece里面存储的是整首曲子的信息，包括所有的音轨，乐器，音符，音符的间隔等。可以使用基本语法部分里面讲到的很多语法来对乐曲类型current_piece进行乐理上的玩转，比如转调，升调，降调，切片，倒序等等。

## 把乐理类型写入MIDI文件，以方便DAW中查看和编辑

使用write函数可以把一个音符类型，和弦类型，乐曲类型等乐理类型写入MIDI文件。

实际上play函数里面就有使用到write函数，然后调用pygame播放生成的MIDI文件。

```python
write(current_chord,
      bpm=120,
      channel=0,
      start_time=None,
      name='temp.mid',
      instrument=None,
      i=None,
      save_as_file=True,
      msg=None,
      nomsg=False,
      ticks_per_beat=960,
      **midi_args)
```

* current_chord: 想要写入的乐理类型，可以为音符类型，和弦类型，乐曲类型，音轨类型，鼓点类型，也可以是一个和弦类型的列表。

* bpm: 想要写入的曲子的BPM（曲速）。

* channel: 想要写入的频道编号，需要注意的是9对应的是鼓的轨道。

* start_time: 想要写入的曲子的起始时间（单位为小节），如果为None，则使用和弦类型自带的start_time。

* name: 想要的MIDI文件名，生成的MIDI文件的名字。

* instrument: 想要写入的轨道的乐器类型，对应的是General Midi的乐器，编号从0~127,可以输入乐器名（General Midi的乐器列表里的乐器名），也可以直接输入数字（0-127之间的任何一个整数）。

* i: 与instrument参数一样

* save_as_file: 设置为True的时候，在本地写入或者生成MIDI文件，不返回任何值。设置为False的时候，返回一个MIDI文件流。默认值为True。

* msg: 你想要写入MIDI文件的其他的MIDI信息的列表

* nomsg: 可以设置是否写入其他的MIDI信息,为True的时候将不会写入

* ticks_per_beat: MIDI文件每拍的tick数，设置得越高，分辨率越高

* midi_args: MIDI文件的其他参数，详情请参考[mido的官方文档](https://mido.readthedocs.io/en/latest/lib.html#midi-files)

## 我专门为musicpy这个项目写了一个IDE供大家使用

[musicpy_editor](https://github.com/Rainbow-Dreamer/musicpy_editor)是我专门为musicpy语言开发的一个IDE。

这个编辑器里面有很多方便快速写musicpy代码的功能，并且可以实时听到对应的音乐。

接下来详细介绍一下这个编辑器。

首先，在上面的框输入musicpy代码，在下方的框显示结果，实时运行，不使用print和自动补全都是默认打开的，不用的时候也可以到对应的打钩框关闭。自动补全让你在写代码时可以只打一两个字符就给出一个包含这几个字符的函数方法的列表，让你快速选择，鼠标点击和方向盘上下键加上回车选择都可以。自动补全在一个对象的"."之后也会开启，此时的自动补全列表是这个对象所拥有的类方法和属性。小括号和中括号自动配对补全（打出左边的括号，会自动填上右边的括号），并且输入的光标自动放在两个括号之间。文件栏里面有打开musicpy代码文件（只要是文本文件格式都可以），保存当前写的musicpy代码为文件，以及设置。这个编辑器也支持语法高亮，同时大家也可以在设置里自己定制语法高亮的内容和对应的颜色。

我还给这个编辑器加入了输入以及输出界面的开灯/关灯功能，让大家可以在白底黑字主题和黑底白字主题来回切换，以适应不同程序员的的口味。同时，大家可以自己选择字体类型和字体大小，以及自己定制编辑器的部件的背景颜色，字体颜色以及鼠标光标移到部件上显示的颜色。设置文件config.py里的所有参数都可以修改，而且不需要打开config.py修改，只需要打开编辑器，然后点击左上角的文件——设置，在弹出的设置窗口里修改参数，然后点击保存按钮就可以了。某些参数修改后需要重启编辑器才会看到修改后的效果，我接下来在介绍设置里的参数的部分会说。

在设置里的参数的说明：

* bakground_image: 可以选择背景图片的文件路径，点击“更改”按钮就可以打开一个文件浏览框，选择自己想要设置的背景图片的文件路径，也可以手动输入文件路径。修改之后点击保存就会重新加载背景图片
* background_places: 背景图片的像素位置，修改之后点击保存就会重新加载背景图片的位置
* pairing_symbols: 自动补全符号的列表，可以自己定制想要自动补全的符号
* font_type: 输入和输出窗口的字体类型
* font_size: 输入和输出窗口的字体大小
* background_mode: 开灯/关灯模式的参数，`white`表示开灯模式，`black`表示关灯模式，在编辑器的主界面会有一个按钮可以切换开灯和关灯
* syntax_highlight: 语法高亮的字典，键为颜色名称，值为需要语法高亮为这种颜色的单词的列表
* background_color: 编辑器里的部件的背景颜色
* foreground_color: 编辑器里的部件的字体颜色
* active_background_color: 鼠标光标移到编辑器里的部件上显示的背景颜色
* button_background_color: 关闭编辑器时保存文件的窗口的按钮的背景颜色
* active_foreground_color: 鼠标光标移到编辑器里的部件上显示的字体颜色
* language: 编辑器使用的语言

不使用print如果打勾，在输入一行代码时，在每一行，如果有可以显示出来的东西，下面的框就会显示，等价于自动加上了print。实时运行如果打勾，编辑器会在你写的代码发生改变时实时运行你写的代码，并且在下方的框里显示出结果（如果不使用print打勾的话）。在这个musicpy编辑器里，除了实时运行，自动补全，不使用print的几个打勾框和文件栏之外，还有保存的按钮，保存当下写的代码为文件；运行的按钮，运行当下写的代码，如果当前的代码运行会出现错误，则显示出错误信息在下方的框里，并且不会影响到编辑器的正常工作。在实时运行的时候，如果当前的代码运行会出现错误，则在下方的框不会显示任何东西，此时点击运行的按钮可以看到错误信息；自动换行的按钮，可以让下面的框显示的运行结果自动换行。

这个musicpy编辑器有几个非常好用的功能，接下来一一介绍。

1. 在一行musicpy代码之前加上`/`,就会直接播放这段代码代表的音乐，并且是内部播放，并不会打开任何电脑上的播放器。与此同时也会在musicpy文件夹里生成当前的musicpy代码对应的MIDI文件。这个语法等价于在这行代码放在play函数里面，play函数的参数设置可以用英文的逗号跟在代码的后面，比如bpm（曲速），instrument(乐器)等等的选择。建议实时运行同时打开（默认是打开的），就可以musicpy代码写到哪，加上`/`就可以马上听到。比如在编辑器里写 `/C('Dmaj7') * 4 | C('Em7') * 4, 150` 就可以直接听到这段musicpy语言对应的音乐了。

2. 在一行musicpy代码前（特别是表示一个和弦的代码）加上`?`可以得到这个和弦按照乐理逻辑判断出来的和弦类型名称，比如 `?chord(['C','E','G','B'])` 会得到`Cmaj7`。
   这个语法等价于表示一个和弦的musicpy代码放在detect函数里面，detect函数的参数配置也可以用英文的逗号跟在代码的后面。

3. 点击鼠标右键可以弹出IDE的菜单，可以选择播放选中的musicpy语句，也可以选择可视化播放选中的musicpy语句，可视化的窗口是我写的另一个音乐相关的项目，智能钢琴[Ideal Piano](https://github.com/Rainbow-Dreamer/Ideal-Piano)的主体。

4. 在鼠标右键菜单中，可以选择导入MIDI文件，（在文件栏里也有）选择一个MIDI文件之后会自动生成一个默认参数的read函数的代码，并且赋值给一个变量new_midi_file，变量名可以自行修改。停止播放是用来在播放musicpy代码时，如果只想听一段，不想听完整首曲子，那么可以马上停止播放。

5. 这个编辑器里有内置很多电脑键盘的快捷键的组合，分别对应多种不同的功能。

```
ctrl + e 停止播放  

ctrl + d 导入MIDI文件  

ctrl + w 打开文件(文本文件)  

ctrl + s 保存当前的代码为文本文件  

ctrl + q 关闭编辑器  

ctrl + r 运行当前的代码  

ctrl + g 开灯/关灯   

ctrl + 鼠标滚轮可以调节字体大小，往上滚字体变大，往下滚字体变小

右下角的Line Col显示的是当前输入光标所在的行数和列数，以方便在代码出错的时候查看对应的位置

alt + z 播放选中的musicpy代码  

alt + x 可视化播放选中的musicpy代码
```

这个musicpy编辑器我之后还会多加完善，希望大家用的开心~

## 将musicpy的数据结构存储为单独的数据文件

你可以使用`write_data`函数将任何的musicpy的数据结构存储为一个单独的二进制数据文件，建议的文件后缀名为`mpb` (musicpy binary), 比如

```python
write_data(C('C'), name='C major chord.mpb')
```

这行代码会在当前的文件目录下创建一个名为`C major chord.mpb`的musicpy的数据结构的二进制数据文件。

你可以使用`load_data`函数加载任何的mpb文件为musicpy的数据结构，比如

```python
result = load_data('C major chord.mpb')

>>> result
chord(notes=[C4, E4, G4], interval=[0, 0, 0], start_time=0)
```

在有些时候，当你不想存储musicpy代码或者其生成的MIDI文件时，存储为二进制数据文件是一个不错的选择。

## 音符和频率之间的转换

你可以使用`get_freq`函数来得到一个音符的频率，可以设置标准音高A4的频率以获得不同标准下的音符的频率。  
你可以使用`freq_to_note`函数来把一个频率转换为最接近这个频率的音符，可以设置标准音高A4的频率以获得不同标准下的最接近这个频率的音符。

```python
# get_freq函数的参数可以是表示音符的字符串或者音符类型
>>> get_freq('C5') # 获得音符C5的频率，默认的标准音高A4为440 hz
523.2511306011972

>>> get_freq('C5', standard=415) # 获得音符C5的频率，使用巴洛克标准音高415 hz
493.5209527261292

>>> freq_to_note(500) # 获得最接近500 hz的频率的音符，默认的标准音高A4为440 hz，返回的是音符类型
B4

>>> freq_to_note(500, to_str=True) # 设置to_str参数为True，返回表示音符的字符串
B4

>>> freq_to_note(500, standard=415) # 获得最接近500 hz的频率的音符，使用巴洛克标准音高415 hz
C5
```

## play函数在选择乐器的时候可以用简写

在使用play函数指定乐器演奏的时候，可以用instrument=乐器，现在加入了另一个比较简短的参数，比如演奏a这个和弦类型，可以写

```python
play(a, i=乐器)
```

等价于

```python
play(a, instrument=乐器)
```

## 当读取的MIDI文件是把所有不同的乐器的音符全部放在一个MIDI音轨里如何转换为乐曲类型

我最近在读取一些东方的官方MIDI文件的时候，发现ZUN貌似基本上都是把所有不同的乐器的音符全部放在了一个MIDI音轨里，一般正常来说一个MIDI文件应该是有最多16个MIDI通道，有多个MIDI音轨，每个MIDI音轨可以有自己的乐器设定，有自己的乐器演奏的音符，并且每一个MIDI音轨还可以有自己的名字，还有MIDI通道编号。可是在我试着读取一些ZUN的东方曲官方MIDI文件时(从THBWiki上面下载的)，发现ZUN制作的MIDI文件是把所有的不同乐器的MIDI通道的音符全部放在了一个MIDI音轨里，每个音符有一个MIDI通道编号的属性，以区分每个音符是属于哪个MIDI通道的，并且在MIDI文件的开头也有program change的信息，分别指明了每个MIDI通道的编号的乐器类型，可是实际上只有1个MIDI音轨，我必须设计一些算法将这些音符分配到他们属于的MIDI音轨上，并且设定好每个MIDI音轨的乐器，然后才方便将MIDI文件转换为乐曲类型，而且因为ZUN把所有的不同的MIDI通道（对应着不同的乐器）的音符全部放在了1个MIDI音轨里，因此音符之间的间隔并不是分开之后每个MIDI音轨里的音符之间的间隔，我需要重新计算每个MIDI音轨的正确的音符的间隔，还有音符的长度，以及每个MIDI音轨的开始时间。如果大家在读取MIDI文件的时候遇到了这种情况，在使用read函数的时候，把`split_channels`设置为True，就可以进行以上的操作。

```python
# 读取《东方红魔乡》的第一首曲子，也就是标题主题曲《赤より紅い夢》的官方MIDI文件，
# 也就是ZUN自己做的并且公开在游戏里的MIDI文件

# th06自然指的是东方系列正作游戏的第六作，前五作为旧作，都是在90年代末发行的。
# 《东方红魔乡》作为第六作，也是新作的第一作。

a = read('th06_01.mid', split_channels=True)
```

## 新增对于小写字母音名的支持

在2021年的4月份加入了对于小写字母音名的支持，现在的最新版本的musicpy在构建任何的乐理类型需要输入音名时，之前大写的音名都可以写成小写的，同样也可以正常运行，比如

```python
a = note('A', 5)
a = note('a', 5)

b = note('Gb', 5)
b = note('gb', 5)

c = N('A6')
c = N('a6')

d = get_chord('Bb3', 'm11')
d = get_chord('bb3', 'm11')

e = C('Bbm11', 3)
e = C('bbm11', 3)

f = S('A major')
f = S('a major')

g = chord('C5, Eb5, G5, A#5, D#6, Bb6, F5')
g = chord('c5, eb5, g5, a#5, d#6, bb6, f5')
```

## eval_time函数新增返回时间数值的功能

`eval_time`的参数mode设置为`number`的时候，返回秒数的数值，数值类型为小数。  
另外，除了和弦类型，`eval_time`函数同样也适用于乐曲类型。

```python
>>> a.eval_time(80, mode='number') # 计算一个和弦类型的时间长度，返回一个数值
92.56
```

## reverse函数的重新设计

在2021年6月，`reverse`函数有了重新的设计，重新设计过后的`reverse`函数的算法得到的结果与音频文件的反向本质上是一致的 (除了声音的开始和结束的反向的听感)。 
如果你想使用以前的reverse函数的算法，可以使用`reverse_chord`函数。这个改变使用于和弦类型，乐曲类型和音轨类型。

## 新增读取其他种类的MIDI信息的功能

现在除了musicpy中已经定义的MIDI信息的类型(包括`tempo`, `pitch_bend`, `pan`, `volume`)，在读取MIDI文件时，musicpy可以读取更多其他类型的MIDI信息，统一存放在`other_messages`这个属性里，`other_messages`是一个列表。`other_messages`一般来说只会读取到和弦类型与乐曲类型中。

在构建弦类型与乐曲类型的时候，也可以设定`other_messages`，把想要写入的其他的MIDI信息放在`other_messages`这个列表里，在使用`play`或者`write`函数时会默认写入其中存储的MIDI信息。

在使用`read`函数的时候，如果选择返回和弦类型，那么`other_messages`会添加到和弦类型的属性中,如果你不想要其他的MIDI信息，那么可以在读取之后把返回结果的和弦类型的`other_messages`使用列表的`clear`方法清空，比如`a.other_messages.clear()`。

如果选择返回乐曲类型，那么`other_messages`会添加到每一个音轨的和弦类型中，也会合并所有的音轨的其他的MIDI信息作为一个整体的`other_messages`添加到乐曲类型的属性中。在对于读取过后的乐曲类型提取某条音轨时，提取出来的音轨类型中的`content`(是一个和弦类型)也会有之前读取时添加的`other_messages`。

由于目前musicpy中已定义的其他的MIDI信息包括`program_change`，也就是改变乐器的信息，此时如果修改乐曲类型或者音轨类型的乐器，就很有可能因为`other_messages`中有着`program_change`的信息而导致在写入MIDI文件后乐器并没有发生变化，此时你可以把`play`函数和`write`函数中的`nomsg`参数设置为True，可以忽略`other_messages`(也就是不进行写入)，这样就可以让乐器发生变化。或者你也可以把`other_messages`的列表清空。如果你还想保留`other_messages`中的其他的MIDI信息，你可以把`other_messages`中的为`program_change`类型的MIDI信息去除，比如`a.other_messages = [i for i in a if i.type != 'program_change']`

在使用`play`函数或者`write`函数的时候，当写入的和弦类型或者乐曲类型（或者其他的乐理类型）没有`other_messages`属性或者`other_messages`的列表为空/None的时候，你可以指定`msg`参数，会写入`msg`参数的列表中的MIDI信息。如果写入的乐理类型有`other_messages`的属性并且不为空/None时，则会写入乐理类型自带的`other_messages`，此时`play`函数或者`write`函数的`msg`参数会被忽略。

如果你不想写入任何`other_messages`，那么可以设置`nomsg`参数为True (默认值为False)，会忽略传入的乐理类型的`other_messages`以及指定的`msg`参数。

### event类型

在musicpy里，event类型表示MIDI信息。

所有的event类型都有`type` (MIDI信息类型的字符串), `track` (音轨编号，从0开始), `start_time` (开始时间，从0开始，单位为小节)这3个参数，用法都是一样的。

event类型的`type`参数以mido库的MIDI信息类型为准，除了`track`和`start_time`之外的其他参数以mido库的对应的MIDI信息类型的参数为准，请参考mido的官方文档的[Message Types](https://mido.readthedocs.io/en/latest/message_types.html)和[Meta Message Types](https://mido.readthedocs.io/en/latest/meta_message_types.html)。

```python
event(type, track=0, start_time=0, is_meta=False, **kwargs)
```

## 和弦类型和乐曲类型的清除速度变化信息和弯音信息的函数的改进

现在和弦类型和乐曲类型的`clear_tempo`函数和`clear_pitch_bend`函数增加了`cond`参数，你可以指定一个条件函数(推荐使用lambda函数) 
作为想要清除掉的速度变化信息和弯音信息的筛选器，条件函数里面描述的是你想要清除的信息需要满足的条件，如果让条件函数返回True,则会被清除。

```python
# 清除和弦类型的速度变化和弯音信息，使用cond参数，乐曲类型的可以类比
a.clear_tempo(cond=lambda s: s.start_time == 1 and s.bpm == 120)
a.clear_pitch_bend(cond=lambda s: s.start_time == 1 and s.value == 0)
```

## 停止目前所有正在播放的声音

你可以使用`stopall`函数来停止目前所有正在播放的声音，具体来说是由`play`函数播放出来的声音。

```python
play(C('C') * 8)
stopall() # 停止目前正在播放的声音
```

## 音程名称的使用

在musicpy的database模块里有着完整的音程名称的定义。比如`major_third`(大三度)的值为4 (半音数), `perfect_eleventh`(完全十一度)的值为17 (半音数)。 
此外，最近也加入了音程名称的简写的定义，比如你可以使用`M3`来代替`major_third`(大三度), `m3`来代替`minor_third`(小三度), `M7`来代替`major_seventh`(大七度)等等。在这里我会展示出musicpy里完整的音程名称的定义，在这里的所有的音程名称都可以用`database.major_third`这样的方式使用。

```python
perfect_unison = diminished_second = P1 = d2 = 0
minor_second = augmented_unison = m2 = A1 = 1
major_second = diminished_third = M2 = d3 = 2
minor_third = augmented_second = m3 = A2 = 3
major_third = diminished_fourth = M3 = d4 = 4
perfect_fourth = augmented_third = P4 = A3 = 5
diminished_fifth = augmented_fourth = tritone = d5 = A4 = 6
perfect_fifth = diminished_sixth = P5 = d6 = 7
minor_sixth = augmented_fifth = m6 = A5 = 8
major_sixth = diminished_seventh = M6 = d7 = 9
minor_seventh = augmented_sixth = m7 = A6 = 10
major_seventh = diminished_octave = M7 = d8 = 11
perfect_octave = octave = augmented_seventh = diminished_ninth = P8 = A7 = d9 = 12
minor_ninth = augmented_octave = m9 = A8 = 13
major_ninth = diminished_tenth = M9 = d10 = 14
minor_tenth = augmented_ninth = m10 = A9 = 15
major_tenth = diminished_eleventh = M10 = d11 = 16
perfect_eleventh = augmented_tenth = P11 = A10 = 17
diminished_twelfth = augmented_eleventh = d12 = A11 = 18
perfect_twelfth = tritave = diminished_thirteenth = P12 = d13 = 19
minor_thirteenth = augmented_twelfth = m13 = A12 = 20
major_thirteenth = diminished_fourteenth = M13 = d14 = 21
minor_fourteenth = augmented_thirteenth = m14 = A13 = 22
major_fourteenth = diminished_fifteenth = M14 = d15 = 23
perfect_fifteenth = double_octave = augmented_fourteenth = P15 = A14 = 24
minor_sixteenth = augmented_fifteenth = m16 = A15 = 25
major_sixteenth = diminished_seventeenth = M16 = d17 = 26
minor_seventeenth = augmented_sixteenth = m17 = A16 = 27
major_seventeenth = M17 = 28
semitone = halfstep = 1
wholetone = wholestep = tone = 2
```

## 修改乐理类型的属性的同时返回新的乐理类型

常见的乐理类型都可以使用`reset`函数，重新设置任意多个属性，并且返回全新的乐理类型，这个函数只接受关键字参数。

```python
reset(**kwargs)

a = C('C')
>>> a
chord(notes=[C4, E4, G4], interval=[0, 0, 0], start_time=0)
>>> a.reset(interval=[1,1,1])
chord(notes=[C4, E4, G4], interval=[1, 1, 1], start_time=0)
```

## 将音乐数据结构转换为JSON格式

你可以将音乐数据结构转换为JSON文件，作为一种更便携的存储方法，并从JSON文件中读取，以获得存储在其中的音乐数据结构。

使用`write_json`函数将音乐数据结构转换成JSON文件。参数及其用法与`write`函数类似，支持的数据结构包括音符、和弦、乐曲、轨道、鼓。在转换过程中，为了简洁起见，所有的数据结构都将转换为乐曲实例。JSON文件将被写入给定的文件路径中。

```python
write_json(current_chord,
           bpm=120,
           channel=0,
           start_time=None,
           filename='untitled.json',
           instrument=None,
           i=None,
           msg=None,
           nomsg=False)
```

* filename: 转换后的JSON文件的文件名
* 其他参数的用法与`write`函数相同

使用`read_json`函数读取一个由`write_json`函数导出的JSON文件，并提取其中存储的音乐数据结构。返回值是一个乐曲实例。

```python
read_json(file)
```

* file: JSON文件的文件名

## 读取和写入musicxml文件

你需要通过`pip install partitura`来安装partitura来使用这个功能。

你可以使用`read_musicxml`函数来读取musicxml文件，并将其转换为乐曲实例。在返回的结果中，musicxml独有的信息以字典的形式存储在`musicxml_info`属性中。

```python
read_musicxml(file, load_musicxml_args={}, save_midi_args={})
```

* file: musicxml文件的文件名
* load_musicxml_args: 传递给`partitura.load_musicxml`函数的关键字参数字典。
* save_midi_args: 传递给`partitura.save_score_midi`函数的关键字参数字典。

你可以使用`write_musicxml`函数把音乐数据结构写到musicxml文件中。支持的数据结构与`write`函数相同。

```python
write_musicxml(current_chord, filename, save_musicxml_args={})
```

* current_chord: 你想写入的音乐数据结构
* filename: musicxml文件的文件名
* save_musicxml_args: 传递给`partitura.save_musicxml`函数的关键字参数字典。

