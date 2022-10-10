# 常见的问题

以下是你在使用musicpy时你可能会遇到的一些问题以及解决方法，其中一些是musicpy的python依赖库的现有bug，或者只是在函数参数被设置为默认时没有给你想要的结果。

* ## 我试图把从MIDI文件中读到的乐曲实例播放或写入一个新的MIDI文件，但当我在DAW中看到新的MIDI文件时，有些音符变得很长，与原来的MIDI文件的长度完全不同

这是因为有些MIDI文件在某些轨道上有重复的音符，这里的重复音符指的是这些音符的音高完全一样，在同一时间开始，或者有某些部分重叠，在这种情况下，一些python MIDI库可能会把这些重复的音符视为一个完整的音符，从而导致问题发生。

解决方法：你可以把`write`或`play`函数的参数`remove_duplicates`设置为True，使写入的MIDI文件与你在DAW中查看原始MIDI文件时一样。

* ## 我在Linux上得到`pygame.error: 无法打开/etc/timidity/freepats.cfg`错误
在Linux上，pygame库使用freepats声音库来播放MIDI文件，所以你需要安装freepats。

解决方法：打开终端，运行`sudo apt-get install freepats`，就可以修复错误了。这个解决方法主要适用于Ubuntu，对于其他Linux发行版，你需要在互联网上搜索安装指令，为它们安装freepats，在一些Linux发行版上，你可能还需要安装timidity++。

* ## 当我在IDE中运行代码时，play函数不能发出任何声音
当你使用Pycharm或VS Code这样的python IDE时，会出现这个问题，因为这些IDE不会等到播放MIDI文件的pygame函数结束，它们会在所有代码执行完毕后停止整个过程，而不等待MIDI文件的播放。在交互性更强的Python IDE中不会遇到这个问题，比如Jupyter Notebook, Wing IDE，或者直接在终端使用Python的交互式shell。

解决方法：你可以在`play`函数的参数中加入`wait=True`，这将锁住这个函数直到播放结束，所以你可以听到声音。

* ## 我试图用read函数读取一个MIDI文件，但它给了我索引超出范围的错误
这是mido的一个错误，mido是musicpy使用的python MIDI库之一。当一些MIDI文件的元信息包含空数据，mido试图从中获取属性时，并不是每一种元信息的解码函数都有边界检查数据是否为空，并通过索引直接获取数据元素，因为假设数据不是空的，这就导致了索引超出范围的错误。

解决方法：要解决这个问题，你需要到`Lib\site-packages\mido\midifiles\meta.py`，为元信息的每个解码函数添加数据的边界检查，看它是否为空，也就是在代码试图通过索引从数据中获取元素之前添加`if data:`。我已经做了修改，并在github上mido的issue部分创建了一个issue，请求修复这个bug。下面是一个例子：
```python
class MetaSpec_key_signature(MetaSpec):
    type_byte = 0x59
    attributes = ['key']
    defaults = ['C']

    def decode(self, message, data):
        key = signed('byte', data[0])
        mode = data[1]
        try:
            message.key = _key_signature_decode[(key, mode)]
        except KeyError:
            if key < 7:
                msg = ('Could not decode key with {} '
                       'flats and mode {}'.format(abs(key), mode))
            else:
                msg = ('Could not decode key with {} '
                       'sharps and mode {}'.format(key, mode))
            raise KeySignatureError(msg)
```
改为：
```python
class MetaSpec_key_signature(MetaSpec):
    type_byte = 0x59
    attributes = ['key']
    defaults = ['C']

    def decode(self, message, data):
        if data:
            key = signed('byte', data[0])
            mode = data[1]
            try:
                message.key = _key_signature_decode[(key, mode)]
            except KeyError:
                if key < 7:
                    msg = ('Could not decode key with {} '
                           'flats and mode {}'.format(abs(key), mode))
                else:
                    msg = ('Could not decode key with {} '
                           'sharps and mode {}'.format(key, mode))
                raise KeySignatureError(msg)
```

* ## 我注意到每次我播放musicpy的音符/和弦/乐曲/音轨实例时，在当前目录下都会产生一个MIDI文件，musicpy能否只播放我写的东西而不写MIDI文件?
是的，musicpy的`play`函数的默认机制是首先将musicpy的数据结构写入一个MIDI文件，然后使用pygame的混合器模块来播放这个MIDI文件，但是你可以通过将`play`函数的参数`save_as_file`设置为False来改变内部播放机制，而不产生任何MIDI文件。通过这样做，MIDI文件数据流被生成并保存在内部，pygame能够直接播放MIDI文件流，没有MIDI文件被生成。

解决方法：将`play`函数的参数`save_as_file`设为False。