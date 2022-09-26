# Frequently Asked Questions

Here are some issues you may encounter when using musicpy with solutions, some of them are existing bugs of the python libraries that musicpy has dependencies with, or just not giving you the desired result when the function parameters are set as default.

* ## I read a MIDI file using the read function of musicpy, but all of the messages of tracks are read into one track
Normally, a MIDI file is made with multiple individual tracks that is actually separated when read by python MIDI libraries such as mido, midiutil (which are the MIDI libraries that are used in musicpy), but some producers or the software that exports the MIDI files put them into a single track.

You may encounter this issue when you try to read some MIDI files that are back in the past, when the producers made the MIDI files with only one track but still with multiple individual tracks that each has its own instruments, this is actually possible since each note on/off messages (and other MIDI messages) could has its own channel number, so when you playback the MIDI files like this, you can still hear all of the individual tracks. 

Solution: You can separate the tracks by setting the parameter `split_channels` of the `read` function to True, musicpy will separate the messages in the single track to multiple individual tracks which stores in a piece instance as the return value by algorithms.

* ## I tries to play or write the piece instance I read from a MIDI file to a new MIDI file, but when I see the new MIDI file in DAW, some notes become extremely long, which has totally different lengths from the original MIDI file

This is because some MIDI files has duplicate notes in some tracks, here duplicate notes means these notes has the exact same pitch, start at the same time or has some part overlapped, in this case, some python MIDI libraries might consider these duplicate notes as a whole note, which makes the issue happen.

Solution: you can set the parameter `remove_duplicates` of the `write` or `play` function to True to make the written MIDI file the same as when you view the original MIDI file in the DAW.

* ## I get `pygame.error: Couldn't open /etc/timidity/freepats.cfg` error on Linux
On Linux, the pygame library uses the freepats sound libraries to play MIDI files, so you need to install freepats.

Solution: open the terminal and run `sudo apt-get install freepats` and it will fix the errors. This solution works mostly for Ubuntu, for other Linux distributions you need to search the internet to find the install command to install freepats for them, on some Linux distributions you may also need to install timidity++.

* ## When I run the code in the IDE, the play function does not make any sound
When you use python IDE like Pycharm or VS Code, this issue will occur, since these IDE won't wait till the pygame's function that plays MIDI file ends, they will stops the whole process after all of the code are executed without waiting for the playback of the MIDI file. You won't encounter this issue with more interactive python IDE, such as Jupyter Notebook, Wing IDE, or directly use the interactive shell of python in terminal.

Solution: You can add `wait=True` in the parameter of the `play` function, which will block the function till the playback ends, so you can hear the sounds.

* ## When I tries to play/write a piece instance to a MIDI file with `deinterleave=True`, I get `builtins.IndexError: pop from empty list` error
This is a known bug of midiutil, which is one of the python MIDI libraries that musicpy uses. This issue has been mentioned by someone several years ago at the issue section of midiutil on github [here](https://github.com/MarkCWirt/MIDIUtil/issues/24), and the fixes of this bug is actually pretty easy, which is adding a boundary check, but the author of midiutil does not fix this bug till now.

Solution: Navigate to the python installation path of the python version that you are currently using, then go to `Lib\site-packages\midiutil`, open the file `MidiFile.py` in an IDE or text editor, find the function `deInterleaveNotes` of the class `MIDITrack`, change this part:
```python
elif event.evtname == 'NoteOff':
    if len(stack[noteeventkey]) > 1:
        event.tick = stack[noteeventkey].pop()
        tempEventList.append(event)
    else:
        stack[noteeventkey].pop()
        tempEventList.append(event)
```
to this:
```python
elif event.evtname == 'NoteOff':
    if len(stack[noteeventkey]) > 1:
        event.tick = stack[noteeventkey].pop()
        tempEventList.append(event)
    else:
        if stack[noteeventkey]:
            stack[noteeventkey].pop()
            tempEventList.append(event)
```
And then save the file.

* ## I tries to read a MIDI file using the read function but it gives me index out of range error
This is a bug of mido, which is one of the python MIDI libraries that musicpy uses. When some meta messages of the MIDI file contains empty data and mido tries to get attributes from it, not all of the decode function of every type of meta messages has the boundary check for data to see if it is empty, and straightly get the data element by indexing as assuming the data is not empty, which causes index out of range error.

Solution: To fix this issue, you need to go to `Lib\site-packages\mido\midifiles\meta.py`, add boundary check for data to see if it is empty for every decode function of the meta messages, which is adding `if data:` before the code tries to get element from data by indexing. I have already made the changes and created an issue on the issue section of mido on github to ask for the fix of this bug. Here is an example:
```python
class MetaSpec_key_signature(MetaSpec):
    type_byte = 0x59
    attributes = ['key']
    defaults = ['C']

    def decode(self, message, data):
        key = signed('byte', data[0])
        mode = data[1]
        try:
            message.key = _key_signature_decode[(key, mode)]
        except KeyError:
            if key < 7:
                msg = ('Could not decode key with {} '
                       'flats and mode {}'.format(abs(key), mode))
            else:
                msg = ('Could not decode key with {} '
                       'sharps and mode {}'.format(key, mode))
            raise KeySignatureError(msg)
```
changes to:
```python
class MetaSpec_key_signature(MetaSpec):
    type_byte = 0x59
    attributes = ['key']
    defaults = ['C']

    def decode(self, message, data):
        if data:
            key = signed('byte', data[0])
            mode = data[1]
            try:
                message.key = _key_signature_decode[(key, mode)]
            except KeyError:
                if key < 7:
                    msg = ('Could not decode key with {} '
                           'flats and mode {}'.format(abs(key), mode))
                else:
                    msg = ('Could not decode key with {} '
                           'sharps and mode {}'.format(key, mode))
                raise KeySignatureError(msg)
```

* ## I notice that every time I play a musicpy note/chord/piece/track instance, there is a MIDI file generated in current directory, can musicpy just play what I write without writing a MIDI file?
Yes, the default mechanism of the `play` function of musicpy is firstly write the musicpy data structure to a MIDI file, and then use pygame's mixer module to play the MIDI file, but you can change to another mechanism to play internally without any MIDI file generated by setting the parameter `save_as_file` of the `play` function to False. By doing this, the MIDI file data stream is generated and held internally, and pygame is able to play the MIDI file stream directly, there are no MIDI files will be generated.

Solution: Set the parameter `save_as_file` of the `play` function to False.
