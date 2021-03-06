# 7MU007-Musical-Human-Computer-Interaction
A real-time musical performance software synthesiser controlled via a PlayStation 3 pad. Built using pure data https://puredata.info/

##Background

Inspired by Brian Eno's generative music project "Scape" (http://www.generativemusic.com/scape.html), a tool that he claims possesses the ability to make music that thinks for itself. Eno (2012) believes that generative music requires guidelines to establish its boundaries, but it creates its own music. He describes the process as setting up the initial conditions and then letting the programme run, comparing the outcome to the growth of a seed. Believing that like a seed, something unique will grow out of the music. Neither the listener nor the musician will know exactly what it will be, but each has a general idea or range of expectations (http://longnow.org/seminars/02006/jun/26/playing-with-time/).

This project employs that same methodology, in two main areas especially. Firstly a few parameters are set into place, the scale and key boundaries to be precise. Once the note selection and ratchet values are set to random, the project selects from a filtered down range of notes and ways to play them. Even if the exact same variables are selected each time, the music will not sound the same. Certain themes will be recognisable but they will not be identical.

Also the synthesiser pad works on this principle, as the first note in the sequence is randomly selected, that value is sent to the pad to be used as the root note. Depending on the key and scale selected the pad takes the root note value a creates a chord that will fit into whatever parameters are selected. This process between the random note selection and continuous synthesiser pad will progress indefinitely and will constantly generate new combinations.

##Prerequisites

In order to be able to run this project you will need to have pure data extended installed on your system. Pure Data is free and available here https://puredata.info/downloads/pd-extended follow the guides online on how to set Pd up on your OS.

You will also need to have a PlayStation 3 controller to be able to play the synthesiser the way it was intended to be used, due to the way its controls are mapped to each parameter.

To use the PS3 controller within pure data you will need to download the [hid] object available here https://puredata.info/downloads/hid follow the guide online on how to install this object on your OS.

##Interface

Open LorkSynthPS3.pd

![1](https://github.com/alexchilton1/7MU007-Musical-Human-Computer-Interaction/blob/master/1.png)

The is synthesiser is capable of much more than it currently does within this patch but some of its parameters have been limited for the sake of the group project, where multiple of these kind of real-time instruments link up and interact as a digital orchestra. If the parameters did not work within these constraints then it would be impossible, or at least extremely difficult, to have any control over this synthesiser in the group performance. The limitations set in place for this project allow full control over the instrument at any given point of the performance, so the player has the ability to react to any other changes made by the other instruments in the orchestra during the performance.

######Connecting the PS3 Controller

Plug the controller into one of the USB (Universal Serial Bus) ports on the computer, then navigate to the controller section.

![2](https://github.com/alexchilton1/7MU007-Musical-Human-Computer-Interaction/blob/master/3.png)

Inside the controller abstraction there are two messages boxes called [refresh] and [print], click on these boxes respectively and in the Pd window a list of connected devices will appear. In this case the one that is needed is the one relating to the device number of the PS3 controller.

![3](https://github.com/alexchilton1/7MU007-Musical-Human-Computer-Interaction/blob/master/2.png)

Select the corresponding device number on the radio selection box and engage the toggle box attached to the hid object. Hid (Human Interface Device) is an object for reading data from USB HID devices like keyboards, mice, joysticks, gamepads, keypads and all sorts of other esoteric controllers like USB knobs, touchscreens, Apple IR Remotes. It represents the data with a cross-platform message scheme which is then translated to the underlying native API for input devices. The PS3 controller should now be connected to the pure data patch.

######How The Controller Works

The controller makes use of modifier keys to extend the amount of controls available in the patch. A modifier key is a special key (or combination) that temporarily modifies the normal action of another key when pressed together. The L2 and R2 keys
can be held down in conjuction with the four d-pad (directional pad) and four shape buttons.

![4](https://github.com/alexchilton1/7MU007-Musical-Human-Computer-Interaction/blob/master/4.png)

The analogue sticks are only used as push buttons and not as moving potentiometers. When clicked the left lowers the wet amount of the reverb and the right raises it. L1 and R1 toggle the random ratchet and selection on and off, select toggles the LFO on and off while start engages and disengages the polyphonic synth pad. As previously stated, the other buttons have three possible applications each. They all work in the same way, when the unmodified key is pressed the number 100 is triggered. When L2 is held during this it adds 10 to the overall number, the same is true when holding down R2 but instead it adds 20. These values run into a selection box choosing between 100, 110 and 120 and outputting numbers 1, 2 and 3 respectively.

![5](https://github.com/alexchilton1/7MU007-Musical-Human-Computer-Interaction/blob/master/5.png)

The final numbers are sent to various sections of the instrument which take the value and use it to trigger a bang either raising or lowering the value of its chosen parameter. This happens by using a fairly simple yet effective system.

![6](https://github.com/alexchilton1/7MU007-Musical-Human-Computer-Interaction/blob/master/6.png)

The down and up controls flow through a spigot and, when allowed to pass through, bang an integer either adding to or subtracting from the final output value. This value is sent into the second inlet on the integer object being converted into an integer and stored for later use. The same value is being sent up to the float inlet of the two spigots. The left spigot specifies the lowest number that can pass through while the right specifies the maximum. These both run through separate reverse boxes before being passed into their respective spigots, this is the clever and important part. When the lowest or highest value has been reached the corresponding spigot gets shut off, this is what provides the ability to cap each parameter between two specific values making it adaptable across any range of numbers. All of the buttons that use the modifier keys employ this scaling function.

Here is the full list of parameters and their respective controls.

![7](https://github.com/alexchilton1/7MU007-Musical-Human-Computer-Interaction/blob/master/7.png)

######Functionality

The first main functions are the key and scale selection. There are five scales to choose from; Major, Minor, Lydian, Mixolydian and Phrygian. Selecting 0-4 on the scale radio will write the corresponding scale to the "scaler" table, these messages contain the appropriate intervals used in each scale. 

![8](https://github.com/alexchilton1/7MU007-Musical-Human-Computer-Interaction/blob/master/8.png)

The key selection section offsets the chosen value by a number ranging from 0-11. Each number coincides with a note from the chromatic scale. The chromatic scale is a musical scale with twelve pitches, each a semitone above or below another. On a modern piano or other equal-tempered instrument, all the semitones have the same size (100 cents). In other words, the notes of an equal-tempered chromatic scale are equally spaced. For example, 0 would be equal to note C and 1 would be equal to C#/Db.

![9](https://github.com/alexchilton1/7MU007-Musical-Human-Computer-Interaction/blob/master/9.png)

These two features are important and are used in conjunction with the note selection sliders. Before that stage is reached though there are a few aspects to introduce. The first is the tempo control, it takes the millisecond value, divides it by 60000 (that equals 1 minute) and produces a final value. That value is how many beats per minute will occur using the current millisecond number. This flows into a metro (metronome) object, which triggers repeated steps that are divided to produce eight separate tiggers per sequence. This millsecond value is divided by eight and sent to a second metro object, it is divided like this to ensure it triggers at the start of every sequence. This metro triggers eight sets of random numbers between 0 and 22 which are passed to the faders below, their values are then passed into float objects where the numbers are stored until the corresponding step in the sequence is triggered. This is where the key and scale features become important, the number between 0 and 22 from each fader is passed to the eight boxes labelled pd at the bottom of the abstraction. For each fader a number is received, passed through the table with the selected scale intervals and pushed up or pulled down to meet the nearest interval in that scale. The key offset is added and the outcome is a number that sits within the chosen scale set in a particular key.

![10](https://github.com/alexchilton1/7MU007-Musical-Human-Computer-Interaction/blob/master/10.png)

As this part of the instrument is an 8-Step Monophonic Sequencer all of the values get picked up by a single mtof (MIDI to frequency) object and passed onto the next section.

![11](https://github.com/alexchilton1/7MU007-Musical-Human-Computer-Interaction/blob/master/11.png)

The next section contains the ratchet, step selection, attack and decay functions. The first inlet in the abstraction receives the toggle state from the global tempo toggle and the second inlet receives the millisecond value after it has been divided by eight, meaning it will produce a new set of data every time the eight step sequence resets. The output of this metro object runs into two separate places. The first in the random ratchet abstraction, there are eight of these; one for every step. Inside these is an expression that randomly chooses a number bewteen 0 and 2 which selects the respective ratcheting value. Even though the number range says 0-3 in the picture below the amount will never reach 3. It is always capped at the value below the final one chosen.

![12](https://github.com/alexchilton1/7MU007-Musical-Human-Computer-Interaction/blob/master/12.png)

The random ratchet values run into a series of gates that allow for this function to be turned on and off whenever the user presses the L1 button. 

![13](https://github.com/alexchilton1/7MU007-Musical-Human-Computer-Interaction/blob/master/13.png)

If the random ratchet option is selected then the radio objects are set to a number between 0 and 2 which gets delivered to the eight "pd ratchet" abstractions. Inside here the initial note in the sequence gets triggered on and off but if a ratchet option is selected the tempo is divided by one of two numbers. If number 1 is selected the ratchet is divided by four, this allows two pulses to be played in the space of one note. The reason it is divided by four instead of two is because the process requires four triggers. It needs a value to trigger the initial note on, another to trigger the delay before turning that note off and two more to repeat this process for the second note. This process is doubled for the last value which triggers four notes on and off.

![14](https://github.com/alexchilton1/7MU007-Musical-Human-Computer-Interaction/blob/master/14.png)

These ratchet values flow into another gate system, functioning the same as the previously mentioned one, but this time either sends the information to every step of the sequence or only the ones it randomly selects. The random select behaves similarly to the random ratchet section, once selected, this tool chooses a number between 0 and 1. Zero meaning the step does not trigger an output and one meaning the selected output gets triggered.

Both of the choices get routed to the attack and decay section. This is important as even if the step does not trigger a sound output, it will still trigger a value that tells the attack and decay section to remain at zero, this is important as this section knows it has not missed a step and can safely move onto the next step. This process is triggered on and off with the R1 button. The attack and decay section works by receiving the previously stated values, triggering the velocity after a specified amount of milliseconds and then going to zero in a selected amount of time, after waiting for the specified attack time to pass. The calculation is as follows: 1 $1, 0 $2 $1. The dollar signs act as variables, so an example would be: go to 1 after waiting 100 milliseconds ($1), then go to 0 after waiting for the 100 milliseconds to pass ($2) in 200 millseconds ($1). These values are routed to a vline object and passed to the outlet. The vline object generates linear ramps whose levels and timing are determined by messages sent to it, the messages consist of a target value, a time interval and an initial delay.

![15](https://github.com/alexchilton1/7MU007-Musical-Human-Computer-Interaction/blob/master/15.png)

The next part in the chain is the portamento section, here the MIDI notes are passed to the oscillators in sequence however the portamento value determines how long it will take to slide up or down, in milliseconds, to the next note. This happens by using a line object and specifying the note value and the time in millseconds it takes to reach the next note.

![16](https://github.com/alexchilton1/7MU007-Musical-Human-Computer-Interaction/blob/master/16.png)

Next is the FM modulation amount, the FM synthesis is modulated using a low-frequency osciallator (LFO). This LFO uses the [sig] object which converts numbers to audio signals. A number is converted into an audio signal, which is read by the mtof and driven by a triangle wave. This is multipled by the FM dial to modulate the signal and fed into the oscillator section to create a more complex wave. FM synthesis is a form of audio synthesis where the timbre of a simple waveform is changed by modulating its frequency with a modulator frequency that is also in the audio range, resulting in a more complex waveform and a different-sounding tone that can also be described as "gritty". The frequency of an oscillator is altered or distorted. There is also a secondary wave being added alongside the original wave to add width to the synthesiser sound.

![17](https://github.com/alexchilton1/7MU007-Musical-Human-Computer-Interaction/blob/master/17.png)

The attack and decay values enter next, they are routed into the main mulitplier box and dictate whether the signal is turned on or off. Next is the standalone LFO, this runs alongside the original wave and has been set to trigger on and off with the main wave. It uses the cutoff frequency argument of a state-variable filter, the cutoff frequency dial runs into pack and line objects to smooth the dial turns, this is sent into an addition box to be summed. Next an oscillator is set to modulate slowly at 0.25Hz, multiplied by 400 and then has had 400 added to it. Which means it modulates between 400 and 800, this is connected to a multiplier box and a second knob divides it by 127 for control over the LFO rate. These two signals get summed together into the cutoff frequency inlet of the [svf]. This runs alongside the main oscillator.

![18](https://github.com/alexchilton1/7MU007-Musical-Human-Computer-Interaction/blob/master/18.png)

Finally there are two filters running in series. They are low and high pass filters. They run between 0 and 145, using the power calculation to scale quickly between 0 and 21025. Rather than having to map 20000+ steps for the controller when changing the filter values this technique allows the filter to sweep up and down in four movements relatively quickly.

![19](https://github.com/alexchilton1/7MU007-Musical-Human-Computer-Interaction/blob/master/19.png)

The next audio generation section involves the synthesiser pad, which runs in polyphony and uses four voices. It has the ability to adapt to whatever scale is currently selected, as the main scale is altered the pad uses a gate to switch between different intervals used in the various scales. The first step in the sequence triggers a MIDI note value which is routed into the synthesiser pad, a number between 0 and 1 is chosen randomly which switches between two octaves. If 0 is selected the note plays at its original value, however, if 1 is selected the note is raised by an octave therefore raising the whole chord it generates by an ocatve. This works by selecting the first value, which acts as the root note. For example, if a major scale is selected the first number is the root and two more numbers are chosen alongside this, 3rd and 5th. The root has 4 added to it creating the 3rd which then has 3 added to it creating the 5th. This is because a major chord's structure is root, 3rd, 5th. In note form, a C major chord would be 48(C), 52(E) and 55(G). The intervals chosen are the intervals used in whatever scale is selected. These interval values are then sent it a makenote object, with the velocity and note duration set to the same amount. This is so that the notes always trigger on and off, allowing for an ADSR (Attack, Decay, Sustain and Release) envelope to be used. Each individual note has the option to use one of four voices, this is what makes the instrument polyphonic.

![20](https://github.com/alexchilton1/7MU007-Musical-Human-Computer-Interaction/blob/master/20.png)

It sends the polyphonic data out to the four voices stored inside the voice management subpatch, these voices use a global ADSR to control how the pad sounds dynamically and are all added together in a single expression.

![21](https://github.com/alexchilton1/7MU007-Musical-Human-Computer-Interaction/blob/master/21.png)

Inside each voice there is an oscillator section and unique ADSR. The incoming signal is split with the MIDI note being sent to the oscillator section and the trigger data going to the envelope. 

![22](https://github.com/alexchilton1/7MU007-Musical-Human-Computer-Interaction/blob/master/22.png)

The ADSR waits for two pieces of information, note on and note off. There is a select 0 object in the voice 1 abstraction, the left hand side passes the note on data and the right hand side lets the envelope know that the note is no longer being played. The envelope works by going to 1 in the specified time set by the attack fader, waiting for that amount of time and setting the sustain volume at a level after waiting for a specific amount of time set by the decay fader. After the note off trigger is sent, the release fader states how long, in milliseconds, it take for the note to completely fade out.

![23](https://github.com/alexchilton1/7MU007-Musical-Human-Computer-Interaction/blob/master/23.png)

The MIDI note is sent to a mtof and is converted into frequency. Inside the oscillator bank this frequency flows into sinewave and squarewave oscillators which are sent out into a multiplier box.

Back in the main patch, there is a subpatch for the ADSR controls. This is simply a global send to all the individual envelope generators.

![24](https://github.com/alexchilton1/7MU007-Musical-Human-Computer-Interaction/blob/master/24.png)

The penultimate subpatch contains the reverb section. For this project, in the interest of time, the Freeverb~ object was used instead of building one from scratch. The dry signal, damping and roomsize are all set to static values. The control comes in the form of the wet amount, the player can dial in as much or as little as necessary by pressing the analogue 1 and 2 buttons.

![25](https://github.com/alexchilton1/7MU007-Musical-Human-Computer-Interaction/blob/master/25.png)

Finally this stereo signal is routed out to the output~ object which controls the overall volume and provides fast access to the DSP.
