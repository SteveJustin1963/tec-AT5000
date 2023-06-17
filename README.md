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


## lets review the dialer

the TEC has a 6-digit screen and hex keyboard input. a high-level summary of what the program does and how it works:

1. **Display Limitation:** The program can display up to six digits on the TEC screen. It doesn't have a scrolling feature, so it's limited to displaying six digits at any one time.
2. **Input Mechanism:** As keys are pressed on the device, the corresponding numbers are displayed on the screen from left to right. When all six spots on the screen are filled, the program has reached its limit and cannot accept any more input until it is reset.
3. **Memory and Scan Rate:** The screen buffer (the area of memory where the currently displayed information is stored) is located at address 0900. The speed at which the program scans the keyboard for input is determined by the value stored in register B (located at addresses 082E and 082F). You can make the program scan the keyboard faster or slower by changing the value of B and/or adjusting the speed of the TEC's clock.
4. **Resetting:** When the screen is full, or when you want to input a new number, you need to reset the TEC and press the 'GO' button. This will clear the screen and allow a new number to be entered.
5. **Input Acceptance and JRNC Instruction:** The program only accepts the numbers 0-9 as input. The JRNC (Jump if Register Not Carry) instruction is used to divide the keyboard in two, disregarding any keys corresponding to the hexadecimal numbers A-F. If the Carry flag is NOT SET (meaning there's no "carryover" from a previous operation), the program will jump to another part of the code. This decision is based on the previous instruction being a 'COMPARE', which subtracts a data byte from the accumulator. If a borrow is needed (i.e., the number in the accumulator is smaller than the number being subtracted), the Carry flag is set, and the JRNC instruction won't cause a jump.

The Z-80 phone dialer program uses the CP OA instruction to subtract the hexadecimal value OA (10 in decimal) from the accumulator, which holds the key value. If the key pressed is less than 10 (like 6), it sets the carry flag due to the borrowing required for subtraction. This affects the JR NC instruction (Jump if No Carry), which continues down the program without jumping if the carry flag is set.

The JR NC instruction can be better understood through the double negative logic: if it's NOT, NOT doing something, it means it's doing it. So, if it's NOT, NOT going to jump, it means it's going to jump.

To run the program, it must be typed into the memory starting at location 0800, with the display conversion table at 0880. After pressing RESET and GO, the display blanks, and pressing any key combination will show the corresponding numbers on the display, provided those are number keys.

The scan rate of the program can be adjusted by modifying the value of register B. Several experiments can be done with the program, such as reversing the scan direction, limiting the scan to 5 displays, allowing letter inputs on the screen, and turning the output into a code for a code-breaking game.

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

