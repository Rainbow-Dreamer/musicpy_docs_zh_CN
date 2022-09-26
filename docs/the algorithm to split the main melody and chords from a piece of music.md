# the algorithm to split the main melody and chords from a piece of music

Recently, while writing some musicpy features, I came across the need to separate the main melody and chord parts of a piece. For example, there is a function that performs intelligent composition analysis of a piece. Let's say now we read a MIDI file, which is a piano piece, and the main melody and chord parts are all put in one track. Let's say this piano piece has a total of 1400 notes, including the main melody and the chord part. Now we need to perform a tonal analysis of these 1400 notes, (ignoring for the moment the modulation and tonicization of the piece) for example to determine whether the piece is in a major or minor key.

Here we need to mention the concept of relational major and minor music theory. The relationship between major and minor is the same set of notes, but one is in a major key and the other is in a minor key. For example, if C major and A minor are related to each other, their constituent notes are CDEFGAB, but the scale order of A minor is ABCDEFG. For example, if the majority of the 1400 notes come from CDEFGAB, and if we ignore the church modes for the moment, then this piece may be in C major or A minor.

Generally speaking, a piece in a major key starts and ends with a major chord, especially if it ends on the 1st major chord, then it is basically a major key. For example, in the key of C major, there are seven notes, CDEFGAB, and the first chord is a C chord (there are many different kinds of chords, for example, a first natural triad is a C major triad, and a first natural seventh chord is a C major seventh chord, etc.). If the piece is in a minor key, then it is likely to begin and end with a minor chord, especially if it ends on the 1st chord of the minor key (the 1st chord of the minor key is also the 6th chord of its relation to the major key, which is a minor chord), then it is basically determined to be in a minor key. However, this method is very prone to errors, because there are so many exceptions. For example, I have written a lot of music in which the key cannot be determined in this way. A much more reliable method is to look for the note around which the piece revolves, that is, to look for the dominant note. In addition to finding the dominant, since the most important dominant chord in C major is its 1st degree chord (C major triad, C,E,G) and the most important dominant chord in A minor is its 1st degree chord (A minor triad, A,C,E), we can compare the frequency of the C major triad in the main melody and the frequency of the A minor triad in the main melody. Since the two notes C and E are common to both C major triads and A minor triads, we only need to compare the frequency of the G note or the A note in the main melody. (After testing, this method of determining the major and minor tendencies of the main melody is very practical and accurate, provided that the piece has only 7 notes and constitutes a major scale or its derivatives, minor being the 6th derivative of major.) This method, combined with the previous method of looking at the first and last chords, can also achieve better results.

In addition to the tonality, the chords of a piece need to be analyzed, so the chords need to be extracted separately (including the need to look at the first and last chords before). Some people may think that extracting the main melody of a song is just a matter of capturing the highest notes, but it's really not that simple. First of all, after the MIDI file is read out, the order of the notes depends on the time when each note appears (the note_on information is read). musicpy's chord class data structure is designed to store the notes in a list, and then the interval between each two adjacent notes is stored in another list. Each note has its own parameter duration, which records the duration of the note. The order of the notes in the list is the same as the information in the MIDI file, which note appears first in the list to start playing. The main melody and the chord part are placed together in this piano piece. The difficulty is that many of the chord notes, especially the highest notes of the broken chords, can be very high and played just a little bit below the current main melody. These chord notes, which are played within the range of the main melody, are intended to harmonize the main melody and to enrich the layering and rhythm of the piece. However, when we listen to the human ear, we can still distinguish those chords that play into the main melody, because they do not form a "coherent melody" like the main melody, so they are generally not classified as such by the listener. (Although not without exceptions, there are occasional piano pieces where part of the main melody is also the highest note of the current chord)

The range of this piano piece is very wide, with the main melody spanning about three octaves and the chord part also spanning about three octaves, so it is obviously not practical to find a single note as a dividing line between the main melody and the chord part.

Now for my algorithm. First, find all the groups of notes that are spaced at 0, (that is, the notes played together at the same time) and keep only the highest notes. This is because if there is a main melody and some notes are played at the same time, then these notes are generally chord notes or ornamental notes that enrich the harmony. Next, a loop is started, traversing the entire list of notes. Two original concepts of my algorithm are mentioned here: melody tolerance and chord tolerance, which have been tested many times and have default values of minor seventh and major sixth. First, for the first note, if the pitch difference between the second note and the first note is greater than or equal to the chord tolerance threshold, then the first note is judged to be a chord note and the second note is the first note judged to be a melody, otherwise the second note is a chord note and the first note is the main melody note. Now we have the first melodic note. In the process of traversing the entire list of notes, the average pitch of the notes classified as melodic within the length of the last bar (8 units of note length) is calculated, and if the total length of the notes currently classified as melodic is less than one bar, the average pitch of all the notes currently classified as melodic is calculated. If the pitch of the current note is higher than this average pitch, it is determined to be the main melody note. If it is lower than the average pitch, but the pitch difference is less than or equal to the melodic categorization tolerance threshold, then the current note is most likely the main melodic note. On the basis of this condition, if the difference in pitch between the most recent note judged to be the dominant melody and the current note is less than the chord categorization tolerance threshold, then the current note is judged to be the dominant melody. (Because if the nearest note of the main melody needs to suddenly jump a long way down to the current note, then even if the composer intended the current note to be the main melody as well, it would be difficult for the listener to develop a sense of melodic coherence, since a melody is essentially a set of notes of relatively similar pitch, contrasted with chord notes of large pitch difference and treated as melodic.) If the current note is lower than the average pitch and the pitch difference exceeds the melodic categorization tolerance threshold, the current note is determined to be the melodic note when and only when the pitch difference between the nearest melodic note and the current note and the pitch difference between the next note and the current note are both less than the chord categorization tolerance threshold. This is the end of the algorithm. I have tested this algorithm with many of my own compositions, and it works surprisingly well, extracting almost 100% of the complete and correct melody notes, regardless of the range of the melody. Once the main melody purification algorithm is successful, the chord extraction is very simple, which means that after the main melody is found, the remaining notes are the chord notes or the main melodic ornamental harmonies. I also came up with an algorithm to decompose the chord parts into individual chords, which I will leave for the next issue. This morning I finally wrote an algorithm with a high accuracy rate, including pieces with modulation and tonicization, various church modes and so on. I will explain these algorithms in other wiki entries.

In musicpy I wrote this original algorithm for separating the main melody and chord notes of a piece as a function called `split_melody`, which can separate the main melody of a piece with a chord type as a new chord type. There is also the `split_chord` function that separates out chord notes as new chord types. The `split_all` function returns 2 chord types, the main melody and chord notes of the piece.

The arguments to the `split_melody` function are, in order

```python
split_melody(current_chord,
             mode='index',
             melody_tol=minor_seventh,
             chord_tol=major_sixth,
             get_off_overlap_notes=True,
             average_degree_length=8,
             melody_degree_tol=toNote('B4'))
```

- current_chord: the chord type you want to separate the main melody from the chord notes (a chord type itself can store a complete piece for a single instrument, such as a piano piece)

- mode: an expression of what is returned after separating the main melody, mode='index' returns the position of all the main melody notes in x (the first note), mode='notes' returns a list of all the main melody notes, mode='chord' returns a new chord type composed of all the main melody notes, whose note intervals The note spacing is recalculated so that it is the same as the spacing of the remaining notes after the chord notes are actually removed.

- melody_tol: This parameter is the melody tolerance threshold that I described above when describing my algorithm.

- chord_tol: This parameter is the chord tolerance I described above when describing my algorithm, you can set your own interval size, the default value is major_sixth.

- get_off_overlap_notes: This parameter is used to deal with the situation where some MIDI files have a lot of overlapping notes (homophonic overlap) that may cause the algorithm to fail to achieve the desired effect.

- average_degree_length: The current average pitch of the algorithm is calculated for each passing bar. The default value is 8 bars.

- melody_degree_tol: This is the third tolerance threshold I recently added after improving the algorithm, called melody degree tolerance. It allows for a more flexible separation of the melody and chord notes, and improves the purity of the separation of the melody and chord notes. The default value is the note B4.

Next I will describe how to do this.

First we prepare the piece, which can be any MIDI file, and then use the `read` function to read the track you want to separate. for example

```python
bpm, a, start_time = read('example.mid').merge()
```

The `read` function returns a piece type, using the `merge` function you can get a tuple with 3 elements, respectively the tempo of the piece (BPM), the chord type of the piece, and the start time of the piece (in bars)

Then we can use

```python
example_melody = a.split_melody(mode='chord')
```

Then we can use the `play` function or `write` function to write the main melody into a new MIDI file (the name of the `play` function can be left out, the name of the output MIDI file is temp.mid by default)

```python
play(melody, bpm, name='example melody.mid')
```

or

```python
write(melody, bpm, name='example melody.mid')
```

Then the MIDI file of the main melody of the example will be generated in the musicpy folder, which you can take to DAW or use on other occasions as needed.

You can use the `split_chord` function to extract the chord notes of a piece, which actually does the same thing as `split_melody`, similar to the above, except that the output MIDI file becomes the chord notes of a piece, for example, called `example_chords`.

Using the `split_all` function, you can get both the main melody and the chord notes of a piece in 2 chord types, and everyone can output them separately as new MIDI files in the same way. But the `split_all` function, when `mode='chord'`, returns not only the two chord types of the main melody and the chord note, but also a third return value, shift, which indicates how many bars further back the chord note starts than the main melody (if the chord note starts before the main melody, the shift value is a negative number). The shift return value is useful when you want to merge a new piece in musicpy after making changes to the main melody or chord notes, and you want to start it in the same position as the beginning of the previous piece.

```python
example_melody & (example_chords, shift)
```

to get a new piece that starts at the same position as the previous piece.

Note that the 2 chord types which are the main melody and chords returned by `split_all` function both contain the same pitch_bend and tempo types as the piece before the separation in their notes list, because the non-note types are not involved in the separation algorithm, but are retained in the separation result. So when you merge the main melody with the chord notes, please clear the pitch_bend and tempo types of one of the chord types before merging, to avoid duplication of the pitch_bend and tempo types, and it is recommended to use the `only_notes` function that comes with the chord type to get a chord type containing only the note types. For example:

```python
example_melody & (example_chords.only_notes(), shift)
```

The start times of the pitch_bend and tempo types reserved for the main melody and chord notes are relative to the original piece, so in case where the main melody or chord notes do not appear from the very beginning of the original piece, the start times of the pitch_bend and tempo types may be off when the main melody or chord notes are played individually, if you want to get the precise start times of pitch_bend and tempo types relative to the main melody or chord notes, you can use the third returned value of the `split_all` function, `shift`, to shift the start times of the pitch_bend and tempo types (you don't need to change the start times if you want to merge them into the original piece), and use the chord types' own `apply_ start_time_to_changes` function with the argument `start_time`, which is a numeric value that adds `start_time` to the start times of the non-note types in the chord type. If shift is positive, then it means that the main melody starts from the beginning and the chords starts some time after the main melody starts, so the main melody does not need to modify the start times of the non-note types, and the start times of the non-note types of the chord notes are all subtracted from the value of `shift` to synchronize with the start times of the pitch bends and tempo changes of the original piece when the chord notes are played individually. If shift is negative, then the chord notes is not modified and the start times of the non-note types of the main melody is subtracted by the value of `-shift`. For example:

```python
example_melody_individual = copy(example_melody)
example_chords_individual = copy(example_chords)
if shift >= 0:
    example_chords_individual.apply_start_time_to_changes(-shift)
else:
    example_melody_individual.apply_start_time_to_changes(shift)
```

For different pieces, setting different 3 tolerance thresholds may give better results. The default 3 tolerance thresholds will work for most pop or jazz piano pieces.

This algorithm I developed based on musicpy to extract the main melody and all the chords from a complete song is so good that it has been used by a programmer who developed a DAW for arranging music and a development team who researched AI music composition and asked me to license it for practical use. I have also included this algorithm as one of the features in an intelligent piano software I wrote, where you can choose to separate the main melody and listen to the chords only after selecting the MIDI file.

