# Basic syntax of note type



## Contents

- [Construct a note using its name and octave number](#construct-a-note-using-its-name-and-octave-number)
- [Construct a note by entering the note name directly (note name + number of octaves)](#construct-a-note-by-entering-the-note-name-directly-note-name--number-of-octaves)
- [Set the note length and volume for existing notes](#set-the-note-length-and-volume-for-existing-notes)
- [Converting pitch numbers of notes to note types and getting pitch numbers from note types](#converting-pitch-numbers-of-notes-to-note-types-and-getting-pitch-numbers-from-note-types)
- [To raise or lower a note](#to-raise-or-lower-a-note)
- [The sharp and flat signs of notes are converted into each other](#the-sharp-and-flat-signs-of-notes-are-converted-into-each-other)
- [Combining multiple notes to form a chord](#combining-multiple-notes-to-form-a-chord)
- [Note types have a new music theory function that allows you to add chord names to get chord types](#note-types-have-a-new-music-theory-function-that-allows-you-to-add-chord-names-to-get-chord-types)
- [Note type adds the function of generating chords according to the interval relationship](#note-type-adds-the-function-of-generating-chords-according-to-the-interval-relationship)
- [Usage of dotted notes](#usage-of-dotted-notes)
- [Reset note pitch](#reset-note-pitch)



## Construct a note using its name and octave number

```python
a = note('D', 6)
```

a is the note D6.
Other parameters of note are duration (note length in bars), volume (note strength, corresponding to MIDI 0~127, i.e. any integer between 0 and 127 (including 0 and 127))
For example

```python
a = note('C', 5, 0.5, 100)
```

means that a is note C5, the note length is 0.5 bars, and the note strength is 100.

## Construct a note by entering the note name directly (note name + number of octaves)

We can use the toNote function

```python
toNote('E5')
```

The note E5 is obtained.

A shortened version of the toNote function would be

```python
N('E5')
```

N is the initial capitalization of the note note

## Set the note length and volume for existing notes

```python
a.set(duration, volume) # Returns a new chord type
a % (duration, volume) # Advanced syntax
a = a % (duration, volume) # Write it this way if you want to replace the original note type directly, or just use it alone
```

duration is the note length in bars, volume is the note strength, corresponding to MIDI 0~127, any integer between 0 and 127 (including 0 and 127)

You can set only one of these parameters, (you can leave it blank if you don't set it) or you can set both of them. Using the built-in function set for note types returns a new note type with modified parameters. It is completely independent from the previous note type, because when writing musicpy code, you can build blocks to write parameters such as pitch, note interval, note duration, volume level, etc. in one sentence. This is also one of my requirements for the syntax design of musicpy. The set methods for other music types such as chord types are similar to this, I will explain this in more detail when I talk about chord types.

## Converting pitch numbers of notes to note types and getting pitch numbers from note types

You can convert an integer to a note using the degree_to_note function, for example

```python
degree_to_note(60)
```

Gets a note type C5.


To get the number of pitches of a note type a, you just need

```python
a.degree
```

and that's it


## To raise or lower a note

```python
a.up(n)
```

Raises the a note by n semitones

```python
a.down(n)
```

means lower the a note by n semitones

Advanced syntax:

```python
+++a #(note a raised by 3 semitones)
---a #(note a lowered three semitones)
a + n #(note a raised by n semitones)
a - n #(note a lowered by n semitones)
```

(analogous to the rising and falling notes of the chord class)

## The sharp and flat signs of notes are converted into each other

For example, if the note a has the note name C#, then

```python
~a
```

gets the flat representation of the note a

```python
Db
```

For example, if the note b has the sound name Eb, then

```python
~b
```

gets the sharp representation of the note b

```python
D#
```


## Combining multiple notes to form a chord

For example, if you have four notes a, b, c, and d, you can join them with a + sign to form a chord with the notes a, b, c, and d in that order.

```python
A = a + b + c + d
```

The chord A is composed of the notes a, b, c, d

## Note types have a new music theory function that allows you to add chord names to get chord types

Note types can be passed in as functions or generators to get chord types, e.g.

```python
A3 = N('A3')
>>> print(A3('sus'))
[A3, D4, E4] with interval [0, 0, 0]
# Also supports inversion, polychords, and other C functions that support the syntax of parsed chord names
```

## Note type adds the function of generating chords according to the interval relationship

You can use the `with_interval` function of the note type to specify an interval to form a chord type with two notes. The two notes are the current note type and the note type that forms the specified interval relationship with this note type.

```python
a = N('C5')
>>> a.with_interval(major_seventh) # form a chord representing the major seventh interval of C5
[C5, B5] with interval [0, 0]
```

## Usage of dotted notes

There are several ways you can use dotted notes. First, note types can use the `dotted` function to change the length of a note to the length of a dotted note, allowing you to customize the number of dots attached. Chord types can also use the `dotted` function to change a note, some notes or all notes in a chord type to dotted notes, and you can also customize the number of dots.

In addition, when using the advanced syntax and translate functions to construct chord types and drum types, you can also use dotted notes, with the syntax of adding `. `, you can add as many dots as you want, and the length and spacing of the notes will be calculated based on the number of dots you add.

```python
a = N('C5')

>>> a.duration
0.25

b = a.dotted() # Get the dotted notes of note type a (single dotted)

>>> b.duration
0.375

c = a.dotted(2) # Get the dotted notes of note type a (double dotted)

>>> c.duration
0.4375

# The dotted function for the chord type
dotted(ind=-1, num=1, duration=True, interval=False)

# ind: index of the note that becomes dotted, can be an integer index, starting from 0
# or 'all', which means all notes become dotted, or a list of indices, starting from 0, default is -1

# num: the number of dots, the default is 1

# duration: whether to change the length of the note to a dotted note, default is True

# interval: whether to change the note interval to the length of the dotted notes, the default value is False

a = C('C')

>>> a.get_duration()
[0.25, 0.25, 0.25]

b = a.dotted() # change the last note of chord type a to dotted (single dotted)

>>> b.get_duration()
[0.25, 0.25, 0.375]

c = a.dotted([0, 2]) # turns the 1st and 3rd notes of chord type a into dotted notes (single dotted)

>>> c.get_duration()
[0.375, 0.25, 0.375]

a = chord('C5[.8.;.] , D5[.8;.] , E5[.8.;.] , F5[.8;.]')
# The first and third notes are dotted eighth notes, the second and fourth notes are normal eighth notes

a = translate('C5[.8.;.] , D5[.8;.] , E5[.8.;.] , F5[.8;.]') # Same as above, using the translate function

a = drum('0[.8.;.] , 1, 2[.8.;.] , 1') # Same as above, drum type
```

## Reset note pitch

You can use `reset_pitch` function of the note type to reset the note name of a note type, while the octave remains unchanged, `reset_octave` function to reset the octave of a note type, while the note name remains unchanged, `reset_name` function to reset the note name along with the octave by a note name string like `C5`. All of these functions return a new note instance with the new note pitch, while other attributes remain unchanged.

```python
a = N('C5', duration=2)

b = a.reset_name('A5')

>>> b
A5

>>> b.duration
2

c = a.reset_pitch('E')

>>> c
E5

d = a.reset_octave(3)

>>> d
C3
```

