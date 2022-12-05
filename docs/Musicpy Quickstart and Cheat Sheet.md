# Musicpy快速入门和速查表

如果你觉得文档太长，太复杂，难以阅读，难以找到你想要的东西，这是一个musicpy的快速入门和速查表的章节。

# 速查表
(当musicpy以`from musicpy import *`的方式导入）

注意1：muscipy的和弦类型有一个`interval`属性，它是和弦的每两个相邻的音符开始之间的小节长度的列表，请不要把它与乐理中的音程混淆，后者是两个音符之间的音高差异（例如大三度，半音）。在速查表中，我们用 `音程`来表示乐理中的音程。

注意2：所有函数的默认的下标起始值都为0，除非特别说明下标从1开始。

|功能 |语法（推荐） |其他写法 |
| :----------------------------------------------------------: | :----------------------------------------------------------: | :----------------------------------------------------------: |
|构建一个音符 C5 | N('C5') | N('c5'); note('C', 5) |
|构建和弦 Cmaj7 | C('Cmaj7') | chd('C', 'maj7') |
|构建一个起始音符为C5的C大七和弦 | C('C5:maj7') | C('Cmaj7', pitch=5);<br/>chd('C', 'maj7', pitch=5) |
|用音符 C5、E5、G6 构建一个和弦 |                     chord('C5, E5, G6')                      | chord('c5, e5, g6');<br/>chord(['C5', 'E5', 'G6']);<br/>chord(['c5', 'e5', 'g6']);<br/>chord([N('C5'), N('E5'), N('G6')]) |
|构建C大调音阶|                         S('C major')                         |                     scale('C', 'major')                      |
|构建一个乐曲 |build(track(content=C('C'), instrument=1, start_time=0),<br/>track(content=C('D'), instrument=47, start_time=1), bpm=150) | P(tracks=[C('C'), C('D')],<br/>instruments=[1, 47],<br/>start_times=[0, 1],<br/>bpm=150) |
|构建轨道 |track(content=C('C'), instrument=1, start_time=0)| |
|构建一组鼓点 |drum('K, H, H, K, S, H, H, H, t:1')| |
|将和弦/音符 A 向上/向下移调 n 个半音 |A + n; A - n | A.up(n); A.down(n) |
|提高/降低和弦 A n 半音的第 i 个音符 | A + (n, i); A - (n, i) | A.up(n, i); A.down(n, i) |
|提高/降低和弦 A n 个半音的第 ith 个音符到第 j 个音符 |                 A + (n, i, j); A - (n, i, j)                 |                A.up(n, i, j); A.down(n, i, j)                |
|连接和弦 A 和和弦 B |                            A \| B                            |                            A + B                             |
|在和弦 A 之后以 n 小节间隔连接和弦 B |                         A \| (B, n)                          |                          A + (B, n)                          |
|将剩余的 n 小节添加到和弦 A |                            A \| n                            |                          A.rest(n)                           |
| 将和弦A连接n次，每次间隔i条 |                         A \| (n, i)                          |                          A * (n, i)                          |
| 堆叠和弦A和B | A & B | |
| 叠加和弦B，在和弦A之后叠加n个小节 |                          A & (B, n)                          |                                                              |
| 叠加和弦A n次 | A & n | |
|叠加和弦An次，每次间隔i个小节 | A & (n, i) | |
| 堆叠和弦A和B | A & B | |
| 叠加和弦B，在和弦A之后叠加n条 | A & (B, n) | |
| 叠加和弦A n次 | A & n | |
|叠加和弦An次，每次间隔i个小节 | A & (n, i) | |
| 重复和弦A n次 | A * n | |
| 获取和弦A的第i个音符 | A[i] | |
| 获得和弦A的第i至j个音符 | A[i:j] | |
| 设置和弦A的第i个音符为j | A[i] = j | |
| 获得和弦A的音符数 | len(A) | |
|获取和弦A的反向音符 | ~A | A.reverse() |
|                   将和弦/音符A升高一个半音                   |                              +A                              |                            A.up()                            |
| 将和弦/音符A降低一个半音 | -A | A.down() |
| 设置和弦A的持续时间、间隔时间和音量，并返回一个新的和弦A |               A % (duration, interval, volume)               |              A.set(duration, interval, volume)               |
| 获取和弦A的第n个转位 (从1开始) | A / n | A.inversion(n) |
| 将和弦A的第n个音转换为最高音 (从1开始) | A ^ n | A / -n; A.inversion_highest(n) |
|将和弦A的第n个音反转为最低音 (从1开始) | A @ n | A.inv(n) |
|       从和弦A的音符中按照索引组成一个新的和弦（从1开始）       | A @ [1, 2, 3, 2, 1.1, 2, 3, 2] ||
|从和弦A的音符中按照索引组成一个新的和弦（从1开始），增加八度数| A / [1, 2, 4, 3] ||
| 将和弦A从音阶i转调到音阶j | A.modulation(i, j) | |
| 删除和弦A的第i个音符 | del A[i] | |
| 将音符i附加到和弦A的音程j上，并返回一个新的和弦A | A + i | |
| 将音符i追加到和弦A中，间隔时间为j | A.append(i, j) | |
| 从和弦A中删除音符i，并返回一个新的和弦A | A - i | |
| 从和弦A中删除i音符 | A.remove(i) | |
| 从和弦A中取出第i个音符 | A.pop(i) | |
| 在索引i处插入音符j，间隔时间为k的和弦A | A.insert(i, j, interval=k) | |
| 和弦A的drop n voicing | A.drops(n) | |
| 和弦A在音阶B上的负和声 | A @ B | A.negative_harmony(B) |
| 获取和弦A从第i小节到第j小节（以0为基准）的和弦| A.cut(i, j) | |
| 根据和弦A的音符变化，得到一个新的和弦 | A('omit 3, b9') | |
|获取和弦A的总长度（小节） | A.bars() | |
| 获得和弦A的音符持续时间 | A.get_duration() | |
|获取和弦A的音符间隔 | A.interval | |
| 设置音符A的持续时间、音量和通道 |               A % (duration, interval, volume)               |              A.set(duration, interval, volume)               |
| 改变音符A的变音记号（#到b，b到#），并返回一个新的音符 | ~A | |
| 获得音符A的附点音符，有n个附点 | A.dotted(n) | |
| 获得音符A的持续时间 | A.duration | |
| 提高/降低音阶A的第n个半音 |                         A + n; A - n                         |                      A.up(n); A.down(n)                      |
| 提高/降低音阶A的第i个音n个半音 |                    A + (n, i); A - (n, i)                    |                   A.up(n, i); A.down(n, i)                   |
|           提高/降低音阶A的第i个音到第j个音n个半音            |                 A + (n, i, j); A - (n, i, j)                 | A.up(n, i, j); A.down(n, i, j) |
|                    提高/降低音阶A 1个半音                    |                            +A; -A                            |A.up(); A.down()|
|获取音阶A的第i级音 | A[i] | |
|获取音阶A的第i级音 (从1开始) | A.get_degree(i) | |
| 获取音阶A的第i级三和弦 | A(i) | A.pick_chord_by_degree(i)|
| 获取音阶A的第i级七和弦 | A(i, num=4) | A.pick_chord_by_degree(i, num=4)|
| 获得音阶A的第i个调式 (从1开始) | A / i | A.inversion(i) |
| 按照索引获取音阶A的和弦 | A @ [0, 2, 4] | A.pick_chord_by_index([0, 2, 4]) |
| 获得音阶A的反转音阶 | ~A | A.reverse() |
| 按照级数从音阶A获取和弦进行 (从1开始) | A % 6451 | A.pattern(6451)|
| 获取乐曲A的第1个音轨 | A[i] | |
| 获得乐曲A的第i个音轨的和弦类型 | A(i) | A.tracks[i] |
| 提高/降低乐曲A n个半音 | A + n; A - n | |
| 提高/降低乐曲A的第1个音轨n个半音 | A[i] += n; A[i] -= n | |
| 重复乐曲A n次 | A * n | |
| 将乐曲A和乐曲B连接起来 | A + B | A \| B |
| 堆叠乐曲A和乐曲B | A & B | |
| 删除A乐曲的第1个轨迹 | del A[i] | |
| 获取A乐曲的反向 | ~A | A.reverse() |
| 获得A乐曲的轨道数 | len(A) | |
| 在A乐曲上添加一个新的轨道n | A.append(n) | |
|在A乐曲的索引i处插入一个新的音轨n | A.insert(i, n) | |
| 获取乐曲A的总长度（小节） | A.bar() | |
| 读取一个MIDI文件到乐曲类型 | read(path) | |
| 将一个音符/和弦/乐曲/音轨/鼓的类型A写到一个MIDI文件中 | write(A) |  |
| 播放一个音符/和弦/乐曲/音轨/鼓的类型A，速度为bpm 100 | play(A, bpm=100) | |
| 停止播放 | stopall() | |
| 连接一个和弦列表[A, B, C] | concat([A, B, C]) | |
| 连接和弦列表[A, B, C]和额外的间隔i | concat([A, B, C], extra=i) | |
| 堆叠和弦列表[A, B, C] | concat([A, B, C], mode='&') | |
| 叠加一个和弦[A, B, C]的列表，其中有额外的间隔i | concat([A, B, C], mode='&', extra=i) | |
| 从MIDI度数i获得音符 | degree_to_note(i) | |
| 获得音符A的度数（MIDI音符编号） | A.degree | |
| 获取音符A的音符名称 | A.name | |
| 获得音符A的八度数 | A.num | |
| 获得和弦A的音程 | A.intervalof() | |
| 构建一个bpm为150、开始时间为0的节奏变化实例 | tempo(150, start_time=0) | |
| 构建一个pitch_bend实例，值为100分，开始时间为0 | pitch_bend(100, start_time=0) | |
| 构建一个pan实例，值为100%，开始时间为0 | pan(100, start_time=0) | |
| 构建一个volume实例，数值为100%，开始时间为0 | volume(100, start_time=0) | |
| | | |
| | | |
| | | |
