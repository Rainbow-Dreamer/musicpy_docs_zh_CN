# 用于分析乐曲调性与转调的算法

在musicpy的`algorithms`模块中，实现了若干算法用于分析乐曲或音乐片段的调性。

此处的调性涵盖`database`模块中定义的所有自定义音阶，包括大调、小调、所有教会调式、旋律小调、和声小调等，用户还可通过音程自定义新音阶。

目前共实现三种分析算法，分别封装在`detect_scale`、`detect_scale2`、`detect_scale3`函数中。其中第三种算法具备分析乐曲调性与转调的能力，其余两种仅能分析调性。

本文首先介绍第三种算法，该算法目前在三种算法中实用性最强。

## Usage
```python
detect_scale3(current_chord,
              get_scales=False,
              most_appear_num=3,
              major_minor_preference=True,
              unit=5,
              key_accuracy_tol=0.9,
              is_chord=True)
```
* current_chord: 待分析的和弦实例
* get_scales: 若设为True，则返回音阶实例；若设为False，则返回表示调性与转调关系的字符串
* most_appear_num: 可能音阶的最大数量
* major_minor_preference: 将大调或小调置于结果首位
* unit: 用于分析调性与转调的小节单位长度
* key_accuracy_tol: 分析范围内音符与音阶匹配比例达到的最小百分比阈值
* is_chord: 设为True时直接分析给定和弦实例，设为False时将输入合并为和弦实例列表后分析

当`get_scales`设为False时，输出格式为`[起始1, 终止1] 音阶1, 音阶2, ..., [起始2, 终止2] 音阶1, 音阶2, ..., [起始n, 终止n] 音阶1, 音阶2, ...`，每个起止点对应输入中出现的特定调性所在小节范围。

## Examples

```python
import musicpy as mp
current_piece = mp.read('test.mid')
current_chord, current_bpm, current_start_time = current_piece.merge()
current_scales = mp.alg.detect_scale3(current_chord)
>>> print(current_scales)
[0, 25] G major, E minor, [25, 44.25] G minor, A# major
```

