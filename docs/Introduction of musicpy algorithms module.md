# Musicpy algorithms 模块介绍

有一些我正在开发的基于musicpy的音乐分析算法，你可以在musicpy包的algorithms模块中找到它们。

algorithms模块在musicpy中默认以`alg`的名称导入，所以你可以像这样使用musicpy的algorithms模块:

```python
import musicpy as mp

chord_type = mp.alg.detect(chord('C,E,G,B'))
>>> print(chord_type)
Cmaj7
# detect函数是algorithms模块中的音乐分析算法之一
```

算法模块中的大部分音乐分析算法仍在开发中，我将在业余时间尝试为一些相对成熟的算法写文档。

