# Basic syntax of tempo type



## Contents
- [Inserting real-time tempo changes into a piece](#inserting-real-time-tempo-changes-into-a-piece)



## Inserting real-time tempo changes into a piece

You can use tempo types to insert into a chord type, and you can set the exact position where the tempo type is inserted.

It is also possible to directly follow the position of the insertion as the position of the tempo change. Inserting a real-time tempo change allows you to dynamically change the tempo of your piece.

### The composition of tempo type
```python
tempo(bpm, start_time=None, channel=None, track=None)
```
- bpm: the speed of the piece you want to change to
- start_time: the time at which you want to change position, in bars, can be an integer, decimal or fraction, can be unset, default is None. If start_time is not set, the start_time will be the exact position of the chord type tempo is inserted into
- channel: MIDI channel number
- track: MIDI track number

### insert the tempo type into the chord type
Just insert the tempo type as a note type into the chord type.
```python
a = chord(['C5', 'D5', 'E5', tempo(150), 'F5', 'G5', 'A5', 'B5', 'C6']) % (1/8,1/8)
play(a, 80)
# The chord type a will be played at 80 BPM from the beginning to E5, and 150 BPM thereafter
```

### Set start_time to choose where to start the tempo change
```python
a = chord(['C5', 'D5', 'E5', 'F5', 'G5', 'A5', 'B5', tempo(150, 0.5)]) % (1/8,1/8)
play(a, 80)
# Chord type a will play at 80 BPM from the beginning to bar 0.5, and 150 BPM thereafter (starting with bar 0)
```

### Syntax for tempo type construction written in a string
```python
a = chord('C5, D5, E5, tempo;150, F5, G5, A5, B5, C6') % (1/8,1/8)
a = chord('C5, D5, E5, F5, G5, A5, B5, C6, tempo;150;1.5') % (1/8,1/8)
# Note that since the tempo type is not a note type per se, but just a message informing of a change in tempo, in the syntax written in a string
# There is no syntax for setting the length, interval, etc. of the note type using parentheses, e.g. tempo;150(.8;.) This will not work.
```

