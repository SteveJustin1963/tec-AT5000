# tec-AT5000

![](https://github.com/SteveJustin1963/tec-AT5000/blob/master/pics/mp1.png)
![](https://github.com/SteveJustin1963/tec-AT5000/blob/master/pics/mpad1.png)

## Overview
This project explores the design of a **retro-inspired autodialer**, modeled after the **AT-5000 Power Dialer** (as seen on *The Simpsons*) and influenced by classic *Talking Electronics* projects.  
The aim is to blend **vintage components**, **TEC-1 hardware**, and **modern logic** to recreate a 1980s-style autodialer complete with LEDs, knobs, mechanical counters, and authentic DTMF tones.

---

## Core Features

- **Indicators:** Use TEC-1 LEDs to show functions such as *Play*, *Record*, *Dial*, *Redial*, and *Power*.
- **Retro Counter:** Integrate an old **Veeder-Root mechanical counter** for the first five digits.
- **Retro Tech:** Maximise reuse of classic components wherever possible.
- **Buttons:** Repurpose TEC-1 keypad buttons for *Play*, *Record*, *Power*, *Keypad*, *100*, *Manual*, *Auto*, and *Tape-Red*.
- **Display:** Use TEC-1 LED array as a minimalist display interface.
- **Tape Control:** Add a left/right knob for tape-style control.
- **Speaker:** Use the TEC-1 speaker for sound effects and DTMF tone output.
- **Connectivity:** RJ-45 connector on rear for analog line or optional **VoIP coupling**.
- **Circuit:** Include microphone input, opto-coupler isolation, diode bridge, and DTMF decoder.
- **Voice Module:** Integrate **LX20LYA 20-second voice module**, keeping the last 5 seconds for a high-speed rewind “retro” effect.

---

## Project Plan

Develop a TEC-1-based autodialer inspired by:

- The **AT-5000 autodialer** featured in *The Simpsons*  
- Hardware design from **TE Dial-Alarm-2**  
- Software ideas from **Talking Electronics Issue 14, page 16**  

See the project Wiki for ongoing updates and schematics.

---

## Mockup Layout

![layout](https://user-images.githubusercontent.com/58069246/205056653-5459de57-910f-4eac-83e6-3f8193f32d24.png)
![image](https://user-images.githubusercontent.com/58069246/209416173-8975d636-3432-493c-9972-6893980a00f0.png)

---

## Development & Iteration

- Learn DTMF generation and decoding:
  - On a **PIC** microcontroller  
  - On the **Z80** (TEC-1 environment)  
  - Within the **Monitor ROM**, which already generates tones for games
- Experiment with **remote control** via DTMF:
  - Dial from mobile or radio  
  - Send tones that the autodialer decodes to perform actions

---

## Build Notes / Hardware BOM

| Component / Subsystem | Description | Notes |
|------------------------|--------------|-------|
| **CPU Board** | TEC-1 or compatible Z80 SBC | Core control unit |
| **DTMF Decoder** | MT8870 or CM8870 | Converts tone to digital nibble |
| **Voice Module** | LX20LYA 20 s recorder/player | For retro message playback |
| **Display** | TEC-1 LED array | Status indicators |
| **Keypad** | TEC-1 hex keypad | Multifunction input |
| **Counter** | Veeder-Root 5-digit mechanical | Real mechanical feedback |
| **Audio Out** | Piezo / speaker | For tone output and FX |
| **Opto-Isolator** | 4N25 or similar | Line isolation |
| **Interface Jack** | RJ-45 / RJ-11 | Analog or VoIP connection |
| **Bridge Rectifier** | 1N4148 × 4 | Line protection |
| **Microphone Input** | Electret capsule | Audio sensing |
| **Power Supply** | 5 V regulated | Shared with TEC-1 mainboard |
| **Misc.** | Potentiometers, knobs, retro dials | For tape / control feel |

Additional modules under consideration:  
- **DTMF generator** IC (e.g., TP5089 or HT9200B) for tone transmission  
- Optional **Bluetooth-to-VoIP adapter** for modern integration

---

## Version Roadmap

### **Phase 1 — Research & Mockup**
- Collect all available reference material (Simpsons clip, TE Dial Alarm-2, TE14 code).  
- Breadboard a basic DTMF generator circuit.  
- 3D or paper mockup of front-panel layout.

### **Phase 2 — Hardware Integration**
- Interface TEC-1 with DTMF decoder and voice module.  
- Add Veeder-Root counter driver and LED status mapping.  
- Design small backplane for RJ-45 I/O and power.

### **Phase 3 — Software Prototype**
- Implement tone generation and decoding routines on TEC-1 monitor code.  
- Write simple auto-dial macro sequence in MINT-Octave or assembler.  
- Record/playback module control via keypad input.

### **Phase 4 — Refinement & Expansion**
- Integrate optional VoIP or Bluetooth-relay module.  
- Implement “auto-redial” and “voice replay” features.  
- Polish enclosure design for full retro aesthetic.

### **Phase 5 — Documentation & Release**
- Publish full schematics, firmware, and build guide.  
- Document calibration and tone frequency testing.  
- Create demonstration video and printable front-panel artwork.

---

## References

- [Talking Electronics – Dial Alarm 2](http://www.talkingelectronics.com/projects/Elektor/Dial%20Alarm-2/DialAlarm-2.html)  
- [Talking Electronics Issue 14 (PDF, p.16)](https://github.com/SteveJustin1963/tec-BOOKS/blob/master/TE/Mag/talking_electronics_14.pdf)  
- [YouTube – The Simpsons AT-5000 Clip](https://www.youtube.com/watch?v=9rh50BSv_CE)

---

> **“I'll wait till this cop goes and I’ll grab my AT5000...”**

![](https://github.com/SteveJustin1963/tec-AT5000/blob/master/pics/feds.png)
