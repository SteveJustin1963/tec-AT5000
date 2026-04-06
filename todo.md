# tec-AT5000 TODO

## Critical — Fix Assembly (`PHONE-DIALLER-Part 1.z80`)

- [ ] Add labels to all jump instructions (`JR NZ`, `JR NC`, `JR Z`, `JR`, `DJNZ`) — currently won't assemble
- [ ] Fix `LD A(HL)` → `LD A,(HL)` (missing comma)
- [ ] Fix `LD DE 0880` → `LD DE,0880H` (missing comma, add H suffix)
- [ ] Add `H` suffix to all hex literals throughout the file
- [ ] Remove use of `I` register as general data storage — use a RAM byte at a defined address instead
- [ ] Explain or replace `RRC C` output selection logic with commented alternative
- [ ] Verify the full listing against the original TE14 p.16 scan — likely OCR transcription errors

## Critical — Missing Core Functionality

- [ ] Write DTMF tone generation routine — software-timed dual-frequency output via TEC-1 speaker
  - DTMF pairs: rows 697/770/852/941 Hz, columns 1209/1336/1477 Hz
  - Use Z80 timer loops or extend existing monitor tone code
- [ ] Define Z80 I/O port map for MT8870 interface
  - Pick an unused port address
  - Wire MT8870 STD (Steering Digit) strobe to Z80 interrupt or polling loop
  - Map 4-bit nibble output (Q1–Q4) to Z80 data bus
- [ ] Write MT8870 read routine — poll or interrupt-driven, decode nibble to digit

## Hardware BOM Fixes

- [ ] Change RJ-45 to **RJ-11** — analog POTS lines use RJ-11, not RJ-45
- [ ] Design Veeder-Root counter driver circuit — transistor + flyback diode to pulse the counter coil from a Z80 output port
- [ ] Confirm LX20LYA control interface (trigger pins, record/playback timing) and map to Z80 I/O

## Design Gaps to Resolve

- [ ] Define complete I/O port map for all peripherals:
  - MT8870 decoder
  - LX20LYA voice module
  - LED status indicators
  - Veeder-Root counter
  - DTMF generator IC (TP5089 or HT9200B)
- [ ] Choose between software DTMF generation (Z80 tone loops) vs hardware IC (TP5089/HT9200B) — don't implement both without a clear reason
- [ ] Drop or properly specify VoIP path — "Bluetooth-to-VoIP adapter" is not a real design; decide between:
  - Pure POTS analog line (RJ-11 + DAA)
  - External ATA box (keeps Z80 out of SIP stack)
  - Drop VoIP from scope entirely for v1

## Phase 2 Hardware Tasks

- [ ] Breadboard DTMF generator circuit and verify tones with a phone or DTMF decoder app
- [ ] Breadboard MT8870 decoder, confirm STD strobe and nibble output
- [ ] Opto-isolator (4N25) wiring and line interface test
- [ ] Diode bridge line protection test
- [ ] LX20LYA record/playback test with TEC-1 trigger lines

## Phase 3 Software Tasks

- [ ] Port or rewrite phone dialler routine with correct labels and syntax
- [ ] Implement dial sequence: off-hook detect → dial digits → on-hook
- [ ] Keypad input handler — map TEC-1 hex keys to dial digits 0–9, *, #
- [ ] LED status driver — map Play/Record/Dial/Redial/Power to TEC-1 LED outputs
- [ ] Auto-redial routine with configurable retry count
- [ ] Voice playback trigger on call connect / disconnect events

## Documentation

- [ ] Draw schematic for MT8870 ↔ Z80 interface
- [ ] Draw schematic for Veeder-Root counter driver
- [ ] Document final I/O port map in README
- [ ] Scan or link original TE14 p.16 listing for reference
- [ ] Add wiring diagram for RJ-11 line interface with opto-isolator and bridge
