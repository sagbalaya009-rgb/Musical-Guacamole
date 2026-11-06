# Decision Log — Musical Guacamole

This log distinguishes agreed product requirements, finalized architecture decisions and hardware choices that still require datasheet evidence. A design target is not a measured result.

## Finalized architecture decisions

| ID | Decision | Rationale |
|---|---|---|
| D-01 | Use commercial surface exciters | The first prototype tests panel acoustics, not custom actuator manufacturing. |
| D-02 | Plan for two identical exciters; test one first | Matching improves repeatability; staged purchase reduces risk. |
| D-03 | Keep exciter positions adjustable | Panel modes and coupling determine useful positions; placement must be measured. |
| D-04 | Bluetooth Classic A2DP for music | Standard music streaming from ordinary phone apps. |
| D-05 | Bluetooth receiver uses analogue stereo line output | Simplifies receiver-to-DSP integration and avoids early I²S compatibility work. |
| D-06 | Use a separate two-channel DSP | Processing remains independently configurable and measurable. |
| D-07 | ADAU1701/SigmaStudio is the reference learning platform | Graphical signal-flow programming, analogue ADC/DAC and enough processing for initial EQ/filter/routing experiments. Exact board must be checked before purchase. |
| D-08 | DSP provides at least two independent outputs | One separately controlled signal path per exciter. |
| D-09 | Mix L+R inside DSP, then duplicate mono-compatible signal | Avoids assuming two panel exciters behave like conventional stereo speakers; avoids electrically shorting L/R. |
| D-10 | Start with flat EQ and conservative level | Establish a repeatable baseline before tuning. |
| D-11 | Start full-range only within safe actuator limits; add protective high-pass as justified | Avoids arbitrary crossover choices while protecting the exciter from unsafe low-frequency excursion. |
| D-12 | Use one stereo two-channel amplifier, one channel per exciter | Compact and independently driven; matched mono boards are an acceptable alternative. |
| D-13 | External certified isolated regulated DC supply | Avoids battery/charging complexity and keeps mains wiring out of the bench prototype. |
| D-14 | Modular/hybrid off-the-shelf electronics | Replaceable and easier to debug than a custom PCB. |
| D-15 | No microcontroller in first prototype | No essential first-build function requires custom firmware. |
| D-16 | PC/USB DSP tuning first | Vendor tool provides direct configuration without building a phone app. |
| D-17 | Defer phone DSP controls; later add separate BLE/control path | A2DP carries audio and is not a custom DSP parameter-control interface. |
| D-18 | No custom PCB, battery or enclosure in first prototype | These do not validate the core acoustic hypothesis. |
| D-19 | Test panel target approximately 1 m × 1 m | Practical provisional bench target; not a full-wall performance surrogate. |
| D-20 | Panel material remains experimental | Material, thickness and support alter panel modes and must be compared. |
| D-21 | Manufacturer-prescribed exciter mounting | Coupling is central to performance; improvised mounting invalidates comparisons. |
| D-22 | Measure acoustic, mechanical and electrical behaviour | SPL/frequency response alone cannot explain vibration, power or thermal limits. |

## Reference audio path

Phone music app  
→ Bluetooth Classic A2DP receiver with analogue L/R line output  
→ DSP (L/R analogue input; two independent processed outputs)  
→ stereo two-channel amplifier  
→ exciter 1 and exciter 2  
→ test panel.

The amplifier's supply is selected after its exact model and load are known. Lower-voltage receiver/DSP boards receive only their specified rails.

## Why not integrate everything into one board?

An integrated Bluetooth/DSP/amplifier board can reduce wiring, but it makes it harder to isolate faults and may constrain DSP routing, output power, cooling or future module replacement. A modular first prototype is easier to learn from and compare. Integration can be reconsidered after the acoustic design is validated.

## Why not stream digital I²S directly into the DSP?

I²S can reduce an extra digital-to-analogue conversion, but it requires compatible clock master/slave configuration, sample rate, word length, channel mapping and electrical pinout. For a first mechatronics/audio prototype, analogue line output is the lower-risk interface. Revisit I²S only if measured noise or signal-quality limitations justify it.

## Why not use the phone as the DSP?

The phone is already responsible for streaming and user interaction. A phone-based audio-processing path would introduce operating-system routing, latency, app and device compatibility issues. A dedicated DSP gives a stable repeatable signal chain independent of the phone model.

## Still-open hardware selections (not architecture decisions)

These remain deliberately unresolved until exact datasheets and availability are compared:

1. Exact surface exciter model and impedance/power rating.
2. Exact two-channel amplifier and its supply requirements.
3. Exact ADAU1701 development/module board or alternative DSP board.
4. Exact A2DP receiver with analogue line output.
5. Exact regulated DC supply and any lower-voltage converters.
6. Exact panel material, thickness, dimensions and support.
7. Measurement microphone, accelerometer and acquisition method.

**Selection order:** exciter → amplifier → DSP board → Bluetooth receiver → power supply → panel construction and mounting details. The receiver/DSP interface is already defined as analogue line-level, but exact modules still need matching.

## Future feature decisions

| Feature | Decision |
|---|---|
| Phone volume/EQ/bass/treble/per-channel control | Desired future feature; not first-build scope |
| BLE control | Likely option, only after DSP control interface is verified |
| Custom phone app | Defer until hardware and parameter-control protocol are proven |
| Automatic room/panel calibration | Research after repeatable measurement system exists |
| Custom PCB/enclosure | Revisit only after module-level prototype is validated |

## Change control

When a decision changes, record the ID, previous decision, new decision, evidence/reason, date and impact on purchased parts or drawings. Never mark a component selected until its exact model and datasheet have been reviewed.
