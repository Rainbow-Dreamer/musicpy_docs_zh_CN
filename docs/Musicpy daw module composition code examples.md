# Musicpy宿主模块作曲代码示例

使用musicpy的宿主模块，你可以制作更加真实的音乐，也可以不仅限于MIDI文件，可以读取和输出mp3, ogg, wav等音频文件，加载音频采样，SoundFont音源文件等作为宿主的音轨上的音源。这个章节是一些使用musicpy的宿主模块作曲的代码示例。

**请注意，这些示例里加载的音源文件，有些是SoundFont文件，因为可能的版权问题，在这里不提供下载，可以到网上按照名字找这些SoundFont文件。在每个示例的最后都会提供这些代码示例生成的音乐文件的试听链接。**



## 1. 一小段电子舞曲
```python
from musicpy.daw import *

current_daw = daw(3)
current_daw.load(0, 'EMU II ACOUSTIC GUITAR.sf2')
current_daw.load(1, 'Arachno.sf2')

bass1 = chord('A1') % (1,) * 4
bass2 = (chord('A1, A2') % (1/16, 1/8) * 4 |
         chord('G1, G2') % (1/16, 1/8) * 4 |
         chord('F1, F2') % (1/16, 1/8) * 4 |
         chord('D1, D2') % (1/16, 1/8) * 4)

guitar1 = (C('Am/A', 3) @ [1,2,3,4,2,3,4,3] % (1, 1/8) |
C('G/A', 3) @ [1,2,3,4,2,3,4,3] % (1, 1/8) |
C('F/A', 3) @ [1,3,1.1,2.1,3,1.1,2.1,1.1] % (1, 1/8) |
C('Dm/A', 3) @ [1,3,1.1,2.1,3,1.1,2.1,1.1] % (1, 1/8))

guitar2 = (C('Am', 4) @ [2,3,1.1,2.1,3,1.1,2.1,1.1] % (1, 1/8) |
C('G', 4) @ [2,3,1.1,2.1,3,1.1,2.1,1.1] % (1, 1/8) |
C('F', 4) @ [2,3,1.1,2.1,3,1.1,2.1,1.1] % (1, 1/8) |
C('Dm', 4) @ [2,3,1.1,2.1,3,1.1,2.1,1.1] % (1, 1/8))

drum1 = drum('S[l:.8; i:.; r:4], S[l:.16; i:.], S[l:.8; i:.], S[l:.16; i:.], S[l:.8; i:.], S[l:.8; i:.]').notes
drum2 = drum('H, a:.16;., r:16').notes
drum1 &= drum2
drum3 = drum('K, H, S, H, K[l:.16; i:.], K[l:.8; i:.], H[l:.16; i:.], S[l:.8; i:.], H[l:.8; i:.]').notes

synth_pad1 = chord('E5, G5, C6') % (1,) | chord('E5, B5, D6') % (1,) | chord('E5, A5, C6') % (1,) | chord('D5, F5, A5') % (1,)
synth_pad1.set_volume(80)

bass_part = (bass1 * 2 | bass2 * 4) + 3
guitar_part = (guitar1 * 2 | guitar2 * 4) + 3
drum_part = (drum1 * 4 | drum3 * 16)
synth_pad_part = (synth_pad1 * 4) + 3

result = P(tracks=[bass_part, guitar_part, drum_part, synth_pad_part],
           instruments=[34, 3, 1, 51],
           channels=[0, 1, 9, 2],
           daw_channels=[1, 0, 1, 1],
           start_times=[0, 0, 4, 8],
           bpm=165)
current_daw.play(result)
```
[点击这里试听](https://drive.google.com/file/d/1j66Ux0KYMiOW6yHGBidIhwF9zcbDG5W0/view?usp=sharing)



## 2. 使用musicpy还原一小段Iron Maiden的The Trooper
```python
from musicpy.daw import *

current_daw = daw(3)
current_daw.load(0, 'Arachno.sf2')

guitar11 = translate('D, G, D, a:1/8;., E[l:1/4; i:.], G[l:1/8; i:.] | F#, G, F#, G, a:1/16;., n:1, B[l:1/8; i:.], G[l:1/8; i:.], u:1, G[l:1/8; i:.], E[l:1/8; i:.], E[l:1/4; i:.]') * 2

guitar12 = translate('D, G, D, a:1/8;., E[l:1/4; i:.]')

guitar1_part = guitar11 * 2 + guitar12

bass1 = translate('D2, G2, D2, a:1/8;.')

bass2 = translate('E2[l:.4; i:.] | E2[l:.16; i:.; r:2], E2[l:.8; i:.], n:1, u:1, r:4, E2[l:.16; i:.; r:2], D2[l:.8; i:.; r:3]')

bass3 = translate('C2[l:.4; i:.] | C2[l:.16; i:.; r:2], C2[l:.8; i:.], n:1, u:1, r:4, C2[l:.16; i:.; r:2], D2[l:.8; i:.; r:3]')

bass4 = translate('D2, G2, D2, a:1/8;., E2[l:.4; i:.]')

bass_part = bass1 + bass2 * 2 + bass3 + bass2[:-3] + bass4

drum_part = drum('C;S;K[l:1/4; i:.], H, S, H, K, H, S, H, K, H, S, H, S[r:3], r:4, C;S;K[l:1/4; i:.]').notes

guitar21 = translate('F#, G, F#, a:1/8;., G[l:1/4; i:.], B4[l:1/8; i:.] | A, B, A, B, a:1/16;., n:1, D5[l:1/8; i:.], B4[l:1/8; i:.], u:1, B[l:1/8;i:.], G[l:1/8; i:.], G[l:1/4; i:.]') * 2

guitar22 = translate('F#, G, F#, a:1/8;., G[l:1/4; i:.]')

guitar2_part = guitar21 * 2 + guitar22

guitar2_part.set_volume(80)

result = P(tracks=[guitar1_part, bass_part, drum_part, guitar2_part],
           instruments=[31, 34, 1, 31],
           channels=[0, 1, 9, 2],
           daw_channels=[0, 0, 0, 0],
           start_times=[0, 0, 3/8, 0],
           bpm=165)
current_daw.play(result)
```
[点击这里试听](https://drive.google.com/file/d/1lspnOVY4GGQGQTkV8j-yOA581hESkD8-/view?usp=sharing)



## 3. 使用musicpy还原一小段Rolling Star - Yui
```python
from musicpy.daw import *

current_daw = daw(3)
current_daw.load(0, 'Arachno.sf2')

bass11 = translate('B1[l:.8; i:.; r:8], D2[l:.8; i:.; r:8], A1[l:.8; i:.; r:8], G1[l:.8; i:.; r:8]')
bass12 = translate('G1[l:.8; i:.; r:6], A1[l:.8; i:.; r:2]')
bass1 = bass11 * 2 | bass12

guitar11 = translate('B3[l:.4; i:.], D4[l:.8; i:.], E4[l:3/8; i:.], D4[l:.8; i:.], E4[l:.8; i:.]')
guitar12 = guitar11.down(2, 0)
guitar13 = guitar12.down(1, [1, 3])
guitar14 = translate('G3[l:.4; i:.], B3[l:.8; i:.], A4[l:5/8; i:.]')
guitar1 = (guitar11|guitar12|guitar13|guitar14) * 2

rhythm_guitar11 = C('B5(+octave)',2) % (1/8, 0) * 8
rhythm_guitar12 = (rhythm_guitar11 | rhythm_guitar11 + 3 |
                   rhythm_guitar11 - 2 | rhythm_guitar11 - 4)
rhythm_guitar13 = C('G5(+octave)',2) % (1/8, 0) * 5 | C('A5(+octave)',2) % (1/8, 0) * 3
rhythm_guitar = rhythm_guitar12 * 2 | rhythm_guitar13
rhythm_guitar.set_volume(70)

drum11 = drum('C;K, H, S, H | K, H, S, H, r:5, C;K, H;S[r:2], PH, OH, S;OH[r:3]').notes
drum12 = drum('C;K;H;S, H;S[r:2], PH, OH, S;OH[r:3]').notes
drum1 = drum11 * 2 | drum12

synth11 = translate('D5[l:.8; i:.], B5[l:.8; i:.], r:16')
synth1 = synth11

result = P(tracks=[bass1, guitar1, rhythm_guitar, drum1, synth1],
           instruments=[34, 31, 28, 1, 91],
           channels=[0, 1, 2, 9, 3],
           daw_channels=[0, 0, 0, 0, 0],
           start_times=[0, 0, 0, 0, 4],
           bpm=165)

current_daw.play(result)
```
[点击这里试听](https://drive.google.com/file/d/1vWXdNa232J500rlYxlziKwMA75x5SElS/view?usp=sharing)



## 4. 有点神秘空灵的电钢琴配乐
```python
from musicpy.daw import *

current_daw = daw(3)
current_daw.load(0, 'Arachno.sf2')
current_daw.instruments(0) < 'Electric Piano'

part1 = get_chord('bb2', 'm11')%(1/2,1/8)@[1,3,5,4.1,2.2,6.1,5.1,4.1]
part2 = get_chord('g2', 'M9#11')%(1/2,1/8)@[1,3,5,4.1,2.2,6.1,5.1,4.1]
part3 = get_chord('gb2', '13sus')%(1/2,1/8)@[1,3,4,5,2.1,6,4.1,5.1]
result = (part1 * 4 | (part1-2) * 2 | part2 | part3) * 2

current_daw.play(result, bpm=100)
```
[点击这里试听](https://drive.google.com/file/d/14hp-y_n-GqlI6ZGSPDBRL1Vt9cxLjpuv/view?usp=sharing)
