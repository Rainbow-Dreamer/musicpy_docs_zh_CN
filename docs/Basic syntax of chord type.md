# Basic syntax of chord type



## Contents

- [Setting the interval and length of a chord](#setting-the-interval-and-length-of-a-chord)
- [Get a chord based on its root note and chord name](#get-a-chord-based-on-its-root-note-and-chord-name)
- [Constructing chords by entering note names, note lengths and note spacing](#constructing-chords-by-entering-note-names-note-lengths-and-note-spacing)
- [Chord representation](#chord-representation)
- [Get the inversion of a chord](#get-the-inversion-of-a-chord)
- [Splicing of two chords](#splicing-of-two-chords)
- [Adding notes to a chord](#adding-notes-to-a-chord)
- [Removing a note from a chord](#removing-a-note-from-a-chord)
- [Repeating a chord a certain number of times](#repeating-a-chord-a-certain-number-of-times)
- [Arranging a chord in reverse order](#arranging-a-chord-in-reverse-order)
- [Get the interval relationship of a chord](#get-the-interval-relationship-of-a-chord)
- [Indexing a chord, slicing (by indexing worth to a certain note of a chord, or a fragment of a range)](#indexing-a-chord-slicing-by-indexing-worth-to-a-certain-note-of-a-chord-or-a-fragment-of-a-range)
- [Raise or lower a chord](#raise-or-lower-a-chord)
- [Sequencing a chord](#sequencing-a-chord)
- [Transpose a chord (or a piece)](#transpose-a-chord-or-a-piece)
- [Get the names of all notes of a chord](#get-the-names-of-all-notes-of-a-chord)
- [Merge and join two chords or two musical fragments (voices merge, voices splice)](#merge-and-join-two-chords-or-two-musical-fragments-voices-merge-voices-splice)
- [Making changes to the notes within a chord](#making-changes-to-the-notes-within-a-chord)
- [Deleting some notes of a chord, or inserting a new note in a certain position](#deleting-some-notes-of-a-chord-or-inserting-a-new-note-in-a-certain-position)
- [Adding a lowest note to a chord (adding a bass note)](#adding-a-lowest-note-to-a-chord-adding-a-bass-note)
- [Find what note is the first note in a chord](#find-what-note-is-the-first-note-in-a-chord)
- [Adding a rest to the end of a chord](#adding-a-rest-to-the-end-of-a-chord)
- [Chord name parsing structure](#chord-name-parsing-structure)
- [More advanced manipulation of a chord (alterations, omissions, polychords)](#more-advanced-manipulation-of-a-chord-alterations-omissions-polychords)
- [Invert a note of a chord to the highest note](#invert-a-note-of-a-chord-to-the-highest-note)
- [Invert a note of a chord to the lowest note](#invert-a-note-of-a-chord-to-the-lowest-note)
- [Transpose all the notes of a chord to within one octave](#transpose-all-the-notes-of-a-chord-to-within-one-octave)
- [Normalizing a chord](#normalizing-a-chord)
- [Sorting a chord by pitch number](#sorting-a-chord-by-pitch-number)
- [Get the negative harmony of a chord according to the set scale](#get-the-negative-harmony-of-a-chord-according-to-the-set-scale)
- [Extracting the notes within a chord by index value, including the higher and lower octaves of the note shift](#extracting-the-notes-within-a-chord-by-index-value-including-the-higher-and-lower-octaves-of-the-note-shift)
- [Build a set of chord processing rules for any chord type](#build-a-set-of-chord-processing-rules-for-any-chord-type)
- [View a list of note intervals and note lengths for a chord type](#view-a-list-of-note-intervals-and-note-lengths-for-a-chord-type)
- [View a list of note volumes for a chord type](#view-a-list-of-note-volumes-for-a-chord-type)
- [chord function can parse the string of notes](#chord-function-can-parse-the-string-of-notes)
- [The toNote function can now parse strings of note names without a specified pitch number](#the-toNote-function-can-now-parse-strings-of-note-names-without-a-specified-pitch-number)
- [How to build an empty chord](#how-to-build-an-empty-chord)
- [Advanced syntax for setting length and volume of notes](#advanced-syntax-for-setting-length-and-volume-of-notes)
- [Advanced syntax for chord setting note length and note interval and note volume](#advanced-syntax-for-chord-setting-note-length-and-note-interval-and-note-volume)
- [Another way to set the volume of a chord type](#another-way-to-set-the-volume-of-a-chord-type)
- [Get only note types of a chord type](#get-only-note-types-of-a-chord-type)
- [Get the chord type according to the guitar's fret count and guitar tuning standard](#get-the-chord-type-according-to-the-guitars-fret-count-and-guitar-tuning-standard)
- [Calculate the actual playing time of a chord type according to the specified BPM](#calculate-the-actual-playing-time-of-a-chord-type-according-to-the-specified-BPM)
- [Slicing chord types by range of bars](#slicing-chord-types-by-range-of-bars)
- [Calculate the total number of bars of a chord type](#calculate-the-total-number-of-bars-of-a-chord-type)
- [Slice chord types according to the range of realistic playing times](#slice-chord-types-according-to-the-range-of-realistic-playing-times)
- [Extract the first n bars of a chord type](#extract-the-first-n-bars-of-a-chord-type)
- [Extract the part of a chord type for a particular bar](#extract-the-part-of-a-chord-type-for-a-particular-bar)
- [Extract the contents of each bar of a chord type](#extract-the-contents-of-each-bar-of-a-chord-type)
- [Count the number of occurrences of a note name within a chord type](#count-the-number-of-occurrences-of-a-note-name-within-a-chord-type)
- [Get the most occurrences of a chord type](#get-the-most-occurrences-of-a-chord-type)
- [Unify all sharp and flat signs of notes of a chord type](#unify-all-sharp-and-flat-signs-of-notes-of-a-chord-type)
- [Re-quantize the chord type with note length and note interval according to the tempo change inside](#re-quantize-the-chord-type-with-note-length-and-note-interval-according-to-the-tempo-change-inside)
- [Calculating the range of bars between two indices in a chord type](#calculating-the-range-of-bars-between-two-indices-in-a-chord-type)
- [Extracting real-time tempo changes or pitch bends in a chord type](#extracting-real-time-tempo-changes-or-pitch-bends-in-a-chord-type)
- [Building a chord progression](#building-a-chord-progression)
- [View information about the musical analysis of a chord type](#view-information-about-the-musical-analysis-of-a-chord-type)
- [Getting a chord type for a sus](#getting-a-chord-type-for-a-sus)
- [Delay the same chord type n times or play it n times at intervals](#delay-the-same-chord-type-n-times-or-play-it-n-times-at-intervals)
- [Input chord name parsing with new add and sus syntax](#input-chord-name-parsing-with-new-add-and-sus-syntax)
- [Unify accidentals of chord type](#unify-accidentals-of-chord-type)
- [Filter the notes that meet the specified conditions in the chord type](#filter-the-notes-that-meet-the-specified-conditions-in-the-chord-type)
- [Filter the notes within the specified pitch range of the chord type](#filter-the-notes-within-the-specified-pitch-range-of-the-chord-type)
- [Generate chord types according to the interval relationship](#generate-chord-types-according-to-the-interval-relationship)
- [Find a certain degree of a chord type](#find-a-certain-degree-of-a-chord-type)
- [Confirm that a note is what degree of a chord](#confirm-that-a-note-is-what-degree-of-a-chord)
- [Obtain the chord voicing according to the degree of the chord](#obtain-the-chord-voicing-according-to-the-degree-of-the-chord)
- [Adjust the notes of the current chord type to a place closer to the notes of another chord type](#adjust-the-notes-of-the-current-chord-type-to-a-place-closer-to-the-notes-of-another-chord-type)
- [Make arpeggios quickly](#make-arpeggios-quickly)
- [Reset the overall octave of chord type](#reset-the-overall-octave-of-chord-type)
- [Construct chord types with other MIDI messages](#construct-chord-types-with-other-MIDI-messages)
- [Write chord types by polyphony](#write-chord-types-by-polyphony)
- [Concatenate chord types in a list](#concatenate-chord-types-in-a-list)
- [Distribute multiple notes evenly in proportion to their length to the specified bar length](#distribute-multiple-notes-evenly-in-proportion-to-their-length-to-the-specified-bar-length)
- [Use translate function to write chord types according to drum syntax](#use-translate-function-to-write-chord-types-according-to-drum-syntax)
- [Reset the overall pitch of chord types](#reset-the-overall-pitch-of-chord-types)
- [Chord type extracts notes from the index list to form a new chord type](#chord-type-extracts-notes-from-the-index-list-to-form-a-new-chord-type)
- [Replace notes and chords in a chord type](#replace-notes-and-chords-in-a-chord-type)



## Setting the interval and length of a chord

For example, if you have a chord A and you want to set the note interval to 1 bar and the note length to 1.5 bars, you can use the built-in method set of the chord class. The first parameter of set is the note length duration, and the second parameter is the note interval interval.

```python
A.set(1.5, 1)
```

The result is a new chord type with all the note lengths of chord A set to 1.5 and all the note intervals set to 1. Note that instead of modifying the internal properties of chord A, a new chord is returned with the modified internal properties of chord A. If you have different settings for note length or note spacing for each note, you can pass in a list, for example

```python
A.set([0.5, 0.5, 1, 1], [1, 1, 2, 2])
```

Returns a chord with note lengths of 0.5, 0.5, 1, 1 (in bars) and note intervals of 1, 1, 2, 2 (in bars).

Advanced syntax:

```python
A % (1.5, 1)
```

## Get a chord based on its root note and chord name

Introducing a very handy function: getchord. This function allows you to get the type of chord you want, simply by giving the root note of the chord and the chord type. Since the name of this function is relatively a bit long, you can also call getchord with the name chd (short for chord). e.g.

```python
Am7 = chd('A', 'm7')
```

This way we create a minor seventh chord of A. It is represented as

```python
[A5, C6, E6, G6] with interval [0, 0, 0, 0]
```

There are many types of chord types, which can be found in the chordTypes file in database.py. You can also add your own chord types by writing the chord name and the corresponding chord interval in the same format as in chordTypes.
The first parameter of the chd function is the root note of the chord, and if no specific pitch is specified (e.g. C5, D6 is a note with a specific pitch), then by default it is treated as the 4th octave, e.g. chd('A', 'm7'), which is equivalent to
chd('A4', 'm7'), and if you specify a specific pitch for the root note, for example, if you now want to write a major 7th chord with root note D6, you can write.

```python
Dmaj7 = chd('D6', 'maj7')
```

In addition, the chd function has a very large number of parameters that can be used to set the chord's omission, altered notes, added notes, as well as the exact length of each note of the chord, the interval of the notes, etc. You can even enter the intervals of the notes to build the chords (you can also set the value of cummulative to determine whether you want to enter the interval of each note to the root note or the interval between each two notes). In musicpy, the name of each interval is already defined and can be used directly, for example, the value of major_third is 4, which is the number of semitones in the major third. For example, to construct a C major seventh chord according to the intervals, you can write it like this

```python
chd('C', interval=[major_third, perfect_fifth, major_seventh])
```

The result is

```python
[C5, E5, G5, B5] with interval [0, 0, 0, 0]
```

When a chord has an interval of all 0s, the chord is all notes together at the same time. If an interval is 0, it means that two notes start playing together at the same time.

## Constructing chords by entering note names, note lengths and note spacing

For example, if we want to build a major seventh chord with a root note of C5, a note spacing of one bar between each note, and a note length of two bars, then we can write it like this

```python
chord(['C5','E5', 'G5', 'B5'], interval=1, duration=2)
```

If the note spacing and note length are not the same, then you can pass a list in, e.g.

```python
chord(['C5','E5', 'G5', 'B5'], interval=[0.5, 0.5, 0, 2], duration=[1, 2, 0.5, 1])
```


## Chord representation

In musicpy, a chord type is represented as

```python
[note1, note2, note3, ...] with interval [interval1, interval2, interval3, ...]
```

For example, A is a C major seventh chord with a root note of C5.

```python
A = chd('C5', 'maj7')
```

The chord A is represented as

```python
[C5, E5, G5, B5] with interval [0, 0, 0, 0]
```


## Get the inversion of a chord

inversion gets the inversion of a chord, with a parameter num representing the inversion of the chord. For example.

```python
chd('A', 'm7').inversion(1) 
```

You can get the first inversion of the minor seventh chord of A, expressed as follows.

```python
[C6, E6, G6, A6] with interval [0, 0, 0, 0]
```

Advanced syntax: (I designed symbolic syntax for many functions of musicpy to write faster)

```python
chd('A', 'm7') / 1
```

The result is the same as chd('A', 'm7').inversion(1).
You can also write the note you want to invert after '/', and if the note is within a chord it will invert that note to the lowest note, e.g.

```python
chd('A', 'm7') / 'E'
```

The result obtained is

```python
[E6, G6, A6, C7] with interval [0, 0, 0, 0]
```

Which is the second inversion of the A minor seventh chord.


## Splicing of two chords

If you want to splice two chords (say a and b), then you just need

```python
a + b
```

and that's it. What you get is the new chord after splicing.

## Adding notes to a chord

If you want to add a note x to chord A, then just

```python
A + x
```

and that's it. What you get is a new chord with the note x added.

## Removing a note from a chord

If you want to remove a note x from a chord A, then you just need

```python
A - x
```

and that's it. What you get is a new chord with the note x removed.

## Repeating a chord a certain number of times

If you want to repeat a chord A n times, then you just need

```python
A * n
```

and that's it. What you get is a new chord repeated n times.

Note that if chord A is a chord with all intervals of 0 (all notes start playing together at the same time), then A * n will make the notes of the repeated A's overlap.
In this case, if you want to make multiple chord A repeats not overlap, then you need to write

```python
A % n
```

This syntax will make A repeat without overlapping the previous chord.


## Arranging a chord in reverse order

If you want to arrange the notes inside a chord A in reverse order, then simply

```python
A.reverse()
```

and that's it. What you get is the new chord in reverse order.

Advanced syntax:

```python
~A
```

You can also get the chord A in reverse order.

```python
'''
Note that chord A here is not necessarily just a chord in music theory, but can be a melody, or even a melody with a chord underlay. This is because the definition of a chord class in musicpy is "a collection of notes". Therefore, an instance of a chord class in musicpy can store information about a whole piece. This information includes all the notes in the piece, the length of each note, the velocity, and the interval between each two notes.
'''
```

## Get the interval relationship of a chord

The intervalof function returns the interval relationship between the constituent notes of a chord. The parameter cumulative is set to True to return the interval between all the constituent notes of the chord (except the root note) and the root note, and False to return the interval between the lowest and highest notes of the chord. The default value is True, e.g.

```python
chd('C','maj').intervalof()
```

You will get [4,7], which means that there are four semitones between the second note and the root note inside the C major triad (C,E,G) (major third), and seven semitones between the third note and the root note (perfect fifth). If you want to see the names of the intervals in music theory, then you can set the parameter translate to True, then you can see the names of the corresponding intervals. For example.

```python
chd('C','maj').intervalof(translate = True)
```

will get ['major third', 'perfect fifth'], which means major third and perfect fifth.

When cummulative is set to False returns the interval between every two notes of the chord from low to high, e.g.

```python
chd('C','maj').intervalof(translate = True, cummulative=False)
```

will get ['major third', 'minor third'], which means that a C major third chord is composed of a major third and a minor third.

## Indexing a chord, slicing (by indexing worth to a certain note of a chord, or a fragment of a range)

For example, now we have a chord A.

```python
A = chd('C', 'maj7')
```

If we want to get the 1st note of the chord A, then we write

```python
A[0]
```

If we want to get the 2nd note of the chord A, then write.

```python
A[1]
```

If we want to get the last note of chord A, then write.

```python
A[-1]
# Note that the index value of the chord starts from 0 as the first note, that is, 0 corresponds to the first note of the chord, 1 corresponds to the second note of the chord, and so on.
# To get the penultimate first note of the chord, the index value is -1, the penultimate note is -2, and so on.
```

If you want to intercept a part of a chord/melody/track A, then just A

```python
[start position:end position]
```

and that's it. What you get is a new chord with the notes in it as the intercepted part.

For example, if chord B has 6 notes, and you want to get the first 5 notes of chord B, then you can write.

```python
B[:5]
# The ending position 6 is not included, so it corresponds to the 1st, 2nd, 3rd, 4th, and 5th notes of B
```

If you want to get the 2nd to 5th notes of chord B, then you can write.

```python
B[1:5]
# Note that here the left is closed and the right is open, which means that the ending position 5 is not included in the result, so B[1:5] is extracting the 2nd to the 5th note of B, for a total of 4 notes.
```

If you want to get the 2nd note of chord B and all the notes after it, then you can write.

```python
B[1:]
```

To get the last 3rd to the last 1st note of the chord B, write

```python
B[-3:]
```

## Raise or lower a chord

If you want to raise or lower a chord (or of course a melody or musical fragment), then you can use the up and down functions. Given a chord A, the

```python
A.up(x)
```

means to raise the overall chord A by x semitones (x can also be a negative number, which means to lower the absolute value of x by one semitone), and

```python
A.down(x)
```

means to lower the chord A by x semitones.

Advanced syntax:

```python
+A
```

means to raise the whole chord A by one semitone, as many plus signs as there are semitones, e.g.

```python
+++A
```

means raise the chord A by three semitones overall, but since the chord A will be overwritten again with each plus sign, (equivalent to A.up().up().up())

So I also devised a one-step writeup that

```python
A + 3
```

means to raise the chord A by three semitones overall, which is equivalent to A.up(3),

```python
-A
```

means to lower the overall chord A by one semitone, as many minus signs as there are semitones, e.g.

```python
---A
```

means to lower the chord A by three semitones, which is equivalent to A.down().down().down()

```python
A - 3
```

means to lower the chord A by three semitones overall, which is equivalent to A.down(3)

If you only want to raise a certain note in the chord, then just

```python
A.up(x, k)
```

or

```python
A + (x, k)
```

Lowering then is

```python
A.down(x, k)
```

or

```python
A - (x, k)
```

where k denotes the index of the note. For example

```python
chd('C','maj').up(1)
```

gives a new chord, each note of which is a semitone higher than the previous chord.
The notes of the chord are C#, F, G#.

```python
chd('C','maj').up(1,0)
```

will get a new chord with only the first note raised a semitone and the notes of the chord are C#, E, G.

If you want to raise and lower each note differently, then you can let x equal a list where each element of the list is the number of chromatic notes raised and lowered.
For example.

```python
A.up([1,2,0,-1])
```

or

```python
A + [1,2,0,-1]
```

means to raise the first four notes of the chord A by one semitone, raise them by two semitones, leave them unchanged, and lower them by one semitone.
The down case goes on like that.

## Sequencing a chord

If you want to sequence the constituent notes of a chord A, then just

```python
A.sort(x)
```

where x is a list documenting the order.
For example, if you have a minor seventh chord of A, and for its 4 constituent notes A,C,E,G you want to sort them in the order of CEAG, then write

```python
A.sort([2, 3, 1, 4])
```

and that works.

Advanced syntax:

```python
A / [2, 3, 1, 4]
```

Equivalent to A.sort([2, 3, 1, 4])

## Transpose a chord (or a piece)

If you want to transpose a melody (including the chord underlay), then you can use the modulation function. For example, if you want to transpose a piece of music A right now, then you can

```python
A.moulation(the_previous_key, the_key_to_transpose_to)
```

We'll talk about how to represent a tonic next.
For example, if you have a music clip p in the key of A major, and you want to shift to A minor, you can write:

```python
p.modulation(scale('A', 'major'), scale('A', 'minor'))
```

This will shift the music clip p from A major to A minor.

## Get the names of all notes of a chord

First, if you just get a list of the notes of chord A, then you just need

```python
A.notes
```

to get a list of the notes of chord A. For example, if A is an A minor seventh chord, then you get

```python
[A5, C6, E6, G6]
```

If you want to get a list of all the note names of the chord A, then you can use one of the built-in functions names of the chord, for example

```python
chd('A','m7').names()
```

and you get

```python
['A', 'C', 'E', 'G']
```

## Merge and join two chords or two musical fragments (voices merge, voices splice)

The built-in function add of the chord type can merge two chords (or two music fragments).
For example, there are two pieces of music (the chord type itself can also be a piece of music) A and B.

The parameter mode can be used to select the mode of merging.

When mode == 'head', the

```python
A.add(B, mode='head')
```

You can get the new music clip after merging the two music clips A and B. The beginning of B is aligned to the beginning of A, that is, the music clip where both A and B are playing from the beginning at the same time.

The mechanism of the merge is to recalculate the interval of the merged notes, and then to reorder the notes of A and B according to the calculated interval.
The add function also has a parameter, start, which can be used to set where B should be merged from A, i.e. where the beginning of B should be aligned to A.
The unit is bars, in other words, how many bars after the beginning of A does B start to play. For example.

```python
A.add(B, mode='head', start=8)
```

You can get the merged piece of music that B plays from bar 8 of A.

Advanced syntax:

```python
A & B
A & (B, start)
```

When mode == 'tail', the

```python
A.add(B, mode='tail')
```

You can get the new music clip after the music clip B is appended to the music clip A. Note, however, that this mode appends the note list of B directly to the note list of A after
If the last few notes of music clip A have 0 intervals, then the beginning notes of music clip B may overlap with the last few notes of A. If you are sure that the last few notes of A are not 0 intervals, then this mode can be used safely.
Equivalent to

```python
A + B
```

When mode == 'after', the

```python
A.add(B, mode='after')
```

You can get the new music clip after the music clip B is appended to the music clip A. The difference with the tail mode is that this mode will specifically calculate whether more notes need to be spaced between A and B
This mode differs from the tail mode in that it specifically calculates whether more notes need to be spaced between A and B to avoid the situation where the end of A and the beginning of B overlap in some cases in the tail mode. So it is best to use this mode when you are not sure if the last few notes of A are zero.

Advanced syntax:

```python
A // B
```

or

```python
A | B
```

## Making changes to the notes within a chord

I've already talked about how to access the notes within a chord by index value, e.g. A[0] will give you the first note of chord A.
If you want to modify the notes within a chord, for example, if chord A is a C major seventh chord

(in the original form, C, E, G, B) We want to change the second note (third) of chord A to F, that is, turn chord A into a maj7sus4 chord, we can write it like this

```python
A[1] = 'F5'
```

Then we print the chord A, again

```python
[C5, F5, G5, B5] with interval [0, 0, 0, 0]
```

You will see that the second note of chord A has changed from E5 to F5.
Note that the changed note can be either a note type (note('A', 5) as obtained by the note initialization function) or a string representing the note.
But it must have an octave, not just the note name, like 'F' can't be used here, it must have an octave, like 'F5'.

## Deleting some notes of a chord, or inserting a new note in a certain position

You can refer to the logic of python's list, using

```python
del A[n]
```

You can delete the nth note of a chord A, using

```python
A.pop()
```

You can remove the last note of a chord A and return the last note of the chord A, using

```python
A.insert(i, b)
```

The note b can be inserted at the i-th position of the chord A

```python
A.append(b)
```

You can add the note b to the chord A
In addition, the extend, remove functions use the same logic as the built-in methods of the same name in the list.
When you want to remove a note from chord A, you can use the '-' sign to do so, for example

```python
A - 'B5'
```

If the chord A has the note B5, a new chord with the note B5 removed is returned, if not, the unmodified chord A is returned.

## Adding a lowest note to a chord (adding a bass note)

For example, if chord A is a G major chord (G major triad) with the notes G5, B5, D6, and we want to add a C note as the lowest note to the chord A.
Then we can write (the parameter could be a string or a note instance)

```python
A.on('C5')
```

The result is a new chord with C5 as the lowest note.
This is expressed as

```python
[C5, G5, B5, D6] with interval [0, 0, 0, 0]
```

Advanced syntax:

```python
A % 'C5'
```

## Find what note is the first note in a chord

For example, if the chord A is an Fmaj9 chord with the root note F5, in the original position, and you want to find what note 'E6' is in, then you can write

```python
A.index('E6')
```

The result is 3, which means that the note E is the 4th note of the Fmaj9 chord.
If the note is not in the chord, -1 is returned.
You can also omit the number of octaves and just write the name of the note to find it, returning the position of the first note with the same name, e.g.

```python
A.index('G')
```

The result is 4, which means that the note G is the fifth note of the Fmaj9 chord.

## Adding a rest to the end of a chord

If you want to add an n-bar rest to the chord A, then you can use the built-in function rest of the chord class

```python
A.rest(n)
```

which adds a rest of n bars to chord A. The result is a new chord with chord A plus a rest of n bars.

Advanced syntax:

```python
A // n
```

or

```python
A | n
```

Regarding the current definition of rest in musicpy, rest itself is a data structure that can be constructed with `rest(duration)` and can be added to chord types, but it is only reflected in the note interval and does not exist in the note list of the chord type itself; rests are only meaningful when constructing chord types.

## Chord name parsing structure

The trans function can be used to parse the full chord name and return the corresponding chord. It supports root position chord representation, inverted chord representation, polychord representation, etc.
The first parameter of the trans function is the chord name, the second parameter is the pitch of the root note of the chord (the default value is 4), and the third parameter is the duration.
The third parameter is duration (the length of the note, default is 0.25), and the fourth parameter is interval (the interval of the note, default is None, the returned chord interval is 0).
For example

```python
trans('Dmaj7')
```

You can get the Dmaj7 chord

```python
[D5, F#5, A5, C#6] with interval [0, 0, 0, 0]
```

```python
trans('F/C')
```

You can get the second inversion of the F major triad

```python
[C5, F5, A5] with interval [0, 0, 0]
```

The inversion parsing can also take numbers, for example

```python
trans('C/1')
```

You can get the first inversion of the C major triad E, G, C

```python
trans('C/-1')
```

You can get the C major triad by putting the 1st note to the highest note of the chords E, G, C

```python
trans('C/1!')
```

You can get the inversion E, C, G for a C major triad that puts the second note to the lowest note (as opposed to the traditional inversion)

```python
trans('C')
```

You can get a C major triad

```python
[C5, E5, G5] with interval [0, 0, 0]
```

```python
trans('Am/Gm')
```

You can get a polychord with the A minor triad over the G minor triad

```python
[G5, A#5, D6, A6, C7, E7] with interval [0, 0, 0, 0, 0, 0, 0]
```

```python
trans('G/C', 6, 1, 1)
```

You can get a G major triad plus a C chord on the lowest note

```python
[C6, G6, B6, D7] with interval [1, 1, 1, 1]
```

A shortened way of writing the trans function.

```python
C(chord name, other arguments)
```

C is the initial capitalization of chord

## More advanced manipulation of a chord (alterations, omissions, polychords)

For example, if chord A is a C7 chord, then to get a C7#9 chord, you can write it like this

```python
A('#9')
```

The result is

```python
[C5, E5, G5, A#5, D#6] with interval [0, 0, 0, 0, 0]
```

The C7b9 chord is

```python
A('b9')
```

For example, if B is a Cmaj9 chord, then if you want to omit the third, you can write it like this

```python
B('omit3')
```

or

```python
B('no3')
```

Multiple altered and omitted notes can be separated by English commas, for example

```python
A('#5, b9, omit3')
```

If chord A is a C7 chord, then what you get is a C7#5b9 chord omitting a third.

To build a polychord, you only need to join two chords with '/', except in the case of two chords stacked on top of each other
There is also a polychord formed by adding a lowest note underneath a chord, in both cases a '/' is sufficient.
For example

```python
C('Amaj7') / 'D'
```

The result is

```python
[D5, A5, C#6, E6, G#6] with interval [0, 0, 0, 0, 0]
```

That is, the Amaj7 chord with D as the lowest note underneath.
The case of a polychord of two chords, e.g.

```python
C('A') / C('G')
```

yields

```python
[G5, B5, D6, A5, C#6, E6] with interval [0, 0, 0, 0, 0, 0, 0]
```

This is a polychord of G major triad superimposed under A major triad.

In the chord name parsing structure, it is possible to enter and parse the advanced operations of the chord directly by simply separating them with commas, for example, the C major 7th chord omitting the 5th

```python
C('Cmaj7,no5')
```

The result is

```python
[C4, E4, B4] with interval [0, 0, 0]
```

## Invert a note of a chord to the highest note

For example, if chord A is a C major triad in the original positions C, E, G, and you want to invert E to the highest note, then you can write

```python
A.inversion_highest(2)
```

That is, put the second note of the chord A to the highest note, and the resulting chord will have the notes C, G, E

Advanced syntax:

```python
A / -2
```

or

```python
A ^ 2
```

(A / -n means invert the nth note of chord A to the highest note)

## Invert a note of a chord to the lowest note

For example, if chord A is a C major triad in the original position C, E, G, and you want to invert E to the lowest note, then you can write

```python
A.inv(2)
```

or

```python
A.inv('E')
```

That is, putting the second note of the chord A to the lowest note gives a chord with the notes E, C, G.
Note that this is different from the normal first inversion of a C major triad (E, G, C).

Generally speaking, a chord is inversioned in the classical sense by raising the lowest note (the root note in the first inversion) by an octave.

However, a more generalized inversion requires only that the lowest note is the one inversioned.

Advanced syntax:

```python
A @ 2
```

```python
A @ 'E'
```

## Transpose all the notes of a chord to within one octave

For example, if chord A has 5 notes, and the highest ones are distributed in octaves higher than the lower ones, then

```python
A.inoctave()
```

will return a chord type that puts all the notes of chord A into the same octave, with the number of octaves being the number of octaves of the first note of chord A.

## Normalizing a chord

To normalize a chord A, you can use the standardize function

```python
A.standardize()
```

The new chord is obtained after standardizing chord A.

The definition of standardization here is

    1. If there are repeated note names within the chord (even in different octaves),
       then only the lowest pitch (smallest number of octaves) is kept.
    2. Limit all notes to within two octaves, i.e. to the 15th of the lowest note,
       and if there are any notes that are more than two octaves higher than the lowest note,
       then subtract these notes by an octave down until all the notes are within two octaves from the lowest note.
    3. Standardize all the note names into a format with no sharp and flat signs or only # signs,
       and if there are b signs, convert them to equal pitch names with # signs in the 12 equal temperament.

After standardization, the pitch of the lowest note of the chord is the same as before standardization. The chord returned is the new chord after the chord A has been normalized.

## Sorting a chord by pitch number

If the notes in a chord are not sorted by pitch, for example, if the notes of chord A are E5, C5, G5, then you can write

```python
A.sortchord()
```

The return is a new chord with the notes of chord A sorted from lowest to highest in pitch.

## Get the negative harmony of a chord according to the set scale

For example, if chord A is a C major 7th chord and you want to get the negative harmony of chord A with respect to the C major scale, then you can write.

```python
negative_harmony(scale('C', 'major'), A)
```

What you get is a new chord composed of the notes of chord A about the negative harmony of the C major scale after the transformation.

Other parameters of the negative_harmony function.

get_map_dict, when True, returns a dictionary of the notes of the first parameter scale mapped to the negative harmony.

When False, if no chord type is passed in, then the negative harmonic scale type is returned for the passed in scale type, e.g.

```python
negative_harmony(scale('C', 'major'))
```

gets

```python
scale name: C5 minor scale
scale intervals: [2, 1, 2, 2, 1, 2, 2]
scale notes: [C5, D5, D#5, F5, G5, G#5, A#5, C6]
```

There is also a parameter sort, which, when True, will sort the notes by pitch from lowest to highest after converting the chord a to a negative harmonic version. The default value is True.

## Extracting the notes within a chord by index value, including the higher and lower octaves of the note shift

For example, if chord A is a C major triad with C5, E5, G5, we want to get a string of Alberti basses, i.e. a left-hand arpeggio accompaniment of a triad played in alternating root, fifth, third and fifth, we want to get a string of notes C5, G5, E5, G5, C6, G5, E5, G5 ( Note that the fifth note is C6, an octave above the root note, which is a typical left-hand arpeggio accompaniment of Alberti's bass. This string of notes can be easily extracted from chord A using the get function of the chord type.

```python
A.get([1, 3, 2, 3, 1.1, 3, 2, 3])
```

The result obtained is

```python
[C5, G5, E5, G5, C6, G5, E5, G5] with interval [0, 0, 0, 0, 0, 0, 0, 0, 0, 0]
```

Here 1, 2, 3 are the notes in chord A. The decimal numbers 1.1 indicate the number of octaves (the number before the decimal point) that the first note (the number after the decimal point) is raised.

If you want a note to be lowered by a few octaves, then just write a negative decimal number, for example -2.1 for the note after lowering the second note by one octave.

Advanced syntax:

```python
A @ [1, 3, 2, 3, 1.1, 3, 2, 3]
```

## Build a set of chord processing rules for any chord type

The easiest way to do this is to use the syntax of python's own lambda function, e.g.

```python
rules = lambda x: C(x) @ [1, 2, 3, 1.1, 2.1, 3, 1.1, 2.1] % (1/8, 1/8) * 2
```

Now rules is a chord processing rule (used as a function) that allows you to enter a chord name and then form a new chord type (a chord accompaniment) with this chord in the order of 1st note, 2nd note, 3rd note, 1st note 1 octave higher, 2nd note 1 octave higher, 3rd note, 1st note 1 octave higher, 2nd note 1 octave higher, and then put this Then set the length of each note of the chord accompaniment to 0.5 bars and the interval of each note to 0.5 bars, and repeat the chord accompaniment twice.

```python
A = rules('Cmajor') | rules('C7sus4') | rules('G7/B') - octave | rules('Cmajor')
```

Now we can use the chord processing rules rules to process the C major triad, the C7sus4 chord, the G7/B chord, and the C major triad, and then connect them with the '|' sign to form a little piece of music.

Play part A at 115 BPM (speed)

```python
play(A, 115)
```

In addition to the lambda function that comes with python, you can also use the function exp inside musicpy to implement the construction of chord rules.

The exp function has two modes, 'tail' and 'whole'. In the tail mode, a processing rule is passed in that follows a chord type, for example

```python
rules = exp('up(2).reverse()', mode='tail')
```

At this point rules is a function that takes a chord type and raises it by two semitones overall, then arranges the notes in reverse order. rules itself is a function that can be passed directly into the chord type, e.g.

```python
rules(C('Dmaj7'))
```

In whole mode, just write the same as the expression of the lambda function. The argument obj_name is the name of the lambda variable, and the default value is 'x', for example

```python
rules = exp('(x / 1) % ([1/4, 1/2, 1/2], [1/4, 1/4, 1/2])', mode='whole')
```

At this point rules is a chord processing rule (which is a function that can be passed directly into the chord type) that gets a chord type inversion 1, and then sets the note lengths of the chords to 1/4, 1/2, 1/2 (in bars), respectively.

The note intervals are set to 1/4, 1/4, 1/2 (in bars).

Using the lambda function to express this is

```python
rules = lambda x: (x / 1) % ([1/4, 1/2, 1/2], [1/4, 1/4, 1/2])
```

## View a list of note intervals and note lengths for a chord type

```python
a = C('Dmaj7')
## View a list of note intervals for a chord type
>>> print(a.interval)
[0, 0, 0, 0]

# View a list of note lengths for a chord type
>>> print(a.get_duration())
[0.25, 0.25, 0.25, 0.25]
```

## View a list of note volumes for a chord type

```python
a = C('Dmaj7')
a.setvolume([80,80,100,100])
# View a list of note volumes for a chord type
>>> print(a.get_volume())
[80, 80, 100, 100]
```

## chord function can parse the string of notes

For example, now we want a C major seventh chord with C5 as the root note, so we can write

```python
chord('C5, E5, G5, B5')
```

or

```python
chord('C, E, G, B', rootpitch=5)
```

The value of rootpitch defaults to 4, so if you want a C major seventh chord with C4 as the root note, you can just write

```python
chord('C, E, G, B')
```

The chord function takes a string with no pitch number at all, and normalizes the chord output according to the standardize function.
That is, it arranges the pitches of the notes in the string in the order of their names, and then forms the chord in the original position, for example

```python
chord('C, E, G, B, D')
```

will give you

```python
[C4, E4, G4, B4, D5] with interval [0, 0, 0, 0, 0]
```

The chord function can take either a string of sound names or a list of strings of sound names, for example

```python
chord(['C', 'E', 'G', 'B', 'D'])
```

will get

```python
[C4, E4, G4, B4, D5] with interval [0, 0, 0, 0, 0, 0]
```

Of course, it's most straightforward to receive a list of note types, like

```python
chord([N('C'), N('E'), N('G'), N('B'), N('D')])
```

Note, however, that if the chord function receives a list of note types or strings with a definite number of pitches  
then no chord normalization is performed, because the pitch numbers of all notes are determined at this point.  
If you want to receive a string without chord normalization in the chord function, you can specify the pitch of the notes manually, for example

```python
chord(['C4', 'E4', 'G4', 'B4', 'D4'])
```

or

```python
chord('C4, E4, G4, B4, D4')
```

In addition, the chord function can take strings or lists of strings that are a mixture of notes without a specified pitch number and notes with a specified pitch number.

## The toNote function can now parse strings of note names without a specified pitch number

Previously, the string received by the toNote function (or by the N function) for a note name had to specify a pitch number, for example

```python
N('C5')
```

Now, with my modification, you can enter a string that does not specify a pitch number, and then you can specify the pitch number using the pitch parameter.
The default value of pitch is 4, and you can choose to set it or not, for example

```python
N('B')
```

will get the note type B4, and

```python
N('B5')
```

and

```python
N('B', pitch=5)
```

will both get the note type B5.

## How to build an empty chord

If you want to build an empty chord with no notes, you can write

```python
chord([])
```

will give you

```python
[] with interval []
```

An empty chord can be used as an initial chord for adding new chords in a loop, or as a chord type when treated as a piece (you can add new melodies or chord notes to it), for example

```python
chord([]) | C('Am') | C('F') | C('G')
```

## Advanced syntax for setting length and volume of notes

When using the toNote function or the N function, it is possible to write the note length and volume in a string with a new syntax
For example, now we want a note type with note length of 1 and volume of 80 for C5, so we can write

```python
N('C5(1;80)')
```

If you only want to set the note length, then you can write

```python
N('C5(1)')
```

Note that the symbol separating the arguments inside the brackets must be `;`.
Then the brackets can be any of parentheses(), middle brackets[], or curly brackets{}, for example

```python
N('C5(1;80)')
```

can also be written as

```python
N('C5[1;80]')
```

or

```python
N('C5{1;80}')
```

All arguments inside the brackets can have spaces between them, except for the left bracket after the note name, which must be immediately after the note name.
Note length and volume can be integers, decimals or fractions. I also added a syntactic sugar here, if you want to input notes like 2nd, 4th, 8th, 16th, etc.
then you can write `.n` for nth note, which is the equivalent of 1/n note length. If it's a note length like 3/4, then you can just write 3/4, because the toNote function supports fractional representation.
For example, now we want a D5 quarter note with no volume setting (the default is 100), so we can write

```python
N('D5(.4)')
```

In addition, it is also supported that the string indicating the name of the note without specifying the pitch is followed by these setting parameters, and the specific number of pitches can be specified with pitch, which has a default value of 4, for example

```python
N('D(.4)', pitch=5)
```

will get the note type D5, with a note length of 1/4 bar (quarter note)

## Advanced syntax for chord setting note length and note interval and note volume

When using the chord function, when writing a string of note names or a list of string of note names, it is possible to write the note length, note interval and volume in the string with a new syntax.
For example, now we want to write a melody

```python
example = chord('G5, F5, E5, F5, E5, E5, D5, E5, D5, C5, B4, G4')
```

The list of note lengths for this melody is

```python
example_duration = [3/4, 1/8, 1/8, 3/4, 1/8, 1/8, 1/4, 1/4, 1/4, 1/2, 1/2, 1/2]
```

The list of note intervals for this melody is the same as the list of note lengths

```python
example_interval = example_duration.copy()
```

If the traditional approach is

```python
example % (example_duration, example_interval)
```

With the new syntax it can be written as

```python
example = chord('G5[3/4;3/4], F5[.8;.8], E5[.8;.8], F5[3/4;.3/4], E5[.8;.8], D5[.8;.8], E5[.4;.4], D5[.4;.4], C5[.2;.2], B4[.2;.2], G4[.2;.2]')
>>> example
[G5, F5, E5, F5, E5, D5, E5, D5, C5, B4, G4] with interval [0.75, 0.125, 0.125, 0.75, 0.125, 0.125, 0.25, 0.25, 0.5, 0.5, 0.5]
```

The format and syntactic sugar of the chord function to receive the sound name string of this new syntax is the same as that of the toNote function written before, which also uses `;` as the separator between arguments, and can also use any of the parentheses, brackets, or braces, and can also have spaces between arguments, and also has `.n` for the syntactic sugar of n notes, and the order of the arguments is [note length The order of the parameters is [note length; note interval; note volume], and the parameters are also variable, so you can set only the note length, or only the note length and note interval, or all three parameters. The biggest advantage of this new syntax is that if you need to change the note length of a note or the note interval between a note and the next note when writing a melody, you can change it directly after the note name, instead of going to the note length list and the note interval list to find the position of the note you want to change. chord function also supports this new syntax for the note name string The chord function also supports this new syntax for the list of note names.

In addition, the new syntax of the chord function has a unique syntactic sugar, that is, when the note length and note interval are the same, the note interval can be abbreviated as `. `, and you can follow this abbreviation with any addition, subtraction, multiplication or division operation, such as `. *2`, `. /2`, and the chord function will also do the calculation. When the note interval is abbreviated as `. `, it means that the note interval takes the same value as the note length. For example

```python
chord('G5[3/4;.] , F5[.8;.] , E5[.8;.]')
```

To insert rests, you can use syntax like `r[duration]` as notes in the string, the nth note syntactic sugar is also accepted, the unit of the duration of rests is bar, for example

```python
example = chord('G5[3/4;3/4], F5[.8;.8], E5[.8;.8], F5[3/4;1/4], r[.2], E5[.8;.8], D5[.8;.8], E5[.4;.4], D5[.4;.4], C5[.2;.2], B4[.2;.2], G4[.2;.2]')
>>> example
[G5, F5, E5, F5, E5, D5, E5, D5, C5, B4, G4] with interval [0.75, 0.125, 0.125, 0.75, 0.125, 0.125, 0.25, 0.25, 0.5, 0.5, 0.5]
```

## Another way to set the volume of a chord type

```python
# You can use the setvolume method to set the volume of a chord type
a = C('Emaj7')
a.setvolume(80)
# You can also use the set function or the advanced syntax % to set the volume as the third argument after the first note length and interval
a = a.set(1/8,1/8,80)
a = a % (1/8,1/8,80)
```

## Get only note types of a chord type

If you want to keep only the note types of a chord type and remove all other types (such as tempo types, pitch_bend types, etc.), then you can use the built-in function `only_notes`.

```python
bpm, a, start_time = read('example.mid').merge()
a = a.only_notes()
```

## Get the chord type according to the guitar's fret count and guitar tuning standard

You can use the `guitar_chord` function to get the chord type by the number of frets of the guitar's 6 strings and the guitar's string standard (which can be left unset, the default is the standard 6 string guitar tuning)

```python
guitar_chord(frets,
             return_chord=True,
             tuning=['E2', 'A2', 'D3', 'G3', 'B3', 'E4'],
             duration=0.25,
             interval=0,
             **detect_args)
# frets: A list of the number of frets from the lowest to the highest note of the 6 strings of your guitar, the number of frets is an integer, if it is an empty string write 0, if it is a non-playing string write None
# return_chord: Whether or not to return the chord type, if False it will determine what chord is played by the number of frets you pressed and return the specific chord name, default is True.
# tuning: the tuning of the guitar, the default value is the standard tuning of a 6-string guitar, you can also customize the tuning of your guitar
# duration: a list of note lengths for the type of chord returned
# interval: A list of note intervals for the chord type
# detect_args: parameters to determine the specific type of chord, i.e. parameters of the detect function, set by keyword parameters

# For example, if a standard guitar C major triad in the first three frets is 3 frets on the 5th string, 2 frets on the 4th string, 3 frets on the 3rd string, 1 frets on the 2nd string, and 1 frets on the 1st string, then you can write
>>> guitar_chord([None, 3, 2, 0, 1, 0])
[C3, E3, G3, C4, E4] with interval [0, 0, 0, 0, 0, 0]
>>> guitar_chord([None, 3, 2, 0, 1, 0], return_chord=True)
'Cmajor'
```

## Calculate the actual playing time of a chord type according to the specified BPM

You can use the built-in function `eval_time` to calculate the actual playing time of a chord type by specifying a BPM

```python
a = C('Cmaj7') | C('Dm7') | C('E9sus') | C('Amaj9', 3)
>>> a.eval_time(80)
'3.0s'

eval_time(bpm, ind1=None, ind2=None, mode='seconds', start_time=0)
# bpm: the specified speed BPM

# ind1, ind2: select the bar interval, 0 as the first bar, if not set then default to the whole song

# mode: you can choose 'seconds' to return the string of time in seconds,
# or 'hms' to return the string of time in hours, minutes, seconds,
# or 'number' to return the float of time in seconds

# start_time: the start time of the chord
```

## Slicing chord types by range of bars

If we want to extract a slice of a chord type from bar 6 to bar 8, we can use the built-in function ``cut``

```python
cut(ind1=0, ind2=None, start_time=0)
# ind1, ind2: the range of the number of bars to extract, ind2 if not set, then extract to the end, ind1 default value is 0, that is, from the beginning of bar 0 to extract.
# start_time: When reading a MIDI file, the notes of a MIDI channel will have their own start time, which can be set here as a chord type delay, the unit here is bars
# The cut function returns a new chord type with a slice in the range of the specified number of bars
a.cut(6, 8)
# Extracts the part of the chord type a from the 6th bar to the 8th bar (from the beginning of the 6th bar to the beginning of the 8th bar)
```

## Calculate the total number of bars of a chord type

Use the built-in function ``bars`` to get the total number of bars of a chord type

```python
a = C('Cmaj7') | C('Dm7') | C('E9sus') | C('Amaj9', 3)
>>> a.bars()
1.0
```

## Slice chord types according to the range of realistic playing times

Using the built-in function `cut_time`, specifying a BPM, you can select a time range for slicing chord types.  
For example, if chord type a is played at 100 BPM, and the part from the 10th to the 20th second is extracted, you can write

```python
a.cut_time(100, 10, 20)
# If the right side of the range is not set, then it will extract to the end by default, and the left side will default to extract from the very beginning
```

## Extract the first n bars of a chord type

```python
a.firstnbars(n)
```

## Extract the part of a chord type for a particular bar

```python
a.get_bar(n)
```

## Extract the contents of each bar of a chord type

```python
a.split_bars()
# You can get a list of chord types consisting of parts of each bar of the chord type a
```

## Count the number of occurrences of a note name within a chord type

```python
## If you want to get the number of occurrences of the note named F# in chord type a, you can write
a.count('F#')
# You can also specify the number of octaves and count only the notes with the same name and octave
a.count('F#5')
```

## Get the most occurrences of a chord type

```python
a.most_appear()
## The choices parameter sets which notes to select from, if not it defaults to all 12 notes
```

## Unify all sharp and flat signs of notes of a chord type

```python
# Let's say a chord type has notes C5, Eb5, G5, A#5, D#6, Bb6, F5 (a random melody).
# If we want to unify the notes with a rising and falling sign,
# convert all the notes with a falling sign to the equivalent of the notes with a rising sign in pitch
# that is, C5, D#5, G5, A#5, D#6, A#6, F5, then we can write
a = chord('C5, Eb5, G5, A#5, D#6, Bb6, F5')
>>> print(a)
[C5, Eb5, G5, A#5, D#6, Bb6, F5] with interval [0, 0, 0, 0, 0, 0, 0, 0]
a = a.standard_notation()
>>> print(a)
[C5, D#5, G5, A#5, D#6, A#6, F5] with interval [0, 0, 0, 0, 0, 0, 0, 0, 0]
```

## Re-quantize the chord type with note length and note interval according to the tempo change inside

For example, now we have a chord type that holds a piece with some tempo types (tempo change types), i.e. different parts of a piece will have different tempos. If we want to unify the tempo of the whole piece, but still want to keep the different tempos of the previous parts, then we need to re-quantize the different tempos of each part in proportion to the tempo we want to unify If we want to unify the tempo of the whole piece, but still want to keep the different tempo of the previous parts, then we need to recalculate the note length and note spacing of each part in proportion to the different tempo of each part relative to the desired unified tempo. In some classical music MIDI files, there are many complex, frequent, fast and subtle tempo changes, and unifying the tempo of the whole piece can facilitate the music theory processing afterwards, because unifying the tempo is equivalent to recalculating the note lengths and note intervals of each part to the actual equivalent of the relatively unified tempo. The note lengths and note intervals of each tempo are recalculated to their actual equivalents, which facilitates algorithmic processing during the analysis of the music theory (since there is no need to consider the actual playing time changes due to tempo changes in each part)

```python
# Use the built-in function normalize_tempo to re-quantize notes according to the tempo change type stored in a chord type.
# The normalize_tempo function supports both chord types (chord types) and piece types (piece types)
a = read('example.mid') # a is the piece type converted to after reading the MIDI file example.mid
a.normalize_tempo() # parameter bpm, default value is None, if bpm is None, use the tempo parameter that comes with the piece type
a.normalize_tempo(100) # set bpm to 100, unify bpm to 100 for the whole piece, re-quantize each part of the notes

bpm, b, start_time = read('example.mid').merge() # b is the chord type after reading the MIDI file example.mid and merging all the tracks
b.normalize_tempo(bpm=100, start_time=0) # The bpm parameter is the tempo you want to normalize, and the start_time parameter is the time in bars when the chord type starts playing, the default is 0
# Here we unify the speed of the chord type b to 100 BPM
```

## Calculating the range of bars between two indices in a chord type

Using the built-in function `count_bars` of a chord type, you can calculate the range of bars between two indices, for example, if there is a chord type with 100 notes.  
We want to calculate the range of bars between the 2nd note and the 10th note, i.e. the range that starts with the number of bars where the 2nd note is and ends with the number of bars where the 10th note is.  
Or maybe we want the number of bars (the range of bars) that pass between the 2nd note and the 10th note. We can write it like this.

```python
count_bars(ind1, ind2, bars_range=True)
# ind1, ind2 is the index (the first note) of the beginning and ending note, as an integer
# ind1 is the index of the first note, and bars_range is True, which returns the range of bars as a list [number of starting bars, number of ending bars]
# False returns the length of the bars elapsed between the two indices, the value is a numeric value
a = chord('A5, B5, C6, G5, G#5, ...') % (duration, interval) # chord a has 100 notes
bar_length = a.count_bars(2, 10)
>>> print(bar_length)
[1.5, 7.5]
bar_length = a.count_bars(2, 10, False)
>>> print(bar_length)
6
# The values here are for example only
```

## Extracting real-time tempo changes or pitch bends in a chord type

Using the built-in function `split` of a chord type, you can extract any music theory type contained in a chord type, i.e. from the list of notes of the chord type.  
This function makes it easy to extract note types, tempo types, pitch_bend types, and other musical types from a chord type by passing in the type name.

```python
split(return_type, get_time=False, sort=False)
# return_type is the musicpy data structure you want to extract, and the return value is the chord type consisting of this music theory type contained in the list of notes in the specified chord type.
# get_time is True to calculate the start time for a tempo type or a pitch_bend type that does not have a specified start time, and to specify the start time.
# sort is True to sort the tempo types or pitch_bend types in order of their start times.
a = chord(['A5', 'B5', 'C5', tempo(150), 'D5', pitch_bend(50), 'E5', 'F5', tempo(170)])
>>> print(a.split(tempo))
[tempo change to 150, tempo change to 170] with interval [0, 0]
>>> print(a.split(pitch_bend))
[pitch bend up by 50.0 cents] with interval [0]
>>> print(a.split(note))
[A5, B5, C5, D5, E5, F5] with interval [0, 0, 0, 0, 0, 0, 0]
# The chord type returned is the set of music types you want to extract and can be used as a list.
```

## Building a chord progression

Earlier, when talking about the basic syntax of scale types, it was mentioned that scale types can extract chord progressions by steps, so if you are generating chord progressions directly from the perspective of chord names, you can use the `chord_progression` function, which is a global function and does not require any musical type as a prerequisite.

```python
chord_progression(chords,
                  durations=1 / 4,
                  intervals=0,
                  volumes=None,
                  chords_interval=None,
                  merge=True)
chords: string of chord names or a list of chord types, where chord names can also be meta-tuples of C function arguments
durations: the length of each chord, either as a value or as a list
intervals: the interval of each chord, either as a value or as a list
volumes: default volume of each chord, can be a value or a list, if None, no setting
chord_interval: the interval between each chord, can be a value or a list
merge: if True, returns the chord type after merging all chords in the chord progression, if False, returns a list of chord types in the chord progression

chord_progression_example = chord_progression(['F', 'G', 'Am', 'Em'])
chord_progression_example = chord_progression([('F',4), ('G',4), ('C',5), ('Am',4)])
chord_progression_example = chord_progression([C('F')^2, C('G')^2, C('Am')^2, C('Em')^2])
>>> print(chord_progression_example)
[F4, C5, A5, G4, D5, B5, A4, E5, C6, E4, B4, G5] with interval [0, 0, 0.25, 0, 0, 0, 0.25, 0, 0, 0, 0, 0.25, 0, 0, 0, 0]
# If there is a higher requirement for the arrangement of the notes of each chord (such as inversion or playing in a certain pattern of note arrangement, etc.), it is recommended to pass in the chord type, and the chord type can also be written directly to the melody.
# If it is a string it is limited to the range that the C function can parse.
```

## View information about the musical analysis of a chord type

Using the built-in function `info` for chord types, you can view the chord type, root note, and chord peculiarities (root position, inversion, polychord, etc.) of a chord type's notes, as well as omitted notes, altered notes, chord voicings, non-chord bass note if it has. If the chord instance does not contain a chord, which means it contains only a note or interval, the info will show the note name or interval name.

```python
>>> print(chord('A,C,F').info())
chord name: Fmajor/A
root position: Fmajor
root: F
chord type: major
chord speciality: inverted chord
inversion: 1 inversion

>>> print(chord('A').info())
note name: A4

>>> print(chord('C,E').info())
interval name: major third
root: C4

# you can set `get_dict=True` to get a chord info dictionary,
# the `other` key contains another dictionary which contains the info of
# omitted notes, altered notes, non-chord bass note and voicings
>>> print(C('Cmaj9,omit3').info(get_dict=True))
{'type': 'chord', 'chord name': 'Cmaj9 omit 3', 'root position': 'Cmaj9', 'root': 'C', 'chord type': 'maj9', 'chord speciality': 'root position', 'inversion': None, 'other': {'omit': [3], 'altered': None, 'non-chord bass note': None, 'voicing': None}}

>>> print(chord('A').info(get_dict=True))
{'type': 'note', 'note name': 'A4', 'whole name': 'note A4'}

>>> print(chord('C,E').info(get_dict=True))
{'type': 'interval', 'interval name': 'major third', 'root': 'C4', 'whole name': 'C with major third'}
```

## Getting a chord type for a sus

If we want to get a chord type sus4 or a variant of sus2, then we can use the built-in `sus` function of the chord type, which defaults to 4

```python
a1 = chord('C, E, G')
>>> print(a1.sus())
[C4, F4, G4] with interval [0, 0, 0]
>>> print(a1.sus(2))
[C4, D4, G4] with interval [0, 0, 0]
# The sus function can take 2 or 4 as arguments, and sus a chord type with a 3rd for the lowest note, replacing it with a 2nd or 4th note.
# Not only for triads, but also for more complex chords such as 7th, 9th, 11th, etc.
```

## Delay the same chord type n times or play it n times at intervals

Recently, the syntax to delay a chord type n times and play it n times at intervals has been added, using the `&` and `|` symbols, respectively.
These two symbols are used to play a chord type simultaneously or sequentially with another chord type, but now if you pass in an tuple then you can get the result of repeating the same chord type with a delay.

```python
a1 = chord('C, E, G')
>>> print(a1 & (3, 1/8)) # Play the chord type a1 3 times, each time with 1/8 more bars of delay than the beginning
[C4, E4, G4, C4, E4, G4, C4, E4, G4] with interval [0, 0, 0.125, 0.0, 0.0, 0.0, 0.125, 0.0, 0.0, 0.0, 0.25]

>>> print(a1 | (3, 1/8)) plays the chord type a1 3 times, each time after the other, but with an interval of 1/8 of a bar first
[C4, E4, G4, C4, E4, G4, C4, E4, G4] with interval [0, 0, 0.375, 0, 0, 0, 0.375, 0, 0, 0, 0]
```

## Input chord name parsing with new add and sus syntax

You can use the `C` function to parse a string of chord names to get the corresponding chord type, by adding parentheses to the chord type and entering the chord name.  

You can also use the C function to input the chord name and then input advanced music theory operations such as alteration, omission, etc. with `,` as interval.  

The syntax of add and sus has been added, so that by entering `addN` you can add a certain number of degrees from the lowest note, and by entering `susN` you can get the previous chord type sus2 or sus4 chords.

```python
>>> print(C('C,add9'))
[C4, E4, G4, D5] with interval [0, 0, 0, 0]

>>> print(C('Cm7,add11'))
[C4, D#4, G4, A#4, F5] with interval [0, 0, 0, 0, 0, 0]

>>> print(C(Cmaj7,sus4))
[C4, F4, G4, B4] with interval [0, 0, 0, 0, 0]

>>> print(C('Bb9,sus'))
[Bb4, D#5, F5, G#5, C6] with interval [0, 0, 0, 0, 0, 0]
```

## Unify accidentals of chord type

You can use the chord type `same_accidentals` function to unify the ascending and descending notes of a chord type,

```python
a = chord('C5, D#5, F5, Ab5, E5, D5, C#5')
>>> a.same_accidentals('#')
[C5, D#5, F5, G#5, E5, D5, C#5] with interval [0, 0, 0, 0, 0, 0, 0, 0]
>>> a.same_accidentals('b')
[C5, Eb5, F5, Ab5, E5, D5, Db5] with interval [0, 0, 0, 0, 0, 0, 0, 0, 0]
```

## Filter the notes that meet the specified conditions in the chord type

You can use the `filter` function of the chord type to filter out the notes that do not meet the conditions in the chord type by specifying conditions, and filter the notes that meet the specified conditions in the chord type.
For example, filter notes with a volume between 20 and 80, notes with a note length between 1/16 bar and 1 bar, notes with a pitch between A0 and C8, and so on. You can also specify a function to operate on the filtered notes.

```python
filter(self, cond, action=None, mode=0, action_mode=0)

# cond: The filter condition function must be a function whose parameter is a note and the return value is a Boolean value. It is recommended to use the lambda function

# action: Operation function, if it is not None, the operation of this function is performed on the filtered notes, but the filtered notes are not extracted separately.
# Returns the modified chord type, if it is None, it returns the chord type composed of the filtered notes and the start time of the first filtered note

# mode: If it is 1, it returns the index list of the filtered notes

# action_mode: If it is 0, the return value of the action function will replace the note it is acting on. If it is 1, the action function will directly act on the note.

a = chord('C, E, G, B') # Initialize a chord
a.setvolume([10, 20, 50, 90]) # Set the volume of the note
>>> a.filter(lambda s: 20 <= s.volume <80) # Filter the notes with a volume between 20 and 80 in the chord type
([E4, G4] with interval [0, 0], 0) # Return the chord type composed of the filtered notes and the start time of the first filtered note

# For notes with a volume between 20 and 80, the volume is set to 50
b = a.filter(lambda s: 20 <= s.volume <80, action=lambda s: s.setvolume(50), action_mode=1)
>>> b
[C4, E4, G4, B4] with interval [0, 0, 0, 0] # Return the chord type whose volume is modified by the action function
>>> b.get_volume() # Get the volume of the new chord type
[10, 50, 50, 90] # The volume of the notes between 20 and 80 are now 50
```

## Filter the notes within the specified pitch range of the chord type

You can use the `pitch_filter` function of the chord type to filter the notes within the specified pitch range of the chord type. This function is very important in many occasions, such as when you read a MIDI file,
It is necessary to map the notes of the MIDI file to a piano in the software for display. Generally speaking, the piano has 88 keys and the pitch range is A0 ~ C8, but the MIDI file can store notes that exceed this pitch range.
It may be lower than A0 or higher than C8, so we need to limit the pitch of the notes of the read MIDI file to between A0 and C8, and the notes that do not belong to this range need to be removed. This function can do this,
And it will recalculate the interval of all the notes after filtering, so it will not affect the position of the notes when outputting to MIDI files again. The more generalized `filter` function also recalculates the interval of all notes.

```python
pitch_filter(self, x='A0', y='C8')

# x: The lowest pitch of the pitch range, the default value is A0

# y: The highest pitch of the pitch range, the default value is C8

a = chord('Ab0, C5, E5, G5, B5, G10') # this is a chord with 2 notes that the pitches do not belong to A0 ~ C8
>>> a.pitch_filter() # Use the default pitch range A0 to C8 to filter the notes of the chord type
([C5, E5, G5, B5] with interval [0, 0, 0, 0], 0)
# Return the chord type composed of the filtered notes within the specified pitch range, and the start time of the first filtered note

b = chord('Ab0, C5, E5, G5, A5, C7, G10')
>>> b.pitch_filter('C5','C6') # Filter the notes whose pitch is between C5 and C6
([C5, E5, G5, A5] with interval [0, 0, 0, 0], 0)
# Return the chord type composed of the filtered notes within the specified pitch range, and the start time of the first filtered note
```

## Generate chord types according to the interval relationship

You can use the `getchord_by_interval` function to generate chord types from a list of interval relations. You can select the interval relationship with the root note or the interval relationship between every two adjacent notes.
The note type can also use this function, and the note type itself will be used as the starting note when used.

```python
getchord_by_interval(start,
                     interval1,
                     duration=0.25,
                     interval=0,
                     cummulative=True)

# start: The starting note of the chord type, which can be a string representing a note or a note type
# interval1: Represents a list of interval relations, the elements are integers
# duration: the note length of the generated chord type
# interval: The note interval of the generated chord type
# cummulative: If True, the interval relationship is the interval relationship with the initial note. If False, the interval relationship is the interval relationship between every two adjacent sounds. The default value is True

>>> getchord_by_interval('C5', [major_third, perfect_fifth, major_seventh])
# Obtain the chord type composed of the initial note C5, and C5 in turn to form the major third, the complete fifth and the major seventh
[C5, E5, G5, B5] with interval [0, 0, 0, 0] # Get the C major seventh chord

>>> getchord_by_interval('C5', [major_third, minor_third, major_third], cummulative=False)
# Get the chord type composed of the initial note C5 and the adjacent intervals as the major third, minor third, and major third.
[C5, E5, G5, B5] with interval [0, 0, 0, 0] # Get the C major seventh chord

a = N('C5')
>>> a.getchord_by_interval([major_third, perfect_fifth, major_seventh]) #The note type calls this function
[C5, E5, G5, B5] with interval [0, 0, 0, 0]
```

## Find a certain degree of a chord type

Now, for example, you have a C minor eleventh chord, and you want to get its 3rd and 9th degrees. At this time, you can use the `interval_note` function of the chord type and enter a degree to find it. If the current chord type does not contain notes of the specified degree, then `None` will be returned. The number of degrees supported for search includes from 1 degree (root note) to 13 degrees, as well as the case of changing sounds, such as `#5`, `b9`.

```python
interval_note(self, interval, mode=0)

# interval: The degree you want to find, it can be an integer or a string representing the degree

# mode: When it is 0, if the note of the specified degree cannot be found, None will be returned. When it is 1, the starting note of the chord type plus the note type of the specified degree will be returned.

>>> C('Cm11') # C minor eleven chord
[C4, D#4, G4, A#4, D5, F5] with interval [0, 0, 0, 0, 0, 0]

>>> C('Cm11').interval_note(3) # Find the 3rd note of the C minor eleventh chord
D#4 # Return to the 3rd of the C minor eleventh chord. If it is more musically rigorous, it should be Eb4.
# The reason why this is D#4 is because musicpy’s default note representation is to give priority to the # number

>>> C('Cm11').interval_note(9) # Find the 9th note of the C minor eleventh chord
D5 # Returns the 9th degree of the C minor eleventh chord

>>> C('Cm11').interval_note(d5, mode=1) # Returns the first 5th of the C minor eleventh chord,
# Note that the degree here cannot be a string, because the note type plus the string will be interpreted as forming a chord type
F#4 # Return to the first note of the C minor eleventh chord with a 5th falling pitch (here, strictly speaking, it should be Gb4, for the same reason as before)
```

## Confirm that a note is what degree of a chord

What is done here is the reverse of `Find a certain degree of a chord type`. You can use the `note_interval` function of the chord type to confirm the degree of a note in a chord type.
If the specified note is not in the chord type, the relationship between the initial note of the chord type and the degree of this note will also be returned.

```python
note_interval(self, current_note, mode=0)

# current_note: The note whose degree you want to confirm, it can be a string or a note type

# mode: When it is 0, it will return the degree expressed by the rise and fall signs and numbers, when it is 1, it will return to the pure English interval representation

>>> C('Cm11') # C minor eleven chord
[C4, D#4, G4, A#4, D5, F5] with interval [0, 0, 0, 0, 0, 0]

>>> C('Cm11').note_interval('Eb4') # Confirm that the note Eb4 is a few degrees of the original C minor eleventh chord starting with C4
'b3' # The returned result is 3 degrees (b3 means 3 degrees lower, or 3 degrees lower, because the 3 degrees that are not lowered are 3 degrees higher)

>>> C('Cm11').note_interval('D5') # Confirm that the note D5 is a few degrees of the original C minor eleventh chord starting with C4
'9' # The returned result is 9th (large 9th)

>>> C('Cm11').note_interval('Db5') # Confirm that the note Db5 is a few degrees of the original C minor eleventh chord starting with C4
'b9' # C minor eleventh chord does not include the note Db, but this function will also return the interval relationship formed by the initial note C4 and Db5 of the chord, which is b9 (lower nine degrees or minor nine degrees)

>>> C('Cm11').note_interval('D5', mode=1) # Confirm that the note D5 is a few degrees of the original C minor eleventh chord with C4 as the starting note, and return to the pure English representation
'major ninth'
```

## Obtain the chord voicing according to the degree of the chord

There are many ways to arrange and combine the notes of a chord. Each unique combination is a kind of voicing. By placing the notes of the chord in different orders and octaves, a chord can have various voicings, each kind of voicing has a unique sense of hearing, whether it is columnar chords, split chords or arpeggios, you can hear different tastes. For example, a Cm11 chord can be played in the order of 1, 3, 5, 7, 9, and 11th degree in the original position.
You can also follow the order of 1, 5, 9, 3, 7, and 11th degree to create a more dreamy and beautiful sense of hearing. Therefore, we need a convenient function to obtain a chord's voicing by the degree of the chord, including the order of the sound, the omission of the sound, and the repetition of the sound in a higher octave all need to be considered. The chord type's `get_voicing` function I designed recently can do this.

```python
get_voicing(self, voicing)

# voicing: A list of chord vocal positions, that is, a list that indicates the order of the degree of the chord. The element can be an integer or a string indicating the degree

# It should be noted that the chord vocal position list must all have the degrees of the current chord type

>>> C('Cm11').get_voicing([1,5,9,3,11,7]) # C minor eleventh chord according to root, 5th, 9th, 3rd, 11th, 7th degree,
# and redistribute the octaves of all notes according to the rule that the following notes are higher than the preceding notes
[C4, G4, D5, D#5, F5, A#5] with interval [0, 0, 0, 0, 0, 0]
# Return to the voicing chord type of the C minor eleven chord arranged according to the specified chord vocal position list

play(C('Cm11').get_voicing([1,5,9,3,11,7])% (1, 1/8), 150) # Play with fast arpeggios
```

## Adjust the notes of the current chord type to a place closer to the notes of another chord type

When considering the arrangement of chord parts, if you want a smoother connection between the next chord and the previous chord, then one of the ways is to place the sound of the next chord closer to the sound of the previous chord.
In this way, the connection of different parts in a chord progression will be smoother, because the movement of the parts is relatively small. For example, the original position of the Am chord is connected to the original position of the F chord, that is, A C E is connected to F A C. If you want to connect these two chords more smoothly,
You can adjust the order of the notes of the F chord so that each note of the F chord is as close as possible to the original position of the Am chord. For example, adjust it to ACF so that the first two notes of the F chord are in the original position of the Am chord. The first two notes of are the same,
The third note F is only one semitone higher than the original third pitch of the Am chord, so the two chords connected will sound smoother and smoother.

You can use the `near_voicing` function of a chord type to adjust the sound of the current chord type to the order of the pitch of another chord type. If the pitch difference between the two chords is relatively large,
Then it will also move the pitch of the current chord type to the pitch range of another chord type. You can also fix the lowest note of the current chord type without adjusting the closest pitch distance, because sometimes the lowest note changes
The chord inversion obtained is not what we want.

```python
near_voicing(self, other, keep_root=True, root_lower=False)

# other: The chord type used as a standard for adjusting the pitch of the current chord type

# keep_root: When it is True, keep the lowest note of the current chord type after the adjustment is still the lowest note

# root_lower: When you choose to keep the lowest note of the current chord type, when it is True,
# The lowest note of the current chord type will be below the lowest note of the standard chord type, and above it when it is False.

>>> C('F').near_voicing(C('Am'), keep_root=False) # Get the voicing of the closest distance to the original position of the F chord with respect to the original position of the Am chord, without keeping the lowest pitch
[A4, C5, F5] with interval [0, 0, 0]

# Write a smooth voice type connection of the 2516 chord progression in C major
chord1 = C('Dm7', 3).get_voicing([1, 7, 3])% (1,[1/4,1/4,1/2])
chord2 = C('G7, omit5', 4).near_voicing(chord1, keep_root=True, root_lower=True)% (1,[1/4,1/4,1/2])
chord3 = C('Cmaj7, omit5', 4).near_voicing(chord1, keep_root=True)% (1,[1/4,1/4,1/2])
chord4 = C('Am7, omit5', 4).near_voicing(chord1, keep_root=True)% (1,[1/4,1/4,1/2])
play(chord1 | chord2 | chord3 | chord4, 165)
```

## Make arpeggios quickly

New in August 2021 is the function `arpeggio` to quickly create chord arpeggios. You can specify the chord type, the octave range of the chord arpeggio, the note length and the note interval, and you can choose to play either the top line arpeggio or the bottom line arpeggio, or both. You can also use `arp` as shorthand.

```python
arpeggio(chord_type,
          start=3,
          stop=7,
          durations=1 / 4,
          intervals=1 / 32,
          first_half=True,
          second_half=False)

# chord_type: String indicating the chord type, either using the syntax supported by the C function, or the chord type

# start: the number of octaves the chord arpeggio starts in

# stop: the number of octaves the chord arpeggio ends in

# durations: note length of the chord arpeggio

# intervals: note intervals of the chord arpeggio

# first_half: the chord arpeggio that generates the top line

# second_half: generates the lower chord arpeggio

Cmaj7_arpeggio = arpeggio('Cmaj7')

>>> Cmaj7_arpeggio
[C3, E3, G3, B3, C4, E4, G4, B4, C5, E5, G5, B5, C6, E6, G6, B6] with interval [0.03125, 0.03125, 0.03125, 0.03125, 0.03125, 0.03125, 0.03125, 0.03125, 0.03125, 0.03125, 0.03125, 0.03125, 0.03125, 0.03125, 0.03125, 0.03125, 0.03125, 0.03125]

Cmaj7_arpeggio = arp('Cmaj7', 3, 6)

>>> Cmaj7_arpeggio
[C3, E3, G3, B3, C4, E4, G4, B4, C5, E5, G5, B5] with interval [0.03125, 0.03125, 0.03125, 0.03125, 0.03125, 0.03125, 0.03125, 0.03125, 0.03125, 0.03125, 0.03125, 0.03125, 0.03125, 0.03125]
```

## Reset the overall octave of chord type

You can use the `reset_octave` function to set the overall octave of a chord type to the octave of the 1st note of the chord type and move the chord type to the octave you set, returning a new chord type. This method is useful in cases where you don't know the overall octave of the original chord.

```python
a = C('Cmaj7', 5)

>>> a
[C5, E5, G5, B5] with interval [0, 0, 0, 0]

b = a.reset_octave(3)

>>> b
[C3, E3, G3, B3] with interval [0, 0, 0, 0]
```

## Construct chord types with other MIDI messages

Generally, you can pass in the `other_messages` attribute to add other MIDI messages when building a chord type, but if you want to add other MIDI messages after the chord type is built, you can either set the `other_messages` attribute of the chord type directly, or you can use the chord type's `with_other _messages` function to get a new chord type with additional MIDI messages. Note that `other_messages` is a list of other MIDI messages.

```python
a = C('Cmaj7')

b = a.with_other_messages([controller_event(controller_number=1, parameter=50)])

>>> b.other_messages

[controller_event(track=0, channel=0, time=0, controller_number=1, parameter=50)]
```

## Write chord types by polyphony

You can use the `multi_voice` function to combine multiple chord types as multiple voices, returning the new chord type after the multiple voices are combined. This method is useful when writing polyphonic melodies and harmonies for the same instrument, such as when writing A cappella and complex drums.

```python
multi_voice(*current_chord, method=chord, start_times=None)

# *current_chord: you can pass in any number of chord types as voices

# method: If you pass in a string that represents a chord, you can choose the syntax of the chord parsing here, you can choose chord, translate and drum

# start_times: a list of the start times of the chord types after the first chord type.
# If you want to set the start times of the other voices after the first chord type relative to the first chord type, you can set this parameter in bars

a = multi_voice(chord('C2') % (1, 1) % 2,
                C('G') % (1/8, 1/8) % 4)

>>> a
[C2, G4, B4, D5, G4, B4, D5, G4, B4, C2, D5, G4, B4, D5] with interval [0, 0.125, 0.125, 0.125, 0.125, 0.125, 0.125, 0.125, 0.125, 0.125, 0.125, 0.0, 0.125, 0.125, 0.125, 0.125, 0.125]
```

## Concatenate chord types in a list

You can use the `concat` function to merge all the chord types in a list by specifying a merge method, resulting in a new merged chord type, which can be specified as `tail`, `head`, `after`, with `+`, `&`, `|` respectively.

```python
concat(chordlist, mode='+', extra=None, start=None)

# chordlist: list of chord types to merge

# mode: mode of concatenation, can receive values '+', '&', '|', corresponding to 'tail', 'head', 'after' respectively, default value is '+'

# extra: extra interval to be added when merging two adjacent chord types, in bars

# start: The start value of the concatenation, if it is None, then use the first element of the list as the start value

chord_list = [C('C'), C('D'), C('E')]
>>> chord_list
[[C4, E4, G4] with interval [0, 0, 0], [D4, F#4, A4] with interval [0, 0, 0], [E4, G#4, B4] with interval [0, 0, 0]]

combined_chord = concat(chord_list, '|')

>>> combined_chord
[C4, E4, G4, D4, F#4, A4, E4, G#4, B4] with interval [0, 0, 0.25, 0, 0, 0, 0.25, 0, 0, 0, 0]
```

## Distribute multiple notes evenly in proportion to their length to the specified bar length

You can use the `distribute` function to distribute multiple notes evenly over a specified bar length in proportion to their length. This is useful when writing special rhythmic patterns and polyrhythms.

```python
distribute(current_chord,
           length=1 / 4,
           start=0,
           stop=None,
           method=translate,
           mode=0)

# current_chord: string or list of notes representing the chord

# length: total length of the chord to be assigned, in bars

# start: index of the note in the chord to start assigning, starting from 0, default starts from the 1st note

# stop: index of the end note of the chord to be assigned, starting from 0 and ending with the last note if it is None, default is None

# method: The method used to parse the chord when the string is passed in, the default value is translate, you can choose chord, translate

# mode: when 0, the notes are equally distributed in proportion to their respective lengths and intervals to the specified bar length.
# with 1, the note interval takes the same value as the note length.

# Distribute the 5 notes of the Cmaj9 chord evenly over 1/2 bar length, with the same note length and interval for the 5 notes before distribution
a = distribute(C('Cmaj9') % (1/8, 1/8), 1/2)

>>> a
[C4, E4, G4, B4, D5] with interval [0.1, 0.1, 0.1, 0.1, 0.1]

>>> a.get_duration()
[0.1, 0.1, 0.1, 0.1, 0.1, 0.1]

# Distribute the note lengths of 2 notes in 2 cents, 2 notes in 4 cents respectively (repeated 2 times) evenly over 1/2 bar length
b = distribute('C[.2;.] , D[.4;.] , {2}', 1/2)

>>> b
[C4, D4, C4, D4] with interval [0.1666666666666666666666666, 0.083333333333333333333, 0.166666666666666666666666666, 0.08333333333333333]

>>> b.get_duration()
[0.16666666666666666666666, 0.08333333333333333, 0.16666666666666666666666, 0.0833333333333333333]
````

## Use translate function to write chord types according to drum syntax

As mentioned in the previous section on drumming syntax, you can use the `translate` function to apply drumming syntax to notes, enabling you to write chord types in drumming syntax. Here's a more detailed explanation. One of the differences between the `translate` function and the drum type construction is that the default note spacing for the `translate` function is 0, while the default note spacing for the drum type is 1/8, and the default note length for both the `translate` function and the drum type is 1/8. The default note duration, interval and volume could be set using the same parameters when initializing the drum type. Here I will give a few examples of using the `translate` function to write chord types.

```python
a = translate('A2[1](2),[1],D3[1](2)')

>>> a
[A2, A2, D3, D3] with interval [1, 1, 1, 0]

b = translate('C5[.8;.] (3), D5[.16;.] (2), E5[.8;.] , {2}')

>>> b
[C5, C5, C5, D5, D5, E5, C5, C5, C5, D5, D5, D5, E5] with interval [0.125, 0.125, 0.125, 0.0625, 0.0625, 0.125, 0.125, 0.125, 0.125, 0.125, 0.0625, 0.0625, 0.125]

c = translate('C5, E5, G5')

>>> c
[C5, E5, G5] with interval [0, 0, 0]
```

## Reset the overall pitch of chord types

You can use the `reset_pitch` function of a chord type to move the overall pitch of the chord type to another pitch, using the 1st note of the chord type as the standard, and the return is a new chord type. The argument can be a string representing a note or a note type. For example a Cmaj7 chord that you want to move to an Emaj7 chord, but you don't want to use `up` or `+` to do the shifting because it requires counting the number of semitones from C to E, then you can use this method.

```python
a = C('Cmaj7')

>>> a
[C4, E4, G4, B4] with interval [0, 0, 0, 0]

>>> a.reset_pitch('E')
[E4, G#4, B4, D#5] with interval [0, 0, 0, 0]

>>> a.reset_pitch('E3')
[E3, G#3, B3, D#4] with interval [0, 0, 0, 0]
```

## Chord type extracts notes from the index list to form a new chord type

When you have picked some notes from the chord type by index and want to take them out, but still want to keep the distance relationship between the original notes, then you can use the `pick` function of the chord type, which returns a new chord type composed of the extracted notes.

```python
pick(indlist)

# indlist: a list of the indices of the notes
```

## Replace notes and chords in a chord type

To replace a note in a chord type with index, you can simply write `a[index] = new_note`, where `a` is a chord type.

To replace a chord in a chord type, which is to replace multiple notes in a chord type, you can use `replace_chord` function of the chord type to replace the notes between 2 indices.

```python
replace_chord(ind1, ind2=None, value=None, mode=0)

# ind1: the start index

# ind2: the end index, if set to None, then it will be calculated as `ind1 + len(value)`

# value: the chord to replace, could be a chord type or any data structures that `chord` function could parse

# mode: if set to 0, the notes and intervals between ind1 and ind2 will be replaced by the notes and intervals of the new chord type,
# if set to 1, only the note pitch will be replaced, other attributes of the notes between ind1 and ind2 and the intervals remain unchanged

a = chord('C5, D5, E5, F5, G5, A5, B5', interval=1/8)

>>> a
[C5, D5, E5, F5, G5, A5, B5] with interval [0.125, 0.125, 0.125, 0.125, 0.125, 0.125, 0.125]

a.replace_chord(ind1=1, value=C('A', duration=2))

>>> a
[C5, A4, C#5, E5, G5, A5, B5] with interval [0.125, 0, 0, 0, 0.125, 0.125, 0.125]

>>> a.get_duration()
[0.25, 2, 2, 2, 0.25, 0.25, 0.25]

b = chord('C5, D5, E5, F5, G5, A5, B5', interval=1/8)

>>> b
[C5, D5, E5, F5, G5, A5, B5] with interval [0.125, 0.125, 0.125, 0.125, 0.125, 0.125, 0.125]

b.replace_chord(ind1=1, value=C('A'), mode=1)

>>> b
[C5, A4, C#5, E5, G5, A5, B5] with interval [0.125, 0.125, 0.125, 0.125, 0.125, 0.125, 0.125]

>>> b.get_duration()
[0.25, 0.25, 0.25, 0.25, 0.25, 0.25, 0.25]
```

