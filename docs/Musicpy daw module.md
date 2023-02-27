# Musicpy宿主模块

我在2021年6月为musicpy写了一个宿主模块，用于加载音源和导出音频文件（包括wav、mp3、ogg等），这将非常有用，因为现在你不再局限于MIDI文件（其实你也可以把musicpy导出的MIDI文件放在DAW中加载音源和导出音频文件）。

一些基本的混音和编辑功能已经实现，包括反向、左右声道混音位置、ADSR包络（目前只有音频音量）、淡入/淡出效果。

你还可以建立合成器上出现的基本波形，包括正弦波、锯齿波、方波、三角波和白噪音。

用musicpy宿主的这些基本波形制作新的音色，可能在不久的将来就能实现。

## 目录
- [导入前的准备工作](#导入前的准备工作)
- [初始化一个宿主对象](#初始化一个宿主对象)
- [支持的音源](#支持的音源)
- [加载音源](#加载音源)
- [播放和导出音频文件](#播放和导出音频文件)
- [修改宿主的属性](#修改宿主的属性)
- [添加、删除和清除通道](#添加删除和清除通道)
- [混音和编辑](#混音和编辑)
- [生成和播放波形](#生成和播放波形)
- [变调器 (只用一个录音音频文件就可以制作一个完整的音源)](#变调器只用一个录音音频文件就可以制作一个完整的音源)
- [更多关于MDI音源格式](#更多关于MDI音源格式)
- [一些其他的提醒和想法](#一些其他的提醒和想法)

## 导入前的准备工作

首先，打开cmd/terminal，运行`pip install sf2_loader pedalboard scipy numpy`来安装运行musicpy daw所有需要的python模块和包。如果你使用的是Linux或者macOS，依赖库sf2_loader有一些必须的配置步骤，详情请看[这里](https://github.com/Rainbow-Dreamer/sf2_loader#installation)。

接下来，请在[Release](https://github.com/Rainbow-Dreamer/musicpy/releases/latest)中下载`ffmpeg.zip`，从文件中提取ffmpeg文件夹，然后把文件夹放到`C:`，如果你是在Windows上，或者在macOS/Linux上的同等根路径。

然后将路径`C:\ffmpeg\bin`添加到系统的环境变量中（如果你是在macOS/Linux上，路径会有所不同，这取决于你放置ffmpeg文件夹的路径）。

(这一步在某些系统可能不需要) 最后，重新启动计算机。

然后你可以在IDE或cmd/terminal中用这一行导入宿主模块。
```python
from musicpy.daw import *
```

## 初始化一个宿主对象

假设我们想做一首新歌，我们将初始化一个名为`my first song`的宿主。

这首歌将包含3种乐器，每种乐器有一个通道，所以总共有3个通道。

```python
new_song = daw(3, name='my first song')
>>> print(new_song)
[daw] my first song
Channel 1 | not loaded
Channel 2 | not loaded
Channel 3 | not loaded
```
现在你已经初始化了一个有3个通道的宿主对象。`not loaded`意味着目前这个通道还没有加载任何音源。
接下来我们需要知道musicpy宿主目前支持哪些类型的音源并能成功加载。

## 支持的音源

Musicpy宿主目前可以加载的音源格式有: SoundFont音源文件(.sf2, .sf3, .dls)，音频文件(比如.wav, .mp3, .ogg)，还有一个我自己发明的音源文件格式`MDI`。

### SoundFont音源文件
SoundFont是一种非常流行的音源文件格式，你可以加载任何的SoundFont音源文件到musicpy的宿主中，支持.sf2, .sf3, .dls。

这个宿主模块自带[sf2_loader](https://github.com/Rainbow-Dreamer/sf2_loader)，这是我的另一个项目，使用教程可以看这个项目的readme，你可以使用sf2_loader的语法来切换加载的SoundFont的当前的乐器，单独播放一段musicpy代码等等。你可以通过`current_daw.instruments(i)`来得到第i个通道上加载的音源对象(从0开始)，(current_daw是你当前的宿主的变量名) 如果第i个通道上加载的是SoundFont文件，那么将会返回一个sf2_loader对象。

### 音频文件
Musicpy宿主可以加载一个文件夹的音频文件作为每个通道的音源，音频文件的格式可以是混合的（例如，文件夹可以包含wav、mp3、ogg等文件的混合物）。强烈建议将每个音频文件命名为一个音高，例如，`C5.wav`，这是一个带有八度数的音符名称。如果音符名中含有降号，建议改成带有升号的同等音名（在十二音平均律中），例如，如果是`Ab5.wav`，改成`G#5.wav`。

如果一些音频文件的名称不是音高，你将需要改变musicpy音符名称到这些音频文件的文件名的默认映射，以便将音高与你想要的相应音频文件正确匹配。我将在后面讲到这一点。

### MDI音源文件
`MDI`是我自己发明的一种音源格式，它可以把一个音频文件的文件夹（也许还有一个设置文件）合并成一个二进制文件，更方便操作和储存。

你可以使用`make_mdi`函数来轻松制作mdi文件。
```python
make_mdi(path_of_instruments_folder, name_of_instruments_you_want_to_have)
```
一个mdi文件将被生成在当前工作目录下。

我计划将来在musicpy宿主中增加对VST的支持（这是一种很常用的音频插件格式）。

## 加载音源

你可以为宿主对象中的每个通道加载一个音源。
```python
new_song.load(channel_number, path_of_instruments) # channel number从0开始

new_song.load(0, 'piano') #加载一个名为'piano'的其中有音频文件的文件夹作为第一个通道的音源

new_song.load(0, 'test.sf2') # 加载一个名为'test.sf2'的soundfont音源文件作为第一个通道的音源

new_song.load(0, 'piano.mdi') # 载入一个mdi文件作为第一个通道的音源
```

## 播放和导出音频文件

你可以使用宿主对象的`export`功能将musicpy数据结构转换为音频文件，或者使用加载的音源播放使用宿主对象的`play`功能来播放。 

支持导出或播放的musicpy数据结构包括音符、和弦、乐曲和轨道。 

如果输入的不是乐曲类型，你还可以指定使用哪个通道来播放或导出。如果输入的是乐曲类型，你可以用乐曲类型的`daw_channels`属性来指定每个音轨使用哪个通道。

```python
play(current_chord,
     channel_num=0,
     bpm=None,
     length=None,
     extra_length=None,
     track_lengths=None,
     track_extra_lengths=None,
     soundfont_args=None,
     wait=False)

# current_chord: 你想由宿主播放的musicpy数据结构

# channel_num：用于播放的通道的索引，基于0，当current_chord是音符或和弦类型时使用

# bpm：用于播放的BPM（速度），如果没有指定，宿主将使用自己的默认bpm

# length: 你可以指定渲染出来的音频的总长度，单位为秒 (用在有音效的情况下)

# extra_length: 你可以指定渲染出来的音频的额外长度，单位为秒 (用在有音效的情况下)

# track_lengths: 当current_chord是piece类型的实例时，每一个音轨的length的设置，可以是一个list或者tuple

# track_extra_lengths: 当current_chord是piece类型的实例时，每一个音轨的extra_length的设置，可以是一个list或者tuple

# soundfont_args: 参考export函数

# wait: 与musicpy的play函数中的`wait`参数作用相同


export(obj,
       mode='wav',
       action='export',
       filename='Untitled.wav',
       channel_num=0,
       bpm=None,
       length=None,
       extra_length=None,
       track_lengths=None,
       track_extra_lengths=None,
       export_args={},
       show_msg=False,
       soundfont_args=None)

# obj：你想由宿主导出的musicpy数据结构

# mode：你想拥有的导出音频文件的音频格式，支持wav、mp3、ogg等

# action：当设置为'export'，即默认值时，将输入的musicpy数据结构导出为一个音频文件;
# 当设置为'play'时，首先将输入的musicpy数据结构转换为一个AudioSegment实例（pydub中的一个音频类型），
# 然后播放AudioSegment实例；当设置为'get'时，将输入的musicpy数据结构转换为AudioSegment实例，
# 并返回AudioSegment实例供进一步使用

# filename：你想要的导出的音频文件的文件名

# channel_num：用于导出的通道索引，基于0，当current_chord为音符或和弦类型时使用

# bpm：用于导出的BPM（速度），如果不指定，宿主将使用自己的默认bpm

# length - track_extra_lengths: 和play函数相同

# export_args: 导出的音频文件的格式设置参数的关键字参数字典，
# 具体请参考pydub的AudioSegment的export函数的参数

# show_msg: 为True的时候，会在渲染为音频时打印当前的进度

# soundfont_args: 当音源为SoundFont文件的时候，渲染音频的关键字参数的字典，
# 这里的参数对应的是sf2_loader里的export_chord函数的参数


# 播放/导出和弦类型的例子
new_song.play(C('C')) # 用加载了通道1的音源的宿主对象播放一个C大三和弦
new_song.play(C('C'), 2) # 除了使用通道2，做同样的事情
new_song.play(C('C'), 2, bpm=165) #做同样的事情，除了使用通道2和BPM 165

new_song.export(C('C'), mode='wav') # 输出一个C大三和弦为wav文件，使用通道1的音源
new_song.export(C('C'), mode='mp3') # 将一个C大三和弦导出为mp3文件，使用通道1的音源
new_song.export(C('C'), channel_num=2, mode='wav', filename='my first song.wav') # 将一个 C大三和弦导出为
# 一个 wav 文件，使用通道 2 的音源，导出的 wav 文件的名字是 "my first song.wav"

#播放/导出乐曲类型的例子
rule1 = lambda x: x % (1 / 8, 1 / 8) @ [1, 2, 3, 2]
part1 = rule1(C('D#', 5) | rule1(C('F', 5) | rule1(C('Gm', 5) ) * 2
part1_bass = chord('D#3[.2;.], F3[.2;.], G3[.2;.], G3[.4;.], F3[.4;.')
part1_harmony = part1_bass + perfect_fifth
drums = drum('K, H, S, H, r:2').notes
drums.set_volume(80)
current_song = P([part1 * 2 | (part1 + database.octave) * 2, part1_bass * 4, part1_harmony * 2, drums * 4],
                 bpm=120,
                 start_times=[0, 0, 4, 4.03],
                 daw_channels=[0, 1, 1, 2])
new_song.play(current_song) # 播放当前的乐曲类型 current_song
new_song.export(current_song, filename='my first song.wav') # 将current_song的乐曲类型导出到
# 一个名为'my first song.wav'的wav文件
new_song.export(current_song, action='play') # 这将播放current_song的乐曲类型
# 但首先要把它编译成一个音频对象，这与play函数不同，后者是
# 即时播放，这种方法在开始播放时会比较慢，但会有更稳定的播放效果
```

## 修改宿主的属性

你可以通过修改`channel_names`属性来改变宿主对象的通道名称。
```python
new_song.channel_names = ['Piano', 'Bass', 'Electric Guitar']
>>> print(new_song)
[daw] my first song
Piano | not loaded
Bass | not loaded
Electric Guitar | not loaded

new_song.channel_names[0] = 'Piano 2' # 将第一个通道的名字改为'Piano 2'

# 或者你可以使用"set_channel_name"方法来设置通道的名称，索引是基于0的
new_song.set_channel_name(1, 'Piano 2') # 将第一个通道的名称改为'Piano 2'
```

你可以通过修改`channel_dict`属性来改变宿主对象的通道映射。 
如果不是所有加载的音源文件夹中的音频文件都被命名为音高，而你又懒得去重命名，那么改变一些通道的映射字典将是必要的。
```python
# 改变宿主对象的第3个通道的一些映射关系
# 注意，这些映射将总是一个音高映射到一个文件名（没有文件扩展名）
new_song.channel_dict[2]['C2'] = 'Kick'
new_song.channel_dict[2]['E2'] = 'Snare'
new_song.channel_dict[2]['F#2'] = 'CH1'
```
**请注意，当你修改了某个通道的映射字典后，如果这个通道目前已经加载了音源，那么你需要重新加载这个通道上的音源才会让映射字典的修改起效。**

你可以通过2种方式改变每个通道的音源。
```python
# 使用新的路径为通道加载新的音源
new_song.load(0, new_path_of_instruments)

# 或者在宿主对象的"channel_instrument_names"属性中修改某些通道的音源的路径
# 然后为修改了音源路径的通道重新加载
new_song.channel_instrument_names[0] = new_path_of_instruments
new_song.reload_channel_sounds(0)
```

你可以通过修改`name`属性来改变宿主对象的名称。
```python
new_song.name = 'another song'
>>> print(new_song)
[daw] another song
Channel 1 | not loaded
Channel 2 | not loaded
Channel 3 | not loaded
```

每个宿主对象都有一个默认的bpm，当你在没有指定bpm参数的情况下播放或导出时，它将被使用。 
一个宿主对象的默认bpm值是120。你可以通过修改`bpm`属性来修改宿主对象的默认bpm。
```python
new_song.bpm = 150 # 将new_song的默认bpm改为150
```

## 添加、删除和清除通道

要给宿主对象添加一个新通道，请使用`add_new_channel`方法。
```python
# 这个方法需要一个参数"name"，它是新通道的名称。
# 如果没有指定name参数，它将是"Channel number"，其中number是新通道的编号
new_song.add_new_channel('Strings') 
>>> print(new_song)
[daw] my first song
Channel 1 | not loaded
Channel 2 | not loaded
Channel 3 | not loaded
Channel 4 | not loaded
Strings | not loaded

new_song.add_new_channel()
>>> print(new_song)
[daw] my first song
Channel 1 | not loaded
Channel 2 | not loaded
Channel 3 | not loaded
Strings | not loaded
Channel 5 | not loaded
```

要删除宿主对象的一个通道，请使用`delete_channel`方法，索引是基于0的。
```python
new_song.delete_channel(1) # 删除第一个通道
# 或者你可以使用del关键字，索引也是基于0的
del new_song[0] # 删除第一个频道
```

要清除宿主对象的一个通道，使用`clear_channel`方法，索引是基于0的。 
(清除一个通道意味着重置该通道的名称、音源和其他设置的属性)
```python
new_song.clear_channel(1) # 清除第一个通道
# 第一个通道的名字将被重置为"Channel 1"，音源也将被重置为'not loaded'，
# 如果第一个通道已经加载了音源，则加载的音源将被卸载
```
要清除宿主对象的所有通道，使用`clear_all_channels`方法。 
(注意，这个方法是删除宿主对象的所有通道，所以使用这个方法后，由于宿主对象中没有通道，所以宿主对象的通道数将为0，宿主对象的名称不会被重置)

```python
new_song.clear_all_channels()
>>> print(new_song)
[daw] my first song
```

## 混音和编辑

目前一些基本的混音和编辑功能已经在musicpy daw中实现。这里我们将逐一介绍所有这些功能。 
注意，不同的混音效果可以相互结合。

### musicpy代码上的音频效果的一般用法
在musicpy宿主模块中实现了一个名为`effect`的类，它可以存储一种音频效果，比如反向, 偏移, 淡入, 淡出, ADSR包络，你可以定制自己的音频效果函数。

你可以使用`effect`类来打包一个音频效果函数，并把它作为一个元素放在一个列表中，这个列表可以作为`effects`的值在播放和导出函数中使用。

已经有一些预定义的effect实例，如`reverse`, `fade`, `adsr`。下面我将逐一描述这些预定义的效果。

在此之前，这里是`effect`类的用法，它告诉你如何制作一个effect实例，如何传入参数来制作一个更具体的effect实例，如何将效果用于musicpy数据结构。

### effect类型的定义
```python
effect(func, name=None, *args, unknown_args=None, **kwargs)

# func：处理音频效果的函数，第一个参数必须是一个pydub AudioSegment实例。
# 其他参数可以随意定制，返回值必须是一个pydub AudioSegment实例。

# name: 音频效果的名称

#args, **kwargs：位置参数和关键字参数，用于处理音频效果。
# 处理音频效果的函数可能会收到

# unknow_args: 处理音频效果的函数可能收到的关键字参数。
# 但在打包时不能知道（例如，实时的bpm）。
```
注意，`unknown_args`必须是一个字典，因为它代表关键字参数，例如，`unknown_args`可以是`{'bpm': None}`。

### 制作一个effect实例的例子

假设你已经写了一个名为 `delay` 的函数，它接收一个pydub AudioSegment实例（必须是第一个参数），并通过用户定义的相邻延迟声音的时间间隔和延迟声音的数量添加延迟效果，然后返回修改后的pydub AudioSegment实例。

`delay`函数的参数是 `interval`, `unit`, 默认值是0.5 (s) 和6。

所以现在你如果想制作一个名为`delay_effect`的effect实例，效果名称为`delay`，你可以写。
```python
delay_effect = effect(delay, 'delay')
```
注意，没有为`delay_effect`指定参数，这是可以接受的，因为你可以在后面传递参数（我将在一分钟内讲到），或者音频效果函数可以简单地不接受AudioSegment实例以外的参数，如反向。

### 一个effect实例的表示
```python
>>> delay_effect
[effect]
name: delay
parameters: [(), {}] unknown arguments: {}
```

### 传入参数来设置音频效果函数的参数值

除了在初始化一个effect实例时为音频效果函数设置参数外，你也可以在一个已经定义的effect实例中传入参数，为其采取的音频效果函数设置参数，从而返回一个新的effect实例，你传入的参数可以是位置参数和关键字参数。比如说
```python
delay_effect1 = delay_effect(interval=0.2, unit=10) # 使用关键字参数
# 设置音频效果函数'delay'的参数，间隔=0.2，单位=10。
# delay_effect1是一个新的effect实例，具有相同的音频效果函数和效果名称。
# 除了音频效果函数的参数改变为你传入的参数之外

>>> delay_effect1
[effect]
name: delay
parameters: [(), {'interval': 0.2, 'unit': 10}] unknown arguments: {}

delay_effect2 = delay_effect(0.2, 10) # 或者使用位置参数

>>> delay_effect2
[effect]
name: delay
parameters: [(0.2, 10), {}] unknown arguments: {}
```

### 如何在musicpy数据结构上使用effect实例

你可以使用`set_effect`函数来给常见的musicpy数据结构 (note, chord, track, piece) 添加一个效果列表。 
`set_effect`函数的返回值是被它包裹的相同的musicpy数据结构，但有一个属性`effects`，当它被传递到宿主的播放和导出函数时，将被读取。增加的属性`effects`是一个由`set_effect`函数传入的effect实例的列表。
```python
set_effect(sound, *effects)

# sound：常见的musicpy数据结构

# *effects：你可以尽可能地把effect实例作为位置参数传入。
# 或者传入一个effect实例的列表

# 例子

Cmaj7_chord = C('Cmaj7')

Cmaj7_chord = set_effect(Cmaj7_chord, [reverse, delay_effect(interval=0.2, unit=6)])

# 或者你可以直接使用effect实例作为参数
Cmaj7_chord = set_effect(Cmaj7_chord, reverse, delay_effect(interval=0.2, unit=6))
```

### 介绍effect_chain类型
一个effect实例只能单独持有一个效果，有一个叫做`effect_chain`的类型，它可以收集多个effect实例，可以直接接收musicpy的数据结构来添加它所包含的效果。这有时会使音频效果组合的处理更简单。

通过`effect_chain`类，你可以建立类似DAW中的混音器的东西，并应用一系列的音频效果。

下面是如何建立一个`effect_chain`实例和如何使用它的例子。
```python
mixer1 = effect_chain(delay_effect(interval=0.2, unit=6), fade_in(duration=2000), fade_out(duration=3000))
# 在初始化一个effect_chain实例时，你将effect实例作为位置参数传入。

# 一个效果链实例的表示方法
>>> mixer1
[effect chain]
effects:
[effect]
name: delay
parameters: [(), {'interval': 0.2, 'unit': 6}] unknown arguments: {}

[effect]
name: fade in
parameters: [(), {'duration': 2000}] unknown arguments: {}

[effect]
name: fade out
parameters: [(), {'duration': 3000}] unknown arguments: {}

# 如何在musicpy数据结构上使用一个effect_chain实例

Cmaj7_chord = C('Cmaj7')

Cmaj7_chord = mixer1(Cmaj7_chord)
# 你可以简单地在musicpy数据结构上应用effect_chain实例，把它包装起来。
# 返回值是相同的musicpy数据结构，并存储了效果
```

### 音频效果如何在musicpy宿主模块中被处理
当你使用宿主的播放和导出函数时，如果传入的musicpy数据结构有一个属性`effects`，并且是一个非空的列表，那么musicpy数据结构的渲染音频将被effect实例的音频效果函数逐一处理。

现在我们来谈谈预定义的effect实例。

### 反向
你可以通过使用`reverse`为常见的musicpy数据结构添加一个反向效果。
```python
a = C('Cmaj9') % (1, 1/8)
new_song.export(set_effect(a, reverse), 1) #导出一个带有反向效果的Cmaj9和弦

current_song = P([A1, set_effect(B1, reverse), C1, D1], daw_channels=[0, 1, 2, 3])
new_song.export(current_song)
#导出当前歌曲类型，其中B1轨道具有反向效果

current_song2 = P([A1, B1, C1, D1], daw_channels=[0, 1, 2, 3])
new_song.export(set_effect(current_song2, reverse))
#导出current_song2这个乐曲类型，整个乐曲类型有一个反向效果
```

### 偏移
你可以通过使用`offset`为常见的musicpy数据结构添加一个偏移。偏移的单位是小节（在4/4拍子中）。给音频添加偏移意味着音频将从偏移的位置开始播放。换句话说，偏移位置之前的音频将被切断。偏移效果需要一个未知的参数`bpm`，如果你事先知道bpm，可以通过通常的关键字参数或初始化时的`unknown_args`来设置，或者你可以不设置，因为在宿主的播放和导出功能中，当前bpm将默认传递给偏移实例。
```python
a = C('Cmaj9') % (1, 1/8)
new_song.export(set_effect(a, offset(1/8)), 1)
# 输出一个Cmaj9和弦，偏移量为1/8小节，这意味着音频将在1/8小节的地方开始播放

current_song = P([A1, B1, C1, D1], daw_channels=[0, 1, 2, 3])
new_song.export(set_effect(current_song, offset(1)) #导出的乐曲类型current_song，偏移量为1 bar
```

### 淡入/淡出效果
你可以通过使用`fade_in`和`fade_out`为常见的musicpy数据结构添加淡入/淡出效果。这两个函数都接受一个参数`duration`，用来设置音频淡入/淡出的时间，持续时间的单位是毫秒，注意淡入/淡出意味着音频的音量从0%到100%/从100%到0%逐渐变化。还有一种`fade`效果，你可以同时设置淡入时间和淡出时间。
```python
a = C('Cmaj9') % (1, 1/8)
new_song.export(set_effect(a, fade_in(500)), 1)
# 输出一个Cmaj9和弦，淡入效果为0.5s，这意味着音频将从开始淡入0.5s

new_song.export(set_effect(a, fade_out(500)), 1)
#导出一个Cmaj9和弦，淡出效果为0.5s，这意味着音频将淡出到最后0.5s

new_song.export(set_effect(a, fade(500, 2000)), 1)
# 输出一个Cmaj9和弦，淡入效果为0.5s，淡出效果为2s。
# 这意味着音频将从开头淡入0.5s，淡出2s，直到结束
```

### 添加ADSR包络
你可以通过使用`adsr`将ADSR包络添加到常见的musicpy数据结构中。注意，目前添加到musicpy代码中的ADSR包络只控制音频的音量，这是ADSR包络工作的最经典方式。`adsr`函数需要4个响应ADSR的参数：`attack`, `decay`, `sustain`, `release`。`attack`指定音频从开始淡入的时间，`decay`指定音频减少音量的时间，`sustain`指定音频减少到的sustain水平，`release`指定音频淡出到结束的时间。ADSR包络是一个很好的工具，不仅适用于musicpy的数据结构，也适用于musicpy宿主生成的基本波形，因为当ADSR包络与基本波形（正弦波, 三角波等）结合时，在音色方面有很多可能性，attack, decay和release的时间单位都是毫秒，持续的音量单位是百分比，其中0%是静音，100%是最大声（可以是整数或浮点数）。
```python
a = C('Cmaj9') % (1, 1/8)
new_song.export(set_effect(a, adsr(500, 1000, 20, 2000)), 1)
#导出一个带有ADSR包络的Cmaj9和弦，其中attack=0.5s，decay=1s，sustain=20%，release=2s
```

### 制作你自己的音频效果
音频效果函数的effect实例的第一个参数必须是一个pydub AudioSegment实例，你可以从中读取原始音频数据，然后使用numpy, scipy等python包编写算法来进行音频信号处理，以达到理想的混音效果。effect实例的音频效果函数的返回值也必须是一个pydub AudioSegment实例。

### 待办事项
计划在以后加入EQ, reverb, compression, delay等一些预先定义好的effect实例。

### 左右声道混音位置和通道的音量
一个乐曲类型（或音轨类型）的通道的左右声道混音位置和音量也适用于musicpy宿主, 它们的作用与输出的MIDI文件相同。



### 从musicpy数据结构中获取宿主生成的音频
你可以使用`audio`函数从宿主对象中获得一个AudioSegment实例。AudioSegment实例本身可以被导出并做许多其他的音频编辑操作，关于更多的细节，你可以参考pydub的API文档。AudioSegment实例也可以放入musicpy的数据结构中，如和弦类型和乐曲类型，并传递给宿主来播放或导出。
```python
audio(obj, daw, channel_num=0, bpm=None)

# obj：要处理的musicpy数据结构，可以是音符/和弦/乐曲/音轨

# daw：你希望obj传递的宿主对象

# channel_num：你想在宿主对象中使用的通道索引，从0开始

# bpm: 用于生成音频的BPM(速度)，如果没有指定，则使用宿主的默认bpm

audio(C('C'), new_song) # 用new_song宿主对象的第一个通道生成一个C大三和弦的AudioSegment对象
```

### 将一个音频对象的列表转换成和弦类型
如果你有一个AudioSegment对象的列表，想把它转换成和弦类型，你可以使用`audio_chord`函数。
```python
audio_chord(audio_list, interval=0, duration=1/4, volume=127)

# audio_list: 要转换为和弦类型的AudioSegment对象的列表

# interval: 和弦类型的间隔，你可以参考和弦类型构造函数中的间隔参数

# duration：和弦类型的持续时间，你可以参考和弦类型构造函数中的持续时间参数

# volume: 和弦类型的音量，你可以参考和弦类型构造函数中的音量参数

audio_chord([audio(C('Cmaj7') + i, new_song) for i in range(10)], 1/8, 1, 50)
# 生成一个和弦类型的AudioSegment对象, interval = 1/8, duration = 1, volume = 50
```

### 将音频文件从路径加载到音频对象中
将音源加载到宿主对象中，可能方便对整个musicpy数据结构进行播放或导出操作。
但有时我们需要对更多的单个音频文件逐一进行操作。你可以使用`sound`类从文件路径加载一个音频文件 
并可以在和弦类型或乐曲类型中使用它的`sounds`属性（它是一个AudioSegment对象）作为一个音频剪辑。一个AudioSegment对象可以被用作和弦类型中的一个音符，由于乐曲类型是建立在和弦类型之上的，AudioSegment对象也可以在乐曲类型中使用。

```python
sound(path, format=None)

# path: 你想加载的音频文件的文件路径

# format: 你想加载的音频文件的格式，如果没有指定，则从文件名扩展名中自动检测音频文件的格式

sound_effect_1 = sound('sound effect.wav') # 把音频文件'sound effect.wav'作为一个声音对象加载
sound_effect_1.play() # 你可以使用声音对象的函数play来播放音频
sound_effect_1.stop() # 停止播放音频

>>> len(sound_effect_1) # 你可以使用 len(sound) 来获得声音对象中音频片段的长度，单位为毫秒
2000

curren_audio = audio_chord([sound_effect_1.sounds, other_audio_objects])
# 用声音对象的sounds属性作为音频片段
```

### 全局性地播放和停止音频片段
你可以使用`play_audio`函数来全局播放AudioSegment对象和声音对象(和音调对象，这将在后面讲到），你可以使用`stop`函数来停止当前播放的所有声音。
```python
play_audio(sound_effect_1) # 播放声音对象sound_effect_1的音频片段
stop() # 停止当前播放的所有声音
```

## 生成和播放波形

Musicpy宿主可以生成一些合成器上出现的基本波形，包括正弦波、锯齿波、方波、三角波和白噪音。这里我们将通过musicpy宿主模块中可以生成基本波形的功能。 
### 正弦波
要生成正弦波，你可以使用`sine`函数，这是一个全局函数（它不是宿主类的方法，而是全局使用的）。
```python
sine(freq=440, duration=1000, volume=0)

# freq: 正弦波的频率

# duration: 正弦波的持续时间，时间单位是毫秒

# volume: 正弦波的音量大小，单位是百分比，从0%到100%，可以是整数或小数

sine_A4 = sine(440, 2000, 20) # 产生一个频率为440 hz（A4）、持续时间为2s、音量为20%的正弦波音频段
```

### 三角波、锯齿波、方波
要生成这些基本波形，参数与正弦波相同，你只需要把函数的名称从`sine`改为`triangle` / `sawtooth` / `square`。

### 白噪音
为了产生白噪音，你可以使用`white_noise`函数
```python
white_noise(duration=1000, volume=0)

# duration: 白噪音的持续时间，时间单位是毫秒

# volume: 白噪音的音量大小，单位是百分比，从0%到100%，可以是整数或小数

white_noise(2000, 20) # 产生一个持续时间为2s、音量为20%的白噪音
```

### 生成一个和弦类型的基本波形
好了，现在你可能会想：是的，我们可以生成这些基本波形，这有点酷，但我们如何在musicpy宿主中使用它们?

我们怎样才能得到这些基本波形的和弦类型甚至是乐曲类型? 这里有答案。

要生成和弦类型的基本波形，你可以使用`get_wave`函数（这是一个全局函数），这是一个从musicpy的数据结构中生成基本波形的方便的函数。
```python
get_wave(sound, mode='sine', bpm=120, volume=None)

# sound：要处理的musicpy数据结构，必须是和弦类型的

# mode：你想生成的基本波形的类型，必须是 "square", "triangle", "sawtooth", "square"中的一个,
# 或者一个接收3个参数的函数, 参数分别是frequency, duration, volume (必须要是这个顺序) 并且返回一个AudioSegment对象,
# 你可以参考daw.py里的`sine`, `triangle`, `sawtooth`, `square`, `pulse`函数

# bpm：基本波形的BPM（速度），这将决定生成的基本波形的绝对持续时间

# volume：基本波形的音量大小，可以是一个整数或小数，也可以是一个音量大小列表，单位是百分比
# 如果设置为 None，则使用 musicpy 数据结构的音量大小

get_wave(C('C') % (1, 1/8), 'sine', volume=20) # 产生一个C大三和弦的正弦波的和弦类型，音量为20%
get_wave(C('C') % (1, 1/8), triangle, volume=20) # 产生一个C大三和弦的三角波的和弦类型，音量为20%
```
生成的基本波形的和弦类型可以作为常规的musicpy数据结构用于宿主对象的播放/导出功能中作为常规的musicpy数据结构使用，并且可以放在一个乐曲类型（或一个音轨类型）中，由宿主对象播放/导出。

### 生成更加一般形式的波形
你可以使用`pulse`函数来生成更加一般形式的波形，你可以指定波形的占空比 (duty cycle)。  

实际上这仍然只限于脉冲波（包括方波），但你可以从这个函数中获得更多的新音色。

```python
pulse(freq=440, duty_cycle=0.5, duration=1000, volume=0)

# freq: 波形的频率

# duty_cycle：脉冲波的占空比，注意方波的占空比是50% (duty_cycle = 0.5)

new_waveform = pulse(440, 0.2, 2000, 20)
# 产生一个占空比为20%的脉冲波，频率为440赫兹（A4），持续时间为2秒，音量为20%
```

如果你想从数学和统计函数中生成更加一般形式的波形，你可以从pydub导入SignalGenerator类。
```python
from pydub.generators import SignalGenerator
```
写一个新的类，继承SignalGenerator（设置SignalGenerator为父类），并重载`__init__`和`generate`方法，其中`__init__`是构造函数，`generate`是每次调用时产生采样数据作为迭代器的函数。你可以参考[这里](https://github.com/jiaaro/pydub/blob/master/pydub/generators.py)，看看如何重载SignalGenerator的`generate`函数，使用数学和统计函数来生成更加一般形式的波形。


## 变调器（只用一个录音音频文件就可以制作一个完整的音源)

在musicpy宿主模块中写有一个音高变换器，用于改变音频片段的音高。

**请注意: 使用这部分的功能需要安装一些额外的python库, 你可以在cmd/terminal里运行`pip install librosa soundfile`来进行安装。**

你可以通过半音或更多的微调单位来改变音频片段的音高或降低。

你可以使用`pitch`类从文件路径加载音频文件，你可以使用pitch对象做一些有趣的事情。

你可以使用pitch对象的`pitch_shift`函数来改变音频片段的音调，单位是半音，可以是 
单位是半音，可以是整数或小数，有两种方法来改变音高，'pydub'或'librosa'方法，你可以选择其中之一。
我将在后面解释这两种方法的区别。音调变化的结果是一个AudioSegment对象。

简而言之，你可以在一个半音值的pitch对象后面使用`+`和`-`来执行音高变化。
比如`pitch + 2', `pitch + 1.5', `pitch - 1', 注意当你使用这种语法时将会使用'librosa'方法。

你可以使用pitch对象的`get'函数，从提供的音高名称中获得一个AudioSegment对象。

你可以使用音高对象的`set_note`函数来重置音高对象的初始音。

你可以使用音高对象的`play`和`stop`函数来播放和停止音高对象中加载的音频片段。

要使用音高对象中加载的音频文件生成整个音源，你可以使用音高对象的`export_sound_files'函数 
音高对象的`export_sound_files`函数，音高对象的`generate_dict`函数将生成一个包含音高名称和它们的 
相应的音频片段。
```python
pitch(path, note='C5', format=None)

# path: 你想加载的音频文件的文件路径

# note: 音频文件的初始音高，可以是一个音高字符串或一个音符类型

# format：音频文件的格式

pitch_1 = pitch('voice.wav', note='C5') # 加载一个人的声音的音频文件，初始音高为 C5
new_pitch_1 = pitch_1.pitch_shift(1) # 将pitch_1的音调上移1个半音，并得到一个AudioSegment对象
# 默认的pitch shift方法是 'librosa'，你可以用模式参数指定pitch shift方法为'pydub'
new_pitch_1 = pitch_1.pitch_shift(1, mode='pydub') # 使用'pydub'方法，将 pitch_1 的音高上移 1 个半音
```
'pydub'和 'librosa'方法的区别是，'pydub'方法是通过改变音频片段的采样率来改变音高。
pydub'方法通过改变音频片段的采样率（或者你可以说是帧率）来改变音高，但这导致在改变音高的同时，音频片段的速度也会改变。
但这导致了在进行音高变化时音频片段的速度变化。

在'pydub'方法中，音调越高，音频片段的速度就越快。
相反，如果音调变低，音频片段就会变慢，换句话说，音频片段的长度就会改变。
变化，这在很多情况下不是我们想要的，尽管'pydub'方法比'librosa'方法要快得多。

在'librosa'方法中，它对音频片段的原始音频数据进行FFT处理，并得到一个包含FFT结果的numpy数组。
FFT的结果，并使用这个数组进行转换，以实现音高变化。
这种方法比较慢，因为它要进行一些数学和统计计算，但是音频片段的速度不会改变。
但音频片段的速度不会改变，音频片段的长度在音高改变之前将保持不变。

注意，与 'pydub'方法相比，'librosa'方法可能会导致音高变化后的音频质量降低。
特别是当对长的音频片段（例如几分钟）进行音调改变时，如果它是一个不超过几秒钟的音频片段，这就不那么明显了。

pitch对象的pitch_shift函数的默认方法是'librosa'方法。
```python
pitch_1.get('D5') # 得到 pitch_1 的 pitch D5 的 pitchSegment 对象(初始音高为 C5)

pitch_1.set_note('C6') # 重置 pitch_1 的初始音高

pitch_1.pitch_shift(1.5) # 音调上移1.5个半音
pitch_1 + 1.5 # pitch shift up by 1.5 semitones
pitch_1.pitch_shift(-0.5) # pitch shift down by 0.5 semitones
pitch_1 - 0.5 # 音调下移 0.5 个半音

pitch_1_dict = pitch_1.generate_dict(start='A0', end='C8')
# 产生一个音高名称和音高偏移的 AudioSegment 对象的字典

pitch_1.export_sound_files(path='. ', folder_name="someone's voice", start='A0', end='C8', format='wav')
#导出一个从A0到C8的pitch_1范围的整个音源，命名为"someone's voice"到当前工作目录。
# 这个音源的音频文件格式是 wav，因为 pitch_1 的初始音高是 C5。
# 你将会有一个由 pitch_1 中的音频片段组成的、经过音高变换的文件夹，名为 "someone's voice"。
# 在你指定的路径中，这个文件夹可以作为一个音源加载到宿主对象中。

# 注意，对于 generate_dict 和 export_sound_files 方法，你可以用模式参数指定进行音高改变的方法。

>>> len(pitch_1) # 你可以用 len(pitch) 来获得pitch对象中音频片段的长度，单位是毫秒
2000

# pitch对象的播放和停止功能与声音对象类似
pitch_1.play()
pitch_1.stop()
```

## 更多关于MDI音源格式

MDI是我自己发明的一种音源格式，把一个文件夹的音频文件（也许还有一个设置文件）打包成一个单一的二进制文件，更便于操作和储存。MDI的名字是Musicpy Daw Instrument的缩写。
Musicpy daw是我的其他项目之一，它是一个完整的音乐宿主和tracker（或者你可以称它为一个简单的DAW），与musicpy完全兼容。
你可以使用musicpy daw加载音源，用musicpy制作音乐，用各种音频文件格式导出音频文件。
Musicpy daw有GUI，所以你可以用musicpy和一些GUI的帮助来制作音乐。这个项目在github上（点击[这里](https://github.com/Rainbow-Dreamer/musicpy-daw)），如果你喜欢它，你可以加个星或fork。

事实上，musicpy宿主模块是musicpy中musicpy daw的移植。我首先完成了musicpy daw的开发，然后将musicpy daw的功能移植到musicpy中，
这就形成了musicpy中的一个新模块，叫做musicpy daw。所以musicpy daw模块本质上是musicpy daw的非GUI版本。
我几乎把musicpy daw的所有功能都移植到musicpy daw模块中,所以musicpy daw和musicpy daw模块有着几乎相同的功能。
正如MDI的名字一样，这个音源的格式来自于musicpy daw。
你可以在musicpy daw中制作、加载和解压MDI文件。

你可以使用`make_mdi`函数从一个音频文件的文件夹中制作MDI音源。每次你从一个音频文件文件夹中制作MDI文件时，你将会在同一文件路径上生成一个MDI文件。MDI文件自带用来解压音源文件的信息，因此它可以独立使用。

除了音频文件，你还可以在制作MDI文件的文件夹中添加一个设置文件，这是一个文本文件（.txt），包含一些信息，用来设置音源的通道字典的映射。 
格式很简单，你在每一行写上 `音高,值`来表示一个映射变化，注意空白处会被计入音高和值中，所以建议不要在每行中添加任何空格。

我将举例说明设置文件中信息的格式。 
如果我们想改变鼓声模块的某个通道的一些映射，例如:
```python
new_song.channel_dict[2]['C2'] = 'Kick'
new_song.channel_dict[2]['E2'] = 'Snare'
new_song.channel_dict[2]['F#2'] = 'CH1'
```
然后我们可以在鼓声模块文件夹的设置文件中这样写:
```
C2,Kick
E2,Snare
F#2,CH1
```
然后使用
```python
new_song.load(2, 'drum.mdi')
```
来加载鼓的音源 (如果鼓的MDI文件的名称是drum.mdi)

你可以使用`unzip_mdi`函数将MDI文件解压成一个音频文件的文件夹（如果有的话还有一个设置文件），以便进一步使用。 

```python
unzip_mdi(file_path, folder_name=None)

# file_path: MDI文件的文件路径

# folder_name: 你解压到的文件夹的名称

unzip_mdi('drum.mdi', 'drum from mdi')
# 将drum.mdi中的音频文件解压到一个名为"drum from mdi"的文件夹中。
```


## 一些其他的提醒和想法

1. 请注意，速度变化(tempo changes)和音高弯曲(pitch bends)对musicpy宿主不起作用，但也有一些方法可以解决。 
   对于速度变化，使用`normalize_tempo`函数，你可以将和弦类型或乐曲类型的速度变化正常化，因为速度变化会直接出现在音符的长度和间隔上，和之前的完全相同但没有速度变化。  
   对于音高弯曲，它对SoundFont文件有效，但对音频取样无效。它对musicpy宿主中的音频取样不起作用的原因是，目前音频取样（音频文件）的音高主要不能被修改，所以musicpy宿主在使用音频取样时忽略了音高弯曲。但我写了一些代码，使其能够作用于加载的SounFont文件，所以如果你使用SoundFont文件作为声音模块在通道上使用SoundFont文件，那么它将完美地工作。  
   如果你想在导出的音频文件中使用音频取样时有音高弯曲，你可以在声音模块中设置音频取样本身的音高弯曲，并在通道的映射中设置一个特殊的音高与音高弯曲的音频取样。

2. 如果你在播放时或在导出的音频文件中听到一些音符开始或结束时的噼啪声，你可以在和弦类型或乐曲类型中的每个音符上添加一些微小的淡化效果来消除噼啪声。  
   通常20毫秒就可以完全消除噼啪声，而且非常有效，因为淡入和淡出的时间很短，你不会注意到，而噼啪的声音完全被删除了。你可以使用列表解析式或者for循环，以便为和弦类型或乐曲类型中的每个音符添加微小的淡出效果。比如说
   ```python
   # 为和弦类型中的所有音符添加微小的淡化效果
   piano.notes = [set_effect(i, fade(20, 20)) for i in piano.notes]
   
   # 为所有乐曲类型中的所有音符添加小的渐变效果
   for each in current_song.tracks: each.notes = [set_effect(i, fade(20, 20)) for i in each.note]
   ```
