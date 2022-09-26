# Basic syntax of piece type



## Contents

- [piece class for dealing with multiple tracks and each track with its own instrument](#piece-class-for-dealing-with-multiple-tracks-and-each-track-with-its-own-instrument)
- [Constructing a piece type can be done using an alternative syntax that is a bit more intuitive](#constructing-a-piece-type-can-be-done-using-an-alternative-syntax-that-is-a-bit-more-intuitive)
- [List of piece type editing operations](#list-of-piece-type-editing-operations)
- [Merge a piece type into a chord type](#merge-a-piece-type-into-a-chord-type)
- [New attribute of piece name and BPM display for piece type](#new-attribute-of-piece-name-and-BPM-display-for-piece-type)
- [Some new syntax for piece type](#some-new-syntax-for-piece-type)
- [Clear pan and volume of piece type](#clear-pan-and-volume-of-piece-type)
- [Clear MIDI messages of program changes](#clear-MIDI-messages-of-program-changes)
- [Clear other MIDI messages](#clear-other-MIDI-messages)
- [Quickly change the instruments of tracks of the piece type](#quickly-change-the-instruments-of-tracks-of-the-piece-type)
- [Move the position of tracks of the piece type by bar length](#move-the-position-of-tracks-of-the-piece-type-by-bar-length)
- [Filtering a type of MIDI message in chord type and piece type](#filtering-a-type-of-MIDI-message-in-chord-type-and-piece-type)
- [Transpose the whole piece type](#transpose-the-whole-piece-type)
- [Apply function to the piece type as a whole](#apply-function-to-the-piece-type-as-a-whole)
- [Reset MIDI channel number and track number](#reset-MIDI-channel-number-and-track-number)
- [Get the total number of notes in a piece instance](#get-the-total-number-of-notes-in-a-piece-instance)
- [Exclude the drum tracks of a piece type](#exclude-the-drum-tracks-of-a-piece-type)



## piece class for dealing with multiple tracks and each track with its own instrument

Let's say you now want to write a piece which is divided into four voices, the main melody (instrument: piano), the bass (instrument: electric bass), the chordal and ornamental notes (instrument: harp), and the drums (instrument: drum set).

This way you have four instruments, each responsible for four different tracks, making up a complete piece of music. And each of these four tracks has its own start time (in bars).

Now let's say you have written melodies for each of the four voices, stored in their respective chord types, called C1, C2, C3, and C4.  

You want the piece to have a BPM of 150 and the four voices to start at 0, 2, 2, 4 (in bars), with 0 meaning from the very beginning of the piece.

The instrument section of the piece class corresponds to instruments 0 to 127 of the General Midi, you can directly enter the numbers 0 to 127 to select the corresponding instrument, or you can enter the name of the instrument.

The dictionary of the 128 instruments of General Midi can be viewed in the file database.py.

The parameters of the piece class (in positional order) are:

```python
piece(tracks,
      instruments=None,
      bpm=120,
      start_times=None,
      track_names=None,
      channels=None,
      name=None,
      pan=None,
      volume=None,
      other_messages=[],
      sampler_channels=None)
```

* tracks: a list of chord types for each track

* instruments: a list of instruments for each track

* bpm: the tempo of the track (BPM)

* start_times: a list of the start times of each track (in bars)

* track_names: the name of each track

* channels: a list of channel numbers corresponding to each track, 0-based

* name: the name of the piece

* pan: a list of pan messages of each channel

* volume: a list of volume messages of each channel

* other_messages: list of other MIDI messages of the piece

* sampler_channels: specify the corresponding sampler channel number of each track in the sampler module

For our current requirements, we can build a piece class like this.

```python
C1 = chord('G4, D5, B5, F#5') % (1, 1/8) % 4
C2 = (chord('C2, C2, G1, G1') % (1,1)) % 2
C3 = (chord('F#6, G6') % (1/8, 1/8) % 8 | chord('A5, B5') % (1/8, 1/8) % 8) % 2
C4 = chord('G3, G3, G3, G3') % ([3/8,1/8,1/4,1/4], [3/8,1/8,1/4,1/4]) % 4

new_piece = piece(tracks=[C1, C2, C3, C4],
                  instruments=['Acoustic Grand Piano', 'Electric Bass (finger)', 'Orchestral Harp', 'Synth Drum'],
                  bpm=120,
                  start_times=[0, 2, 2, 6],
                  track_names=['piano', 'bass', 'harp', 'drum'])

>>> new_piece
[piece] 
BPM: 120
track 1 piano | instrument: Acoustic Grand Piano | start time: 0 | [G4, D5, B5, F#5, G4, D5, B5, F#5, G4, D5, B5, F#5, G4, D5, B5, F#5] with interval [0.125, 0.125, 0.125, 0.125, 0.125, 0.125, 0.125, 0.125, 0.125, 0.125, 0.125, 0.125, 0.125, 0.125, 0.125, 0.125]
track 2 bass | instrument: Electric Bass (finger) | start time: 2 | [C2, C2, G1, G1, C2, C2, G1, G1] with interval [1, 1, 1, 1, 1, 1, 1, 1]
track 3 harp | instrument: Orchestral Harp | start time: 2 | [F#6, G6, F#6, G6, F#6, G6, F#6, G6, F#6, G6, F#6, G6, F#6, G6, F#6, G6, A5, B5, A5, B5, A5, B5, A5, B5, A5, B5, A5, B5, A5, B5, A5, B5, F#6, G6, F#6, G6, F#6, G6, F#6, G6, F#6, G6, F#6, G6, F#6, G6, F#6, G6, A5, B5, A5, B5, A5, B5, A5, B5, A5, B5, A5, B5, A5, B5, A5, B5] with interval [0.125, 0.125, 0.125, 0.125, 0.125, 0.125, 0.125, 0.125, 0.125, 0.125, 0.125, 0.125, 0.125, 0.125, 0.125, 0.125, 0.125, 0.125, 0.125, 0.125, 0.125, 0.125, 0.125, 0.125, 0.125, 0.125, 0.125, 0.125, 0.125, 0.125, 0.125, 0.125, 0.125, 0.125, 0.125, 0.125, 0.125, 0.125, 0.125, 0.125, 0.125, 0.125, 0.125, 0.125, 0.125, 0.125, 0.125, 0.125, 0.125, 0.125, 0.125, 0.125, 0.125, 0.125, 0.125, 0.125, 0.125, 0.125, 0.125, 0.125, 0.125, 0.125, 0.125, 0.125]
track 4 drum | instrument: Synth Drum | start time: 6 | [G3, G3, G3, G3, G3, G3, G3, G3, G3, G3, G3, G3, G3, G3, G3, G3] with interval [0.375, 0.125, 0.25, 0.25, 0.375, 0.125, 0.25, 0.25, 0.375, 0.125, 0.25, 0.25, 0.375, 0.125, 0.25, 0.25]
```

If there are drum tracks, you can set its corresponding channel to 9, so that the instrument can choose the type of drums specialized in General Midi, such as Standard, Room, Electronic, etc.

When the number of the instrument is 1, it is the Standard drum set (when the corresponding channel is 9).

Now the new_piece is a piece type that stores the information we want about the piece, and the four tracks are named piano, bass, harp, drum.

To write this piece to a MIDI file, you just need

```python
play(new_piece)
# or
new_piece.play()
```

That is, the play function is written to the MIDI file and then played directly, you can hear the music immediately, if you just write the MIDI file without playing, then write

```python
write(new_piece, name=the_name_of_the_midi_file_you_want)
```

The play and write functions will check if what you pass in is a piece class, and if it is, then they will write the full information about the piece class to the MIDI file. That is, each part is written to a separate track, each track has its own separate instrument, its own start time, its own name (if one is given), and the channel of each track corresponds to the channel you gave it (if given). So the written MIDI file is a complete piece of music with multiple instruments and voices.

```python
piece(...) could be written in simplified form as
P(...)
```

## Constructing a piece type can be done using an alternative syntax that is a bit more intuitive

For example, we now use the piece function to construct a piece type

```python
A1 = C('Cmaj7') % (1, 1/8)
B1 = chord('A2') % (1,)
C1 = chord('C,D,E,F,G') % (1/8, 1/8)
D1 = chord('C5')

new_piece = piece([A1, B1, C1, D1],
                  ['Acoustic Grand Piano', 'Electric Bass (finger)', 'Orchestral Harp', 'Synth Drum'],
                  150,
                  [0, 2, 2, 4],
                  ['piano', 'bass', 'harp', 'drum'])
```

Using the `build` function, it is possible to construct piece types with an alternative syntax, one that puts all the information for one track in a list as track 1, all the information for the next track in the next list as track 2 and so on, with the order of the parameters for each track's information being:  

`[chord type, instrument (optional), start time (in bars) (optional), track number (optional), track name (optional), pan (optional), volume (optional)]`  

The build function can receive a list of any number of tracks. The default value for the speed of the track (BPM) is 120. To specify the tempo of the piece, you must use the keyword argument `bpm`, other parameters of the piece type could also be specified using the keyword arguments, which can be written, for example, as

```python
new_piece = build([A1, 'Acoustic Grand Piano', 0, 1, 'piano'],
                  [B1, 'Electric Bass (finger)', 2, 2, 'bass'],
                  [C1, 'Orchestral Harp', 2, 3, 'harp'],
                  [D1, 'Synth Drum', 4, 4, 'drum'],
                  bpm=150)
```

In addition, the build function can also take the track types as arguments to build the piece type. The track types and the track information lists could be input mixedly.  

You can also pass in a list of track types or track information lists (could be mixed), which could also construct the piece type.

```python
# construct the piece type with multiple track types
new_piece = build(track(A1, instrument='Acoustic Grand Piano', start_time=0, channel=1, track_name='piano'),
                  track(B1, instrument='Electric Bass (finger)', start_time=2, channel=2, track_name='bass'),
                  track(C1, instrument='Orchestral Harp', start_time=2, channel=3, track_name='harp'),
                  track(D1, instrument='Synth Drum', start_time=4, channel=4, track_name='drum'),
                  bpm=150)

# construct the piece type with a list of track types
new_piece = build([track(A1, instrument='Acoustic Grand Piano', start_time=0, channel=1, track_name='piano'),
                  track(B1, instrument='Electric Bass (finger)', start_time=2, channel=2, track_name='bass'),
                  track(C1, instrument='Orchestral Harp', start_time=2, channel=3, track_name='harp'),
                  track(D1, instrument='Synth Drum', start_time=4, channel=4, track_name='drum')],
                  bpm=150)
```

## List of piece type editing operations

The piece type (piece type) can store information about a whole song with several different instruments and different MIDI channels.  

In addition to editing the chord type of any MIDI channel in a piece type, you can also edit the chord type of the whole piece type.  

You can also do a lot of editing on the whole piece type level.

```python
# First build a piece type, A1, B1, C1, D1 are all chord types already written
a = piece(tracks=[A1, B1, C1, D1],
          instruments=[1, 35, 1, 49],
          bpm=150,
          start_times=[0, 8, 8, 16],
          channels=[0,1,9,2],
          track_names=['piano', 'electric bass', 'drums', 'strings'])

# If you want to repeat this track type exactly n times, then you can write
b = a | n

# If you want to copy and paste all MIDI channels of this piece type n times, then you can write
b = a * n
b = a % n
# The chord types are written in different add modes, see the symbolic logic of the chord types

# You can use the index value to see the information of a channel of a piece type, 0 as the first channel
# Using the syntax of a[n] you can get the nth track type
>>> a[0]
[track] 
BPM: 150
channel 0 piano | instrument: Acoustic Grand Piano | start time: 0 | [C4, E4, G4, B4] with interval [0, 0, 0, 0]

# Use the syntax of a(n) to get a list with the chord type, instrument, BPM, start time,
# channel number, track name, channel pan, channel volume of the nth MIDI channel
>>> a(0)
[[C4, E4, G4, B4] with interval [0, 0, 0, 0], 'Acoustic Grand Piano', 150, 0, 0, 'piano', [], []]

# Raise/lower the whole piece by n semitones, again with the same syntax as the note type, chord type, either using the up/down function or the +/- progression syntax
b = a.up()
b = +a
b = a + 2
b = a.down()
b = -a
b = a - 2

# sometimes you don't want to change the notes of the drum track when raise or lower
# the piece type, when use up/down functions of piece type,
# you can set parameter `mode` to 1 in order to
# raise or lower the whole piece except the drum track,
# the track with channel number 9 will be considered as the drum track
b = a.up(mode=1)

# You can add a real-time speed change to the specified position
a.add_tempo_change(bpm=100, start_time=None, ind=None, track_ind=None)
# bpm: the speed you want to change to, in BPM
# start_time: time when the speed change occurs, in bars, if not set then ind prevails
# ind: the position where the tempo change message is inserted, needs to be used with track_ind
# track_ind: you can choose how many MIDI channels to insert the tempo change message, with 0 as the first MIDI channel, ind is the selected MIDI channel in the first position
# If ind and track_ind are not set, then by default the tempo change message is added to the end of the first MIDI channel

# You can add real-time pitch bend to the specified position (pitch bend can simulate the effect of note bend, glissando, vibrato, etc.)
a.add_pitch_bend(value, start_time=0, channel='all', track=0, mode='cents', ind=None)
# value: the amount of pitch change of the note
# start_time: the time at which the pitch change of the note occurs, in bars
# channel: select the channel to insert the pitch bend message into, 0 is the first MIDI channel, if 'all' then the same pitch bend message is inserted into all MIDI channels
# track: MIDI track, normally no need to set it
# mode: the unit of the pitch bend messages, as explained in detail before
# ind: the position where the pitch bend messages is inserted, if start_time is set, then the position of start_time is used

# View the number of MIDI channels of the piece type
>>> len(a)
4

# Add a new MIDI channel
a.append(new_track)
# new_track: the newly added track, must be a track type

# you can use build function to construct a piece type with multiple track types
new_piece = build(track(A1, instrument=1, start_time=0, channel=0, track_name='piano'),
                  track(B1, instrument=35, start_time=1, channel=1, track_name='electric bass'),
                  track(C1, instrument=1, start_time=8, channel=9, track_name='drums'),
                  track(D1, instrument=49, start_time=16, channel=2, track_name='strings'),
                  bpm=150)
# please note, when you pass in track types to the build function to construct a piece type,
# the BPM of the piece type you get only depends on the bpm parameter of the build function,
# which has nothing to do with the BPM of the track types you pass in
```

## Merge a piece type into a chord type

Sometimes we need to merge all the notes of a piece type, i.e. a music theory type with many 
different tracks and many different instruments, into one chord type, to facilitate the 
manipulation of some editing and music theory functions (since many music theory functions are unique to chord types), 
then we can use the built-in function `merge` of the piece type.

```python
a = read('example.mid') # a is the piece type to convert to after reading
# the MIDI file example.mid

a.merge(add_labels=True, add_pan_volume=False, get_off_drums=False)

# add_labels: when set to True, adds a new attribute track_num to each note of each track of the
# piece type with the value of index of the current channel, index is the current number of channels
# starting with 0 for the first channel, and track_num can be called later for each note in each channel,
# you can record which channel the notes came from before the merging

# add_pan_volume: When set to True, the pan and volume messages will be converted to MIDI CC messages,
# and stored in other_messages in the returned chord type

# get_off_drums: when set to True, the result will exclude the drum tracks

bpm, b, start_time = a.merge()
# Use the built-in function merge of the piece type to get the tuple of
# (bpm, merged chord type, start_time), with the start_time in bars
```

## New attribute of piece name and BPM display for piece type

When building a piece type, you can use the `name` parameter to set the name of the piece type, that is, the piece name. When the piece type is printed, the piece name will be shown at the first line (if it is None then it is not shown). In the second line, the BPM of the piece will be displayed.

## Some new syntax for piece type

### Replace a track in a piece type

```python
a[i] = new_track # Replace the i-th track (starting from 0) of a track type a with new_track, where new_track is the track type
```

### Delete a track from a piece type

```python
del a[i] # Delete the i-th track (starting from 0) of track type a
```

### Mute a track of a piece type

You can use the `mute` function to mute a track of a track type, and the `unmute` function to unmute it.

```python
a.mute(i=None) # mute all tracks if i is not specified, or mute the ith track (starting from 0) if i is specified
a.unmute(i=None) # unmute, the use of parameters and mute function is the same
```

## Clear pan and volume of piece type

You can use the `clear_pan` function of the track type to clear the pan information of the track type (left and right channel mix position) and the `clear_volume` function of the track type to clear the volume information of the track type (overall volume level of the track).

```python
a.clear_pan()
a.clear_volume()
```

## Clear MIDI messages of program changes

When you read a MIDI file, often other MIDI messages is read into `other_messages` as attributes of the musicpy data structure it was converted to. If there is `program_change` messages in it, it will be written to the MIDI file the same way when it is written again, forcing the instrument of the track to change, which will be done before This can cause problems when you reset the instruments written to the MIDI file. You can set the value of `nomsg` to `True` in the `write` function that writes to the MIDI file, so that no other MIDI messages will be written, but sometimes you want to keep the MIDI messages of other instruments that are not changed, so what can you do? To solve this problem, you can use the `clear_program_change` function to clear the `program_change` messages of a chord type or a piece type individually, so that when you write the MIDI file again, each track of the MIDI file will be played according to the instrument you set.

```python
a = read('test.mid') # Read a MIDI file and convert it to a piece type

a.clear_program_change() # Clear the program_change messages for the piece type

# The clear_program_change function for a track type has one parameter apply_tracks, if True,
# Clears the program_change messages for the track type itself as well as the program_change messages for each track,
# If False, only the program_change messages of the track type itself will be cleared, the default value is True

a.clear_program_change(apply_tracks=False)

b = a.tracks[0] # extracts the first track of track type a, which is a chord type
b.clear_program_change() # clear program_change messages for chord types
```

## Clear other MIDI messages

You can use the `clear_other_messages` function to clear all other MIDI messages for a chord type or a track type, or to filter the clearing by MIDI message type.  
If you just want to clear all other MIDI messages for a chord type or a piece type, you can just clear the list of their `other_messages` attributes, or reassign them to an empty list.

```python
# Both of these methods can completely empty other MIDI messages for a chord type or a piece type
a.other_messages.clear()
a.other_messages = []

a.clear_other_messages() # Clear all other MIDI messages
a.clear_other_messages(program_change) # clear other MIDI messages of type program_change

# When a is a piece type, similar to the clear_program_change function, there is an apply_tracks argument
# The usage is similar, and the default value is True
```

## Quickly change the instruments of tracks of the piece type

You can use the `change_instruments` function of a track type to quickly change the instruments of each track of the track type as a whole or the instruments of a single track without having to modify the `instruments` and `instruments_numbers` properties to make changes to the instruments of the track. You can pass in the instrument name or the MIDI number of the instrument, and you can do the replacement of instruments for all tracks as a whole, or specify the replacement of instruments for a particular track. Note: If you want to change the instrument of a track by modifying the attributes, you have to change the MIDI numbers in `instruments_numbers`, because they are the real valid messages when writing to the MIDI file, and changes to `instruments` only affect the instrument names when the track type is displayed.

```python
change_instruments(instruments, ind=None)

# instruments: list of instruments (instrument substitution for all tracks) or individual instruments, either by instrument name or MIDI number.
# General MIDI instrument name and MIDI number correspondence can be seen by printing the variable 'INSTRUMENTS' or 'reverse_instruments'

# ind: If None, then the whole instrument of the track of the type of music is replaced, and the instrument_list must be a list or a tuple.
# If it is an integer, then the instrument of the track with the corresponding index is replaced, the index starts from 0

piece1 = P([C('C'), C('D')], [1, 49])
>>> piece1
[piece] 
BPM: 120
track 1 | instrument: Acoustic Grand Piano | start time: 0 | [C4, E4, G4] with interval [0, 0, 0]
track 2 | instrument: String Ensemble 1 | start time: 0 | [D4, F#4, A4] with interval [0, 0, 0]

piece1.change_instruments([2, 47]) # Change the instrument of the overall track
>>> piece1
[piece] 
BPM: 120
track 1 | instrument: Bright Acoustic Piano | start time: 0 | [C4, E4, G4] with interval [0, 0, 0]
track 2 | instrument: Orchestral Harp | start time: 0 | [D4, F#4, A4] with interval [0, 0, 0]

# Or you can write
piece1.change_instruments(['Bright Acoustic Piano', 'Orchestral Harp'])

piece1.change_instruments(5, 0) # Change the instrument on track 1 to the MIDI number 5
>>> piece1
[piece] 
BPM: 120
track 1 | instrument: Electric Piano 1 | start time: 0 | [C4, E4, G4] with interval [0, 0, 0]
track 2 | instrument: Orchestral Harp | start time: 0 | [D4, F#4, A4] with interval [0, 0, 0]

# Or you can write
piece1.change_instruments('Electric Piano 1', 0)
```

## Move the position of tracks of the piece type by bar length

You can use the `move` function of a track type to move the entire track of the track type or a specified track to the right or left by a specified measure length. The return value is a new piece type.

```python
move(time=0, ind='all')

# time: the length of the move in bars, positive means move to the right, negative means move to the left

ind: index of the track to move, if 'all', then move all tracks, if integer, then move the track with the corresponding index, starting from 0

>>> a.start_times
[0, 2, 3]

b = a.move(2) # The piece type a moves the entire length of 2 bars to the right

>>> b.start_times
[2, 4, 5]

c = a.move(2, 1) # the length of the 1st track of the piece type a moved 2 bars to the right

>>> c.start_times
[2, 2, 3]
```

## Filtering a type of MIDI message in chord type and piece type

You can use the `get_msg` function to filter for a type of MIDI message in a chord type, which returns a list of MIDI messages, for example

```python
a.get_msg(program_change) # Filter program_change messages in chord type a
```

The piece type also has the `get_msg` function, which you can use to extract a type of MIDI message from a track by specifying the `ind` parameter, or from all tracks if you don't specify it.

```python
a.get_msg(program_change, ind=0) # Filter program_change messages in the first track
```

## Transpose the whole piece type

You can use the `modulation` function of a piece type to transpose the entire piece type, returning the new piece type after transposition.

```python
modulation(old_scale, new_scale, mode=1, inds='all')

# old_scale: the previous scale type
# new_scale: the new scale type
# mode: when mode is 1, tracks with channel number 9 (drum tracks) will not be transposed
# inds: when inds is 'all', convert all tracks, also can be a list of indexes, convert the indexes in the list corresponding to the tracks
```

## Apply function to the piece type as a whole

Use the `apply` function for a piece type to apply a function to each track of the piece type.

```python
apply(func, inds='all', mode=0, new=True)

# func: the function to apply to each track

# inds: when inds is 'all', apply the function to all tracks, also as an index or a list of indexes

# mode: when mode is 0, the track to which the function is applied is replaced by the result of the application, when mode is 1, only the step of applying the function to the track is performed

# new: when new is True, the new track type is returned, when new is False, the track type is modified directly on the original one
```

## Reset MIDI channel number and track number

The chord type and the track type have the `reset_channel` and `reset_track` functions, which reset the MIDI channel number and MIDI track number of all notes, pitch bend messages, and other MIDI messages. Both of these functions are modified directly on the original object.

```python
# chord type
reset_channel(channel,
              reset_msg=True,
              reset_pitch_bend=True,
              reset_note=True)

# channel: MIDI channel number
# reset_msg: whether to reset the MIDI channel number of all other MIDI messages
# reset_pitch_bend: whether to reset the MIDI channel number of all bend messages
# reset_note: whether to reset the MIDI channel number of all notes

reset_track(track, reset_msg=True, reset_pitch_bend=True)

# track: MIDI track number
# reset_msg: whether to reset the MIDI track number of all other MIDI messages
# reset_pitch_bend: whether to reset the MIDI track number of all bend messages


# piece type
reset_channel(channels,
              reset_msg=True,
              reset_pitch_bend=True,
              reset_pan_volume=True,
              reset_note=True)

# channels: a list of MIDI channel numbers, which can also be an integer, set each track's MIDI channel number to this integer
# reset_msg: whether to reset the MIDI channel numbers of all other MIDI messages
# reset_pitch_bend: whether to reset the MIDI channel numbers of all bend messages
# reset_pan_volume: whether to reset the MIDI channel numbers of all sound phases and track volumes
# reset_note: whether to reset the MIDI channel number of all notes

reset_track(tracks,
            reset_msg=True,
            reset_pitch_bend=True,
            reset_pan_volume=True)

# tracks: a list of MIDI track numbers, or an integer to which each track's MIDI track number will be set
# reset_msg: whether to reset the MIDI track numbers of all other MIDI messages
# reset_pitch_bend: whether to reset the MIDI track number of all bend messages
# reset_pan_volume: whether to reset the MIDI track numbers of all the sound phases and track volumes
```

## Get the total number of notes in a piece instance

You can use `total` function of the piece type to get the total number of notes in a piece instance. This is useful when you want to count the total number of notes in a MIDI file, you can read the MIDI file into a piece instance and use this function. This function will calculate the sum of the number of notes of all tracks in the piece instance.

```python
total(mode='all')

# mode: If it is 'all', then return the total number of notes including pitch bend and tempo change messages of the piece instance,
# if it is 'notes', then return the total number of notes only. The default is 'all'.

a = P([C('C'), C('Cmaj7')])
>>> a.total()
7
```

## Exclude the drum tracks of a piece type

You can use `get_off_drums` function of the piece type to exclude the drum tracks of a piece type. This function works when the channels attribute is not None.

```python
a = P([C('C'), drum('0,1,2,1').notes], channels=[0, 9])

>>> a
[piece] 
BPM: 120
track 1 channel 0 | instrument: Acoustic Grand Piano | start time: 0 | [C4, E4, G4] with interval [0, 0, 0]
track 2 channel 9 | instrument: Acoustic Grand Piano | start time: 0 | [C2, F#2, E2, F#2] with interval [0.125, 0.125, 0.125, 0.125]

>>> a.get_off_drums()
>>> a
[piece] 
BPM: 120
track 1 channel 0 | instrument: Acoustic Grand Piano | start time: 0 | [C4, E4, G4] with interval [0, 0, 0]
```
