# tec-AT5000
 

![](https://github.com/SteveJustin1963/tec-AT5000/blob/master/pics/mp1.png)
![](https://github.com/SteveJustin1963/tec-AT5000/blob/master/pics/mpad1.png)

For the design of our autodialer, we will refer to the AT5000 Power Dialer as described on the Wikipedia page. We will carefully select the desired features from the AT5000 and determine the best approach to combine them. The selected features include:

- Indicators: We will utilize the tec1 LEDs to indicate functions such as play, record, dial, redial, and power.
- use a old Veeder-Root counter for the first 5 digits...
- use as much old retro tech
- Buttons: The tec1 keypad will incorporate buttons for play, record, power, keypad, 100, manual, auto, and tape-red.
- Display: The tec1 LEDs will serve as the display for our autodialer.
- Tape: The tec1 keypad will feature a left/right knob for tape control.
- Speaker: We will utilize the tec1 speaker for audio output.
- Back: Our autodialer will have an RJ45 connector at the back., more advance way is to couple to a voip account
- Circuit: The circuitry will include components such as a microphone, opto coupler, diode bridge, and DTMF decoder.
- Voice: We will incorporate the LX20LYA voice audio module, providing a 20-second voice capability. keep the last 5 seconds of sounds in high speed rewind for that retro effect

By combining these software and hardware elements, we aim to create a functional and inspired autodialer reminiscent of the AT-5000 from the Simpsons.


## plan 
develop a telephone autodialer that draws inspiration from 
- the AT-5000 auto dialer featured in the Simpsons
- review and use hardware elements from TE-Dial-Alarm-2. see wiki
- revier and use dialer software in TE14-pg16. see wiki




##  mockup layout

![layout](https://user-images.githubusercontent.com/58069246/205056653-5459de57-910f-4eac-83e6-3f8193f32d24.png)

![image](https://user-images.githubusercontent.com/58069246/209416173-8975d636-3432-493c-9972-6893980a00f0.png)

## Iterate
- learn how dtmf is done 
  - on the pic chip
  - on the z80, 
  - in the monitor code; makes tones a well as some games. 
  - generating it and detecting it are two diff things.
- dialin with a mobile phone (or radio) then send dtmf that is deocoded for control



 
## Ref
- http://www.talkingelectronics.com/projects/Elektor/Dial%20Alarm-2/DialAlarm-2.html
- pg16  https://github.com/SteveJustin1963/tec-BOOKS/blob/master/TE/Mag/talking_electronics_14.pdf
- https://www.youtube.com/watch?v=9rh50BSv_CE

**Ill waiti till this cop goes and ill grab my AT5000..**


![](https://github.com/SteveJustin1963/tec-AT5000/blob/master/pics/feds.png)


