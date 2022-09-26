# musicpy composition code examples part 2

## 1. Non-functional harmony game bgm (normal speed, 165BPM)
```python
a = C('Cm7',4,1,1/8) @ [1,3,4,2.1,4.1] | 3/8
piano = a % 4 | (a - 3) % 4
bass = (chord('C2, C2, G2, C2, A#2, C2, C3, C2') % 4) % (1/8, 1/8)
bass += (bass - 3)
play(P([piano, bass], [1, 34], 165, [0, 0]) * 2)
```

## 2. Non-functional harmonic game bgm (speedcore, 500BPM)
```python
a = C('Cm9',4,1,1/8) @ [1,3,5,2.1,4.1] | 3/8
piano = a % 4 | (a + 3) % 4
bass = (chord('C2, C2, G2, C2, A#2, C2, C3, C2') % 4) % (1/8, 1/8)
bass += (bass + 3)
import random
drum = concat([chord(random.choice(['D2','E2','G2'])) for i in range(64)]) % (1/8, 1/8)
play(P([piano, bass, drum], [1, 34, 1], 500, [0, 0, 0], channels=[1,2,9]) * 2)
```

## 3. Non-functional harmony game bgm (doom metal, 25BPM)
```python
a = C('Cm9',2,1/4,1/8) @ [1,3,5,2.1,4.1] | 3/8
piano = a % 4 | (a + 3) % 4
bass = (chord('C2, C2, G2, C2, A#2, C2, C3, C2') % 4) % (1/8, 1/8)
bass += (bass + 3)
import random
drum = concat([chord(random.choice(['D2','E2','G2'])) for i in range(64)]) % (1/8, 1/8)
play(P([piano, bass, drum], [31, 34, 1], 25, [0, 0, 0], channels=[1,2,9]) * 8)
```

## 4. Scary atmosphere soundtrack
```python
a = C('Caug9') @ [1,3,5,2.1,4.1,5.1] % (1, 1/8) | 1/4
bass = chord('C2, A#1') % ([1, 1], [1, 1]) * 4
piano = (a | a - 2) * 4
string = C('Caug9', 4) % (1, 0)
string |= string - 2
string.setvolume(60)
play(P([piano, bass, string%4], [9, 34, 49], 80, [0, 0, 0]))
```

## 5. Guitar breakup chord play (nylon string)
```python
guitar = C('CM7',4, 1)^2 | C('G7sus', 3, 1)^2 | C('A7sus', 3, 1)^2 | C('FM7', 3, 1)^2
play((guitar%(1/4, 1/8) * 2)-octave, 100, instrument=25)
```

## 6. Guitar breakup chords to play another chord progression (nylon string)
```python
guitar = (C('CM7',4, 1/4, 1/8)^2 | C('G7sus', 3, 1/4, 1/8)^2 
| C('A7sus', 3, 1/4, 1/8)^2 | C('Em7', 3, 1/4, 1/8)^2 | 
C('FM7', 3, 1/4, 1/8)^2 | C('CM7', 4, 1/4, 1/8)@1 |
C('AbM7', 3, 1/4, 1/8)^2 | C('G7sus', 3, 1/4, 1/8)^2)
play((guitar * 2)-octave, 100, instrument=25)
```

## 7. Electric pianos play chord progressions, playing intra-chord notes according to certain rules
```python
rule = lambda x: x @ [1, 3, 4, 2.1, 3.1, 4, 1.1, 3.1] % (1/4, 1/8)
rule2 = lambda x: x @ [1, 3, 4, 2.1, 3.1, 4, 2.1, 3.1] % (1/4, 1/8)
a = (rule(C('Cmaj7',4)) |
     rule(C('Fmaj7',3)) |
     rule2(C('G7, b9, omit 5',3)) |
     rule(C('Bdim7',3)) |
     rule(C('Am7', 3)) |
     rule2(C('D7', 3) ^ 2) |
     rule2(C('FmM7', 3) ^ 2) |
     rule2(C('G7,#5', 3)))
play(a * 2, 150, instrument=5)
```

## 8. Chord progressions in major keys borrowed from the lydian mode
```python
a = (C('Cmaj7') | C('D7') | C('Fmaj7',3) | C('Cmaj7/-3')) % (1, 1/4) * 4
b_duration = [3/4, 1/8, 1/8, 3/4, 1/8, 1/8, 1/4, 1/4, 1/2, 1/2, 1/2]
b_interval = b_duration
b = (chord('G5, F5, E5, F5, E5, D5, E5, D5, C5, B4, G4')
        % (b_duration, b_interval)) * 2
song = build([a, 5, 0],
             [b, 49, 4],
             bpm=165)
play(song)
```

## 9. Chord progressions in major keys borrowed from lydian modes (with accompanying harmonies, second melodic line)
```python
a = (C('Cmaj7') | C('D7') | C('Fmaj7',3) | C('Cmaj7/-3')) % (1, 1/4) * 4
b_duration = [3/4, 1/8, 1/8, 3/4, 1/8, 1/8, 1/4, 1/4, 1/2, 1/2, 1/2]
b_interval = b_duration
b = (chord('G5, F5, E5, F5, E5, D5, E5, D5, C5, B4, G4')
        % (b_duration, b_interval))
cmajor = scale('C5', 'major')
b_harmony = (chord([cmajor(cmajor.names().
             index(i.name))[1]-octave for i in b])
                % (b_duration, b_interval))
b_harmony.setvolume(80)
b &= b_harmony
b *= 2
song = build([a, 5, 0],
             [b, 49, 4],
             bpm=165)

play(song)
```

## 10. Advanced writeup of example 8 (a more concise writeup, a new advanced syntax I recently added)
```python
a = (C('Cmaj7') | C('D7') | C('Fmaj7',3) | C('Cmaj7/-3')) % (1, 1/4) * 4
b = chord('G5[3/4;3/4], F5[.8;.8], E5[.8;.8], F5[3/4;3/4], E5[.8;.8], D5[.8;.8], E5[.4;.4], D5[.4;.4], C5[.2;.2], B4[.2;.2], G4[.2;.2]') * 2
song = build([a, 5, 0],
             [b, 49, 4],
             bpm=165)
play(song)
```

## 11. Relaxing ambient music
```python
a = C('CM9') @ [1,3,5,2.1,4.1,5.1] % (1, 1/8) | 1/4
bass = chord('C2, A#1') % ([1, 1], [1, 1]) * 4
piano = (a | a - 2) * 4
string = C('CM9', 4) % (1, 0)
string |= string - 2
string.setvolume(50)
play(P([piano, bass, string%4], [9, 34, 49], 80, [0, 0, 0]))
```

## 12. Super nice 6451 chord configuration (advanced writing)
```python
q = S('C major', 4)
r = q%(64516458, 1/2, 0.3/4, 5)
r = [i('omit7')^2 for i in r]
play(r*2, bpm=80, instrument=5)
```

## 13. Relaxed urban style ambient music (with transposition)
```python
a = C('CM9') @ [1,3,5,2.1,4.1,5.1] % (1, 1/8) | 1/4
bass = chord('C2, A#1') % ([1, 1], [1, 1]) * 4
piano = (a | a - 2) * 4
string = C('CM9', 4) % (1, 0)
string |= string - 2
string.setvolume(50)
string %= 4
oboe1 = chord('B5[.16;.16], C6[.16;.16], D6[1;1], C6[.2;.2], E6[.2;.2], D6[1;1], C6[7/8; 7/8]')
part1 = P([piano | piano-4, bass | bass-4, string | string-4, oboe1 | oboe1-4], 
          [9, 34, 49, 'Pan Flute'], 
          90,
          [0, 0, 0, 3+7/8])
play(part1)
```
## 14. Video game 8-bit song style
```python
chord_part1 = (C('Cm', 5, 1/8) | 1/4 | C('Bb', 4, 1/8) | 1/4 |
               C('Ab', 4, 1/8) | 1/4 | C('Bb', 4, 1/2) | 3/8)
chord_part2 = (C('Cm', 5, 1/8) | 1/4 | C('Bb', 4, 1/8) | 1/4 |
               C('Abmaj7', 4, 1/8) | 1/4 | C('Gsus', 4, 5/8) | 1/4)
chord_part3 = (C('Cm', 5, 1/8) | 1/4 | C('Bb', 4, 1/8) | 1/4 |
               C('Gm', 4, 1/8) | 1/4 | C('Gm', 4, 1/8) |
               chord('Eb5, Bb5, Ab5, G5',1/8,1/8) | 1/4)
chord_part4 = (C('Cm', 5, 1/8) | 1/4 | C('Bb', 4, 1/8) | 1/4 |
               C('Ab', 4, 1/8) | 1/4 | chord('Bb4[1/8;0], D5[3/8;0], \
               F5[3/8;1/8], Bb4[1/4;.], D5[1/4;.]') |
               C('Csus',5,1)@2 | C('C',5,1)@2)
chord_part = (chord_part1 % 3 | chord_part2 | (chord_part1 % 3) |
              chord_part2 | chord_part1 % 3 | chord_part3 | chord_part4)
bass_part1 = chord('C2[15/8;.], G1[1/8;.], C2[2;.], Ab1[2;.], Bb1[1.5;.], G1[1/2;.]')
bass_part2 = (chord('C2')*32 + chord('Ab1')*16 + chord('Bb1')*12 +
              chord('G1')*4) % (1/8,1/8)
bass_part3 = (chord('Ab1')*8 + chord('Bb1')*8 + chord('C2')*8 +
              chord('D2')*4 + chord('Eb2')*4) % (1/8,1/8)
bass_part4 = (chord('Ab1')*8 + chord('Bb1')*8 + chord('G2')*8 +
              chord('C2,C2,C2,D2') + chord('Eb2')*4) % (1/8,1/8)
bass_part5 = (chord('Ab1')*8 + chord('Bb1')*8 + chord('C2')*8 +
              C('Csus',2)@[1,2,3,1,1.1,2,3,1]) % (1/8,1/8)
bass_part = bass_part1 | bass_part2 | bass_part3 | bass_part4 | bass_part5
play(piece([bass_part, chord_part], [81, 81], 130, [0, 1/4]))
```

## 15. J-rock intro
```python
guitar = ((C('Cmaj7')@1)@[1,2,3,4,1,2,3,2] |
(C('Fmaj7',3)^2)@[1,2,3,4,1,2,3,2] |
(C('G7sus',3)^2)@[1,2,3,4,1,2,3,2] |
C('Am11',3)@[1,2,4,6,1,4,6,4] |
(C('Cmaj7')@1)@[1,2,3,4,1,2,3,2] |
C('Fadd9',3).up(2,1)@[1,2,3,4,1,2,3,2] |
(C('G7sus',3)^2)@[1,2,3,4,1,2,3,2] |
C('Am11',3)@[1,2,4,6,1,4,6,4]) % (1/2,1/8)
bass1 = chord('D2[.8;.],E2[.8;.],D2[.8;.],E2[1;.], F2[1;.], G2[1;.], A1[.2;.], A2[.8;.], G2[.8;.], E2[.8;.], D2[.8;.]')
bass2 = (chord('E2')*8 + chord('F1')*8 + chord('G1')*8 + chord('A1,A1,E2,A1,A2,A1,G2,D2')) % (1/8,1/8) % 4
bass = bass1 | bass2
string1 = chord('B5[1;.],C6[1;.],D6[.2;.],E6[.2;.],\
F6[.2;.],E6[.2;.],C6[1;.],B5[.2;.],C6[.2;.],G5[1;.],\
F5[.2;.],E5[.2;.],C5[1;.],D5[.4;.],E5[.4;.],F5[.4;.],\
E5[.4;.],D5[.2;.],G5[.2;.],E5[1;.]')
play(piece([guitar%3, bass, string1], [28, 34, 49], 135, [0, 4-3/8, 12]))
```
