# tec-AT5000

A telephone autodialler is a device or system that can automatically place telephone calls and typically includes features such as the ability to store a list of phone numbers, play a recorded message or series of messages, and provide options for the recipient to interact with the call, such as by pressing a key to hear more information or transfer to a live operator. Autodiallers are often used for mass communication purposes, such as for emergency alerts, political campaigns, or telemarketing. They can also be used in a business setting to automate calls to customers or clients.

Here we will use a combination of software and hardware to create a telephone autodialler that is inspired by the AT-5000 auto dialer from the Simpsons. The software will come from various sources, including issue TE14's software "Dialer," and we will use hardware from the TE-Dial-Alarm-2. We will also incorporate some ASM (assembly language) and MINT code in the process of hacking these components together.

![](https://github.com/SteveJustin1963/tec-AT5000/blob/master/pics/mp1.png)
![](https://github.com/SteveJustin1963/tec-AT5000/blob/master/pics/mpad1.png)

## Warning
Read about compliance at https://en.wikipedia.org/wiki/Auto_dialer

## Features
convert the at5000 features into our simple system
- indicators: play, record, dial, redial, power > tec1 LEDS
- buttons: play, record, power, keypad, 100, manual, auto, tape-red > tec1 keypad 
- diaplay: > tec1 LEDS
- tape: left/right knob > tec1 keypad
- speaker: > tec1 spkr
- back: > rj45 connector
- in our cct we add a mic, opto coupler, diode bride, dtmf decode, etc 
- a voice audio module 20 sec, LX20LYA

![](https://github.com/SteveJustin1963/tec-AT5000/blob/master/docs/vmc1.png)

the voice audio module can record an audio message via a microphone and its subsequent playback into a loudspeaker or phone line. 
The message length is a maximum of 20s, at a sampling frequency of 3.2 kHz. This can be set using the R0SC resistor. The best
recording quality is 8kHz when sampling, the recording length is then 8s. 
We need to add a electret microphone, loudspeaker, button and or switch control, resistors, capacitors and LED for signaling the recording status. 
The power supply is 2.7V ~ 4.7V, ok from batteries, ie 3 AAA's or button cells.

![layout](https://user-images.githubusercontent.com/58069246/205056653-5459de57-910f-4eac-83e6-3f8193f32d24.png)

 
## Ref
- http://www.talkingelectronics.com/projects/Elektor/Dial%20Alarm-2/DialAlarm-2.html
- pg16  https://github.com/SteveJustin1963/tec-BOOKS/blob/master/TE/Mag/talking_electronics_14.pdf
- https://www.youtube.com/watch?v=9rh50BSv_CE

![](https://github.com/SteveJustin1963/tec-AT5000/blob/master/pics/feds.png)
