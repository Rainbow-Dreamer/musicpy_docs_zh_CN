# 弯音类型的基本语法



## 目录

- [往一首曲子插入实时的弯音,滑音,颤音变化](#往一首曲子插入实时的弯音滑音颤音变化)



## 往一首曲子插入实时的弯音,滑音,颤音变化

你可以使用pitch_bend类型（弯音类型）插入到一个和弦类型的`pitch_bends`列表中，可以实时改变某一个片段的音符的音高，可以很细微地变化音高。
构建pitch_bend类型时，使用mode参数可以选择三种不同的单位:

* 半音 (-2到2之间的任意整数，小数或者分数，不包括-2和2本身)
* 音分 (1个半音 = 100音分，-200到200之间的任意整数，小数或者分数，不包括-200和200本身，音分是默认单位)
* MIDI弯音值 (-8192到8192之间的任意整数，不包括-8192和8192本身)



### pitch_bend类型的构成

```python
pitch_bend(value, start_time=0, mode='cents', channel=None, track=None)
```

- value: 音符的音高变化的量
- start_time: 音符的音高变化的位置，单位为小节
- mode: mode == 'cents' 选择音分作为单位，为默认值，mode如果不设置就选择音分作为单位; mode == 'semitones' 选择半音作为单位; mode == 'values' 选择MIDI弯音值作为单位
- channel: MIDI通道编号
- track: MIDI音轨编号



### pitch_bend类型插入到和弦类型中

把pitch_bend类型插入到和弦类型的`pitch_bends`列表中。

```python
a = chord(['C5', 'D5', 'E5', 'F5', 'G5', 'A5', 'B5', 'C6']) % (1/8,1/8)
a.pitch_bends.append(pitch_bend(value=30, start_time=3/8))
play(a, 80)
# 和弦类型a从开始到E5会以正常的音高演奏，之后的音符会调高30音分进行演奏（也就是0.3个半音）
```
