# Basic syntax of pan type



## Contents

- [Adjusting the left and right mix position of a track](#adjusting-the-left-and-right-mix-position-of-a-track)



## Adjusting the left and right mix position of a track

When you build a piece type, each track has its own individual instrument and you want to adjust the left and right channel placement of each track.  

For example, for track 1 I want to move the channel to the very left side, for track 2 I want to stay in the middle, and for track 3 I want to move to the slightly right side.  

At this point you can use the built-in function `add_pan` of type piece to set the left and right channel mix position for the specified track.  

The pan type is a special type used to store the left and right channel mix positions.

### The composition of pan type

```python
pan(value, start_time=0, mode='percentage', channel=None, track=None)
```

- value: When mode == 'percentage', value refers to the percentage of the left and right channels, and the value is an integer or decimal number from 0 to 100, indicating 0% to 100%, 0% means the leftmost channel, 100% means the rightmost channel, and 50% means right in the middle. When mode == 'value', value refers to the value of MIDI CC 10, which is the value of the pan position message, from 0 to 127, the value is a positive integer, also from the leftmost channel to the rightmost channel.
- start_time: the time, in bars, when the left and right channel mixes start to adjust
- mode: used to set the unit of value
- channel: MIDI channel number
- track: MIDI track number

```python
a = piece([A, B, C, D], [1, 47, 35, 2], 150, [0, 2, 2, 8]) # a is a piece type
a.add_pan(value, ind, start_time=0, mode='percentage') # value, start_time, mode are all construction parameters for the corresponding pan type,
# ind is the index of the track you want to add to the left/right mix, with 0 as the first track
```

In addition, when building the piece type, the two parameters `pan` and `volume` can be used to set the left and right channel mixes and the overall volume level of each track. The values of both parameters are a list with the pan/volume types as elements, which represents the pan/volume settings of each track. If not set, the default value is None, which will be converted to a list of empty lists when building the piece type, with the number of elements being the number of tracks contained in the constructed piece type. When you pass in the pan/volume list when constructing a piece type, the pan/volume of each track could be a single pan/volume type or a list of pan/volume types.

Note: If you want to set the value of the pan and volume parameters, make sure the number of elements in the list is exactly the same as the number of tracks, and if there are some tracks you don't want to set, you can write an empty list in that track's place.

