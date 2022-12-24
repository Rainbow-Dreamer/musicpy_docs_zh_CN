# Musicpy语言作曲代码示例（第一期）

## 1. 东方主旋律
```python
play((get_chord_by_interval('D#4', [5,7,10,7,5], 1/2, 1/8)*3 + get_chord_by_interval('F4', [1,0,-4], 1/2, 1/8)) * 3, 150)
```

## 2. 演奏出A小七和弦后接休止符半拍4遍，然后B小七和弦后接休止符半拍4遍，然后C大七和弦分解和弦1遍。
(以上这两个例子都是没有使用进阶写法的，使用进阶写法可以让musicpy语言看起来更加紧凑，简短)
```python
a = C('Am7') % (1/8, 0)
play(a * 4 | (a + 2) * 4 | get_chord(a[0] + 3, 'maj7').set(interval=1/8), bpm=80)
```

## 3. 一小段钢琴片段
第3个例子使用了很多进阶写法，代码看起来紧凑了很多，而且对于两个和弦类型的连接的进阶写法使用分隔符`|`也可以有类似小节线的感觉，可以增强代码的可读性。
```python
a = C('Dmaj7') % (1/4,1/4) | C('Cmaj7') | C('Fadd9',3) | C('D#maj7',4) | (C('Dmaj7',3)/-2) % (5/4,)
b = chord('F#5, G5, A5, B5, G5', 1/8, 1/8)   
play(a & b, 140, instrument=1)
```

## 4. 半音下行大七小七和弦交替
```python
a = C('Amaj7') @ 2 * 4 | C('G#m7') @ 2 * 4 | C('Gmaj7') @ 2 * 4 | C('F#m7') @ 2 * 4    
play(a | a % (1/4, 1/4), 165, instrument=9)
```

## 5. 一段有恐怖氛围的音乐，管弦乐音色
```python
a = (C('Bmaj9',3)/[2,3,4,1,5]) % (1/8,1/8)
b = (C('Bmaj9',3)/[2,3,4,1,5,2]) % (1/8, 1/8)
q = a + ~a[1:-1]
q2 = b + ~b[3:-1]
t = (q + q2) * 2
adding = chord(['Bb5','Ab5','Gb5','Ab5']) % (1/2,1/2) * 2
t2 = t & adding
play(t2 + (t2 - 3), 100, instrument=47)
```

## 6. 很好听的6451和弦配置，竖琴音色
```python
q = scale('C4', 'major')
r = q%(6451, 0.5/4, 0.3/4, 5)
r = [i.omit(5).inversion_highest(3) for i in r]
play(r*2, bpm=80, instrument=47)
```

## 7. 另一个很好听的6451和弦配置，竖琴音色
```python
q = scale('C4', 'major')
r = q%(6451, 0.5/4, 0.3/4, 5)
r = [i.omit(7).inversion_highest(2) for i in r]
play(r*2, bpm=80, instrument=47)
```

## 8. 恐怖阴森的配乐，使用到的乐器：民谣吉他(steel-string acoustic guitar)和管弦乐
```python
a = C('Baug9', 4) @ [2, 3, 4, 1.1, 5, 1.1, 4, 3] % (1/8,1/8)
b = C('BmM9', 4) @ [2, 3, 4, 1.1, 5, 2.1, 5, 1.1] % (1/8,1/8)
part1 = (a + b)*2
part2 = (C('Baug9',4, 1, 0) | C('BmM9',4, 1, 0)) * 2
part2.set_volume(80)
play(P([part1, part2], [26,49], 100, [0,0]))
```

## 9. 很好听的6451和弦配置，电钢琴音色
```python
q = scale('C4', 'major')
r = q%(64516458, 1/2, 0.3/4, 5)
r = [i.omit(7).inversion_highest(2) for i in r]
play(r*2, bpm=80, instrument=5)
```

## 10. 80年代硬摇滚或者流行金属风格，使用到的乐器：钢琴，合成器音色，电贝斯
```python
a = chord('C2, G1, C2, Ab1, Bb1, G1', [15/8,1/8,2,2,1.5,3/4], [15/8,1/8,2,2,1.5,3/4])
b = C('Cm', 5, 1/8) | 1/4 | C('Bb', 4, 1/8) | 1/4 | C('Ab', 4, 1/8) | 1/4 | C('Bb', 4, 1/2) | 3/8
b2 = C('Cm', 5, 1/8) | 1/4 | C('Bb', 4, 1/8) | 1/4 | C('Abmaj7', 4, 1/8) | 1/4 | C('Gsus', 4, 3/4) | 1/8
c = (b * 3) | b2
a2 = chord('C2',1/8,1/8)*32 + chord('Ab1',1/8,1/8)*16 + chord('Bb1',1/8,1/8)*12 + chord('G1',1/8,1/8)*4
play(piece([a, c, a2 * 2, c * 2], [1, 81, 34, 81], 130, [0, 1/4, 8, 33/4]))
```

## 11. 恐怖氛围音乐，管弦乐音色
```python
a = get_chord('B4','maj9').sort([2,3,4,1,5])
b = get_chord('B4','maj9').sort([2,3,4,1,5,2])
q = a + a[1:-1].reverse_chord()
q2 = b + b[3:-1].reverse_chord()
play((q.set(1/8,1/8) + q2.set(1/8,1/8))*2, 100, instrument=50)
```
