# Basic syntax of pitch_bend type



## Contents

- [Inserting real time pitch bends, glissandos, and vibrato changes into a piece](#inserting-real-time-pitch-bends-glissandos-and-vibrato-changes-into-a-piece)



## Inserting real time pitch bends, glissandos, and vibrato changes into a piece

You can use the pitch_bend type to insert into a chord type, which can change the pitch of a note in a fragment in real time, and can change the pitch very subtly.
When constructing the pitch_bend type, three different units can be selected using the mode parameter:

* semitone (any integer, decimal or fraction between -2 and 2, not including -2 and 2 itself)
* cent (1 semitone = 100 cents, any integer, decimal or fraction between -200 and 200, not including -200 and 200 itself, cent is the default unit)
* MIDI pitch bend value (any integer between -8192 and 8192, not including -8192 and 8192 itself)

### The composition of pitch_bend type

```python
pitch_bend(value, start_time=None, mode='cents', channel=None, track=None)
```

- value: the amount of pitch change of the note
- start_time: the position of the pitch change of the note in bars, if not set, the exact position of the pitch_bend type inserted into the chord type
- mode: mode == 'cents' selects cents as unit, default value, mode selects cents as unit if not set; mode == 'semitones' selects semitones as the unit; mode == 'values' selects MIDI pitch bend values as the unit
- channel: MIDI channel number
- track: MIDI track number

### pitch_bend type is inserted into the chord type

Just insert pitch_bend type as a note type into the chord type

```python
a = chord(['C5', 'D5', 'E5', pitch_bend(30), 'F5', 'G5', 'A5', 'B5', 'C6']) % (1/8,1/8)
play(a, 80)
# The chord type a will be played at normal pitch from the beginning to E5, and the notes after that will be played 30 cents higher (that's 0.3 semitones)
```

### Set time to choose where to start changing the pitch of the notes

```python
a = chord(['C5', 'D5', 'E5', 'F5', 'G5', 'A5', 'B5', pitch_bend(30, 0.5)]) % (1/8,1/8)
play(a, 80)
# The chord type a will be played at normal pitch from the beginning to bar 0.5, and subsequent notes will be played at a pitch of 30 cents (that is, 0.3 semitone) (with bar 0 as the beginning)
```

### Syntax for pitch_bend type construction written in a string

```python
a = chord('C5, D5, E5, pitch;30, F5, G5, A5, B5, C6') % (1/8,1/8)
a = chord('C5, D5, E5, F5, G5, A5, B5, C6, pitch;30;1.5') % (1/8,1/8)
a = chord('C5, D5, E5, F5, G5, A5, B5, C6, pitch;1.37;1.5;semitones') % (1/8,1/8)
# Note that since the pitch_bend type is not a note type per se, but just a message informing of a change in pitch of a note, in the syntax written in a string
# There is no syntax for setting the length, interval, etc. of a note type using parentheses, e.g. pitch_bend;30(.8;.) This does not work.
# In syntax written in a string, the mode argument is the third argument, after value and start_time, and the next two arguments are channel and track
```

