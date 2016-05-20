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
[hid] (Human Interface Device)
