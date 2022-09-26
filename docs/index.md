Welcome to Musicpy's documentation!
===================================

Musicpy is a music programming language in Python designed to write music in very handy syntax through music theory and algorithms.

It is easy to learn and write, easy to read, and incorporates a fully computerized music theory system.

Musicpy can do way more than just writing music. This package can also be used to analyze music through music theory logic, and you can design algorithms to explore the endless possibilities of music, all with musicpy.

With musicpy, you can express notes, chords, melodies, rhythms, volumes and other information of a piece of music with a very concise syntax. It can generate music through music theory logic and perform advanced music theory operations.

You can easily output musicpy codes into MIDI file format, and you can also easily read any MIDI files and convert to musicpy's data structures to do a lot of advanced music theory operations. The syntax of musicpy is very concise and flexible, and it makes the codes written in musicpy very human-readable.

### a J-rock intro in musicpy

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

[Click here to hear what this sounds like (Microsoft GS Wavetable Synth)](https://drive.google.com/file/d/1tMKLt3oFdmiGQPTdFVolGvBE1gVGNSwa/view?usp=sharing)

If you think this is too simple, musicpy could also produce music like [this](https://drive.google.com/file/d/1j66Ux0KYMiOW6yHGBidIhwF9zcbDG5W0/view?usp=sharing) within 30 lines of code (could be even shorter if you don't care about readability). Anyway, this is just an example of a very short piece of electronic dance music, and not for complexity.


## Contents
-------------

### Introduction

* [Introduction](https://musicpy.readthedocs.io/en/latest/Introduction/)

### Data structures

* [Data structures of musicpy](https://musicpy.readthedocs.io/en/latest/Data%20Structures%20of%20musicpy/)

### Basic syntax

* [Basic syntax of note type](https://musicpy.readthedocs.io/en/latest/Basic%20syntax%20of%20note%20type/)
* [Basic syntax of chord type](https://musicpy.readthedocs.io/en/latest/Basic%20syntax%20of%20chord%20type/)
* [Basic syntax of scale type](https://musicpy.readthedocs.io/en/latest/Basic%20syntax%20of%20scale%20type/)
* [Basic syntax of piece type](https://musicpy.readthedocs.io/en/latest/Basic%20syntax%20of%20piece%20type/)
* [Basic syntax of track type](https://musicpy.readthedocs.io/en/latest/Basic%20syntax%20of%20track%20type/)
* [Basic syntax of tempo type](https://musicpy.readthedocs.io/en/latest/Basic%20syntax%20of%20tempo%20type/)
* [Basic syntax of pitch_bend type](https://musicpy.readthedocs.io/en/latest/Basic%20syntax%20of%20pitch_bend%20type/)
* [Basic syntax of pan type](https://musicpy.readthedocs.io/en/latest/Basic%20syntax%20of%20pan%20type/)
* [Basic syntax of volume type](https://musicpy.readthedocs.io/en/latest/Basic%20syntax%20of%20volume%20type/)
* [Basic syntax of drum type](https://musicpy.readthedocs.io/en/latest/Basic%20syntax%20of%20drum%20type/)


### Useful functionality

* [Useful functionality](https://musicpy.readthedocs.io/en/latest/Useful%20functionality/)

### Musicpy composition code examples

* [musicpy composition code examples Part 1](https://musicpy.readthedocs.io/en/latest/musicpy%20composition%20code%20examples%20part%201/)
* [musicpy composition code examples Part 2](https://musicpy.readthedocs.io/en/latest/musicpy%20composition%20code%20examples%20part%202/)
* [musicpy composition code examples Part 3](https://musicpy.readthedocs.io/en/latest/musicpy%20composition%20code%20examples%20part%203/)

### Musicpy sampler module

* [musicpy sampler module](https://musicpy.readthedocs.io/en/latest/musicpy%20sampler%20module/)
* [musicpy sampler module composition code examples](https://musicpy.readthedocs.io/en/latest/musicpy%20sampler%20module%20composition%20code%20examples/)

### Algorithms

* [Introduction of musicpy algorithms module](https://musicpy.readthedocs.io/en/latest/Introduction%20of%20musicpy%20algorithms%20module/)
* [the algorithm to split the main melody and chords from a piece of music](https://musicpy.readthedocs.io/en/latest/the%20algorithm%20to%20split%20the%20main%20melody%20and%20chords%20from%20a%20piece%20of%20music/)
* [the algorithm to determine the chord type of any group of notes according to the logic of music theory](https://musicpy.readthedocs.io/en/latest/the%20algorithm%20to%20determine%20the%20chord%20type%20of%20any%20group%20of%20notes%20according%20to%20the%20logic%20of%20music%20theory/)

### Frequently Asked Questions

* [Frequently Asked Questions](https://musicpy.readthedocs.io/en/latest/Frequently%20Asked%20Questions/)
