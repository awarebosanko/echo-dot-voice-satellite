# Echo Dot Voice Satellite

Reverse-engineering a 3rd-generation Amazon Echo Dot's mic array and
speaker into an open, locally-hosted voice satellite for Home Assistant —
replacing Amazon's proprietary compute layer with an ESP32 running
ESPHome, feeding a self-hosted Whisper → Ollama → Piper voice pipeline.

## Motivation

The Echo Dot's acoustic hardware — a 7-mic far-field array and a
purpose-tuned speaker — is well engineered. The limiting factor is the
locked MediaTek SoC that ties it to Amazon's cloud. This project treats
the device as two independent subsystems: acoustic hardware worth
keeping, and a compute layer worth replacing, and integrates the former
into an open embedded platform.

## System overview

```
[Salvaged mic array] --I2S--> [ESP32-WROOM-32E] --I2S--> [MAX98357A amp] --> [Salvaged speaker]
                                     |
                              Wyoming protocol
                                     |
                              [Home Assistant] --> Whisper (STT) --> Ollama (LLM) --> Piper (TTS)
```

The ESP32 handles wake-word detection (`micro_wake_word`) and audio
streaming only; speech-to-text, response generation, and text-to-speech
run on existing self-hosted infrastructure rather than on-device.

## Hardware

| Component | Source |
|---|---|
| Mic array + LED/button PCB | Salvaged from donor Echo Dot (3rd Gen) |
| Speaker + driver cage | Salvaged from donor Echo Dot (3rd Gen) |
| ESP32-WROOM-32E | Dev board |
| MAX98357A | I2S class-D amplifier breakout |

Full teardown, component identification, and sourcing notes: see
[`docs/teardown.md`](docs/teardown.md).

## Repository structure

```
docs/       Teardown findings, signal analysis, design notes
hardware/   Reference photos, Altium schematics/wiring diagrams
firmware/   ESPHome configuration
```

## Design notes

- **Why not reflash the original SoC:** the MediaTek MT8516 has no
  documented path to running alternate firmware; debug points exist but
  no full firmware replacement has been demonstrated in the hobbyist
  community. Scrapping the processor board and driving the salvaged
  transducers independently was judged the more reliable approach.
  Details in [`docs/teardown.md`](docs/teardown.md).
- **Signal interface:** the mic array's digital output is characterized
  in [`docs/pinout.md`](docs/pinout.md) to confirm I2S compatibility with
  the ESP32 before committing to a wiring approach.

## Status

Teardown and component identification are complete. Signal
characterization, amplifier wiring, and firmware integration are in
progress; this README will be updated with build results and test data
as those are completed.

## License

- Firmware (`firmware/`): [MIT](LICENSE)
- Hardware designs and schematics (`hardware/`): [CERN-OHL-S v2](LICENSE-HARDWARE.md)
