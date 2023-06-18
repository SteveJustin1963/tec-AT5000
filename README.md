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
from the magazie "Dialer" software TE14-pg16 

The following three to four pages explore the development of a Telephone Dialer concept, which possesses the capability to store approximately 30 to 40 names and corresponding phone numbers. The dialer includes features such as dialing functionality and auto redial. It is important to note that this concept is solely a program of ideas, and the output is conveyed through tones emitted from a speaker.

Due to the ambitious nature of the concept, it has been divided into three distinct sections. Each section delineates a standalone program that increases in complexity, culminating in a complete design in the third section.

The first program serves as a relatively simple introduction. It demonstrates the process of obtaining numerical input from the keyboard and subsequently displaying it on the screen.

In the second program, two additional function buttons, namely 'C' and 'E,' are introduced. The 'C' button serves to clear the screen, while the 'E' button denotes the conclusion of inputting a phone number.

The third program represents a significant increase in complexity compared to its predecessors. It incorporates a broader range of features and is designed to keep track of multiple variables and information.

It is emphasized that each program has been created from scratch, as it is challenging to modify an existing program. The recommended approach is to type each of these programs into the TEC (presumably a device or software) and diligently study their functioning, thereby facilitating a comprehensive understanding of their operations.

```
PHONE DIALLER - Part I 

org     0800

start   LD D, 08    ; The first 8 memory locations are cleared so that the program
        XOR A       ; will come on with a blank screen. We need only 6 locations.
        LD HL,0x0900 ; The 7th location is explained in the text. 
L1      LD (HL),A   ; Register A is zeroed, and this value is inserted into 0900- 0907
        INC HL      ; via the HL register, being the pointer register.   
        DEC D 
        JR NZ,L1 
L2      LD A,I      ; The Index register contains the value of the key
        CP 0x0A       ; Compare the accumulator with 0A
        JR NC, B2   ; Jump relative if the key is A or higher
        LD DE,0x0880 ; Load DE with the start of the DISPLAY TABLE
        ADD A,E     ; Add 80 to the key value
        LD E,A      ; Load the result back into E. DE will point to a table-byte
        LD HL,0x0900  ; Load HL with the start of memory
B1      LD A,(HL)   ; Look for the first blank memory location by loading the value
        CP 0x00       ; pointed to by HL into the accumulator and comparing with
        JR Z, F   ; zero until a blank location is found. 
        INC HL 
        JR B1
F       LD A,(DE)   ; When found, load A with the byte pointed to by DE. 
        LD (HL),A   ; Load the table value into the blank memory location.
B2      LD A, 0xFF     ; Change the value of the index register by loading it with FF so
        LD I,A      ; that we can detect the same or another button.
        LD C,0x20     ; Start the scan at the left-hand end of the display. 
        LD HL,0x0900  ; Load HL with the start of memory
        LD D,0x06     ; Load D with 06 for 6 loops of the program
L3      LD B,0x00     ; Load B with delay value for turning ON each digit. 
        LD A,(HL)   ; Load the data at the first memory location into A.  
        OUT (02),A  ; Output to the segment port
        LD A,C      ; Load C into A
        OUT (01),A  ; Output to the cathode port
        RRC C       ; Rotate register C right to access the 2nd display 
        DJNZ L3     ; Create a short delay to display the digit
        XOR A       ; Zero A
        OUT (01),A  ; Output to the cathode port to turn the display OFF
        INC HL      ; Increment to the next location
        DEC D       ; Decrement the loop register
        JR NZ, L3   ; Jump to the start of the loop if D is not zero
        JR L2       ; Jump to the start of the program if D is zero and look for a new key
```



## TE-Dial-Alarm-2


##  mockup layout

![layout](https://user-images.githubusercontent.com/58069246/205056653-5459de57-910f-4eac-83e6-3f8193f32d24.png)

![image](https://user-images.githubusercontent.com/58069246/209416173-8975d636-3432-493c-9972-6893980a00f0.png)

