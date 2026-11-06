# Musical Guacamole

A mechatronics and audio-engineering prototype exploring a concealed surface-exciter system: electromagnetic exciters transfer vibration into an engineered panel so the panel itself radiates sound.

The first build is a **bench prototype using commercially available modules**. It is not a residential installation and does not require a custom PCB or custom exciter enclosure.

## Finalized prototype architecture

**Phone music app → Bluetooth Classic A2DP receiver with analogue line output → two-channel DSP → stereo two-channel amplifier → two surface exciters → test panel.**

- **Music source:** ordinary phone apps (Spotify, Apple Music, YouTube, local files). The phone remains the streaming device.
- **Wireless audio:** Bluetooth Classic A2DP. Use a receiver that exposes a proper analogue line-level L/R output.
- **DSP:** one two-channel-input DSP with at least two independently processed outputs. Preferred learning/reference platform: Analog Devices ADAU1701-based board with analogue ADC inputs, DAC outputs and SigmaStudio support, subject to board-level interface verification.
- **Amplification:** one stereo amplifier module with two genuinely independent channels; one exciter per channel. Two mono amplifier modules are also acceptable if they are electrically matched.
- **Actuators:** two identical commercial surface exciters. Test one before purchasing the second.
- **Panel:** approximately 1 m × 1 m as a provisional target. Material, thickness, support and exact dimensions remain experimental.
- **Power:** certified, isolated, regulated external DC supply selected after amplifier and exciter compatibility checks.
- **Integration:** modular wiring and commercial boards; no custom PCB, battery or custom enclosure in this prototype.
- **Initial signal routing:** sum L+R inside the DSP to a mono-compatible programme, then feed that programme to both independent outputs. Keep per-channel gain available. Do not electrically short Bluetooth L and R together.
- **Initial DSP preset:** flat EQ, conservative input/output levels, protective high-pass filter only when its cutoff is justified by exciter limits, no bass boost, no crossover and no artificial effects.
- **Phone controls:** not part of the first build. Use the DSP vendor's PC/USB configuration tool. A later phone app will need a separate documented control path (for example BLE to a controller/DSP interface); A2DP is for audio, not custom DSP parameter control.

## Why this architecture

1. Analogue line-level Bluetooth output makes the receiver-to-DSP connection straightforward and avoids early I²S clock, format and pin-compatibility work.
2. A separate DSP lets us inspect and change the signal processing without replacing the Bluetooth or amplifier stages.
3. One two-channel amplifier board keeps the build compact while preserving one independent power channel per exciter.
4. A mono-compatible baseline avoids assuming that two exciters on one panel behave like left/right conventional speakers. Placement and panel modes must be measured.
5. Commercial modules let us test the acoustic concept before investing in a custom PCB or enclosure.

## Project documents

- [Puck and prototype engineering plan](docs/PUCK_DESIGN_PLAN.md)
- [Decision log](docs/DECISION_LOG.md)
- [DSP, Bluetooth and audio workflow](docs/DSP_AND_AUDIO_WORKFLOW.md)

## Status

**System architecture is defined; exact components are not yet approved.** The exciter remains the first hardware selection because its impedance, power rating, mounting method and mechanical limits determine the amplifier, supply, panel coupling and protection settings. Do not order a complete set of parts until the exact datasheets have been checked together.
