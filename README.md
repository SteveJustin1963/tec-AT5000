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

