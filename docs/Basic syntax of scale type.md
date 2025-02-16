# 音阶类型的基本语法



## 目录

- [按照音程关系或者调式名构建音阶](#按照音程关系或者调式名构建音阶)
- [转调](#转调)
- [按照音阶的级数选取对应的和弦](#按照音阶的级数选取对应的和弦)
- [通过简谱式语法从音阶度数中创造旋律](#通过简谱式语法从音阶度数中创造旋律)
- [按照五度圈将一个调式进行转调](#按照五度圈将一个调式进行转调)
- [按照和声功能选取一个音阶的和弦](#按照和声功能选取一个音阶的和弦)
- [得到一个调式的关系调和平行调](#得到一个调式的关系调和平行调)
- [对一个音阶进行升降调（整体升降或者个别的音的升降）](#"对一个音阶进行升降调（整体升降或者个别的音的升降）")
- [获得一个音阶的调式](#获得一个音阶的调式)
- [获得一个音阶的倒序](#获得一个音阶的倒序)
- [对一个音阶（调式）名进行解析](#"对一个音阶（调式）名进行解析")
- [按照和弦级数提取和弦进行](#按照和弦级数提取和弦进行)
- [得到一个音阶的所有音按照标准音名标记法的列表](#得到一个音阶的所有音按照标准音名标记法的列表)
- [得到一段音乐的关于一个调式的负面和声](#得到一段音乐的关于一个调式的负面和声)
- [得到一段音乐的逆行(retrograde)与反行(inversion)](#得到一段音乐的逆行retrograde与反行inversion)
- [检测一个音符或者和弦是否在一个音阶内存在](#检测一个音符或者和弦是否在一个音阶内存在)
- [音阶类型按照使用罗马数字表示的和弦级数提取和弦](#音阶类型按照使用罗马数字表示的和弦级数提取和弦)
- [使用音阶类型产生和弦进行](#使用音阶类型产生和弦进行)
- [按照音阶的音级提取音符](#按照音阶的音级提取音符)
- [查找一个音符在音阶里的音级](#查找一个音符在音阶里的音级)
- [查找一个音符在音阶里的标准音名](#查找一个音符在音阶里的标准音名)
- [获得一个音符在音阶中的距离某个音程的音符](#获得一个音符在音阶中的距离某个音程的音符)



## 按照音程关系或者调式名构建音阶

音阶类型可以表示一个特定的音阶。

使用这个类可以快速按照音的间隔来构建调式，比如大调的音的排列是全全半全全全半（全代表全音，半代表半音），

那么如果想构建一个C大调音阶，就可以写

```python
scale('C5', interval=[2,2,1,2,2,2,1])
```

这样就得到了以C5为根音的C大调音阶。

当然，对于大部分知名的调式来说，只需要输入调式的名称就行了。比如

```python
scale('C5', 'major')
```

就可以得到以C5为根音的C大调音阶，

```python
scale('C5', 'minor')
```

得到以C5为根音的C小调音阶。

在`database.py`里面的`scaleTypes`是所有musicpy自带的调式，用户也可以自己定制调式。



## 转调

你可以使用音阶类型的`modulation`方法，将一个和弦类型从一个音阶转换成另一个音阶。例如，如果你有一个A大调的和弦类型，你想把它转换成A小调，你可以写：

```python
current_chord = chord('A,B,C#,D,E') % (1/8, 1/8)

>>> current_chord
chord(notes=[A4, B4, C#5, D5, E5], interval=[1/8, 1/8, 1/8, 1/8, 1/8], start_time=0)

new_chord = current_chord.modulation(scale('A', 'major'), scale('A', 'minor'))

>>> new_chord
chord(notes=[A4, B4, C5, D5, E5], interval=[1/8, 1/8, 1/8, 1/8, 1/8], start_time=0)
```



## 按照音阶的级数选取对应的和弦

比如C大调的4级三和弦是F，6级三和弦是Am，这时候可以用`pick_chord_by_degree`函数，其中的参数`degree1`是和弦级数，从0开始，`num`是选取多少个音，`step`是每一次跨越几个音阶中的音。按照默认值，

```python
scale('C', 'major').pick_chord_by_degree(4)
```

会得到C大调音阶的5级三和弦G大三和弦。

按照音阶的级数选取对应的和弦的函数`pick_chord_by_degree`可以简写成在一个音阶类型后面加上小括号，里面写参数。

进阶写法：

```python
S('C major')(4)
```

`pick_chord_by_degree`还有很多其他的设定参数，可以用来提取自然三和弦，自然七和弦，自然九和弦等等，还可以设定每次按照三度堆叠还是其他度的堆叠，以及设定返回的和弦类型的音符长度和音符间隔等等。

`pick_chord_by_degree`的参数按照顺序为：

```python
degree1: 要提取的和弦的级数，是一个正整数
duration: 返回的和弦类型的音符长度，默认值为1/4
interval: 返回的和弦类型的音符间隔，默认值为0
num: 提取的和弦的音的个数，默认值为3，也就是提取自然三和弦，如果为4，就是提取自然七和弦，以此类推。
step: 提取的和弦每次往下找音阶内的下一个和弦音的步数，默认值为2，也就是按照三度堆叠。
```

因为`pick_chord_by_degree`函数可以简写成一对小括号，因此大家可以在一个音阶类型后直接加上一对小括号，然后在里面写`pick_chord_by_degree`函数的参数，这样写更加简洁，比如

```python
# 构建一个C大调音阶，叫做c_major_scale
c_major_scale = S('C major')
# 提取C大调音阶的3级自然七和弦
a = c_major_scale(2, num=4)
>>> print(a)
# 把和弦显示出来，C大调音阶的3级自然七和弦是Em7和弦
chord(notes=[E4, G4, B4, D5], interval=[0, 0, 0, 0], start_time=0)
```



## 通过简谱式语法从音阶度数中创造旋律

你可以使用音阶类型的`get`方法来获得一个根据音阶度数产生的旋律。这使编写旋律更容易，因为你使用的是相对于音阶的数字，而不是绝对的音高，使用这种方法时，你只需要考虑音阶度数。

其语法与简谱类似。

1. 用1到7作为音阶度数，从当前的音阶中得到一个音（从1开始）。
2. 在数字后面加上 `#` 和 `b` 来提高或降低一个半音，如 `1#`。
3. 在数字后面加上`.`和整数n，以提高或降低n个八度，如`1.1`，`2.1`。
4. 使用`o`和一个数字来设置当前的八度数，如`o5`。
5. 使用`;`将一组音符分组并同时演奏，如`1;3;5`。
6. 也支持像和弦类型的设置区块，所以你可以单独设置每个音符的长度和间隔。
7. 使用`r`作为休止符，`-`作为延续符号。
8. 只接受一个字符串，你需要把所有的音符写在一个字符串中，用`,`来分隔音符。

```python
get(current_ind,
    default_duration=1 / 8,
    default_interval=1 / 8,
    default_volume=100,
    pitch_mode=0)

# current_ind: 一个字符串，包含你想得到的所有音符

# default_duration: 每个音符的默认持续时间

# default_interval: 每个音符的默认间隔时间

# default_volume: 每个音符的默认音量

# pitch_mode：如果设置为0，当选择1到7级音时，八度数是固定的，所以在某些音阶中，高音级的音高比低音级的音高低；如果设置为1，只有1级音的八度数是固定的，高音级的音高将由音阶里的音程来计算
```

下面是一个例子，说明如何使用音阶类型的`get`方法。

```python
>>> S('C major').get('1,1,5,5,6,6,5,-,4,4,3,3,2,2,1,-' )
chord(notes=[C4, C4, G4, G4, A4, A4, G4, F4, F4, E4, ...], interval=[1/8, 1/8, 1/8, 1/8, 1/4, 1/8, 1/8, ...], start_time=0)

>> S('A#5 major').get('1,7,5,3,2,3[.16;.], o4, 1[.8.;.],7,1,-, o3, 6;3.1;1.1, 6;3.1;1.1, 5;7.1;2.1, 5; 7.1; 2.1, 6;3.1;1.1,-')
chord(notes=[A#5, A5, F5, D5, C5, D5, A#4, A4, A#4, G3, ...], interval=[1/8, 1/8, 1/8, 1/8, 1/16, 3/16, 1/8, 1/4, 0, ...], start_time=0)
```



## 按照五度圈将一个调式进行转调

`fifth`函数可以按照五度圈将当前的调式进行转调，其中的参数step是顺着五度圈移动多少步，如果step大于0，那么就会顺时针转调，如果step小于0则是逆时针转调。`fourth`函数同理，只是换成了四度圈。



## 按照和声功能选取一个音阶的和弦

音阶类有内置一组较为完整的调式和弦选取函数。比如现在有一个调式A。那么调式A的主和弦就是

```python
A.tonic_chord()
```

下属和弦就是

```python
A.subdom_chord()
```

属和弦是 

```python
A.dom_chord()
```

等等。想得到调式内某一级的副属和弦可以用

```python
A.secondary_dom(degree)
```

其中degree是级数。



## 得到一个调式的关系调和平行调

`relative_key`函数可以得到关系调，比如大调的关系小调或者小调的关系大调。

`parallel_key`函数可以得到同主音大小调。



## 对一个音阶进行升降调（整体升降或者个别的音的升降）

音符类型，和弦类型的`up`和`down`函数以及其进阶语法也同样适用于音阶。



## 获得一个音阶的调式

使用 `a.inversion(n)`或者`a / n`来获得音阶类型`a`的第n个调式。

```python
current_scale = S('C major')
>>> current_scale.inversion(2)
[scale]
scale name: D4 dorian scale
scale intervals: [M2, m2, M2, M2, M2, m2, M2]
scale notes: [D4, E4, F4, G4, A4, B4, C5, D5]

>>> current_scale / 3
[scale]
scale name: E4 phrygian scale
scale intervals: [m2, M2, M2, M2, m2, M2, M2]
scale notes: [E4, F4, G4, A4, B4, C5, D5, E5]
```



## 获得一个音阶的倒序

使用`a.reverse()`或者`~a`来获得音阶类型`a`的倒序组成的音阶。

```python
current_scale = S('C major')
>>> ~current_scale
[scale]
scale name: C4 phrygian scale
scale intervals: [m2, M2, M2, M2, m2, M2, M2]
scale notes: [C4, Db4, Eb4, F4, G4, Ab4, Bb4, C5]
```



## 对一个音阶（调式）名进行解析

`to_scale`函数可以直接输入一个音阶（调式）名进行解析，返回的是输入的音阶（调式）名对应的音阶类型，主音的八度数（也是音阶所在的八度数）

由第二个参数pitch决定，默认值为4。比如

```python
to_scale('C major')
```

可以得到以C5为主音的大调音阶，

```python
to_scale('G lydian', 6)
```

可以得到以G6为主音的lydian音阶。

简写为

```python
S('C major')
```

S表示的是音阶scale的首字母大写



## 按照和弦级数提取和弦进行

使用音阶类型的内置函数`pattern`，可以输入一串表示和弦级数的字符串或者整数来提取一个调式的和弦进行，比如

```python
S('C major').pattern(6451)
```

可以得到C大调音阶的6,4,5,1进行的和弦的列表，

也可以写

```python
S('C major').pattern('6451')
```

`pattern`函数的其他参数(按照先后顺序)：

* duration: 和弦进行里每个和弦的音符的长度，默认值为0.25，也就是音符长度为1个小节。

* interval: 和弦进行里每个和弦的音符之间的间隔，默认值为0，也就是每个和弦的音符都是一起弹。

* num: 为按照级数提取调式内的和弦时，每一个和弦提取几个音，比如num为3就是提取三和弦, num为4就是提取七和弦等等。

* step: 提取和弦时每一步跳过多少个音阶内的音，默认值为2，比如C大调音阶的1级自然三和弦C E G，每一次都是跳过两个音阶内的音，C，跳两个音到E, 再跳两个音到G。（在这里跳过几个音就是往前走几步的意思，每走一步就是走到音阶内的下一个音）

进阶写法：

```python
S('C major') % 6451
S('C major') % (6451, 1/2, 0)
```



## 得到一个音阶的所有音按照标准音名标记法的列表

在我们表示一个音阶的时候，最常见的比如大调和小调音阶，我们通常会更喜欢用标准音名标记法来表示音阶里的每一个音符，  标准音名标记法就是一个七音的音阶里，C D E F G A B这7个字母一定都要出现一次，前面可以搭配升号，降号，重升号，重降号，还原号等等，  只要是等价于音阶里对应的音就行了，即使并不是像`A#`, `Eb`这样很自然的有升降号的音也没关系。举个例子，比如Eb大调音阶的音按照标准音名标记法来表示就是

```
Eb, F, G, Ab, Bb, C, D
```

G#大调音阶的音按照标准音名标记法来表示就是

```
G#, A#, B#, C#, D#, E#, Fx
```

在musicpy里面，如果只是普通地用scale函数或者S函数构建音阶，那么这个音阶的音名就都是默认按照database.py里面的标准12音的音名标记法来表示，第一个音会按照你输入的音来表示，比如你输入`S('D# major')`第一个音就是`D#`，你输入`S('Eb major')`第一个音就是`Eb`，然后之后的音全部都是只有升号的音，比如`S('D# major')`的音是`scale notes: [D#4, F4, G4, G#4, A#4, C5, D5, D#5]`,`S('Gb major')`的音是`scale notes: [Gb4, G#4, A#4, B4, C#5, D#5, F5, F#5]`，这样的表示是为了尽量减少在处理音符类型上的运算量，尽量让音符名称有且仅有一种，让运算逻辑更加简洁。不过如果你想要得到音阶的按照标准音名标记法的音，可以使用音阶类型的内置函数`standard`，比如

```python
>>> S('Gb major').standard()
['Gb', 'Ab', 'Bb', 'Cb', 'Db', 'Eb', 'F']
>>> S('D# major').standard()
['D#', 'E#', 'Fx', 'G#', 'A#', 'B#', 'Cx']
>>> S('Eb major').standard()
['Eb', 'F', 'G', 'Ab', 'Bb', 'C', 'D']
>>> S('C# minor').standard()
['C#', 'D#', 'E', 'F#', 'G#', 'A', 'B']
>>> S('Ab minor').standard()
['Ab', 'Bb', 'Cb', 'Db', 'Eb', 'Fb', 'Gb']
>>> S('C minor').standard()
['C', 'D', 'Eb', 'F', 'G', 'Ab', 'Bb']
```

还有`relative_note`函数，接收两个参数，都是表示音名的字符串，可以返回后面的音名用标准音名标记法表示前面的音名。比如

```python
>>> relative_note('C', 'D')
Dbb
>>> relative_note('A', 'A#')
A♮
>>> relative_note('F', 'E')
E#
>>> relative_note('B', 'C')
Cb
>>> relative_note('G', 'F')
Fx
```



## 得到一段音乐的关于一个调式的负面和声

使用`negative_harmony`函数可以将一个和弦类型转换为指定调式的负面和声，比如现在想把和弦类型a转换为C大调的负面和声，语法为

```python
negtaive_harmony(S('C major'), a)
```

得到的是和弦类型a按照C大调音阶的负面和声转换后的新的和弦类型。
第1个参数是音阶类型，第2个参数是想要进行转换的和弦类型，第3个参数是转换后是否按照音从低到高的顺序重新排列（比如只是转换一个和弦的话，重新排列音高很重要），默认值为False，第4个参数是转换之后是否返回指定的调式的负面和声的映射字典，为True的时候，会直接返回映射字典，不会对和弦类型进行转换，默认值为False。

此外，第2个参数可以不写，默认值为None，如果在第2个参数不写并且第4个参数为False的时候，会返回指定的调式的负面和声组成的音阶类型，也就是镜像音阶。

另一种写法是

```python
a.negative_harmony(S('C major'))
```

简写方法是

```python
a @ S('C major')
# 如果有其他参数，可以用元组
a @ (S('C major'), True)
```



## 得到一段音乐的逆行(retrograde)与反行(inversion)

十二音技法里有两个很重要的概念，逆行与反行，具体的定义我在这里不多做描述，大家可以去网上查，这里主要讲怎么写。  

```python
# 和弦类型a的逆行
a.retrograde()
# 和弦类型a的反行
a.pitch_inversion()
# 两者结合着来
a.retrograde().pitch_inversion()
a.pitch_inversion().retrograde()
```



## 检测一个音符或者和弦是否在一个音阶内存在

如果我们想看某一个音符或者和弦是否包含在一个音阶内，我们可以使用`in`关键字进行判断，可以判断一个表示音符名称的字符串(音高可以不带)，音符类型或者和弦类型是否包含在一个音阶类型内，比如

```python
>>> print('A' in S('C major'))
True

>>> print('Ab' in S('C major'))
False

>>> print('E4' in S('C major', 5)) 
True
# 音符的实际音高数会被忽略，只要音名有在音阶里就会返回True，如果想要考虑音高数，
# 可以使用'E4' in S('C major', 5).get_scale()

>>> print('A#' in S('Bb major')) # 如果是同音异名的音(十二平均律的标准下)，则会判断为包含，返回True
True

>>> print(N('A#') in S('Bb major'))
True

>>> print(N('C5') in S('Bb major', 5))
True

>>> print(C('Am7') in S('C major'))
True

>>> print(C('Em9') in S('C major'))
False
```



## 音阶类型按照使用罗马数字表示的和弦级数提取和弦

你可以使用音阶类型的`get_chord`函数按照使用罗马数字表示的和弦级数提取和弦。

```python
get_chord(degree, chord_type=None, natural=False)

# degree: 使用罗马数字表示的和弦级数的字符串，比如: 'IM7', 'ii7', 'IIm7', 'V7'，
# 可以是被调用的音阶类型里非自然出现的和弦。也可以是用数字与和弦名称的组合，比如: '1M7', '2m7'

# chord_type: 你可以分开写级数与和弦类型，此时degree为表示级数的字符串，可以是罗马数字，比如: 'I', 'II',
# 也可以是数字，比如: '1', '2'，chord_type为和弦类型，比如: 'M7', 'm7'

# natural: 是否按照音阶类型内自然出现的和弦为准，如果为True，
# 则会在得到的和弦不是音阶类型可以自然出现的和弦的情况下，
# 按照和弦的音符个数和级数在音阶类型内重新提取自然和弦，比如C大调音阶的'Im7'经过natural的转换之后会变成'IM7'，
# 默认值为False

Cmajor_scale = S('C major')

>>> Cmajor_scale.get_chord('IM7')
chord(notes=[C4, E4, G4, B4], interval=[0, 0, 0, 0], start_time=0)

>>> Cmajor_scale.get_chord('ii', '7')
chord(notes=[D4, F4, A4, C5], interval=[0, 0, 0, 0], start_time=0)
```



## 使用音阶类型产生和弦进行

```python
chord_progression(chords,
                  durations=1 / 4,
                  intervals=0,
                  volumes=None,
                  chords_interval=None,
                  merge=True)

# chords: 表示和弦级数的字符串的列表，可以是罗马数字或者数字与和弦名称的组合，
# 或者一个list或者tuple，里面有2个值，分别为罗马数字/数字, 和弦名称。

# durations: 每个和弦类型的音符长度

# intervals: 每个和弦类型的音符间隔

# volumes: 每个和弦类型的音符的音量大小

# chords_interval: 相邻的和弦类型之间的间隔，单位为小节

# merge: 是否合并为一个新的和弦类型，为True的时候返回合并过后的和弦类型，为False的时候，返回一个和弦类型的列表，默认值为True

Cmajor_scale = S('C major')

>>> Cmajor_scale.chord_progression(['IM7', 'Vsus', 'vi7', 'IVM7'])
chord(notes=[C4, E4, G4, B4, G4, C5, D5, A4, C5, E5, ...], interval=[0, 0, 0, 1/4, 0, 0, 1/4, 0, 0, 0, ...], start_time=0)

>>> Cmajor_scale.chord_progression([('I', 'M7'), ('V', 'sus'), ('vi', '7'), 'IV'], intervals=[1/8, [1/8,1/8,1/4], 1/8, 1/8])
chord(notes=[C4, E4, G4, B4, G4, C5, D5, A4, C5, E5, ...], interval=[1/8, 1/8, 1/8, 1/8, 1/8, 1/8, 1/4, 1/8, 1/8, 1/8, ...], start_time=0)

>>> Cmajor_scale.chord_progression(['1M7', '5sus', '6m7', '4M7'])
chord(notes=[C4, E4, G4, B4, G4, C5, D5, A4, C5, E5, ...], interval=[0, 0, 0, 1/4, 0, 0, 1/4, 0, 0, 0, ...], start_time=0)
```



## 按照音阶的音级提取音符

你可以用音阶类型的方法`get_note_from_degree`来按照音阶的音级提取音符，返回的值为音符类型。

```python
get_note_from_degree(degree, pitch=None)

# degree: 音阶的音级，从1开始，如果超过7则按照高八度来计算
# pitch: 初始的八度数，如果不设置则使用音阶本身的八度

current_scale = S('C major')

>>> current_scale.get_note_from_degree(1)
C4

>>> current_scale.get_note_from_degree(9)
D5

>>> current_scale.get_note_from_degree(5, pitch=5)
G5
```



## 查找一个音符在音阶里的音级

你可以用音阶类型的方法`get_scale_degree`来查找一个音符在音阶里的音级，音符可以是字符串或者音符类型。

```python
current_scale = S('C major')

>>> current_scale.get_scale_degree('G')
5
```



## 查找一个音符在音阶里的标准音名

```python
current_scale = S('G# major')

>>> current_scale.get_standard_notation('G')
'Fx'
```



## 获得一个音符在音阶中的距离某个音程的音符

你可以使用音阶类型的方法`get_note_with_interval`来获得一个音符在某个音阶中距离某个音程的音符。

```python
get_note_with_interval(current_note, interval, standard=False)

# current_note: 用来计算的音符，可以是字符串或者音符类型

# interval: 音程度数，从1开始，1表示一度

# standard: 是否将返回值的音名对应到音阶的标准音名

current_scale = S('C major')

>>> current_scale.get_note_with_interval('C4', 3)
E4

>>> current_scale.get_note_with_interval('C4', 9)
D5

>>> current_scale.get_note_with_interval('C4', -5)
F3
```

