# tec-AT5000
Inspired by the Simpsons AT-5000 auto dialer,
use software from TE14 Dialer and hardware from TE-Dial-Alarm-2, hack some code in asm and MINT 

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

https://github.com/SteveJustin1963/tec-AT5000/blob/master/pics/layout.png
 
## Ref
- http://www.talkingelectronics.com/projects/Elektor/Dial%20Alarm-2/DialAlarm-2.html
- pg16  https://github.com/SteveJustin1963/tec-BOOKS/blob/master/TE/Mag/talking_electronics_14.pdf
- https://www.youtube.com/watch?v=9rh50BSv_CE

![](https://github.com/SteveJustin1963/tec-AT5000/blob/master/pics/feds.png)
