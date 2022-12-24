# tec-AT5000

![](https://github.com/SteveJustin1963/tec-AT5000/blob/master/pics/mp1.png)
![](https://github.com/SteveJustin1963/tec-AT5000/blob/master/pics/mpad1.png)


A telephone autodialler is a device or system that can automatically place telephone calls and typically includes features such as the ability to store a list of phone numbers, play a recorded message or series of messages, and provide options for the recipient to interact with the call, such as by pressing a key to hear more information or transfer to a live operator. Autodiallers are often used for mass communication purposes, such as for emergency alerts, political campaigns, or telemarketing. They can also be used in a business setting to automate calls to customers or clients.

Here we will use a combination of software and hardware to create a telephone autodialler that is inspired by the AT-5000 auto dialer from the Simpsons. The software will come from various sources, including issue TE14's software "Dialer," and we will use hardware from the TE-Dial-Alarm-2. We will also incorporate some ASM (assembly language) and MINT code in the process of hacking these components together.



## Warning and compliance 
In Australia, there are strict laws and regulations regarding spam and telemarketing, including the use of autodialers. These regulations apply to both domestic and international enterprises and companies using calls as part of their sales strategy must comply with them. The Do Not Call Registry is a government service that prohibits unsolicited calls and telemarketers and sales representatives must stop all forms of contact with registered individuals within 30 days. The Privacy Act 1988 and the Do Not Call Register Act 2006 also regulate the use of autodialers for telemarketing purposes and telemarketers who use them without consent may face fines of up to AUD 1.8 million for corporations and AUD 360,000 for individuals. It is important for companies using autodialers in Australia to ensure they are following these regulations and obtaining the necessary consent from recipients before making calls. https://en.wikipedia.org/wiki/Auto_dialer

## There are several different types of autodialers, each with their own unique features and capabilities:

Predictive dialers: These dialers automatically place calls and connect them to a live operator once the recipient answers. They are designed to increase the efficiency of telemarketing campaigns by reducing the amount of time operators spend waiting for someone to answer the phone.

Preview dialers: These dialers allow operators to view the information about a call before it is connected, allowing them to tailor their pitch to the specific needs and interests of the recipient.

Power dialers: These dialers automatically place calls and move on to the next call in the list once the recipient hangs up, allowing operators to handle a higher volume of calls in a shorter amount of time.

Progressive dialers: These dialers automatically place calls and connect them to a live operator once the recipient answers, but they also allow operators to work on other tasks while waiting for a call to be connected.

Hosted dialers: These dialers are hosted and managed by a third party, and can be accessed remotely by operators using a web browser or app. This allows companies to set up a call center without the need for on-site hardware.



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
