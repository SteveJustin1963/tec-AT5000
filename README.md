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

## DTMF Decode & Generate Explained

### MT8870 — DTMF Decode (phone → TEC-1)

The MT8870 listens to the phone audio and detects DTMF tones (the dual-frequency tones a keypad produces).

```
  AUDIO_RX ──► IN+
  GND_ISO  ──► IN-
```

The isolated audio from 4N25 #1 feeds the MT8870's differential input. It internally filters and detects which two frequencies are present — each combination maps to a digit (0–9, *, #).

```
  Q1 ──┐
  Q2 ──┼──► Port 03  (nibble)
  Q3 ──┤
  Q4 ──┘
  STD ──────► Z80 /INT
```

- **Q1–Q4** are 4 output bits — a binary nibble representing the detected digit (e.g. `0101` = digit 5)
- **STD (Steering Digit)** goes high when a valid tone pair has been detected and the nibble is stable — wired to the Z80's **/INT pin** so the CPU gets an interrupt the moment a digit arrives rather than polling constantly
- The Z80 interrupt handler reads Port 03, grabs the 4-bit digit, and processes it

**Decoupling cap:** The 100nF cap from +5V to GND sits right next to the MT8870 VDD pin — essential to suppress noise that would cause false detections.

---

### TP5089 — DTMF Generate (TEC-1 → phone)

The TP5089 does the opposite — it synthesises DTMF tones to dial out.

```
  Port 06 ──► D0 ┐
               D1 ┤ TP5089
               D2 ┤
               D3 ┘
```

The Z80 writes a 4-bit digit code to Port 06. The TP5089 reads D0–D3 and synthesises the correct pair of sine-wave frequencies for that digit. No software timing loops needed — the IC handles all tone generation in hardware.

```
  3.58MHz XTAL ─┬─ OSC1
                └─ OSC2
```

The TP5089 needs a precise clock reference to generate accurate frequencies. **3.58 MHz** is the NTSC colour burst crystal — cheap, widely available, and exactly what the TP5089 datasheet specifies. Without it the tones would be off-frequency and the exchange would reject the call.

```
  TONE OUT ──► C2 ──► 4N25 #2
```

The generated tone is AC-coupled through C2 (blocks DC) and fed into 4N25 #2 which injects it onto the phone line.

---

### How They Work Together

```
  INBOUND:  phone line → 4N25 #1 → AUDIO_RX → MT8870 IN+ → Q1-Q4 → Port 03 → Z80
  OUTBOUND: Z80 → Port 06 → TP5089 D0-D3 → TONE OUT → C2 → 4N25 #2 → phone line
```

They are independent paths — the MT8870 handles incoming digits (remote control via DTMF), the TP5089 handles outgoing dialling. Both share the same opto-isolation barrier to the phone line.

---

## TEC-1 Z80 CPU — I/O Port Map Explained

The Z80 communicates with all peripherals via **IN** and **OUT** instructions targeting port addresses. Each port is an 8-bit data bus connection to a specific device.

---

### Port 00 — Keypad (INPUT)

```
  Port 00 ◄───────────────── Keypad scan
```

The Z80 reads Port 00 to scan the TEC-1 hex keypad. The keypad is matrix-wired — the CPU reads back which key is currently pressed as a byte value. Used for entering phone digits, selecting functions (Play, Record, Dial etc.).

---

### Port 01 — LED Array (OUTPUT)

```
  Port 01 ────────────────► LED array
```

The Z80 writes a byte to Port 01 to drive the TEC-1 seven-segment LED displays. Each bit controls a segment or selects which digit is active. The display routine multiplexes across all digits rapidly so they appear simultaneously lit.

---

### Port 02 — Speaker / Tone (OUTPUT)

```
  Port 02 ────────────────► Speaker/tone
```

The Z80 toggles bits on Port 02 at timed intervals to generate square wave tones through the TEC-1 speaker — used for status beeps and feedback sounds. Note: **DTMF tones for dialling come from the TP5089, not this port** — Port 02 is for local audio FX only.

---

### Port 03 — MT8870 DTMF Decoder (INPUT)

```
  Port 03 ◄─── MT8870 Q1–Q4 + STD
```

The Z80 reads Port 03 to get the incoming DTMF digit. The lower 4 bits (Q1–Q4) carry the digit nibble. STD is also monitored here (or via /INT) to know when the nibble is valid. Typical read sequence:

```
  /INT fires → Z80 interrupt handler → IN A,(03H) → mask lower nibble → process digit
```

---

### Port 04 — LX20LYA Voice Module (OUTPUT)

```
  Port 04 ────────────────► LX20LYA ctrl
```

Individual bits control the voice module's function pins:

| Bit | Function |
|-----|----------|
| 0   | PLAY — trigger message playback |
| 1   | REC  — trigger record mode |
| 2   | /CE  — chip enable (active low) |

The Z80 pulses these lines to start/stop recording or playback of the 20-second voice message.

---

### Port 05 — Veeder-Root Counter Driver (OUTPUT)

```
  Port 05 ────────────────► VR counter drv
```

The Z80 writes a pulse to Port 05 to advance the mechanical counter by one digit. A single bit toggles high then low — the 2N2222 transistor amplifies this to drive the counter coil. The Z80 cannot drive the coil directly (too much current); the transistor handles that.

---

### Port 06 — TP5089 DTMF Generator (OUTPUT)

```
  Port 06 ────────────────► TP5089 D0–D3
```

The Z80 writes a 4-bit digit code to Port 06. The lower nibble (D0–D3) selects which DTMF tone pair the TP5089 synthesises and transmits to the phone line. To dial a number the Z80 writes each digit in sequence with appropriate inter-digit timing gaps.

---

### /INT — Interrupt Line (INPUT)

```
  /INT ◄─── MT8870 STD
```

The MT8870's STD pin is wired directly to the Z80's maskable interrupt (/INT). When a valid DTMF tone is detected, STD pulses and the Z80 immediately vectors to the interrupt service routine to read Port 03 — no polling loop needed, no digits missed.

---

### Port Map Summary

| Port | Dir | Connected To | Purpose |
|------|-----|-------------|---------|
| 00 | IN  | Keypad | Read key presses |
| 01 | OUT | LED array | Drive display |
| 02 | OUT | Speaker | Local audio / beeps |
| 03 | IN  | MT8870 Q1–Q4 | Read incoming DTMF digit |
| 04 | OUT | LX20LYA | Control voice module |
| 05 | OUT | 2N2222 base | Pulse Veeder-Root counter |
| 06 | OUT | TP5089 D0–D3 | Send outgoing DTMF digit |
| /INT | IN | MT8870 STD | Interrupt on digit arrival |

---

## Output Peripherals Explained

### 5. LX20LYA — Voice Module (Port 04)

```
  bit0 ──► PLAY
  bit1 ──► REC   LX20LYA
  bit2 ──► /CE
  SPKR ──► Speaker out
  +5V ──[100nF]── GND
```

The Z80 controls the LX20LYA by setting individual bits on Port 04:

- **bit0 → PLAY** — pulse high to trigger playback. The module plays autonomously once triggered; the Z80 doesn't need to do anything further
- **bit1 → REC** — pulse high to start recording from the on-board microphone. Hold high for the duration of recording, then pull low to stop
- **bit2 → /CE** — chip enable, **active low**. Must be pulled low to enable the module before issuing PLAY or REC
- **SPKR** — the module drives the speaker directly from its own internal amplifier; the Z80 doesn't generate audio
- **100nF decoupling cap** — sits on the VDD pin to keep the supply clean during playback current peaks

Typical sequence: Z80 sets /CE low → sets PLAY high → module speaks pre-recorded message → Z80 sets PLAY low when done.

---

### 6. Veeder-Root Counter Driver (Port 05)

```
  Port 05 ──[R 1kΩ]──► B
                  2N2222
             C ──┬── COIL ──── +5V
             E   │
             │  [1N4007 flyback]
            GND  │
                 └──── +5V
```

The mechanical counter coil needs more current than a Z80 output pin can safely supply (~5mA max from Z80, coil needs 50–100mA). The 2N2222 acts as a switch:

- **R 1kΩ** — limits base current to a safe level for the Z80 output pin
- **2N2222 base (B)** — when Port 05 bit goes high the transistor saturates (switches on)
- **Collector (C)** — connects to one end of the counter coil, other end to +5V
- **Emitter (E)** — to GND
- When transistor switches on, current flows +5V → COIL → C → E → GND, energising the coil and advancing the counter one digit

**1N4007 flyback diode** — critical. When the transistor switches off, the collapsing magnetic field in the coil generates a large voltage spike (potentially hundreds of volts). The 1N4007 placed across the coil (cathode to +5V, anode to collector) clamps this spike and routes it safely back to +5V, protecting the transistor from destruction.

To advance the counter: Z80 writes `1` to Port 05 bit → waits ~10ms → writes `0` → counter steps one digit.

---

### 7. Speaker (Port 02)

```
  Port 02 ──► piezo/spkr
```

The Z80 toggles a bit on Port 02 at a set frequency to produce a square wave tone:

- Toggle every 1ms → 500 Hz tone
- Toggle every 0.5ms → 1 kHz tone

Used for **local feedback only** — key press beeps, error tones, status sounds. The actual DTMF dial tones go via the TP5089 → phone line path, not through this speaker. A piezo element is ideal — driven directly from the port pin with no amplifier needed.

---

### How the Three Work Together During a Call

```
  1. User presses key     → Port 00 read  → Port 02 beep confirmation
  2. Digit sent to line   → Port 06 write → TP5089 → phone line
  3. Counter advances     → Port 05 pulse → 2N2222 → Veeder-Root steps
  4. Voice message plays  → Port 04 bits  → LX20LYA → speaker
  5. Incoming DTMF heard  → /INT fires    → Port 03 read → action taken
```

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

## What the Code Does — Plain English

The program is a **phone digit display driver**. It sits in a loop waiting for someone to give it a digit to display, then shows it on the TEC-1's LED display.

### Startup

When the TEC-1 powers on, the code first **wipes the display buffer** — six memory locations that hold what's currently shown on the LEDs. It sets them all to zero so the display comes up blank.

It then sets a **sentinel value (FFh)** in a memory location called `DIGIT_REG`. This is a flag meaning *"no digit waiting yet"*.

### Waiting for a Digit

The code sits in a tight loop, constantly reading `DIGIT_REG`. It's waiting for something else (an interrupt routine, not shown here) to write a digit 0–9 into that location.

As long as `DIGIT_REG` is FFh — or anything 10 or above — it just keeps looping and does nothing.

### When a Digit Arrives

The moment a valid digit (0–9) appears in `DIGIT_REG`, the code:

1. **Looks up the tone pattern** for that digit in a table stored at address `0880h`. Each digit has a corresponding byte that encodes how to display or sound it.
2. **Finds an empty slot** in the 6-byte display buffer by scanning through it until it finds a zero byte.
3. **Writes the tone/display byte** into that empty slot.
4. **Resets `DIGIT_REG` back to FFh** — signalling *"I've processed that digit, ready for the next one"*.

### Driving the Display

It then runs the **multiplexed display routine**. The TEC-1 has 6 LED digit positions but they aren't all driven at once — they're switched on one at a time, very rapidly, so they appear simultaneously lit to the human eye.

For each of the 6 digit positions it:
1. Sends the **segment pattern** (which segments to light) to port 02h
2. Switches **that digit's select line on** via port 01h
3. Holds it on for **128 cycles** (display brightness timing)
4. Switches the **digit select off**
5. Rotates the select pattern to the **next digit**
6. Moves to the **next byte** in the buffer

After all 6 digits have been shown once, it loops straight back to check for the next incoming digit.

### In One Sentence

> It waits for a digit to arrive in a memory location, looks up its display pattern, slots it into the display buffer, then continuously refreshes the 6-digit LED display in a loop.

### What's Missing

- The **interrupt handler** that writes digits into `DIGIT_REG` (would come from keypad or MT8870 DTMF decoder)
- The actual **tone pattern values** in the lookup table at `0880h` — currently all placeholder zeros, needs filling from TE14 p.16 spec

---

## Corrected Code Flow (PHONE-DIALLER-Part 1.z80)

```
  PHONE-DIALLER-Part 1.z80 — Corrected Code Flow
  ================================================

  0800h  START
    │
    ├─ D = 08h
    ├─ A = 0  (XOR A)
    └─ HL = 0900h (BUF)
         │
         ▼
  ┌──────────────────┐
  │  (HL) = 00h      │  0806h  CLEAR
  │  HL++            │  ← zero buffer 0900h–0907h
  │  D--             │
  └────────┬─────────┘
           │ [JR NZ,CLEAR]
           │ loop until D=0
           │
           ▼
    DIGIT_REG = FFh    ← 090Fh, sentinel = no digit pending
           │
           ▼
  ┌──────────────────┐
  │  A = (DIGIT_REG) │  0810h  MAIN
  │  A >= 0Ah ?      │  ← poll for valid digit
  └────┬─────────────┘
       │ YES (NC)           NO (digit 0–9)
       │ [JR NC,MAIN]       │
       └──────────┐         │
                  ▼         ▼
              (wait)   ┌──────────────────┐
                       │  HL = 0880h      │  ← TONE_TBL base
                       │  DE = 00:digit   │
                       │  HL = HL + DE    │  ← 16-bit table lookup
                       │  B = (HL)        │  ← save tone byte in B
                       └────────┬─────────┘
                                │
                                ▼
                       ┌──────────────────┐
                       │  HL = 0900h      │  0823h  FIND_SLOT
                       │  A = (HL)        │  ← scan buffer for empty slot
                       │  A == 00h ?      │
                       └────┬─────────────┘
                            │ NO                  YES
                            │ INC HL              │ [JR Z,STORE]
                            │ [JR FIND_SLOT]      │
                            └──────────┐          │
                                       ▼          ▼
                                   (scan)   ┌──────────────────┐
                                            │  (HL) = B        │  082Bh  STORE
                                            │  ← write tone    │
                                            │  DIGIT_REG = FFh │  ← reset sentinel
                                            └────────┬─────────┘
                                                     │
                                                     ▼
                                            ┌──────────────────┐
                                            │  C = 20h         │  0831h  DISPLAY
                                            │  HL = 0900h      │
                                            │  D = 06h         │  ← 6 digit positions
                                            └────────┬─────────┘
                                                     │
                                                     ▼
                                            ┌──────────────────────────────┐
                                            │  A = (HL)                    │  0838h  DISP_OUTER
                                            │  OUT (02h), A  ← seg data   │
                                            │  A = C                       │
                                            │  OUT (01h), A  ← digit on   │
                                            │  B = 80h                     │
                                            └────────┬─────────────────────┘
                                                     │
                                                     ▼
                                            ┌──────────────────┐
                                            │  DJNZ DISP_INNER │  0840h  DISP_INNER
                                            │  ← 128 cycle     │
                                            │     delay        │
                                            └────────┬─────────┘
                                                     │ B = 0
                                                     ▼
                                            ┌──────────────────┐
                                            │  A = 0           │
                                            │  OUT (01h), A    │  ← digit off (blank)
                                            │  RRC C           │  ← next digit select
                                            │  HL++            │
                                            │  D--             │
                                            └────────┬─────────┘
                                                     │
                                          [JR NZ, DISP_OUTER]
                                          loop 6 times
                                                     │ D = 0
                                                     │
                                          [JR MAIN] ─┘
                                          ← back to poll for next digit


  MEMORY MAP                    I/O PORTS
  ──────────                    ─────────
  0800h  code start             Port 01h  digit select (LED mux)
  084Dh  code end  (77 bytes)   Port 02h  segment data
  0880h  tone table (10 bytes)
  0900h  display buffer (6 bytes)
  090Fh  DIGIT_REG sentinel
```

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
