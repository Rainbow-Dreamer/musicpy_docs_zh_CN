# musicpy composition code examples part 3

## 1. Rolling Star - Yui in musicpy
```python
bass11 = translate('B1(8)[.8;.],D2(8)[.8;.],A1(8)[.8;.],G1(8)[.8;.]')
bass12 = translate('G1(6)[.8;.], A1(2)[.8;.]')
bass1 = bass11 % 2 | bass12

guitar11 = translate('B3[.4;.],D4[.8;.],E4[.4.;.],D4[.8;.],E4[.8;.]')
guitar12 = guitar11.down(2, 0)
guitar13 = guitar12.down(1, [1, 3])
guitar14 = translate('G3[.4;.],B3[.8;.],A4[5/8;.]')
guitar1 = (guitar11|guitar12|guitar13|guitar14) % 2

drum11 = drum('8;0,1,2,1,{1},0,1,2,1,{5},8;0,1;2(2),5,4,2;4(3)').notes
drum12 = drum('8;0;1;2,1;2(2),5,4,2;4(3)').notes
drum1 = drum11 % 2 | drum12

synth11 = translate('D5[.8;.],B5[.8;.],{16}')
synth1 = synth11

result = P([bass1, guitar1, drum1, synth1],
           [34, 31, 1, 91],
           bpm=165,
           channels=[0,1,9,2],
           start_times=[0,0,0,4])

play(result)
```

## 2. Ethereal piano soundtrack
```python
a = (C('G/C',3) @ [1,2,3,2,1.1,2,3,2]) % (2,1/8) | (2, 1)
result = (a | 1 | a - 2) + octave
result.other_messages = [
controller_event(controller_number=10, parameter=0, time=0),
controller_event(controller_number=10, parameter=127, time=2),
controller_event(controller_number=10, parameter=64, time=4),
]
play(result, 165)
```

## 3. background strings with some JRPG feels
```python
w = chord_progression([C('A',3)^2, C('A/G',3)^2, C('A/F#',3)^2, C('Fmaj7',3)^2, C('F/E'), C('Fm/Eb')^2, C('Asus/D').on('A1')], durations=1)
w2 = chord('A2,G2,F#2,F2,E2,Eb2,A1') % (1, 1)
play(w & w2, 150, i=49)
```

## 4. Piano arpeggios with melancholy emotions
```python
w = (C('Cmadd9,add11',5)@[1,3,1.1,4,2.1,5,2.1,4]%(1/2,1/8)%2 |
C('A#add9,add11',4)@[1,3,1.1,4,2.1,5,2.1,4]%(1/2,1/8)%2 |
C('G#add9',4)@[1,3,1.1,4,2.1,4,1.1,3]%(1/2,1/8)%2 |
C('A#add9',4)@[1,3,1.1,4,2.1,4,1.1,3]%(1/2,1/8)%2)

w2 = translate('C2;C3[2], [2], A#1;A#2[2], [2], G#1;G#2[2], [2], A#1;A#2[2]')
play(w | (w&w2), 265)
```

## 5. The opening part of Piano Nocturne
```python
w1 = ((C('Cmadd2') + 'C5')%(1/2,1/8) | 3/8 |
(C('Cmadd2') + 'C5')%(1/2,1/8) | 3/8 |
chord('F5,D#5,D5,D#5,C5')%(1/2,1/8))
w2 = (C('G#add9',3)@[1,3,1.1,4,2.1]%(1/2,1/8) | 3/8 |
C('A#add9',3)@[1,3,1.1,4,2.1]%(1/2,1/8) | 3/8 |
(C('Cmadd9,add11',3)@[1,3,1.1,4,2.1,5,2.1,4]%(1/2,1/8))%2)
play((w1 & (w2,1/2)) | (2,-7/8), 90)
```

## 6. music with a little suspenseful atmosphere
```python
a = chd('bb3', 'm11')%(1/2,1/8)@[1,3,5,4.1,2.2,6.1,5.1,4.1]
b = chd('g3', 'M9#11')%(1/2,1/8)@[1,3,5,4.1,2.2,6.1,5.1,4.1]
c = chd('gb3', '13sus')%(1/2,1/8)@[1,3,4,5,2.1,6,4.1,5.1]
play(a%4 | (a-2)%2 | b | c, 135, i=5)

# another version
play((a%4 | (a-2)%2 | b | c)%2, 100, i=49)
```

## 7. another version of J-rock intro
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
bass2 = (chord('E2')*8 + chord('F1')*8 + chord('G1')*8 + chord('A1,A1,E2,A1,A2,A1,G2,D2')) % (1/8,1/8) % 2
bass3 = (chord('A2')*8 + chord('E2')*8 + chord('F2')*8 + chord('C2,C2,G2,C2,C3,C2,G2,C2')) % (1/8,1/8) % 2
bass = bass1 | bass2 | bass3
rhythm_guitar = (C('A5(+octave)',2, 1) | C('E5(+octave)',2, 1) | C('F5(+octave)',2, 1) | C('C5(+octave)',3, 1/2)| C('G5(+octave)',2, 1/2))%2
string1 = chord('B5[1;.],C6[1;.],D6[.2;.],E6[.2;.],\
F6[.2;.],E6[.2;.],C6[1;.],B5[.2;.],C6[.2;.],G5[1;.],\
F5[.2;.],E5[.2;.],C5[1;.],D5[.4;.],E5[.4;.],F5[.4;.],\
E5[.4;.],D5[.2;.],G5[.2;.],E5[1;.]')
drum1 = chord('C2,F#2,E2,F#2,C2,F#2,E2,F#2,C2,F#2,A#2,E2,F#2,C2,F#2,E2,F#2')%(1/8,1/8)
drum1.interval[10] = 0
drum1.setvolume(112)
result = piece([guitar%2 | guitar2, bass, string1, rhythm_guitar,drum1%4 + 2],
               [2, 34, 49, 31, 1],
               bpm=165,
               start_times=[0, 4-3/8, 12, 16, 16],
               channels=[0,1,2,3,9])
play(result-2)
```

## 8. Relaxing light music, ambient music
```python
result = P([arp('C', second_half=True, intervals=1/8).cut(0, 2)|
            arp('D/F#', second_half=True, intervals=1/8).cut(0, 2),
            translate('A2[1](2),[1],D3[1](2)')],
            [6, 4],
            bpm=100)
play(result)
```

## 9. The Trooper - Iron Maiden in musicpy
```python
a11 = translate('D,G,D,{!1/8;.},E[1/4;.],G[1/8;.],{},F#,G,F#,G,{!1/16;.|$1},\
B[1/8;.],G[1/8;.],$1,G[1/8;.],E[1/8;.],E[1/4;.]') % 2

a12 = translate('D,G,D,{!1/8;.},E[1/4;.]')

a1 = a11 * 2 + a12

a20 = translate('D2,G2,D2,{!1/8;.}')

a21 = translate('E2[.4;.],{},E2(2)[.16;.],E2[.8;.],{$1},$1(4),E2(2)[.16;.],D2(3)[.8;.]')

a22 = translate('C2[.4;.],{},C2(2)[.16;.],C2[.8;.],{$1},$1(4),C2(2)[.16;.],D2(3)[.8;.]')

a23 = translate('D2,G2,D2,{!1/8;.},E2[.4;.]')

a2 = a20 + a21 * 2 + a22 + a21[:-3] + a23

a3 = drum('8;2;0[1/4;.],1,2,1,0,1,2,1,0,1,2,1,2(3),{4},8;2;0[1/4;.]').notes

a41 = translate('F#,G,F#,{!1/8;.},G[1/4;.],B4[1/8;.],{},A,B,A,B,{!1/16;.|$1},\
D5[1/8;.],B4[1/8;.],$1,B[1/8;.],G[1/8;.],G[1/4;.]') % 2

a42 = translate('F#,G,F#,{!1/8;.},G[1/4;.]')

a4 = a41 * 2 + a42

a4.setvolume(80)

result = P([a1, a2, a3, a4], [31, 34, 1, 31], channels=[0, 1, 9, 2], bpm=165, start_times=[0, 0, 3/8, 0])

play(result)
```

## 10. A set of drums with dotted notes
```python
result = drum('0[.8.;.], 1, 2[.8.;.], 1, 0(3)[.16.;.], 2[.8.;.], 1, {2}')
play(result, 165)
```
