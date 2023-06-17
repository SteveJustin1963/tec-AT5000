# tec-AT5000
## warnings
https://github.com/SteveJustin1963/tec-AT5000/wiki


![](https://github.com/SteveJustin1963/tec-AT5000/blob/master/pics/mp1.png)
![](https://github.com/SteveJustin1963/tec-AT5000/blob/master/pics/mpad1.png)

For the design of our autodialer, we will refer to the AT5000 Power Dialer as described on the Wikipedia page. We will carefully select the desired features from the AT5000 and determine the best approach to combine them. The selected features include:

- Indicators: We will utilize the tec1 LEDs to indicate functions such as play, record, dial, redial, and power.
- Buttons: The tec1 keypad will incorporate buttons for play, record, power, keypad, 100, manual, auto, and tape-red.
- Display: The tec1 LEDs will serve as the display for our autodialer.
- Tape: The tec1 keypad will feature a left/right knob for tape control.
- Speaker: We will utilize the tec1 speaker for audio output.
- Back: Our autodialer will have an RJ45 connector at the back.
- Circuit: The circuitry will include components such as a microphone, opto coupler, diode bridge, and DTMF decoder.
- Voice: We will incorporate the LX20LYA voice audio module, providing a 20-second voice capability.

By combining these software and hardware elements, we aim to create a functional and inspired autodialer reminiscent of the AT-5000 from the Simpsons.


## plan 
develop a telephone autodialer that draws inspiration from 
- the AT-5000 auto dialer featured in the Simpsons. 
- hardware elements from the TE-Dial-Alarm-2. 
- "Dialer" software TE14-pg16 


## 

```
PHONE DIALLER - Part I 

LD D, 08 
XOR A 
LD HL,0900 
LD (HL),A 
INC HL 
DEC D 
JR NZ 
LD A,I 
CP 0A
JR NC 
LD DE 0880
ADD A,E 
LD E,A 
LD HL,0900 
LD A,(HL) 
CP 00
JR Z 
INC HL 
JR 
LD A,(DE) 
LD (HL),A 
LD A,FF 
LD I,A 
LD C,20 
LD HL,0900 
LD D,06
LD B,00
LD A(HL) 
OUT (02),A 
LD A,C 
OUT (01),A 
RRC C 
DJNZ 
XOR A 
OUT(01),A 
INC HL 
DEC D 
JR NZ 
JR 
```

## TE-Dial-Alarm-2


##  mockup layout

![layout](https://user-images.githubusercontent.com/58069246/205056653-5459de57-910f-4eac-83e6-3f8193f32d24.png)

![image](https://user-images.githubusercontent.com/58069246/209416173-8975d636-3432-493c-9972-6893980a00f0.png)

