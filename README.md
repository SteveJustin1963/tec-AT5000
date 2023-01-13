# tec-AT5000

![](https://github.com/SteveJustin1963/tec-AT5000/blob/master/pics/mp1.png)
![](https://github.com/SteveJustin1963/tec-AT5000/blob/master/pics/mpad1.png)



Here we will use a combination of software and hardware to create a telephone autodialler that is inspired by the AT-5000 auto dialer from the Simpsons. The software will come from various sources, including issue TE14's software "Dialer," and we will use hardware from the TE-Dial-Alarm-2. We will also incorporate some ASM (assembly language) and MINT code in the process of hacking these components together.



## Chossing our capabilities for the AT5000 will
need to decide what features and we will select and decide how to combine them. wip....
- Power dialer plus xxx
- 
- indicators: 
  - play, record, dial, redial, power > tec1 LEDS
- buttons: 
-   play, record, power, keypad, 100, manual, auto, tape-red > tec1 keypad 
- diaplay: 
-   > tec1 LEDS
- tape: 
-   left/right knob > tec1 keypad
- speaker: 
-   > tec1 spkr
- back: 
-   > rj45 connector
- in our cct 
-   we add a mic, opto coupler, diode bride, dtmf decode, etc 
- a voice 
-   audio module 20 sec, LX20LYA

![](https://github.com/SteveJustin1963/tec-AT5000/blob/master/docs/vmc1.png)

the voice audio module can record an audio message via a microphone and its subsequent playback into a loudspeaker or phone line. 
The message length is a maximum of 20s, at a sampling frequency of 3.2 kHz. This can be set using the R0SC resistor. The best
recording quality is 8kHz when sampling, the recording length is then 8s. 
We need to add a electret microphone, loudspeaker, button and or switch control, resistors, capacitors and LED for signaling the recording status. 
The power supply is 2.7V ~ 4.7V, ok from batteries, ie 3 AAA's or button cells.


## suggested layout

![layout](https://user-images.githubusercontent.com/58069246/205056653-5459de57-910f-4eac-83e6-3f8193f32d24.png)

![image](https://user-images.githubusercontent.com/58069246/209416173-8975d636-3432-493c-9972-6893980a00f0.png)




