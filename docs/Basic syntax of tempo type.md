# 速度类型的基本语法



## 目录
- [往一首曲子插入实时速度变化](#往一首曲子插入实时速度变化)



## 往一首曲子插入实时速度变化

一个和弦类型的属性`tempos`是一个包含速度变化类型的列表，每个速度变化类型必须设定具体的开始时间，在播放曲子时可以让你的曲子的速度进行动态的变化。



### tempo类型的构成
```python
tempo(bpm, start_time=0, channel=None, track=None)
```
- bpm: 想要变化到的曲速
- start_time: 想要在什么时间改变位置，单位为小节，可以是整数，小数或者分数
- channel: MIDI通道编号
- track: MIDI音轨编号



### tempo类型插入到和弦类型中
把tempo类型插入到和弦类型的`tempos`列表中。
```python
a = chord(['C5', 'D5', 'E5', 'F5', 'G5', 'A5', 'B5', 'C6']) % (1/8,1/8)
a.tempos.append(tempo(bpm=150, start_time=3/8))
play(a, 80)
# 和弦类型a从开始到E5会以80BPM的速度演奏，之后会以150BPM的速度演奏
```
