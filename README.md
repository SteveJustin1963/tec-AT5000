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
| **DTMF Generator** | TP5089 or HT9200B | Tone transmission to line |
| **Voice Module** | LX20LYA 20 s recorder/player | For retro message playback |
| **Display** | TEC-1 LED array | Status indicators |
| **Keypad** | TEC-1 hex keypad | Multifunction input |
| **Counter** | Veeder-Root 5-digit mechanical | Real mechanical feedback |
| **Counter Driver** | NPN transistor (e.g. 2N2222) + 1N4007 flyback diode | Z80 cannot drive counter directly |
| **Audio Out** | Piezo / speaker | For tone output and FX |
| **Opto-Isolator** | 4N25 or similar | Line isolation |
| **Interface Jack** | RJ-11 | Analog POTS line — not RJ-45 |
| **Bridge Rectifier** | 1N4148 × 4 | Line protection |
| **Microphone Input** | Electret capsule | Audio sensing |
| **Power Supply** | 5 V regulated | Shared with TEC-1 mainboard |
| **Decoupling Caps** | 100 nF ceramic × 4 (min) | One per IC — MT8870, TP5089, 4N25, LX20LYA |
| **Crystal / Clock** | 3.58 MHz (NTSC colour burst) | Required reference for TP5089 |
| **Misc.** | Potentiometers, knobs, retro dials | For tape / control feel |

Additional modules under consideration:
- Optional external ATA (Analog Telephone Adapter) box for VoIP — do not attempt VoIP on the Z80 directly

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

## Full Circuit Diagram (Fixed)

```
                    tec-AT5000 — Full Circuit Diagram (Fixed)
                    ==========================================

 ╔══ 1. LINE INTERFACE ══════════════════════════════════════════════════════════╗
 ║                                                                               ║
 ║  RJ-11       1N4148 BRIDGE                  4N25 #1  (LINE RX)               ║
 ║                                                                               ║
 ║  TIP ──┬──[D1>]──┬── A    R1 470Ω           ┌─────────────────┐              ║
 ║        │         │    A───/\/\/──────────────┤ A  ▷│   C ──────┼─ AUDIO_RX   ║
 ║      [D2<]     [D3>]                C1 100nF │               E ┤             ║
 ║        │         │    B (GND_ISO) ──┤|├─────┤ K         GND ──┘             ║
 ║  RNG ──┴──[D4<]──┴── B                      └─────────────────┘              ║
 ║                                                                               ║
 ║  TP5089 TONE OUT ──[C2 100nF]──[R2 1kΩ]──┐  4N25 #2  (LINE TX)              ║
 ║                                            └─►A  ▷│  C ──────────── A (line) ║
 ║                                              K       E ── GND_ISO             ║
 ╚═══════════════════════════════════════════════════════════════════════════════╝
                │ AUDIO_RX                              │ TX tone to line
                ▼                                       ▲
 ╔══ 2. DTMF DECODE — MT8870 ════╗   ╔══ 3. DTMF GENERATE — TP5089 ══════════╗
 ║                                ║   ║                                         ║
 ║  AUDIO_RX ──► IN+              ║   ║  Port 06 ──► D0 ┐                      ║
 ║  GND_ISO  ──► IN-   Q1 ─────────────────────────► D1 ┤ TP5089               ║
 ║                     Q2 ──────────► Port 03        D2 ┤                      ║
 ║                     Q3 ──────────► (nibble)       D3 ┘                      ║
 ║                     Q4 ─────────┘                                            ║
 ║                    STD ──────────► Z80 /INT    TONE OUT ──► C2 ──► 4N25 #2  ║
 ║                                                                               ║
 ║  +5V ──[100nF cap]── GND          3.58MHz XTAL ─┬─ OSC1                     ║
 ║  (decoupling)                                    └─ OSC2                     ║
 ║                                    +5V ──[100nF cap]── GND                  ║
 ╚════════════════════════════════╝   ╚═══════════════════════════════════════╝
                │ Q1-Q4, STD                          │ D0-D3
                └──────────────────┬───────────────────┘
                                   ▼
 ╔══ 4. TEC-1 Z80 CPU ═══════════════════════════════════════════════════════════╗
 ║                                                                                ║
 ║        ┌────────────────────────────────────────────┐                         ║
 ║        │              Z80 CPU  (TEC-1)              │                         ║
 ║        │                                            │                         ║
 ║        │  Port 00 ◄───────────────── Keypad scan    │                         ║
 ║        │  Port 01 ────────────────► LED array       │                         ║
 ║        │  Port 02 ────────────────► Speaker/tone    │                         ║
 ║        │  Port 03 ◄─── MT8870 Q1–Q4 + STD           │                         ║
 ║        │  Port 04 ────────────────► LX20LYA ctrl    │                         ║
 ║        │  Port 05 ────────────────► VR counter drv  │                         ║
 ║        │  Port 06 ────────────────► TP5089 D0–D3    │                         ║
 ║        │  /INT    ◄─── MT8870 STD                   │                         ║
 ║        └────────────────────────────────────────────┘                         ║
 ╚════════════════════╤═══════════════════╤══════════════════╤════════════════════╝
                      │ Port 04           │ Port 05          │ Port 02
                      ▼                   ▼                  ▼
 ╔══ 5. LX20LYA ════════╗  ╔══ 6. VEEDER-ROOT DRIVER ══╗  ╔══ 7. SPEAKER ══╗
 ║                       ║  ║                            ║  ║                ║
 ║  bit0 ──► PLAY        ║  ║  Port 05 ──[R 1kΩ]──► B   ║  ║  Port 02 ──►  ║
 ║  bit1 ──► REC  LX20LYA║  ║                  2N2222    ║  ║  piezo/spkr   ║
 ║  bit2 ──► /CE         ║  ║             C ──┬── COIL ──╫──╫── +5V        ║
 ║                        ║  ║             E   │          ║  ║                ║
 ║  SPKR ──► Speaker out  ║  ║             │  [1N4007]   ║  ╚════════════════╝
 ║  +5V ──[100nF]── GND   ║  ║            GND  │flyback  ║
 ╚═══════════════════════╝  ║             └──── +5V      ║
                             ║  Veeder-Root counter ──────╝
                             ╚════════════════════════════╝

 POWER RAIL
 ══════════
  +5V regulated ──────────────────────────────────────────► all VDD pins
  100nF ceramic decoupling cap on every IC (MT8870, TP5089, 4N25×2, LX20LYA)
  GND_ISO (line side) kept separate from digital GND — joined only at bridge output
```

**Key fixes from original design:**
- RJ-11 (not RJ-45) for POTS line
- Two 4N25 opto-isolators — one RX, one TX
- 3.58 MHz crystal reference for TP5089
- 2N2222 + 1N4007 flyback diode for Veeder-Root counter driver
- 100nF decoupling cap on every IC
- GND_ISO kept isolated from digital GND throughout

---

## Line Interface Explained

### Diode Bridge (1N4148 × 4)

```
  TIP ──┬──[D1>]──┬── A
        │         │
      [D2<]     [D3>]
        │         │
  RNG ──┴──[D4<]──┴── B
```

A phone line can present with either polarity — the exchange decides which way round TIP and RING are. The four diodes form a **bridge rectifier** so regardless of polarity, point **A is always positive** and **B is always negative (GND_ISO)**. The circuit works correctly whichever way the line is connected.

The `>` and `<` show diode direction (anode → cathode).

---

### 4N25 #1 — Line RX (phone → TEC-1)

```
  A───/\/\/──────────────┤ A  ▷│   C ──────── AUDIO_RX
               C1 100nF  │             E ┤
  B (GND_ISO) ──┤|├─────┤ K       GND ──┘
```

- **R1 (470Ω)** limits current through the opto LED
- **C1 (100nF)** — the capacitor symbol `┤|├` — AC-couples the audio. DC from the line drives the opto LED steady, but the **audio signal (AC) modulates the LED current**, causing the phototransistor output to follow the audio
- The phototransistor collector outputs **AUDIO_RX** — an electrically isolated copy of the phone audio — which feeds into the MT8870 DTMF decoder
- **GND_ISO** (line-side ground) never touches digital GND — the opto provides the galvanic isolation barrier

---

### 4N25 #2 — Line TX (TEC-1 → phone)

```
  TP5089 TONE OUT ──[C2 100nF]──[R2 1kΩ]──┐
                                            └─►A  ▷│  C ──── A (line)
                                              K       E ── GND_ISO
```

This works in reverse — the **TP5089 DTMF generator** produces audio tones, which are:
- AC-coupled through **C2** (blocks any DC offset)
- Current-limited by **R2 (1kΩ)**
- Driven into the LED of 4N25 #2
- The phototransistor on the isolated side injects the tone signal back onto the phone line

The two optos keep the phone line and the digital circuit **completely electrically separate** in both directions — a legal and safety requirement for anything connecting to POTS.

---

## Hardware Issues — What Needs Fixing

### Must Fix

1. **RJ-45 → RJ-11** — wrong connector for analog phone line; physically incompatible with POTS
2. **Veeder-Root driver circuit missing** — needs NPN transistor + flyback diode between Z80 output port and counter coil; Z80 cannot drive it directly
3. **MT8870 ↔ Z80 interface not designed** — need to define:
   - Which I/O port address
   - How STD strobe is handled (interrupt or polled)
   - How 4-bit nibble (Q1–Q4) connects to the data bus
4. **DTMF generator output path undefined** — TP5089/HT9200B audio output needs to feed back through the opto-isolator to the phone line; signal path not yet defined

### Decide Before Building

5. **VoIP path** — either commit to an external ATA box or drop from v1 scope; "Bluetooth-to-VoIP adapter" is a placeholder, not a design
6. **Software vs hardware DTMF generation** — pick one: Z80 tone loops or TP5089 IC; implementing both without a clear reason adds unnecessary complexity

### Missing from BOM

7. NPN transistor for Veeder-Root driver (e.g. 2N2222)
8. Flyback diode for counter coil (e.g. 1N4007)
9. Decoupling capacitors for MT8870, TP5089, 4N25, LX20LYA (100 nF per IC minimum)
10. 3.58 MHz crystal / clock reference required by TP5089

---

## Project Assessment — What's Right and Wrong

### What's Right

| Item | Notes |
|------|-------|
| MT8870 DTMF decoder | Industry-standard, correct choice |
| Opto-isolator (4N25) | Essential for phone line isolation — correct |
| Diode bridge | Correct line polarity protection |
| LX20LYA voice module | Real, appropriate 20s recorder/player |
| TP5089 / HT9200B DTMF generator | Legitimate ICs for tone transmission |
| TEC-1 as controller | Feasible for this complexity level |
| Separate decode / generate ICs | Architecturally correct — don't mix roles |
| Simulate before hardware | Right approach for development order |

### What's Wrong

**Hardware / BOM:**
- **RJ-45 listed instead of RJ-11** — analog POTS lines use RJ-11; these are physically different connectors
- **No Veeder-Root driver circuit** — the Z80 cannot drive a mechanical counter directly; needs transistor + flyback diode
- **VoIP path undefined** — "Bluetooth-to-VoIP adapter" is not a real design; needs either an ATA box or drop VoIP from v1 scope
- **No Z80 ↔ MT8870 interface defined** — port address, STD strobe handling, and polling/interrupt strategy all missing

**Software:**
- **No DTMF tone generation routine** — the existing code is a display handler only, not a dialler
- **No MT8870 read routine** — nothing decodes the incoming 4-bit nibble

**Assembly code (`PHONE-DIALLER-Part 1.z80`):**
- 7 jump instructions with no target labels — will not assemble
- `I` register misused as data storage — corrupts the interrupt vector
- `LD A(HL)` and `LD DE 0880` — missing commas, syntax errors
- `ADD A,E` for table offset — 8-bit only, fragile
- `B = 00` passed to `DJNZ` — loops 256 times, likely unintentional
- Code appears to be a raw OCR transcription from TE14 p.16, never assembled or tested

---

## Current Code Flow (Faulty — PHONE-DIALLER-Part 1.z80)

```
  PHONE-DIALLER-Part 1.z80 — Code Flow (with faults marked)
  ==========================================================

  START
    │
    ├─ D = 08
    ├─ A = 0  (XOR A)
    └─ HL = 0900h
         │
         ▼
  ┌─────────────────┐
  │  (HL) = 0       │  ← clear memory 0900h–0907h
  │  HL++           │
  │  D--            │
  └────────┬────────┘
           │
           │ [JR NZ] ◄── BUG: no label, won't assemble
           │  loop until D=0
           │
           ▼
  ┌─────────────────┐
  │  A = I          │  ◄── BUG: I is interrupt vector register,
  │                 │         not a data register. Corrupts interrupts.
  └────────┬────────┘
           │
           ▼
  ┌─────────────────┐
  │  A >= 0Ah ?     │  (CP 0A)
  └────┬───────┬────┘
       │ YES   │ NO
       │       │
       ▼       │   [JR NC] ◄── BUG: no label — jump destination unknown
  (exit/err?)  │
               ▼
  ┌─────────────────┐
  │  DE = 0880h     │  (tone lookup table base)
  │  E = E + A      │  ◄── BUG: ADD A,E only adds low byte;
  │                 │         no carry into D — table limited to 256 bytes
  └────────┬────────┘
           │
           ▼
  ┌─────────────────┐
  │  HL = 0900h     │
  │  A = (HL)       │
  │  A == 0 ?       │  (find empty slot in buffer)
  └────┬───────┬────┘
       │ NO    │ YES
       │       │
       │       │   [JR Z] ◄── BUG: no label
       │       ▼
       │   (slot found)
       │
       │ [INC HL]
       └──► [JR] ◄──── BUG: no label — infinite loop?
                        loops back to CP 00 presumably

           ▼ (slot found)
  ┌─────────────────┐
  │  (HL) = (DE)    │  store tone byte into buffer
  └────────┬────────┘
           │
           ▼
  ┌─────────────────┐
  │  A = FFh        │
  │  I = A          │  ◄── BUG: resets interrupt vector to FFxxh
  └────────┬────────┘
           │
           ▼
  ┌─────────────────┐
  │  C = 20h        │  (output select pattern)
  │  HL = 0900h     │
  │  D = 06         │  (outer loop: 6 display positions)
  │  B = 00         │  ◄── NOTE: B=0 means DJNZ loops 256 times
  └────────┬────────┘
           │
           ▼
  ┌─────────────────────────────────┐
  │  A = (HL)    → OUT (02), A      │  ← port 02: segment data
  │  A = C       → OUT (01), A      │  ← port 01: digit select
  │  RRC C                          │  rotate select pattern
  │  B--                            │
  └──────────────┬──────────────────┘
                 │
         [DJNZ] ◄── BUG: no label
         loop 256x
                 │
                 ▼
  ┌─────────────────┐
  │  A = 0 (XOR A)  │
  │  OUT (01), A    │  ← blank digit select
  │  HL++           │
  │  D--            │
  └────────┬────────┘
           │
           │ [JR NZ] ◄── BUG: no label
           │  loop until D=0 (6 display positions done)
           │
           ▼
        [JR] ◄──────────── BUG: no label — probably meant to
                            restart main loop, but destination unknown


  FAULTS SUMMARY
  ──────────────
  1. JR NZ  (init loop)       — no target label
  2. JR NC  (digit range chk) — no target label
  3. JR Z   (find slot)       — no target label
  4. JR     (slot scan loop)  — no target label
  5. DJNZ   (inner output)    — no target label
  6. JR NZ  (outer output)    — no target label
  7. JR     (main restart)    — no target label
  8. LD A,I / LD I,A          — misuse of interrupt vector register
  9. LD A(HL)                 — missing comma
 10. LD DE 0880               — missing comma
 11. ADD A,E                  — only 8-bit offset; table walk is fragile
 12. B = 00                   — DJNZ loops 256x, likely unintentional
```

---

## Hardware Flow

```
                         tec-AT5000 Hardware Flow
                         ========================

         PHONE LINE (POTS)
         ┌─────────────┐
         │   RJ-11     │
         │  Line Jack  │
         └──────┬──────┘
                │ analog audio + DC
                ▼
         ┌─────────────┐
         │   Diode     │
         │   Bridge    │  ← line polarity protection
         └──────┬──────┘
                │
                ▼
         ┌─────────────┐
         │  Opto-      │
         │  Isolator   │  ← galvanic isolation (4N25)
         │  (4N25)     │
         └──────┬──────┘
                │
        ┌───────┴────────┐
        │                │
        ▼                ▼
 ┌────────────┐   ┌────────────┐
 │  MT8870    │   │  TP5089 /  │
 │  DTMF      │   │  HT9200B   │
 │  Decoder   │   │  DTMF Gen  │
 └─────┬──────┘   └─────┬──────┘
       │ 4-bit          │ tone out
       │ nibble         │
       │ + STD          │
       │                │
       └────────┬───────┘
                │ I/O ports
                ▼
         ┌─────────────────────────────────────────┐
         │                                         │
         │         TEC-1  Z80  CPU                 │
         │                                         │
         │  I/O port map:                          │
         │    Port 00 → keypad scan                │
         │    Port 01 → LED / segment drive        │
         │    Port 02 → speaker / tone out         │
         │    Port 03 → MT8870 nibble + STD        │
         │    Port 04 → LX20LYA control lines      │
         │    Port 05 → Veeder-Root counter pulse  │
         │    Port 06 → TP5089 digit select        │
         │                                         │
         └──┬──────┬──────┬──────┬──────┬──────┬──┘
            │      │      │      │      │      │
            ▼      ▼      ▼      ▼      ▼      ▼
       ┌──────┐ ┌─────┐ ┌─────┐ ┌────────────────┐
       │ TEC-1│ │TEC-1│ │TEC-1│ │   Veeder-Root  │
       │Keypad│ │ LED │ │Spkr │ │ Mech. Counter  │
       │      │ │Array│ │     │ │  (5 digits)    │
       └──────┘ └─────┘ └──┬──┘ └───────┬────────┘
                            │            │
                            │            │ pulse via
                            │            │ transistor
                            │            │ driver
                            ▼            ▼
                       ┌─────────┐  ┌──────────┐
                       │ LX20LYA │  │  NPN +   │
                       │  Voice  │  │ flyback  │
                       │ Module  │  │  diode   │
                       │ (20 s)  │  └──────────┘
                       └────┬────┘
                            │
                            ▼
                       ┌─────────┐
                       │  Mic /  │
                       │ Speaker │
                       └─────────┘

  LEGEND
  ──────
  → data / control signals
  ── audio / analog signals
  (all digital I/O via Z80 bus)
```

**Key signal paths:**
- **Inbound:** Phone line → bridge → opto → MT8870 → Z80 (digit received)
- **Outbound:** Z80 → TP5089 → opto → line (dial tones sent)
- **Voice:** Z80 triggers LX20LYA → speaker (recorded message plays)
- **Mechanical:** Z80 pulses transistor driver → Veeder-Root counter advances

> Note: I/O port map is provisional — finalise once free TEC-1 ports are confirmed.

---

## References

- [Talking Electronics – Dial Alarm 2](http://www.talkingelectronics.com/projects/Elektor/Dial%20Alarm-2/DialAlarm-2.html)  
- [Talking Electronics Issue 14 (PDF, p.16)](https://github.com/SteveJustin1963/tec-BOOKS/blob/master/TE/Mag/talking_electronics_14.pdf)  
- [YouTube – The Simpsons AT-5000 Clip](https://www.youtube.com/watch?v=9rh50BSv_CE)

---

> **“I'll wait till this cop goes and I’ll grab my AT5000...”**

![](https://github.com/SteveJustin1963/tec-AT5000/blob/master/pics/feds.png)
