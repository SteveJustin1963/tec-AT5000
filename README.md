# tec-AT5000
Inspired by the Simpsons AT-5000 auto dialer,
use software ideas from TE14 Dialer and hardware from TE-Dial-Alarm-2, hack some code in asm and MINT 

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

## Layout
we can use the tec-1's 
- 7 seg display
- keypad
- spk
we add some
- mic, opto couplers, diode bride and ccts
- cheap digital sound record and play board, upto 30 seconds

![](https://github.com/SteveJustin1963/tec-AT5000/blob/master/docs/vmc1.png)




 
## Ref

http://www.talkingelectronics.com/projects/Elektor/Dial%20Alarm-2/DialAlarm-2.html

pg16   https://github.com/SteveJustin1963/tec-BOOKS/blob/master/TE/Mag/talking_electronics_14.pdf
