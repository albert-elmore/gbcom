# gbcom

A self-contained cell phone inside a Game Boy cartridge. The cartridge holds all phone electronics and power; when inserted, the Game Boy acts as a dumb terminal for calls, texts, and phone status.

**Status:** Planning — no firmware or hardware implementation yet. Design notes live in [`gb-cell.txt`](gb-cell.txt).

## Concept

Retro-futuristic hardware: vintage Game Boy UI meets modern cellular. The cartridge *is* the phone; the console is only the interface.

| Role | Responsibility |
|------|----------------|
| **Cartridge** | Cellular module, MCU, battery, audio, antennas |
| **Game Boy** | Display, buttons, speaker/mic path (no phone power draw) |

## Hardware (planned)

### Cartridge electronics

- **MCU:** RP2040 — cellular AT control, Game Boy bus, audio routing
- **Cellular:** SIM800L or SIM7000G (2G/4G voice + SMS)
- **Power:** ~1000 mAh LiPo, charge circuit + protection; fully self-powered
- **Audio:** Game Boy speaker + hacked microphone, or optional Bluetooth for headset/wireless audio
- **Shell:** Custom “jumbo” cartridge (~20 mm deep) for battery, module, antennas, micro-USB charge port

### Power model

The Game Boy does not power the phone. The cartridge battery runs the cellular stack; the console only provides UI and I/O.

## Software (planned)

### Cartridge firmware (C / RP2040 SDK)

- Calls, SMS, signal strength
- Memory-mapped registers on the Game Boy bus
- Commands from the console (dial, answer, hang up)

### Game Boy UI (C / [GBDK-2020](https://github.com/gbdk-2020/gbdk-2020))

Terminal-style app: signal bars, status (idle / ringing / in call), caller ID. Planned controls:

| Button | Action |
|--------|--------|
| **Start** | Answer |
| **Select** | Hang up |
| **A / B** | Dial / back |

### GB ↔ cartridge protocol

Registers exposed at fixed addresses (draft):

| Address | Function |
|---------|----------|
| `0xA000` | Signal strength (0–5 bars) |
| `0xA001` | Phone status (idle, incoming, in call, …) |
| `0xA002` | Caller ID / text buffer |
| `0xA010` | Command buffer (dial, hang up, …) |

### Mock development mode

UI can be built and tested in an emulator with **simulated** register values before hardware exists (mGBA + GBDK).

## Roadmap

1. Set up GBDK-2020 and mGBA
2. Build mock UI with fake phone state
3. Prototype RP2040 + cellular module
4. Design and 3D-print jumbo cartridge shell
5. Integrate firmware, hardware, and shell

## Repository layout

| Path | Description |
|------|-------------|
| [`gb-cell.txt`](gb-cell.txt) | Original design notes and overview |
| [`LICENSE`](LICENSE) | GNU GPL v3 |

Firmware, hardware designs, and CAD will be added as development starts.

## License

This project is licensed under the [GNU General Public License v3.0](LICENSE).
