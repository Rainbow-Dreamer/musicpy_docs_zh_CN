# musicpy composition code examples part 1

## 1. Touhou Project main theme
```python
play((getchord_by_interval('D#4', [5,7,10,7,5], 1/2, 1/8)*3 + getchord_by_interval('F4', [1,0,-4], 1/2, 1/8)) * 3, 150)
```
## 2. Play the A minor seventh chord followed by a rest half beat 4 times, then the B minor seventh chord followed by a rest half beat 4 times, then the C major seventh chord breaking up the chord 1 time.
(Both of these examples are written without using progressions, using progressions makes the musicpy language look more compact and shorter)
```python
a = C('Am7') % (1/8,0)
play(a%4 | a.up(2)%4 | chd(a[0].up(3), 'maj7').set(interval=1/8), bpm=80)
```

## 3. A short piano piece
(The 3rd example uses a lot of progressions, the code looks much more compact, and the use of the separator '|' for progressions that connect two chord types can also have a similar feel to bar lines, which can enhance the readability of the code. (In addition, the chord separation '|' can also be replaced by '//', depending on your preference.)
```python
a = C('Dmaj7') % (1/4,1/4) | C('Cmaj7') | C('Fadd9',3) | C('D#maj7',4) | (C('Dmaj7',3)/-2) % (5/4,)
b = chord(['F#5','G5','A5','B5','G5'], 1/8,1/8)   
play(a & b, 140, instrument=1)
```

## 4. chromatic downward alternating major seventh and minor seventh chords
```python
a = C('Amaj7')@2%4 | C('G#m7')@2%4 | C('Gmaj7')@2%4 | C('F#m7')@2%4    
play(a | a % (1/4,1/4), 165, instrument=9)
```

## 5. a piece of music with a scary atmosphere, orchestral sound
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

## 6. very nice 6451 chord configuration, harp tone
```python
q = scale('C4', 'major')
r = q%(6451, 0.5/4, 0.3/4, 5)
r = [i.omit(5).inversion_highest(3) for i in r]
play(r*2, bpm=80, instrument=47)
```

## 7. Another great sounding 6451 chord configuration, harp tone
``` python
q = scale('C4', 'major')
r = q%(6451, 0.5/4, 0.3/4, 5)
r = [i.omit(7).inversion_highest(2) for i in r]
play(r*2, bpm=80, instrument=47)
```

## 8. scary and eerie soundtrack, instruments used: steel-string acoustic guitar and orchestra
```python
a = C('Baug9', 4) @ [2, 3, 4, 1.1, 5, 1.1, 4, 3] % (1/8,1/8)
b = C('BmM9', 4) @ [2, 3, 4, 1.1, 5, 2.1, 5, 1.1] % (1/8,1/8)
part1 = (a + b)*2
part2 = (C('Baug9',4, 1, 0) | C('BmM9',4, 1, 0)) % 2
part2.setvolume(80)
play(P([part1, part2], [26,49], 100, [0,0]))
```

## 9. very nice 6451 chord configuration, electric piano sound
```python
q = scale('C4', 'major')
r = q%(64516458, 1/2, 0.3/4, 5)
r = [i.omit(7).inversion_highest(2) for i in r]
play(r*2, bpm=80, instrument=5)
```

## 10. 80s hard rock or pop metal style, instruments used: piano, synthesizer tone, electric bass
``` python
a = chord('C2, G1, C2, Ab1, Bb1, G1', [15/8,1/8,2,2,1.5,3/4], [15/8,1/8,2,2,1.5,3/4])
b = C('Cm', 5, 1/8) | 1/4 | C('Bb', 4, 1/8) | 1/4 | C('Ab', 4, 1/8) | 1/4 | C('Bb', 4, 1/2) | 3/8
b2 = C('Cm', 5, 1/8) | 1/4 | C('Bb', 4, 1/8) | 1/4 | C('Abmaj7', 4, 1/8) | 1/4 | C('Gsus', 4, 3/4) | 1/8
c = (b % 3) | b2
a2 = chord('C2',1/8,1/8)*32 + chord('Ab1',1/8,1/8)*16 + chord('Bb1',1/8,1/8)*12 + chord('G1',1/8,1/8)*4
play(piece([a, c, a2%2, c%2], [1, 81, 34, 81], 130, [0, 1/4, 8, 33/4]))
```

## 11. horror ambient music, orchestral tones
```python
a = chd('B4','maj9').sort([2,3,4,1,5])
b = chd('B4','maj9').sort([2,3,4,1,5,2])
q = a + a[1:-1].reverse_chord()
q2 = b + b[3:-1].reverse_chord()
play((q.set(1/8,1/8) + q2.set(1/8,1/8))*2, 100, instrument=50)
```
