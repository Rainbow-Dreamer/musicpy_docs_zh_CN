# Basic syntax of track type

## Contents

- [The introduction of track type](#the-introduction-of-track-type)
- [Some new syntax for track type](#some-new-syntax-for-track-type)



## The introduction of track type

The piece type can contain several different tracks. The new track types currently added represent the information contained in each track, including chord type, instrument type, start time, tempo (bpm), pan and volume, etc.

### The composition of track type

```python
track(content,
      instrument=1,
      start_time=0,
      channel=None,
      track_name=None,
      pan=None,
      volume=None,
      bpm=120,
      name=None,
      sampler_channel=None)
```

- content: chord type, the content of the track type
- instrument: the instrument type
- start_time: the start time of the track
- channel: the channel number
- track_name: the name of the track
- pan: the pan of the track
- volume: the volume of the track
- bpm: the tempo (BPM)
- name: the name of the piece the track belongs to
- sampler_channel: the sampler channel number the track corresponds to in sampler module


The `__getitem__` built-in function of the piece type (that is, the syntax of `a[index]`, where a is the track type and index is the index of the track, with 0 being the first track) now returns the track type (track type). It has the same complete information as the piece type and can be used to extract type information.  
In the build function, the track type can now also be passed as a parameter to build the track type.  
Now when displaying the piece type and track type, it will show at the beginning whether the currently displayed music type is a piece type or a track type, as follows: `[piece]` for the piece type and `[track]` for the track type.

```python
A1, B1, C1, D1 = [C('C') for i in range(4)]
a = piece([A1, B1, C1, D1], [1, 47, 35, 2], 150, [0, 2, 2, 8], name='demo') # a is a piece type (track type)
>>> print(a)
[piece] demo
BPM: 150
track 1 | instrument: Acoustic Grand Piano | start time: 0 | [C4, E4, G4] with interval [0, 0, 0]
track 2 | instrument: Orchestral Harp | start time: 2 | [C4, E4, G4] with interval [0, 0, 0]
track 3 | instrument: Electric Bass (pick) | start time: 2 | [C4, E4, G4] with interval [0, 0, 0]
track 4 | instrument: Bright Acoustic Piano | start time: 8 | [C4, E4, G4] with interval [0, 0, 0]

>>> print(a[0]) # Print out the first track
[track] demo
BPM: 150
instrument: Acoustic Grand Piano | start time: 0 | [C4, E4, G4] with interval [0, 0, 0]

c = build(track(A1, 1, 0),
          track(B1, 1, 2),
          track(C1, 47, 2),
          track(D1, 35, 8),
          bpm=150,
          name='demo') # Use the build function to build the piece type, you can pass any number of track types as parameters

b = track(C('D'), 5, 12)
a.append(b) # Track types can be added to a track type using the track type's built-in function append. this line adds track type b to track type a
>>> print(a)
[piece] demo
BPM: 150
track 1 | instrument: Acoustic Grand Piano | start time: 0 | [C4, E4, G4] with interval [0, 0, 0]
track 2 | instrument: Orchestral Harp | start time: 2 | [C4, E4, G4] with interval [0, 0, 0]
track 3 | instrument: Electric Bass (pick) | start time: 2 | [C4, E4, G4] with interval [0, 0, 0]
track 4 | instrument: Bright Acoustic Piano | start time: 8 | [C4, E4, G4] with interval [0, 0, 0]
track 5 | instrument: Electric Piano 1 | start time: 12 | [D4, F#4, A4] with interval [0, 0, 0]

# The track type can also be played separately, also using the play function, if the track type has a set tempo.
# then use the tempo of the track type, otherwise set the tempo according to the bpm parameter of the play function
play(b)
b.play()
```

A track type is a type that can store information about a track individually, and can be added to a piece type, and a piece type is composed of several different tracks, although the data structure is not implemented as a collection of track types, but as a summary of the same kind of information for each of the multiple tracks, but I have implemented a track type that can be added to a music function, and a way to extract any track from a track type and rebuild it into a track type, and the track types themselves can be played separately. The build function allows you to pass any number of track types to be built into a track type.

The track types represent the information contained in each track, including chord type, instrument type, start time, tempo (bpm), pan and volume, etc.

## Some new syntax for track type

The syntax for `a + i`, `a - i`, `a | i`, `a & i`, `a % i` in the chord type has now been added to the track type as well, see the usage in the chord type.

