# 常见的问题

以下是你在使用musicpy时你可能会遇到的一些问题以及解决方法，其中一些是musicpy的python依赖库的现有bug，或者只是在函数参数被设置为默认时没有给你想要的结果。

* ## 我在Linux上得到`pygame.error: 无法打开/etc/timidity/freepats.cfg`错误
在Linux上，pygame库使用freepats声音库来播放MIDI文件，所以你需要安装freepats。

解决方法：打开终端，运行`sudo apt-get install freepats`，就可以修复错误了。这个解决方法主要适用于Ubuntu，对于其他Linux发行版，你需要在互联网上搜索安装指令，为它们安装freepats，在一些Linux发行版上，你可能还需要安装timidity++。

* ## 当我在IDE中运行代码时，play函数不能发出任何声音
当你使用Pycharm或VS Code这样的python IDE时，会出现这个问题，因为这些IDE不会等到播放MIDI文件的pygame函数结束，它们会在所有代码执行完毕后停止整个过程，而不等待MIDI文件的播放。在交互性更强的Python IDE中不会遇到这个问题，比如Jupyter Notebook, Wing IDE，或者直接在终端使用Python的交互式shell。

解决方法：你可以在`play`函数的参数中加入`wait=True`，这将锁住这个函数直到播放结束，所以你可以听到声音。

* ## 我注意到每次我播放musicpy的音符/和弦/乐曲/音轨实例时，在当前目录下都会产生一个MIDI文件，musicpy能否只播放我写的东西而不写MIDI文件?
是的，musicpy的`play`函数的默认机制是首先将musicpy的数据结构写入一个MIDI文件，然后使用pygame的混合器模块来播放这个MIDI文件，但是你可以通过将`play`函数的参数`save_as_file`设置为False来改变内部播放机制，而不产生任何MIDI文件。通过这样做，MIDI文件数据流被生成并保存在内部，pygame能够直接播放MIDI文件流，没有MIDI文件被生成。

解决方法：将`play`函数的参数`save_as_file`设为False。
