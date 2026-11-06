# Decision Log — Musical Guacamole

This file separates decisions already made from recommendations and unresolved engineering questions. A design target is not a measured result.

## Accepted project decisions

| ID | Decision | Status / implications |
|---|---|---|
| D-01 | Use an existing commercial surface exciter | Accepted. No custom actuator development for the first prototype. |
| D-02 | Prototype with two exciters | Accepted. Test one first, then add the second. |
| D-03 | Keep exciter positions experimental | Accepted. Use adjustable mounting and a coordinate log. |
| D-04 | Bluetooth Classic for music | Accepted. Phone streaming apps supply the music; no custom streaming service. |
| D-05 | Include DSP | Accepted. Start with a documented, flat baseline preset. |
| D-06 | Independent amplifier channel per exciter | Accepted. Do not wire both exciters to one channel by default. |
| D-07 | External DC supply | Accepted. No battery in first prototype. |
| D-08 | Off-the-shelf modular/hybrid electronics | Accepted in principle; assistant to choose integration details after datasheets are compared. |
| D-09 | Phone controls desired | Master volume, bass, treble, EQ and channel levels. Not yet implemented. |
| D-10 | Primary goals | Sound production, bass and clarity. |
| D-11 | Large test board | Accepted as a direction; 1 m × 1 m is provisional only. |
| D-12 | Material undecided | Choose through comparison and tests. |
| D-13 | Manufacturer-prescribed mounting | Default; actual method depends on selected exciter. |
| D-14 | Test full-range before frequency splitting | Accepted experimental sequence. |
| D-15 | Comprehensive measurements | Acoustic microphone, panel accelerometer and electrical measurements. |
| D-16 | No custom enclosure for now | Accepted. |
| D-17 | No custom PCB for now | Accepted. |

## Assistant engineering recommendations

| ID | Recommendation | Reason |
|---|---|---|
| R-01 | Use a mono-compatible baseline signal duplicated to two independent outputs | Two exciters on one panel are not automatically a conventional stereo pair. |
| R-02 | Use DSP vendor's USB/PC interface for initial tuning | A2DP transports music; it is not a custom DSP control channel. |
| R-03 | Defer a custom phone app and BLE control until hardware is selected | Avoid designing an app around an unknown DSP/control API. |
| R-04 | Buy one exciter first, verify, then buy the second matching unit | Reduce component-selection risk and avoid buying six or two unverified units. |
| R-05 | Select amplifier and power supply from actual datasheet requirements | Headline wattage alone is not a compatibility check. |
| R-06 | Do not add a microcontroller unless a concrete control/protection requirement justifies it | The DSP and receiver may cover initial prototype needs. |

## Open decisions — resolve in this order

| Priority | Question | Evidence needed |
|---|---|---|
| P0 | Which exact surface exciter model? | Manufacturer datasheet, availability, cost, mounting instructions |
| P0 | Which amplifier module? | Output into actual impedance at selected supply voltage, thermal/protection data |
| P0 | Which DSP? | Two independent outputs, input compatibility, EQ/filter features, control interface |
| P0 | Which Bluetooth receiver? | A2DP support, output type/level, power requirements |
| P0 | Which DC supply? | Derived from amplifier voltage/current requirements |
| P1 | What exact panel material and thickness? | Candidate sample comparison and repeatable test fixture |
| P1 | What final test-panel dimensions/support? | Practical build constraints and acoustic experiment goals |
| P1 | What mounting layout? | Placement measurements on the chosen panel |
| P1 | What measurement microphone and accelerometer? | Budget, calibration and data acquisition compatibility |
| P2 | How will phone controls work? | Selected DSP's documented API/control interface; BLE or other transport |
| P2 | Does the design need a microcontroller? | Defined control, sequencing or protection requirements |
| P2 | What is the final enclosure? | Deliberately deferred until electronics and thermal requirements are known |

## Important correction: Bluetooth audio versus phone controls

Bluetooth Classic A2DP is appropriate for streaming music from a phone. It does not automatically let a custom app adjust DSP parameters. The first prototype should stream music over A2DP and tune DSP settings through the DSP manufacturer's USB/PC tool or documented interface. Phone-based controls require a separate control path, for example BLE, and a DSP/controller that exposes a usable API.

## Change control

When a decision changes, record:
- Decision ID
- Previous decision
- New decision
- Reason/evidence
- Date
- Whether the change affects parts already purchased or drawings already produced

Never mark a component as selected until its exact model and datasheet have been reviewed.
