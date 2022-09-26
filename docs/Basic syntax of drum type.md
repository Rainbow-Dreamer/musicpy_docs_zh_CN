# Basic syntax of drum type



## Contents

- [Added support for drum types, and a new set of syntax for writing drums](#added-support-for-drum-types-and-a-new-set-of-syntax-for-writing-drums)
- [Introduction to the construction and usage of drum types](#introduction-to-the-construction-and-usage-of-drum-types)



## Added support for drum types, and a new set of syntax for writing drums

Added in early May 2021, the drum type is a new music theory type that reads a string that follows a specific syntax rule and converts it to a drum beat.

I have designed a new syntax for writing drum beats, and have successfully written an interpreter function that can parse strings that conform to this syntax into a MIDI file of drum notes. I'll explain this syntax for drum beats in code.

First of all, you can define your own dictionary of numbers or letters or special characters to map to different MIDI drum notes, I have defined a default drum mapping dictionary in the database.

Here the length of numbers, letters and special characters can be greater than 1, which means you can write multiple digits, words and multiple special characters as a whole for mapping.
This is the current default drum mapping dictionary.

```python
drum_mapping = {
    '0': 36, # Electric Bass Drum
    '1': 42, # Closed Hi-hat
    '2': 40, # Electric Snare
    '3': 38, # Acoustic Snare
    '4': 46, # Open Hi-hat
    '5': 44, # Pedal Hi-hat
    '6': 39, # Hand Clap
    '7': 35, # Acoustic Bass Drum
    '8': 57, # Crash Cymbal 2
    '9': 49  # Crash Cymbal 1
}
```

Here different strings of numbers map to values that are MIDI's drum note values, different values represent different pitches, and in the case of drums it means different kinds of drums.

For example, 0 maps to 36 which corresponds to Electric Bass Drum for MIDI, 1 maps to Closed Hi-hat, and 2 maps to Electric Snare.

I have also written the names of the drums mapped to different values in the database, you can see the dictionary drum_types.

In short, 0,1,2 now corresponds to bass drum, pedal cymbal and snare drum respectively, now we can write a music theory type drum, its 1st parameter is a string, in this string you can write drums according to the drum syntax I designed, it's very simple and powerful, you can play a lot of macro's minimal operation.

The syntax I designed for writing drums has a total of 12 rules, plus some special cases.

## Rule 1

Monophonic mode, played sequentially with `,` (e.g. bass drum, hihat, snare, hihat, bass drum, hihat, snare, hihat)

```python
'0,1,2,1,0,1,2,1'  
```

## Rule 2

A pattern with several different drums playing at the same time, with `;` indicating simultaneous playing, using comma-separated notes or successive playing

```python
'0,0;1,2,1,0,0;1;5,1;2,1'
```

## Rule 3

If there is an interval between two drums (drums can be monophonic or polyphonic), it is expressed as `[n]`, and n can be any integer, float or fraction in bars.

```python
'0,[1],2,1,[1],0,2,1'
```

## Rule 4

Set the duration, interval and volume for each drum beat, each drum beat can be followed by a `[length;interval;volume level]` setting block, and also drums played at the same time (drums that are joined with `;`). If the 2nd parameter is the same as the 1st, then it can also be omitted by writing `.` as in the advanced syntax for chords, the default volume is 100, or you can pass the length list, the interval list, and the volume list into the build function of the drum type.

```python
'0[1/4],1[1/4],2[1/4],1[1/4],0[1/4],1[1/4],2[1/4],1[1/4]'
```

In the setting block, if there are parameters that you don't want to set, you can write `n` to indicate that you don't want to set them, and use it to set them if there is a batch setting statement afterwards. Please note that the batch setting statement will overwrite the settings of each drum block, unless `n` is written to indicate that it is not set.

```python
'0[1/4;n;80],1[1/4;1/2],2[1/4;1/8],1[1/4];2[1/8]'
```

Please note that if drums are played at the same time (drums that are joined with `;`) for setting blocks, please put them after the last note.

```python
 '0[1/4;.],1[1/4;1/2],2[1/4;1/8],1;2[1/4;1/8],0[1/4],1[1/4;1/8;80],2[1/4],1[1/4]'
```

## Rule 5

Batch set the length, interval and volume level of all drums, after the `!` can be followed by 1 to 3 arguments, also similar to the syntax for drums played simultaneously, which uses the `;` to link together.  

If only 1 parameter is written, then only the length of all drums is set in bulk, if 2 are written, then the length and interval of all drums is set, and the 3rd parameter is the volume level of all drums.  

Please note that the interval of the part where multiple drums are played at the same time will not be set, that is, the part where multiple drums were written to be played at the same time will still be played at the same time (interval of 0).  

Only the set length and volume level will be set, and only the interval of the last note in the drums played at the same time will be set, because it is the interval between the next drum beat.  

This batch setting can only be set at the beginning or at the end, i.e. at the position of the first drum beat or at the position of the last drum beat.  

If there are parameters that you don't want to set (you want to follow the default or you want to follow each one individually), you can write `n` to indicate that you don't want to set them, write `.` to set the same value as the previous parameter. Also, the 3 arguments can be lists, in addition to numbers.

```python
'0,0;1,2,1,0,0;1;5,1;2,1,!1/4;1/4;1/4'
'!1/4;1/4;1/4,0,0;1,2,1,0,0;1;5,1;2,1'
```

## Rule 6

If no settings are made, the default drum duration is 1/8, the drum interval is 1/8 (the intervals in the syntax played at the same time are all 0), and the volume level of the drums is 100. The default duration, interval and volume could be set when initializing the drum instance, or be specified in the block like `{de:duration;interval;volume}`, which will set the default duration, interval and volume for each drum beat between the last block and current block. The following example set the drum beat with default duration at 1/16 bar and default interval at 1/16 bar.

```python
'0,[.16],1,1,2,[.16],0,[.16],0,1,0,0,2,[.16],0,[.16],{de:.16;.}'
```

## Rule 7

If you want to repeat the same drum beat n times, you can add `(n)` after a drum beat, or you can add it after a drum beat with a set block.
Or it can be followed by a set block as well, in either case, the drum beat will be set first according to the set block, and then repeated n times
For syntax where multiple drums are played at the same time, you only need to add `(n)` at the end to repeat n times, regardless of whether there is a set block or a batch set

```python
'0,1(3),2,1(3)'
'0,1(3)[1/4;1/4],2,1(3)[1/4;1/4]'
'0,1[1/4;1/4](3),2,1[1/4;1/4](3)'
'0,1;2(3),2,1;2[1/4;1/8](3),1;2[1/4;1/4;80](3)'
```

## Rule 8

If you want to batch one of the drum beats or repeat it n times, you can use `{!1/4;1/4}` and `{n}` and just write it as if it were a normal drum beat after a drum beat, it will follow the beginning to this truncated statement or the previous truncated statement as the drum passage to be batch processed, and the the batch processing statement can be written inside `{}`.

If you only write a number, it means repeat the drum passage n times, if you want to use a combination of the two, you can use the `|` connection, for example, `{!1/4;1/4|2}` means batch set the specified drum passage and repeat 2 times, the order of the two settings can be arbitrary.

```python
'0,1[1/4;1/4](3),2,1[1/4;1/4](3),{2},0,1[1/4;1/4](2),3,2,1[1/4;1/4](2),3,{2}'
```

## Rule 9

If you want to number, or name, a section of drums, then you can write `$n` in the truncated statement, where `n` is the number, either a number or a letter, and can be greater than 1 in length.
This can also be used in combination with the other two settings, so that if you want to use the numbered drum beat later on, you can write `$n` as if it were a normal drum beat.

```python
'0,1[1/4;1/4](3),2,1[1/4;1/4](3),{$1},0,1[1/4;1/4](2),3,2,1[1/4;1/4](2),3,{$2},$1,$2'
```

The numbered statement can be used as a general drumbeat when called after it is defined, and the same can be done with set blocks, repeating statements like

```python
'$1[1/4;1/4]' # The set block in this case indicates a bulk setting of `$1`, with parameters referable to case 5
'$1(2)' # repeat the numbered 1 drum passage 2 times
```

## Caution

1. Note that when the parameters are lists, `|` must be used as the interval when setting multiple drums that are played simultaneously (or when setting all drums in a batch), and `` ` `` as the interval, in both cases the interval between the three parameters can be `;`  
2. Please note that in syntax where multiple drums are played at the same time, it is ungrammatical to use a set block for each drum, e.g. `'1[1/4;1/4];2[1/8;1/8]'` is ungrammatical  
3. If you want to set the length of each drum beat for multiple drums playing at the same time, put the setting block after the last drum beat, you can use a list as a parameter, in which case you must omit the brackets (`[]`). If you are batch processing in a truncated statement or if you are batch setting all drums using a list as a parameter, you can use `[]`  
4. You can place any numbers of spaces and newline characters, they do not affect the contents that the interpreter function converts to, so you can write the drum codes prettier and more organized.  
5. The durations and intervals of notes could also use the syntactic sugar, use `.n` to represent `1/n`, which is the nth note.

## Introduction to the construction and usage of drum types

Drum types are similar to chord types in that they have a list of notes, can be repeated n times, merged, set overall note parameters, etc., and can also be played directly. Note that when you play a drum type using the play function (either using the built-in drum type function or the global function), the drum type is automatically coded into a track type and the MIDI channel number is set to 9 (starting from 0 as the first, i.e. the 10th MIDI channel, specifically for drum tracks).  

Note that the drum type itself does not contain tempo, or tempo, or bpm, because the drum type is designed to be viewed on the same level as the chord type. Neither the drum type (drum) nor the chord type (chord) contain parameters for tempo; the piece type (piece) and track type (track) do.  

The drum type can be constructed by passing a string or a chord type, where the default position of the 1st parameter is a string, or can be specified with the pattern keyword parameter, or with the notes keyword parameter if you want to pass a chord type. If a string is passed in, the incoming string is parsed for drum syntax and converted to a chord type for storage in the drum type when the drum type is constructed. You can use the name keyword argument to name the drums you write, and you can use the mapping keyword argument to specify a dictionary of characters mapped to drum values, where drum values means the numbers corresponding to the different drums in the general MIDI, and I have written which numbers correspond to the different drums in the drum_types in the database.  

You can use `i` keyword parameter to set the timbre of the drum kit, see the numbers corresponding to the timbre of the drum kit in the general MIDI.  

Also, for chord types you can use the translate function, which uses the same set of syntax as writing drums to write chord types or a piece, just replace the custom characters with note names.

### The composition of drum type

```python
drum(pattern='',
     mapping=drum_mapping,
     name=None,
     notes=None,
     i=1,
     start_time=None,
     default_duration=1 / 8,
     default_interval=1 / 8,
     default_volume=100)
```

- pattern: the string to be parsed to drum beats
- mapping: the drum mapping dictionary, could be customized, the default value is drum_mapping
- name: the name of the drum beat
- notes: the chord type of the drum beat, could be passed in directly as drum beat, the default value is None, which is converting the pattern as drum beat
- i: the instrument type of the drum beat, corresponding to the instrument number of General MIDI
- start_time: the start time of the drum beat, if it is None, then use the start time of the chord type of the drum beat
- default_duration: default duration of drum beat
- default_interval: default interval of drum beat
- default_volume: default volume of drum beat

```python
drum_example = drum('0,1,2,1,0;1,0;1,2,1,{2}')
# This is an example of a drum section with 4 monophonic drums in front, then 2 diatonic drums, then 2 monophonic drums, and finally the whole section repeated 2 times,
# where 0,1,2 means bass drum, pedal and snare respectively, so 0;1 means bass drum and pedal playing at the same time
play(drum_example, 150) # Play the drums at 150bpm
>>> print(drum_example)
[drum] 
[C2, F#2, E2, F#2, C2, F#2, C2, F#2, E2, F#2, C2, F#2, E2, F#2, C2, F#2, C2, F#2, E2, F#2] with interval [0.125, 0.125, 0.125, 0.125, 0, 0.125, 0, 
0.125, 0.125, 0.125, 0.125, 0.125, 0.125, 0.125, 0, 0.125, 0, 0.125, 0.125, 0.125]

# If you use drum type as track in a piece type, you must use the notes attribute of the drum type as chord type,
# and set the corresponding channel number to 9
piano1 = C('Cmaj7') % (1, 1/8)
piece_example = P([piano1, drum_example.notes], [1, 1], channels=[0, 9])
play(piece_example)
```

