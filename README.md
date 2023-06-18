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

In the first program of the Phone Dialer, certain limitations are present. The program can only display up to six digits on the TEC screen, as it lacks a scrolling feature. As numbers are pressed on the keyboard, they fill the screen from left to right. Once the screen is full, the program's capacity is reached.

The screen buffer is located at memory address 0900, and the scan rate is determined by the value of B at addresses 082E and 082F. Adjusting the value of B and the TEC clock speed allows for increasing or reducing the scan rate.

This program does not include any additional features. To enter a new number, the TEC must be reset, and the 'GO' button needs to be pressed, clearing the screen.

The primary function of this simple program is to demonstrate how to retrieve numbers from the keyboard and display them on the screen.

An unfamiliar instruction used in this program is JRNC, which stands for "Jump Relative if the Carry flag is NOT SET." When the preceding instruction is a 'COMPARE,' it can be helpful to substitute the term 'BORROW' for carry, making it easier to understand. This is because the compare instruction subtracts the data byte from the accumulator, and if a borrow is required, the carry flag is SET.

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
The program starts by clearing the first 8 memory locations to ensure a blank screen display, although only 6 locations are necessary. Register A is set to zero, and this value is stored in memory locations 0900-0907. The program then compares the value in the accumulator (A) with OA (hexadecimal 10) to determine the key value.

If the key value is A or higher, the program jumps to a different section. Otherwise, the program loads the DE register with the start of the display table. The program proceeds to find the first available blank memory location by iterating through memory and checking for a zero value. Once a blank location is found, the key value is increased by 80 and stored back into register E. This ensures that the DE register points to a specific table byte.

Next, the program loads HL with the start of memory and searches for the blank memory location. When found, the byte pointed to by DE is loaded into register A, and the table value is stored in the blank memory location.

To detect the same or a different button, the index register is modified by loading it with FF (hexadecimal) so that subsequent button presses can be identified.

The scanning process begins at the left end of the display. HL is loaded with the start of memory, and the program initializes D with 06 to indicate six loops of the program. The delay value for turning on each digit is stored in register B.

The program retrieves the data at the first memory location into register A, outputs it to the segment port, and loads C into A to output it to the cathode port. Register C is rotated right to access the second display, creating a short delay to display the digit. Register A is then zeroed, and the program outputs to the cathode port to turn off the display.

The program increments to the next location, decrements the loop register (D), and jumps back to the start of the loop if D is not zero. If D is zero, indicating the end of the loop, the program jumps back to the start to look for a new key.


## TE-Dial-Alarm-2


##  mockup layout

![layout](https://user-images.githubusercontent.com/58069246/205056653-5459de57-910f-4eac-83e6-3f8193f32d24.png)

![image](https://user-images.githubusercontent.com/58069246/209416173-8975d636-3432-493c-9972-6893980a00f0.png)

