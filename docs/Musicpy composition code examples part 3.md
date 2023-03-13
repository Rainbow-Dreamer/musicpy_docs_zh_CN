# Musicpy语言作曲代码示例（第三期）



## 1. 使用musicpy还原Rolling Star - Yui
```python
bass11 = translate('B1[l:.8; i:.; r:8], D2[l:.8; i:.; r:8], A1[l:.8; i:.; r:8], G1[l:.8; i:.; r:8]')
bass12 = translate('G1[l:.8; i:.; r:6], A1[l:.8; i:.; r:2]')
bass1 = bass11 * 2 | bass12

guitar11 = translate('B3[l:.4; i:.], D4[l:.8; i:.], E4[l:3/8; i:.], D4[l:.8; i:.], E4[l:.8; i:.]')
guitar12 = guitar11.down(2, 0)
guitar13 = guitar12.down(1, [1, 3])
guitar14 = translate('G3[l:.4; i:.], B3[l:.8; i:.], A4[l:5/8; i:.]')
guitar1 = (guitar11|guitar12|guitar13|guitar14) * 2

drum11 = drum('C;K, H, S, H | K, H, S, H, r:5, C;K, H;S[r:2], PH, OH, S;OH[r:3]').notes
drum12 = drum('C;K;H;S, H;S[r:2], PH, OH, S;OH[r:3]').notes
drum1 = drum11 * 2 | drum12

synth11 = translate('D5[l:.8; i:.], B5[l:.8; i:.], r:16')
synth1 = synth11

result = P([bass1, guitar1, drum1, synth1],
           [34, 31, 1, 91],
           bpm=165,
           channels=[0,1,9,2],
           start_times=[0,0,0,4])

play(result)
```



## 2. 空灵钢琴配乐
```python
a = (C('G/C',3) @ [1,2,3,2,1.1,2,3,2]) % (2,1/8) | (2, 1)
result = (a | 1 | a - 2) + database.octave
result.other_messages = [
event('control_change', control=10, value=0, start_time=0),
event('control_change', control=10, value=127, start_time=2),
event('control_change', control=10, value=64, start_time=4)
]
play(result, 165)
```



## 3. JRPG感觉的背景弦乐
```python
w = chord_progression([C('A',3)^2, C('A/G',3)^2, C('A/F#',3)^2, C('Fmaj7',3)^2, C('F/E'), C('Fm/Eb')^2, C('Asus/D').on('A1')], durations=1)
w2 = chord('A2,G2,F#2,F2,E2,Eb2,A1') % (1, 1)
play(w & w2, 150, i=49)
```



## 4. 带有悲凉情感的钢琴琶音演奏
```python
w = (C('Cmadd9,add11',5) @ [1,3,1.1,4,2.1,5,2.1,4] % (1/2,1/8) * 2 |
C('A#add9,add11',4) @ [1,3,1.1,4,2.1,5,2.1,4] % (1/2,1/8) * 2 |
C('G#add9',4) @ [1,3,1.1,4,2.1,4,1.1,3] % (1/2,1/8) * 2 |
C('A#add9',4) @ [1,3,1.1,4,2.1,4,1.1,3] % (1/2,1/8) * 2)

w2 = translate('C2;C3[l:2], i:2, A#1;A#2[l:2], i:2, G#1;G#2[l:2], i:2, A#1;A#2[l:2]')
play(w | (w & w2), 265)
```



## 5. 夜之钢琴曲开头部分
```python
w1 = ((C('Cmadd2') + 'C5') % (1/2,1/8) | 3/8 |
(C('Cmadd2') + 'C5') % (1/2,1/8) | 3/8 |
chord('F5,D#5,D5,D#5,C5') % (1/2,1/8))
w2 = (C('G#add9',3) @ [1,3,1.1,4,2.1] % (1/2,1/8) | 3/8 |
C('A#add9',3) @ [1,3,1.1,4,2.1] % (1/2,1/8) | 3/8 |
(C('Cmadd9,add11',3) @ [1,3,1.1,4,2.1,5,2.1,4] % (1/2,1/8)) * 2)
play((w1 & (w2, 1/2)) | (2, -7/8), 90)
```



## 6. 有点悬疑氛围的音乐
```python
a = get_chord('bb2', 'm11') % (1/2,1/8) @ [1,3,5,4.1,2.2,6.1,5.1,4.1]
b = get_chord('g2', 'M9#11') % (1/2,1/8) @ [1,3,5,4.1,2.2,6.1,5.1,4.1]
c = get_chord('gb2', '13sus') % (1/2,1/8) @ [1,3,4,5,2.1,6,4.1,5.1]
play(a * 4 | (a-2) * 2 | b | c, 135, i=5)

# 另一个版本
play((a * 4 | (a-2) * 2 | b | c) * 2, 100, i=49)
```



## 7. 日系摇滚前奏的其他版本
```python
guitar = ((C('Cmaj7')@1)@[1,2,3,4,1,2,3,2] |
(C('Fmaj7',3)^2)@[1,2,3,4,1,2,3,2] |
(C('G7sus',3)^2)@[1,2,3,4,1,2,3,2] |
C('Am11',3)@[1,2,4,6,1,4,6,4] |
(C('Cmaj7')@1)@[1,2,3,4,1,2,3,2] |
C('Fadd9',3).up(2,1)@[1,2,3,4,1,2,3,2] |
(C('G7sus',3)^2)@[1,2,3,4,1,2,3,2] |
C('Am11',3)@[1,2,4,6,1,4,6,4]) % (1/2,1/8)
guitar2 = (C('Am11',3)@[1,2,4,6,1,4,6,4] |
(C('Em7',3)^2)@[1,2,3,4,1,2,3,2] |
(C('Fmaj9',3)^2)@[1,2,3,4,1,2,3,2] |
(C('Cmaj9')^2)@[1,2,3,4,1,2,3,2] |
C('Am11',3)@[1,2,4,6,1,4,6,4] |
(C('Em7',3)^2)@[1,2,3,4,1,2,3,2] |
(C('Fmaj9',3)^2)@[1,2,3,4,1,2,3,2] |
(C('Cmaj9')^2)@[1,2,3,4,1,2,3,2]) % (1/2,1/8)
bass1 = chord('D2[.8;.],E2[.8;.],D2[.8;.],E2[1;.], F2[1;.], G2[1;.], A1[.2;.], A2[.8;.], G2[.8;.], E2[.8;.], D2[.8;.]')
bass2 = (chord('E2')*8 + chord('F1')*8 + chord('G1')*8 + chord('A1,A1,E2,A1,A2,A1,G2,D2')) % (1/8,1/8) * 2
bass3 = (chord('A2')*8 + chord('E2')*8 + chord('F2')*8 + chord('C2,C2,G2,C2,C3,C2,G2,C2')) % (1/8,1/8) * 2
bass = bass1 | bass2 | bass3
rhythm_guitar = (C('A5(+octave)',2, 1) | C('E5(+octave)',2, 1) | C('F5(+octave)',2, 1) | C('C5(+octave)',3, 1/2)| C('G5(+octave)',2, 1/2)) * 2
string1 = chord('B5[1;.],C6[1;.],D6[.2;.],E6[.2;.],\
F6[.2;.],E6[.2;.],C6[1;.],B5[.2;.],C6[.2;.],G5[1;.],\
F5[.2;.],E5[.2;.],C5[1;.],D5[.4;.],E5[.4;.],F5[.4;.],\
E5[.4;.],D5[.2;.],G5[.2;.],E5[1;.]')
drum1 = drum('K, H, S, H, r:2, K, H, OH;S, H, K, H, S, H').notes
drum1.set_volume(112)
result = piece([guitar * 2 | guitar2, bass, string1, rhythm_guitar,drum1 * 4 + 2],
               [2, 34, 49, 31, 1],
               bpm=165,
               start_times=[0, 4-3/8, 12, 16, 16],
               channels=[0,1,2,3,9])
play(result-2)
```



## 8. 放松的轻音乐，氛围音乐
```python
result = P([arp('C', second_half=True, intervals=1/8).cut(0, 2)|
            arp('D/F#', second_half=True, intervals=1/8).cut(0, 2),
            translate('A2[l:1; i:.; r:2], D3[l:1; i:.; r:2]')],
            [6, 4],
            bpm=100)
play(result)
```



## 9. 使用musicpy还原Iron Maiden的The Trooper
```python
a11 = translate('D, G, D, a:1/8;., E[l:1/4; i:.], G[l:1/8; i:.] | F#, G, F#, G, a:1/16;., n:1, B[l:1/8; i:.], G[l:1/8; i:.], u:1, G[l:1/8; i:.], E[l:1/8; i:.], E[l:1/4; i:.]') * 2

a12 = translate('D, G, D, a:1/8;., E[l:1/4; i:.]')

a1 = a11 * 2 + a12

a20 = translate('D2, G2, D2, a:1/8;.')

a21 = translate('E2[l:.4; i:.] | E2[l:.16; i:.; r:2], E2[l:.8; i:.], n:1, u:1, r:4, E2[l:.16; i:.; r:2], D2[l:.8; i:.; r:3]')

a22 = translate('C2[l:.4; i:.] | C2[l:.16; i:.; r:2], C2[l:.8; i:.], n:1, u:1, r:4, C2[l:.16; i:.; r:2], D2[l:.8; i:.; r:3]')

a23 = translate('D2, G2, D2, a:1/8;., E2[l:.4; i:.]')

a2 = a20 + a21 * 2 + a22 + a21[:-3] + a23

a3 = drum('C;S;K[l:1/4; i:.], H, S, H, K, H, S, H, K, H, S, H, S[r:3], r:4, C;S;K[l:1/4; i:.]').notes

a41 = translate('F#, G, F#, a:1/8;., G[l:1/4; i:.], B4[l:1/8; i:.] | A, B, A, B, a:1/16;., n:1, D5[l:1/8; i:.], B4[l:1/8; i:.], u:1, B[l:1/8;i:.], G[l:1/8; i:.], G[l:1/4; i:.]') * 2

a42 = translate('F#, G, F#, a:1/8;., G[l:1/4; i:.]')

a4 = a41 * 2 + a42

a4.set_volume(80)

result = P([a1, a2, a3, a4], [31, 34, 1, 31], channels=[0, 1, 9, 2], bpm=165, start_times=[0, 0, 3/8, 0])

play(result)
```



## 10. 带附点节奏的一套鼓点
```python
result = drum('K[l:.8.; i:.], H, S[l:.8.; i:.], H, K[l:.16.; i:.; r:3], S[l:.8.; i:.], H, r:2')
play(result, 165)
```
