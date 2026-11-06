# Musical Guacamole

A mechatronics and audio-engineering prototype exploring a concealed surface-exciter system: instead of using a conventional speaker cone, electromagnetic exciters transfer vibration into a purpose-built panel so the panel itself radiates sound.

The first build is a **bench prototype using commercially available modules**. It is not a residential installation and does not require a custom PCB or custom exciter enclosure.

## Current prototype definition

- **Sound source:** phone streaming music over Bluetooth Classic (A2DP).
- **Actuators:** two commercially available surface exciters.
- **Test surface:** a large, approximately 1 m × 1 m board as an initial working target; final dimensions and construction remain subject to practical and acoustic review.
- **Audio processing:** DSP included from the beginning.
- **Amplification:** two independent amplifier channels, one per exciter.
- **Power:** external, regulated, isolated low-voltage DC supply selected after the amplifier and exciter are chosen.
- **Electronics:** modular/hybrid off-the-shelf boards connected with appropriate wiring and connectors.
- **Controls desired in the eventual phone interface:** master volume, bass, treble, EQ profiles and independent exciter/channel level.
- **Primary proof points:** sound production, bass performance and clarity.
- **Measurements:** acoustic, mechanical-vibration and electrical measurements.
- **Enclosures:** custom puck/hub enclosures are out of scope for this prototype.
- **Custom PCB:** out of scope; use development boards and modules.

## Important architecture notes

1. Bluetooth Classic A2DP carries the music stream. It does **not** provide a general-purpose channel for a custom app to change DSP settings. For the first bench prototype, use the DSP manufacturer's USB/PC control interface (or its supported control interface). Add BLE or another control transport only when implementing phone-based DSP controls.
2. Start with a mono-compatible test signal: sum left and right channels correctly in the DSP, then feed the same programme to both independent amplifier channels. This avoids treating two exciters on one panel as a conventional stereo pair. Independent gain/EQ/delay can be explored after baseline measurements.
3. The two exciters must be electrically compatible with their respective amplifier channels. Do not select an amplifier based only on its advertised wattage.
4. Select and validate one exciter model before buying the second. The intended prototype has two, but staged testing reduces cost and risk.
5. A small or medium coupon can validate mounting and wiring; the larger board is needed for more meaningful panel-acoustic experiments. A 1 m × 1 m panel still does not prove full-wall bass performance.

## Project documents

- [Puck and prototype engineering plan](docs/PUCK_DESIGN_PLAN.md)
- [Decision log and unresolved engineering decisions](docs/DECISION_LOG.md)

## Current status

**Concept architecture agreed; component selection and detailed design pending.** No exact exciter, amplifier, DSP, Bluetooth receiver, power supply or panel material has been approved yet. Do not order parts until their datasheets have been checked against the compatibility checklist in the engineering plan.
