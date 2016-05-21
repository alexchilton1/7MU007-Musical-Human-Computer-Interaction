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

The controller makes use of modifier keys to extend the amount of controls available in the patch. A modifier key is a special key (or combination) that temporarily modifies the normal action of another key when pressed together. The L2 and R2 keys
can be held down in conjuction with the four d-pad (directional pad) and four shape buttons.

INSERT PS3 LAYOUT DIAGRAM

The analogue sticks are only used as push buttons and not as moving potentiometers. When clicked the left lowers the wet amount and the right raises it. L1 and R1 toggle the random ratchet and selection on and off, select toggles the LFO on and off while start engages and disengages the polyphonic synth pad. As previously stated, the other buttons have three possible applications each. They all work in the same way, when the unmodified key is pressed the number 100 is triggered. When L2 is held during this it adds 10 to the overall number, the same is true when holding down R2 but instead it adds 20. These values run into a selection box choosing between 100, 110 and 120 and outputting numbers 1, 2 and 3 respectively.

INSERT INSIDE PS3 ABSTRACTION WITH PD LEFT ABSTRACTION

The final numbers are sent to various sections of the instrument which take the value and use it to trigger a bang either raising or lowering the value of its chosen parameter. 

Here is the full list of parameters and their respective controls.

INSERT BUTTON LAYOUT SHEET
