# 声相类型的基本语法



## 目录

- [对于音轨的左右声道混音位置的调节](#对于音轨的左右声道混音位置的调节)



## 对于音轨的左右声道混音位置的调节

当你构建了一个piece类型(乐曲类型)的时候，每一个音轨都各自有自己的独立的乐器，并且想要进行每个音轨的左右声道的位置摆放调节。  

比如，音轨1我想要把声道的位置调到非常偏左的声道，音轨2我想留在中间，音轨3我想调到稍微偏右边一些的声道。  

这时候可以使用piece类型的`add_pan`的内置函数，对于指定的音轨设定左右声道的混音位置。  

pan类型是专门用来存储左右声道混音位置的类型。

### pan类型的构成

```python
pan(value, start_time=0, mode='percentage', channel=None, track=None)
```

- value: 当mode == 'percentage', value指的是左右声道的百分比，值为从0到100的整数或者小数，表示从0%到100%, 0%表示最左的声道, 100%表示最右的声道, 50%则表示正中间。当mode == 'value', value指的是MIDI CC 10的值，也就是pan position信息的值, 从0到127，值为正整数，也是从最左的声道到最右的声道。
- start_time: 声相变化开始的时间，单位为小节。
- mode: 用来设定value的单位
- channel: MIDI通道编号
- track: MIDI音轨编号

```python
a = piece([A, B, C, D], [1, 47, 35, 2], 150, [0, 2, 2, 8]) # a是一个piece类型(乐曲类型)
a.add_pan(value, ind, start_time=0, mode='percentage', channel=None, track=None)
# value, start_time, mode, channel, track都是对应pan类型的构建参数,
# ind是想要加入左右声道混音调节的音轨的index, 以0作为第1条轨道
```

另外，在构建piece类型的时候，两个参数`pan`和`volume`可以用来设定每一条音轨的左右声道混音调节和总体音量大小，这两个参数的值都为一个列表，元素为pan/volume类型的列表，表示每个音轨的pan/volume的设定。如果不设定则默认值为None，在构建piece类型时会转化为一个元素为空列表的列表，元素个数为构建的piece类型所含有的音轨数。在构建乐曲类型中传入pan/volume的列表时，每个音轨的pan/volume可以是单个pan/volume类型，也可以是pan/volume类型的列表。

注意：如果要设定pan和volume参数的值，请将列表的元素个数保证刚好和音轨数相同，如果有某些音轨不想设定，可以在那个音轨的位置写一个空列表。

