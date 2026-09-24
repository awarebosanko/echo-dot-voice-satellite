# Teardown & Component Identification

Donor unit: Echo Dot 3rd Gen, grey fabric, puck-shaped (not the spherical
4th/5th gen).

Cross-referenced against public teardowns to confirm board layout before
cutting anything:
- Pallav Aggarwal, ["Inside the Amazon Echo Dot (3rd Gen): A Complete
  Teardown"](https://pallavaggarwal.in/2023/01/10/teardown-amazon-echo-dot-3rd-gen/)
- Brian Dorey, ["Echo Dot 3rd Gen Smart Speaker
  Teardown"](https://www.briandorey.com/post/echo-dot-3rd-gen-smart-speaker-teardown)
- [iFixit Echo Dot 3rd Gen
  Teardown](https://www.ifixit.com/Teardown/Amazon+Echo+Dot+3rd+Generation+Teardown/138560)

## Confirmed: 2 PCBs total (no separate filter/amp board)

### Board 1 — Main Processor Board (round, double-sided) — **SCRAP**

| Side | Contents |
|---|---|
| Connector side | Barrel power jack, 3.5mm audio out jack, micro USB service port, TI TAS5770 speaker amplifier, L10/L11 LC output filter, P1/P2 spring-loaded contacts to speaker pads |
| Processor side | MediaTek MT8516 SoC, Micron NAND flash, Samsung DDR3 RAM (under metal shield) |

No viable path to reflashing this board — debug points exist but there's
no documented full firmware replacement for 3rd-gen hardware; community
attempts have stalled at "found probable debug UART/USB pins" with no
follow-through, and even a successful shell wouldn't give a usable
alternative OS without the signed bootloader chain.

### Board 2 — Top PCB: mic array / LEDs / buttons — **KEEP**

- 4x analog MEMS microphones → 2x TI TLV320ADC3101 stereo ADC/DSP chips
  (2 mics per ADC)
- MediaTek MT7658 dual-band Wi-Fi/BT controller
- IS31FL3236A RGB LED driver (12-LED ring)
- Ambient light sensor
- Dome switches (volume, mute, action button)

Connected to Board 1 via a single FPC ribbon cable.

### Speaker — **KEEP**

Housed in its own metal chassis/cage, no dedicated PCB. Connects to
Board 1 purely via spring-loaded contacts (P1/P2), not solder — makes
salvage straightforward.

## Photos (my unit)

- `hardware/all-parts.jpg` — fully disassembled, all salvageable components laid out
- `hardware/bottom-of-main-board-w-callouts-sized.jpg` — Board 1 (processor board being scrapped), with power/audio/amp callouts
- `hardware/internal-base.jpg` — internal base/chassis view

## Open question / next step

Need to confirm via logic analyzer whether Board 2's ADC output on the
FPC cable is a clean I2S digital audio stream that can feed directly into
the ESP32, or whether it's a proprietary/multiplexed format requiring the
mics to be re-wired directly instead. This determines whether Board 2
survives mostly intact (best case) or needs individual mic desoldering
(fallback).
