
Div's MIDI Utilities
Introduction

Here is a collection of MIDI utilities I wrote for myself, which you may find useful as well. I previously kept separate collections of MIDI utilities for Windows and for Unix, but due to an increasing amount of overlap, I have combined them into a single package.

Regardless of platform, the utilities are all written according to the Unix design philosophy; most run from the command line instead of providing a GUI, and each is small and dedicated to a specific task. Some work in realtime, while others act upon saved MIDI files.

The Windows utilities should work on any recent version of Windows. They come with ready-to-run binaries in addition to a Microsoft-compatible build harness.

Most of the Unix utilities work on raw MIDI streams, so they should be compatible with just about everything, including ALSA, OpenSound, and MacOS/X. They communicate via the standard Unix file interface, so they can read and write directly to a device, to anonymous pipes, or to named FIFOs. The standard tee(1) and mkfifo(1) utilities will come in very useful when working with them.

Only a few of the Unix utilities have dependencies beyond the standard C library, and they are built conditionally. A few are ALSA-specific, and thus require the ALSA library and headers to be installed. Another interacts with X Windows (although it has no visible GUI), so it requires Xlib to be installed.
News

2012.10.06 - Added onmessage. Added miscellaneous bug fixes and tweaks. Merged many of my other experimental MIDI software projects of various vintages into the "extras" directory (no prebuilt binaries provided), including several incomplete attempts at writing sequencers using different widget kits.

2008.09.13 - Added alsamidi2pipe, pipe2alsamidi, alsamidi2net, and net2alsamidi. Added more options to playsmf.

2008.06.11 - Updated the midifile library to support several common musical and SMPTE time formats, with intelligent handling of meters and tempos. Added utilities to help with seamless gluing of multiple takes: average-tempo, average-velocity, scale-tempo, scale-velocity, offset-tempo, and offset-velocity. Added another map for qwertymidi. Also tidied up the source tree and build harnesses. Wow, ten years since I released the first version of this package!

2007.10.29 - Added a few convenience options to the ALSA version of brainstorm. Also added alsamidipipe to make it easier to use the traditional pipe-based Unix utilities in combination with ALSA, even if you don't have a /dev/midi* device set up for each port you care about. On the Windows side, replaced mciplaysmf with playsmf, which uses the midifile library instead of the MCI sequencer.

2006.06.28 - Combined the Unix and Windows packages into one (the Unix utilities are now under the same licence as the Windows ones). Updated the Unix utilities to handle active sensing properly. Added a fully modern, ALSA-specific version of brainstorm. Also added a preview version of imp, a fancy successor to qwertymidi which understands multiple USB keyboards plugged in at the same time.

2005.03.24 - Added a significant amount of functionality to the midifile library, making it suitable for building a much broader range of software. Added tempo-map, joycc, and the mish compiler. Added new maps for qwertymidi. Also changed the package's license from LGPL to BSD, since it is even more generous to users.

2004.10.09 - Added a simple sampler (beatbox) and MIDI file player (mciplaysmf). The sampler introduces new dependencies on the free sndfile and portaudio libraries, but the necessary parts are included in the kit. The player is here by popular user request, although I cheated and used the MCI sequencer which is part of Windows rather than implementing my own from scratch. Also rewrote brainstorm to use the midifile library for robustness, and gave it some new features as well.

2003.06.27 - By user request, the midifile library now provides control over the MIDI file format (0, 1, or 2).

2003.04.27 - Added pulsar, padpedal, and sendmidi. No more external dependencies to build; the necessary parts of expat are now included in the kit.

2003.03.11 - Added fakesustain and multiecho.

2003.02.22 - Introducing metercaster, a new way to adjust the timing of MIDI sequences without the dehumanizing side effects of quantization. When I went to implement it, I was surprised to find that there was no readily available, free C library for reading and writing standard MIDI files, so I'm also introducing the midifile library, which fills the niche.

2003.01.23 - Brainstorm now outputs more conventional MIDI files (format 1 with PPQ timing, rather than format 0 with SMPTE timing), so they will be compatible with more software.

2003.01.20 - Fixed a critical bug in brainstorm. Also made significant improvements to the build system and packaging.
The Utilities
alsamidi2pipe (Unix) and pipe2alsamidi (Unix)

These utilities act as bridges between ALSA and the pipe-based utilities.

Usage: alsamidi2pipe [ --fifo <fifo> ] [ --name <client name> ] [ --connect <client name or number to which to connect> <port number> ]

Usage: pipe2alsamidi [ --fifo <fifo> ] [ --name <client name> ] [ --connect <client name or number to which to connect> <port number> ]
average-tempo (Windows, Unix), scale-tempo (Windows, Unix), offset-tempo (Windows, Unix), average-velocity (Windows, Unix), scale-velocity (Windows, Unix), and offset-velocity (Windows, Unix)

These command line utilities can be used together for patching up multiple takes of a song to match one another. The ability to analyze a specified section of a song is particularly useful if the tempo varies frequently, as is the case when the song has been through tempo-map or metercaster.

Usage: average-tempo [ --tick | --beat | --mb | --mbt | --time | --hms | --hmsf ] [ --from <time> ] [ --to <time> ] <filename.mid>

Usage: scale-tempo [ --tick | --beat | --mb | --mbt | --time | --hms | --hmsf ] [ --from <time> ] [ --to <time> ] --amount <n> [ --out <filename.mid> ] <filename.mid>

Usage: offset-tempo [ --tick | --beat | --mb | --mbt | --time | --hms | --hmsf ] [ --from <time> ] [ --to <time> ] --amount <n> [ --out <filename.mid> ] <filename.mid>

Usage: average-velocity [ --tick | --beat | --mb | --mbt | --time | --hms | --hmsf ] [ --from <time> ] [ --to <time> ] [ --track <n> ] <filename.mid>

Usage: scale-velocity [ --tick | --beat | --mb | --mbt | --time | --hms | --hmsf ] [ --from <time> ] [ --to <time> ] [ --track <n> ] --amount <n> [ --out <filename.mid> ] <filename.mid>

Usage: offset-velocity [ --tick | --beat | --mb | --mbt | --time | --hms | --hmsf ] [ --from <time> ] [ --to <time> ] [ --track <n> ] --amount <n> [ --out <filename.mid> ] <filename.mid>
beatbox (Windows)

This command line utility is a very simple, percussion-oriented sampler. It plays prerecorded audio files (WAV, AIFF, or AU) in response to MIDI note events. This is most useful when combined with a pattern sequencer, but none is provided here at present.

Usage: beatbox [ --in <n> ] [ --voices <n> ] ( [ --config <filename> ] ) ...
brainstorm (Windows, Unix)

This command line utility functions as a dictation machine for MIDI. It listens for incoming MIDI events and saves them to a new MIDI file every time you pause in your playing for a few seconds. The filenames are generated automatically based on the current time, so it requires no interaction. I find it useful for recording brainstorming sessions, hence the name, and use it more than all the other utilities put together.

Usage (Windows version): brainstorm [ --in <device id> ] [ --timeout <seconds> ] [ --extra-time <seconds> ] [ --prefix <filename prefix> ] [ --verbose ]

Usage (original Unix version): brainstorm <input fifo> <filename prefix> <timeout in seconds>

Usage (ALSA-specific version): abrainstorm [ --prefix <filename prefix> ] [ --timeout <seconds> ] [ --confirmation <command line> ] [ --connect <client name or number> <port number> ]

Note that by default, the ALSA version does not connect up to an existing MIDI output port; rather it creates a new MIDI input port which you can then connect up as you see fit. aconnect, aconnectgui, and qjackctl are common utilities which can be used to do this.

The confirmation option allows you to specify a command to execute whenever a file is saved, so that you know your music is safe. I use it to play a short audio file, in keeping with brainstorm's "interfaceless" design, but you can get fairly elaborate if you want. The filename will be substituted for each %s in the command.
fakesustain (Windows)

This command line utility pre-applies sustain to note events. It is useful for software-based synthesizers which have incomplete MIDI implementations that don't respond to real sustain messages. (VST is a good idea, but can be frustrating in practice.)

Usage: fakesustain [ --in <n> ] [ --out <n> ]
intervals (Unix)

This filter-style utility plays parallel intervals for incoming MIDI messages.

Usage: intervals <input_fifo> <output_fifo> <interval 1> <interval 2> ...
joypedal (Windows) and joycc (Windows)

Joypedal is a command line/text mode interactive utility which requires custom hardware. It looks for an organ style volume/expression pedal attached to the computer's joystick port, and translates its value into MIDI controller messages. I wrote this because one of my old synthesizers hardwired its expression pedal to control volume, rather than making it assignable through MIDI. Extremely simple instructions for building the joystick port adapter (for around 5 USD in parts) are included, but ATTEMPT THIS OR ANY OTHER CUSTOM HARDWARE PROJECT AT YOUR OWN RISK.

Joycc is a command line utility which is functionally similar to joypedal, but is designed to work with real joysticks. Up to three directional axes and four buttons on each of two joysticks can be mapped to separate MIDI controllers.

Usage: joypedal [ --out <n> ] [ --channel <n> ] [ --controller <n> ] [ --graph ]

Usage: joycc [ --out <n> ] [ --channel <n> ] [ --x1 <n> ] [ --y1 <n> ] [ --z1 <n> ] [ --a1 <n> ] [ --b1 <n> ] [ --c1 <n> ] [ --d1 <n> ] [ --x2 <n> ] [ --y2 <n> ] [ --z2 <n> ] [ --a2 <n> ] [ --b2 <n> ] [ --c2 <n> ] [ --d2 <n> ]
jumpoctave (Unix)

This filter-style utility lets you use the pitch bend wheel as an octave jump control.

Usage: jumpoctave <input fifo> <output fifo>
lsmidiins (Windows) and lsmidiouts (Windows)

These command line utilities display a numbered list of the Windows MIDI input and output ports. Most of the other Windows utilities here refer to ports by number.

Usage: lsmidiins

Usage: lsmidiouts
metercaster (Windows, Unix) and tempo-map (Windows, Unix)

Metercaster is a command line utility which implements a brand new, unique algorithm for adjusting the timing of a MIDI sequence. Unlike conventional quantization, meter casting does not replace the human characteristics of your playing with a mechanistic feel. Instead, you place waypoints in the sequence to tell metercaster what the proper time should be for events of your choosing, and it interpolates among them, coming up with the proper times for the rest of the events. This version is meant to be used iteratively, as you specify a single waypoint per invocation. Metercaster is especially useful in conjunction with brainstorm, whose interfaceless design makes a metronome inappropriate.

Tempo-map is a command line utility which can be used to acheive a similar effect to metercaster, but is far faster and more intuitive to use. Inspired by, but more powerful than the "fit to improvisation" feature in Cakewalk, it expects the following usage model: First, you record your performance without a metronome, as with brainstorm. Next, ignoring any beat markers your sequencer might display, you add a click track consisting of one arbitrary note per beat, synchronized with your performance. Tempo-map then processes the file, adjusting the timestamps of existing events and inserting new tempo events so that performance sounds the same when played back, but the notes will line up with beats when displayed in the sequencer. You can then selectively delete the inserted tempo events, resulting in a steady but still completely nuanced recording.

Usage: metercaster [ --left ( marker <name> | cc <number> <value> ) ] [ --old-right ( marker <name> | cc <number> <value> ) ] [ --new-right ( marker <name> | cc <number> <value> ) ] [ --verbose ] <filename>

Usage: tempo-map --click-track <n> [ --out <filename.mid> ] <filename.mid>
midimon (Windows) and dispmidi (Unix)

This command line utility pretty-prints incoming MIDI messages. It can be useful for debugging complex MIDI setups, but there's nothing special about it. The name differs by platform for historical reasons, but it works the same, regardless.

Usage (Windows version): midimon [ --in <n> ] ...

Usage (original Unix version): dispmidi <input fifo>

Usage (ALSA-specific version): adispmidi [ --connect <client name or number> <port number> ]
midithru (Windows)

This command line utility reads from any number of Windows MIDI input ports, merges the streams together, and copies the result to any number of Windows MIDI output ports. You can think of it as a combination of a hardware thru box and merge box.

Usage: midithru [ --in <input device id> ... ] [ --out <output device id> ... ]
mish (Windows, Unix)

This command line utility is a compiler for a text-based music notation language which I invented, called "Mish" (MIDI shorthand). It converts Mish files into standard MIDI files.

Usage: mish --in <input.mish> --out <output.mid>
multiecho (Windows)

This command line utility can add multiple echoes to your performance, with configurable delay, pitch offset, and velocity scaling.

Usage: multiecho [ --in <n> ] [ --out <n> ] [ --echo <delay in ms> <note interval> <velocity percent> ... ]
netmidic (Windows), pipe2net (Unix), alsamidi2net (Unix), netmidid (Windows), net2pipe (Unix), and net2alsamidi (Unix)

These utilities all speak NetMIDI, a trivial network protocol I created which sends standard MIDI messages over a TCP/IP connection as fast as possible. They can be used to connect up the MIDI systems on two different machines over a network connection, even if they are running different operating systems. The clients forward messages from the local MIDI system to a NetMIDI server. The servers forward messages sent by the clients to the local MIDI system. Note that pipe2net and net2pipe are not actually MIDI specific, and are essentially the same thing as the standard netcat utility, but I didn't know that when I wrote them.

Usage (Windows version of the client): netmidic [ --in <n> ] [ --hostname <name> ] [ --port <n> ]

Usage (original Unix version of the client): pipe2net [ <input fifo> ] <hostname> <port>

Usage (ALSA-specific version of the client): alsamidi2net --server <hostname> <port> [ --name <client name> ] [ --connect <client name or number to which to connect> <port number> ]

Usage (Windows version of the server): netmidid [ --out <n> ] [ --port <n> ]

Usage (original Unix version of the server): net2pipe <port> [ <output fifo> ]

Usage (ALSA-specific version of the server): net2alsamidi --port <port> [ --name <client name> ] [ --connect <client name or number to which to connect> <port number> ]
normalizesmf (Windows, Unix) and verbosify (Unix)

Normalizesmf is a command line utility which provides a minimal demonstration of the midifile library. It reads in a MIDI file, then writes it out again. If your sequencer complains that a file is invalid, this normalizer might make it more palatable.

Verbosify is a filter-style utility which normalizes incoming MIDI messages to a canonical form, like a realtime equivalent to normalizesmf. It should not change the way things sound, but may make it easier to postprocess the stream.

Usage: normalizesmf <filename>

Usage: verbosify <input fifo> <output fifo>
notemap (Windows)

This command line utility lets you remap the layout of notes on your MIDI keyboard. I originally wrote it to see whether the guitar concept of "alternate tunings" could be applied to the piano. That experiment was a failure, but notemap has since become essential to me for playing drums from the piano keyboard.

Usage: notemap <filename>.xml
onpedal (Windows) and onmessage (Windows)

Onpedal is a command line utility which runs a program or script every time you press the sustain pedal. If you hold down the pedal for more time, it runs a second program. I use it for turning pages in scanned copies of sheet music displayed on screen. (If you have more than one pedal, you don't have to give up sustain.)

Onmessage is similar, but it lets you assign different commands to different note and controller events.

Usage: onpedal --in <n> --command <str> [ --hold-length <n> ] [ --hold-command <str> ]

Usage: onmessage --in <n> ( --note-command <number> <command> | --note-down-command <number> <command> | --note-up-command <number> <command> | --controller-commmand <number> <command> | --controller-down-command <number> <command> | --controller-up-command <number> <command> | --program-command <number> <command> ) ...
padpedal (Windows)

This command line utility is a variation on the idea of a sustenuto pedal (the middle pedal on most grand pianos) which adds a "pad" accompaniment to your playing.

Usage: padpedal [ --in <port> <channel> ] [ --pad-in <port> <channel> <controller> ] [ --out <port> <channel> ] [ --pad-out <port> <channel> ]
pedalnote (Windows)

This command line utility plays a note whenever you press the sustain pedal. I use it for kick drums.

Usage: pedalnote [ --in <n> ] [ --out <n> ] [ --channel <n> ] [ --note <n> ] [ --velocity <n> ]
playsmf (Windows)

This command line utility is a generic MIDI file player. Nothing special, but it gets along well with the other utilities. Now based on the midifile library, this replaces the older mciplaysmf.

Usage: playsmf [ --out <device id> ] [ -- ] <filename.mid>
pulsar (Windows)

This command line utility pulses any depressed notes according to a configurable rhythm.

Usage: pulsar [ --in <n> ] [ --out <n> ] [ --pulse <time before pulse> <length of pulse> <time after pulse> ] ...
qwertymidi (Windows), delta (Windows), and imp (Windows)

Qwertymidi is a command line/text mode interactive utility lets you use your computer keyboard as if it were a synthesizer keyboard. It supports custom mapping of the keyboard layout by means of a config file. A list of keyboard scan codes will be useful for configuring the mapping. Sample maps are included for the common piano-like tracker keyboard layout, the experimental von Janko piano keyboard layout from the 1850's, the Hayden concertina button layout, and a General MIDI drum kit.

Delta is a fun to play, monophonic variation on qwertymidi which maps keys on the computer keyboard to relative intervals instead of absolute pitches. It was inspired by the unusual Samchillian MIDI controller I read about online. It also supports custom keyboard mappings.

Imp (input mapping program) is a rudimentary but usable attempt to create a more powerful successor to qwertymidi. It is implemented as a true GUI program in order to use newer Microsoft APIs which distinguish amongst multiple USB keyboards plugged into the same computer at the same time.

Usage: qwertymidi [ --out <n> ] [ --channel <n> ] [ --program <n> ] [ --velocity <n> ] [ --map <str> ]

Usage: delta [ --out <n> ] [ --channel <n> ] [ --program <n> ] [ --velocity <n> ] [ --map <str> ]

Usage: imp [ <filename> ]
rw (Unix)

On some operating systems, devices such as /dev/midi can only be opened by a single program at a time, which is a problem if you want different programs to read and write to it simultaneously. This utility, while not necessarily MIDI specific, can be used to split out a device's input and output to separate FIFOs.

Usage: rw <device file> <input fifo> <output fifo>
sendmidi (Windows)

This command line utility sends a single specified MIDI message. It can be used for scripting, or as a poor man's knob box.

Usage: sendmidi [ --out <port> ] ( --note-off <channel> <pitch> <velocity> | --note-on <channel> <pitch> <velocity> | --key-pressure <channel> <pitch> <amount> | --control-change <channel> <number> <value> | --program-change <channel> <number> | --channel-pressure <channel> <amount> | --pitch-wheel <channel> <amount> )
smftoxml (Windows, Unix) and xmltosmf (Windows, Unix)

These command line utilities convert a MIDI file into an ad hoc XML equivalent and back. This can be useful for seeing exactly what is in the file, and how it is arranged in the data structures of the midifile library. Using the two utilities together, you can modify MIDI files with a text editor or even with XSLT.

Usage: smftoxml <filename>

Usage: xmltosmf <filename>.xml <filename>.mid
transpose (Unix)

This filter-style utility transposes incoming MIDI messages by a specified interval.

Usage: transpose <input fifo> <output fifo> <interval>
velocityfader (Windows)

This command line utility scales incoming note velocities according to the current value of a specified continuous controller. This gives you a different sort of volume control than normal, in that it only affects new notes coming in, leaving the sustained ones alone. It can also be used to add pedal-operated volume control to a synth that doesn't support standard expression messages.

Usage: velocityfader [ --in <n> ] [ --out <n> ] [ --controller <n> ] [ --inverted ]
xmidiqwerty (Unix)

This is an experimental, partially completed program which lets you use a MIDI keyboard as a chording text keyboard in X11. It runs as a standard program sending synthetic keyboard events rather than an IME, so it does not require any complex installation nor disrupt the use of your normal keyboard.

Usage: xmidiqwerty (reads from standard input)
Useful Libraries Also Provided
midifile (Windows, Unix)

This powerful and practical library allows you to read and write Standard MIDI Files (SMF), and provides a data structure for MIDI sequences. Essentially, it is the core of a MIDI sequencer without the user interface and the realtime recording and playback functionality.
midimsg (Unix)

This library provides an abstraction around raw realtime MIDI streams. It hides annoying complexities like running status and active sensing. Unnecessary for Windows or native ALSA programs, since this functionality is already provided for you there.
Download

These utilities are free and open source, provided under terms of the BSD license.

midi-utilities-20121006.zip (1.5Mb)

Source is managed in Subversion under the project divs-midi-utilties at Google's code hosting service.
Related Links

On Windows, a MIDI loopback utility is necessary to get the most out of these programs. It will provide extra Windows MIDI ports which allow you to wire multiple MIDI programs together instead of speaking directly to your sound card or synthesizer. While several such programs exist, both free and commercial, I am currently using Tobias Erichsen's free LoopMIDI.

home · dgslomin@alumni.princeton.edu · last updated 2012.10.06
