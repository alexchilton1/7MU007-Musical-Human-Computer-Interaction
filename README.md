# 7MU007-Musical-Human-Computer-Interaction
A real-time musical performance software synthesiser controlled via a PlayStation 3 pad. Built using pure data https://puredata.info/

##Background

##Prerequisites

In order to be able to run this project you will need to have pure data extended installed on your system. Pure Data is free and available here https://puredata.info/downloads/pd-extended follow the guides online on how to set Pd up on your OS.

You will also need to have a PlayStation 3 controller to be able to play the synthesiser the way it was intended to be used due to the way its controls are mapped to each parameter.

To use the PS3 controlling within pure data you will need to download the [hid] object available here https://puredata.info/downloads/hid follow the guide online on how to install this object on your OS.

##Interface

Open LorkSynthPS3.pd

The is synthesiser is capable of much more than it currently does within this patch but some of its parameters have been limited for the sake of the group project, where multiple of these kind of real-time instruments link up and interact as a digital orchestra. If the parameters did not work within these constraints then it would be impossible, or at least extremely difficult, to have any control over this synthesiser in the group performance. The limitations set in place for this project allow full control over the instrument at any given point of the performance, so the player has the ability to react to any other changes made by the other instruments in the orchestra within the performance.

######Connecting the PS3 Controller

Plug the controller into one of the USB (Universal Serial Bus) ports on the computer, then navigate to the controller section.

INSERT PS3 SECTION PICTURE

Inside the controller abstraction there is a message box called [print], click on this box and in the Pd window a list of connected devices will appear. In this case the one needed is the one relating to the device number of the PS3 controller.

INSERT HID PRINTED DEVICES IN PD WINDOW WITH PS3 CIRCLED

Select the corresponding device number on the radio selection box and engage the toggle box attached to the hid object. Hid (Human Interface Device) is an object for reading data from USB HID devices like keyboards, mice, joysticks, gamepads, keypads, and all sorts of other esoteric controllers like USB knobs, touchscreens, Apple IR Remotes, etc. It represents the data with a cross-platform message scheme which is then translated to the underlying native API for input devices. The PS3 controller should now be connected to the pure data patch.

######How The Controller Works

The controller makes use of modifier keys to extend the amount of controls available in the patch. A modifier key is a special key (or combination) that temporarily modifies the normal action of another key when pressed together. The L2 and R2 keys
can be held down in conjuction with the four d-pad (directional pad) and four shape buttons.

INSERT PS3 LAYOUT DIAGRAM

The analogue sticks are only used as push buttons and not as moving potentiometers. When clicked the left lowers the wet amount and the right raises it. L1 and R1 toggle the random ratchet and selection on and off, select toggles the LFO on and off while start engages and disengages the polyphonic synth pad. As previously stated, the other buttons have three possible applications each. They all work in the same way, when the unmodified key is pressed the number 100 is triggered. When L2 is held during this it adds 10 to the overall number, the same is true when holding down R2 but instead it adds 20. These values run into a selection box choosing between 100, 110 and 120 and outputting numbers 1, 2 and 3 respectively.

INSERT INSIDE PS3 ABSTRACTION WITH PD LEFT ABSTRACTION

The final numbers are sent to various sections of the instrument which take the value and use it to trigger a bang either raising or lowering the value of its chosen parameter. This happens by using a fairly simple yet effective system.

INSERT KEYPS3 ABSTRACTION (SPIGOT REVERSE SECTION)

The down and up controls flow through a spigot and when allowed to pass through bang an integer either adding or subtracting from the final output value. This value is sent into the second inlet on the integer object being converted into an integer and stored for later use. The same value is being sent up to the float inlet of the two spigots. The left spigot specifies the lower number that can pass through while the right specifies the maximum. These both run through separate reverse boxes before being passed into their respective spigots, this is the clever and important part. When the lowest or highest value has been reached the corresponding spigot gets shut off, this is what provides the ability to cap each parameter between two specific values making it adaptable across any range of numbers. All of the buttons that use the modifier keys employ this scaling function.

Here is the full list of parameters and their respective controls.

INSERT BUTTON LAYOUT SHEET

######Functionality

The first main functions are the key and scale selection. There are five scales to choose from; Major, Minor, Lydian, Mixolydian and Phrygian. Selecting 0-4 on the scale radio will write the corresponding scale to the "scaler" table, these messages contain the appropriate intervals used in each scale. 

INSERT SCALER ABSTRACTION

The key selection section offset the chosen value by a number ranging from 0-11. Each number coincides with a note from the chromatic scale. The chromatic scale is a musical scale with twelve pitches, each a semitone above or below another. On a modern piano or other equal-tempered instrument, all the semitones have the same size (100 cents). In other words, the notes of an equal-tempered chromatic scale are equally spaced. For example, 0 would be equal to note C and 1 would be equal to C#/Db.

INSERT KEY ABSTRACTION AND ADD NOTES TO NUMBER VALUES

These two features are important and are used in conjunction with the note selection sliders. Before that stage though there are a few aspects to introduce. The first is the tempo control, it takes the millisecond value, divides it by 60000 (that equals 1 minute) and produces a final value. That value is how many beats per minute will occur using the current millisecond number. This flows into a metro (metronome) object, which triggers repeated steps that are divided to produce eight separate tiggers per sequence. This millsecond value is divided by eight and sent to a second metro object, it is divided like this to ensure it triggers at the start of every sequence. This metro triggers eight sets of random numbers between 0 and 22 which are passed to the faders below, their values are then passed to float objects where the numbers are stored until the corresponding step in the sequence is triggered. This is where the key and scale features become important, the number between 0 and 22 from each fader is passed to the eight boxes labelled pd at the bottom of the abstraction. For each fader the number is received, passed through the table with the selected scale intervals and pushed up or pulled down to meet the nearest interval in that scale. The key ofset is added and the outcome is a number that sits within the chosen scale set in a particular key.

INSERT FINAL KEY AND SCALE ABSTRACTION IN 8-STEP SUBPATCH



INSERT 8-STEP ABSTRACTION WITH SECTIONS LABELLED (BPM ETC)
