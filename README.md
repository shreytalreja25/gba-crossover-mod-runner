# GBA Crossover Mod Studio & WebAssembly Runner 🎮

A reverse engineering toolkit and 60 FPS WebAssembly game runner for Game Boy Advance (`.gba`) cartridges.

Includes custom binary patchers, IPS patches, and an in-browser WebAssembly hardware emulator for **Pokémon LeafGreen (Mario & Spider-Man Crossover Mod)** and **Super Mario Bros (Infinite Lives Mod)**.

---

## ⚡ Features

### 1. Pokémon LeafGreen Crossover Mod (`LeafGreen_Modded.gba`)
- **Starter Evolutionary Line Overhaul**:
  - **Slot 1 (Bulbasaur) &rarr; MARIO**: Fire / Fighting &bull; BST 580 (HP 95, Atk 120, Def 90, Spd 115, SpAtk 90, SpDef 70)
  - **Slot 4 (Charmander) &rarr; SPIDER-MAN**: Bug / Fighting &bull; BST 580 (HP 90, Atk 115, Def 85, Spd 130, SpAtk 85, SpDef 75)
  - **Slot 7 (Squirtle) &rarr; BOWSER**: Dragon / Fire &bull; BST 600 (HP 115, Atk 135, Def 125, Spd 65, SpAtk 90, SpDef 70)
  - **Slot 25 (Pikachu) &rarr; PIKACHU-EX**: Electric / Fighting &bull; BST 540
  - **Slot 150 (Mewtwo) &rarr; MEWTWO-EX**: Psychic Buffed
- **Route 1 Wild Encounter Tables (`0x3C8D1C`)**:
  - Tall grass populated with wild Mario, Spider-Man, Bowser, Pikachu-EX, and Mewtwo-EX.
- **Move Data Table (`0x250C50`)**:
  - Tackle (75 pwr), Scratch (75 pwr), Ember (90 pwr), Water Gun (90 pwr), Vine Whip (85 pwr), Thunderbolt (110 pwr), Mega Punch (120 pwr).
- **Dialogue & Monologue Scripts**:
  - Professor Oak intro monologue (`0x1C5CC4`) customized for the Crossover Mod.
  - Pokeball inspection dialogue (`0x18E1EC` - `0x18E2CD`) customized for Spider-Man, Bowser, and Mario.
- **Hardware Checksum**:
  - Re-computed 8-bit complement header checksum (`0x00BD`) for 100% GBA hardware & emulator compatibility.

### 2. Classic NES Series - Super Mario Bros GBA Mod (`Super_Mario_Modded.gba`)
- **HUD Tile Remap (`0x183BA`)**: Modified classic HUD header tiles from `MARIO` to `SUPER`.
- **Infinite Lives Machine Code Patch (`0x98DA1`)**:
  - Replaced `DEC $075A` opcode (`0xCE 0x5A 0x07`) with `NOP NOP NOP` (`0xEA 0xEA 0xEA`).
- **Cartridge Header**: Renamed to `SPIDER MARIO`.

### 3. WebAssembly GBA Hardware Runner (`index.html` & `server.py`)
- Direct in-browser 60 FPS emulator powered by mGBA/VBA-M WebAssembly cores.
- Range-request HTTP server for binary ROM streaming without buffering.
- Plug-and-play Gamepad (Xbox, PlayStation, Switch) and keyboard controls.
- ROM drag-and-drop file picker for any `.gba` binary.

---

## 🛠️ Reverse Engineering Offsets Reference

| Target File | Table / Function | ROM Offset (Hex) | Description |
|---|---|---|---|
| LeafGreen v1.1 | Header Title | `0x00A0..0x00AC` | Cartridge internal title string (`PKMN MARIO`) |
| LeafGreen v1.1 | Header Checksum | `0x00BD` | 8-bit complement checksum byte |
| LeafGreen v1.1 | Pokémon Names | `0x245F37` | 11 bytes per species (Gen 3 `0xBB` font table) |
| LeafGreen v1.1 | Base Stats & Types | `0x2547EC` | 28 bytes per species (HP, Atk, Def, Spd, Types) |
| LeafGreen v1.1 | Move Data | `0x250C50` | 12 bytes per move (Power, Type, Accuracy, PP) |
| LeafGreen v1.1 | Route 1 Wilds | `0x3C8D1C` | 12 slots of `[min_lvl, max_lvl, species_id]` |
| LeafGreen v1.1 | Oak Monologue | `0x1C5CC4` | Intro monologue text string |
| LeafGreen v1.1 | Starter Dialogue | `0x18E1EC` | Oak's Pokeball claim dialogues |
| Super Mario GBA | HUD Tiles | `0x183BA` | NES PRG ROM HUD text tiles |
| Super Mario GBA | Lives Decrement | `0x98DA1` | `DEC $075A` opcode in NES 6502 execution |

---

## 🚀 Quick Start

### 1. Apply the Patches to Your Clean GBA ROMs

#### Option A: Run the Python Binary Modder
```bash
python scripts/mod_gba_games.py
```

#### Option B: Apply IPS Patches
Use any standard IPS patcher (e.g. [Rom Patcher JS](https://www.romhacking.net/patch/) or Lunar IPS) with the patch files in `patches/`:
- `patches/Pokemon_LeafGreen_Mario_Spidey_Mod.ips`
- `patches/Super_Mario_Bros_Infinite_Lives_Mod.ips`

---

### 2. Launch the WebAssembly GBA Player

Start the local server:
```bash
python server.py
```
Open **[http://127.0.0.1:3005](http://127.0.0.1:3005)** in your browser.

---

## 🎮 Controls

| GBA Button | Keyboard Key | Gamepad (Controller) |
|---|---|---|
| **D-Pad** | Arrow Keys (`↑` `↓` `←` `→`) | D-Pad / Left Stick |
| **A Button** | `Z` | A / Cross |
| **B Button** | `X` | B / Square |
| **L / R Shoulders** | `A` / `S` | LB / RB |
| **Start** | `Enter` | Start |
| **Select** | `Shift` | Select |
| **Fast Forward** | `Space` | RT (Right Trigger) |

---

## 📜 License
MIT License. Pokémon, Mario, and Game Boy Advance are trademarks of Nintendo / Creatures Inc. / Game Freak / Marvel. This project provides reverse-engineering scripts and delta patches for educational and research purposes.
