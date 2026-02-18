# 用于分析乐曲和弦进行的算法

在musicpy的`algorithms`模块中，实现了一个和弦进行分析算法，用于分析乐曲或音乐片段中的和弦进行。该算法通过`chord_analysis`函数实现，同时还包含`chord_functions_analysis`函数，用于进一步分析给定调式中每个和弦的功能。

需注意该算法尚处于实验阶段，无法保证在所有乐曲情境下均能有效运行。

`chord_analysis`函数通过分析给定和弦实例（代表音乐片段）拆分出的每个和弦并输出结果。

## Usage
```python
chord_analysis(chords,
               mode='chord names',
               is_chord=False,
               new_chord_tol=database.minor_seventh,
               get_original_order=False,
               formatted=False,
               formatted_mode=1,
               output_as_file=False,
               each_line_chords_number=5,
               functions_interval=1,
               split_symbol='|',
               space_lines=2,
               detect_args={},
               split_chord_args={})
```
* chords: 待分析的和弦实例
* mode: 输出模式，可选项如下：
  * 'chords': 获取每个和弦的和弦实例
  * 'chord names': 获取每个和弦的名称
  * 'inds': 获取每个和弦的索引
  * 'bars': 获取每个和弦的小节范围
  * 'bars start': 获取每个和弦的起始小节
* is_chord: 若设置为False，算法将首先对给定的和弦实例执行旋律与和弦分离操作；若设置为True，则直接使用给定的和弦实例进行分析。
* new_chord_tol: 新和弦容差，音高间隔
* get_original_order: 若设为True，获取给定和弦实例中每个和弦的原始顺序
* formatted: 输出字符串的格式化模式，可取值0或1
* formatted_mode: format mode for the output string, could be 0 or 1
* output_as_file: 当 `formatted` 为 True 时，设为此值可将结果输出为文件
* each_line_chords_number: 输出字符串中每行包含的和弦数量
* functions_interval: 输出字符串中相邻两个和弦间的空格数量
* split_symbol: 输出字符串中相邻两个和弦间的分隔符号
* space_lines: 输出字符串中每两行之间空行的数量
* detect_args: `detect`函数的参数
* split_chord_args: `split_chord`函数的参数

## Examples

```python
import musicpy as mp
current_piece = mp.read('test.mid')
current_chord, current_bpm, current_start_time = current_piece.merge()
current_chord_progressions = mp.alg.chord_analysis(current_chord)
>>> print(current_chord_progressions)
['Dm7 omit F', 'Bm7 sort as [1, 3, 4, 2]', 'Em7 omit G', 'Dm7 omit F']
```
