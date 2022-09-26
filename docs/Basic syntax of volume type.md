# Basic syntax of volume type



## Contents

- [Adjusting the overall volume of a track](#adjusting-the-overall-volume-of-a-track)



## Adjusting the overall volume of a track

If you want to adjust the overall volume level of a given track, i.e. the angle of the volume knob of one of the tracks in the DAW, you can use the built-in function `add_volume` of type piece to set the overall volume level for the specified track.  

The volume type is used to store the overall volume level of a track.

### The composition of volume type

```python
volume(value, start_time=0, mode='percentage', channel=None, track=None)
```

- value: When mode == 'percentage', value refers to the overall volume as a percentage, with values ranging from 0 to 100 as integers or decimals, indicating 0% to 100%, 0% means minimum volume (mute), 100% means maximum volume, and 50% means right in the middle. When mode == 'value', value refers to the value of MIDI CC 7, which is the value of the volume message, from 0 to 127, value is a positive integer, also from the smallest overall volume (mute) to the largest overall volume.
- start_time: the time, in bars, when the left and right channel mixes start to adjust.
- mode: used to set the unit of value
- channel: MIDI channel number
- track: MIDI track number

```python
a = piece([A, B, C, D], [1, 47, 35, 2], 150, [0, 2, 2, 8]) # a is a piece type
a.add_volume(value, ind, start_time=0, mode='percentage') # value, start_time, mode are all construction parameters for the volume type,
# ind is the index of the track you want to add to the overall volume adjustment, with 0 as the first track
```

In addition, when building the piece type, the two parameters `pan` and `volume` can be used to set the left and right channel mixes and the overall volume level of each track. The values of both parameters are a list with the pan/volume types as elements, which represents the pan/volume settings of each track. If not set, the default value is None, which will be converted to a list of empty lists when building the piece type, with the number of elements being the number of tracks contained in the constructed piece type. When you pass in the pan/volume list when constructing a piece type, the pan/volume of each track could be a single pan/volume type or a list of pan/volume types.

Note: If you want to set the value of the pan and volume parameters, make sure the number of elements in the list is exactly the same as the number of tracks, and if there are some tracks you don't want to set, you can write an empty list in that track's place.

