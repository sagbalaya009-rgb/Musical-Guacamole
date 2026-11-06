# Puck and Prototype Engineering Plan

**Project:** Musical Guacamole  
**Document status:** Architecture baseline — Rev 0.2  
**Scope:** Two-exciter bench prototype using commercially available modules  
**Not in scope:** Residential installation, custom actuator, custom enclosure, custom PCB, integrated battery, production phone app.

---

## 1. Product concept and prototype goal

A phone streams music to a Bluetooth receiver. A digital signal processor (DSP) prepares the audio. Two separate amplifier channels drive two electromagnetic surface exciters mounted to a large test panel. The panel vibrates and radiates sound.

The prototype must establish:
1. Whether the exciter/panel combination produces useful sound.
2. Whether bass can be reproduced without unacceptable distortion, over-excursion or mechanical noise.
3. Whether midrange and treble are clear.
4. How exciter position and panel construction affect coverage and response.

This is an experiment, not proof that a finished building wall will work.

## 2. Finalized system architecture

### 2.1 Block diagram

Phone music app  
→ Bluetooth Classic receiver (A2DP; analogue stereo line output)  
→ two-channel DSP (analogue L/R input; at least two independently processed outputs)  
→ stereo two-channel amplifier (independent channel 1 and channel 2)  
→ surface exciter 1 and surface exciter 2  
→ test panel.

A single regulated external DC supply powers the amplifier and any modules whose voltage requirements match it. Use separate regulated conversion for lower-voltage modules if needed. Do not assume all boards share the same supply voltage.

### 2.2 Why this is the selected architecture

**Bluetooth receiver with analogue line output:** This is the simplest first-build boundary. The receiver handles Bluetooth pairing, A2DP decoding and audio conversion. The DSP receives familiar analogue left/right signals. This avoids needing to configure I²S clocks, data formats and digital-audio pin compatibility between two unrelated modules. The receiver must provide a documented line-level output, not amplified speaker output.

**Separate DSP:** It allows us to learn and tune EQ, filters, gain, crossover, limiting and routing without depending on a phone app. It also keeps processing separate from the amplifier's power stage.

**ADAU1701 as the preferred learning/reference DSP platform:** It has analogue ADC inputs, four DAC outputs, I²S-compatible digital ports and is programmed graphically using Analog Devices SigmaStudio. It supports the types of processing needed here (EQ, crossover, bass management and dynamics). This is a reference platform, not a purchased/approved part number: the actual development board must expose the required analogue inputs and outputs and include a workable programming interface. [ADI datasheet](https://www.analog.com/media/en/technical-documentation/data-sheets/adau1701.pdf) · [SigmaStudio overview](https://www.analog.com/en/lp/001/sigma-dsp-audio-processors.html)

**One stereo two-channel amplifier:** It is physically simpler than two separate amplifier boards but still provides independent power channels. Each channel drives one exciter. Two matched mono boards remain an acceptable alternative if their power, gain and input characteristics match.

**Two identical exciters:** Matching makes comparisons easier and avoids introducing two different actuator responses as a confounding variable. Test one first, then acquire the second of the same model after initial electrical and mechanical checks.

**No microcontroller in the first build:** The Bluetooth receiver and DSP handle audio; the DSP vendor's PC tool handles tuning. A microcontroller would add firmware, power and integration work before a concrete need exists. Add one later only for phone control, coordinated startup/mute, telemetry or protection that the chosen modules cannot provide.

**No custom PCB or enclosure:** Neither is necessary to test the core acoustic hypothesis. Use commercial boards, labelled wiring, appropriate connectors and a nonconductive mounting plate.

### 2.3 Audio format and routing

- Phone sends music using Bluetooth Classic A2DP.
- Receiver decodes the wireless stream and provides analogue stereo L/R line output.
- DSP converts/conditions the analogue inputs and performs the signal processing.
- DSP output 1 feeds amplifier channel 1; DSP output 2 feeds amplifier channel 2.
- Amplifier outputs drive the exciters directly, within the selected manufacturer's electrical limits.

For the first preset:
1. Sum left and right **inside the DSP** using an appropriate gain (for example, attenuate each input before summing to preserve headroom).
2. Route the same mono-compatible programme to both outputs.
3. Keep output gain independently adjustable.
4. Start with flat EQ and conservative level.
5. Do not add bass boost or aggressive low-frequency content.
6. Add a protective high-pass filter only after the exciter's low-frequency/mechanical limits are understood.

Do not connect L and R outputs together with a Y-cable or by twisting wires together. Mixing must be done in an active mixer/DSP or a properly designed resistive summing network.

Why mono first: two exciters attached to one panel do not automatically form a conventional stereo speaker pair. Panel modes, placement and phase relationships determine the resulting sound field. A common signal gives a repeatable baseline. Later, test independent EQ, gain, delay and frequency bands one variable at a time.

### 2.4 Signal and power interfaces to verify

Before purchasing, confirm all of the following:
- Bluetooth receiver output is analogue line-level, stereo, and within the DSP's input voltage range.
- DSP board has two analogue inputs and at least two analogue outputs available at the chosen sample rate.
- DSP output voltage and connector match the amplifier's input sensitivity and input type.
- Amplifier is stable at the chosen exciter impedance and delivers suitable power at the actual supply voltage.
- Each exciter is connected to its own amplifier channel.
- Power-supply voltage, continuous current, transient capacity, polarity and connector ratings suit every powered module.
- Any module requiring a different rail receives a suitable regulated supply; do not connect incompatible rails together.

### 2.5 Phone controls: separate from Bluetooth music

A2DP carries the music. It does not automatically expose custom DSP settings to an app. For the first prototype:
- Pair the phone to the receiver for music.
- Connect a PC to the DSP's supported programming interface.
- Use the DSP software to change parameters and save presets.

For a later phone-controlled version, use a separate control link (likely BLE) to a supported DSP control interface or a small controller that writes DSP parameters. Before choosing hardware for that version, verify that the DSP exposes documented parameter addresses/API and that the control interface can operate while audio is playing. Do not build the app before that is confirmed.

---

## 3. Decisions and their rationale

| Area | Final architecture decision | Reason |
|---|---|---|
| Exciter | Commercial surface exciter; two identical units planned | Reduces actuator-development risk and makes channel comparisons meaningful |
| First purchase | Buy/test one, then buy the matching second | Limits cost if the first model is unsuitable |
| Placement | Adjustable and experimental | Panel modes and coupling determine the best position |
| Music transport | Bluetooth Classic A2DP | Standard phone music-streaming path |
| Bluetooth output | Analogue stereo line-level | Simplest reliable interface to the DSP for this prototype |
| DSP | Separate two-channel-input DSP; ADAU1701/SigmaStudio as reference platform | Graphical configuration and suitable audio-processing functions without writing DSP algorithms from scratch |
| DSP outputs | At least two independent outputs | Allows separate gain/EQ/filters per exciter |
| Baseline programme | L+R mixed inside DSP, duplicated to both outputs | Avoids premature stereo assumptions and gives a repeatable baseline |
| Amplifier | One stereo, two-channel board; one channel per exciter | Compact while retaining independent channels |
| Power | External certified isolated regulated DC supply | No battery charging complexity; supply can be sized from amplifier needs |
| Control during prototype | PC/USB through DSP vendor's interface | Fastest route to learning and tuning |
| Phone control | Defer; separate BLE/control path later | A2DP is not a DSP-control API |
| Electronics | Modular/hybrid off-the-shelf boards | Easy to replace and debug |
| Microcontroller | Omit initially | No defined first-build function requires it |
| PCB/enclosure | No custom PCB or enclosure in first prototype | Does not advance the core acoustic experiment |
| Test panel | Approximately 1 m × 1 m provisional target | Large enough for useful panel experiments while remaining a bench prototype |
| Material | Not predetermined; compare candidates | Material, thickness and boundary conditions change panel modes |
| Bass strategy | Flat/full-range baseline first; protective filtering; frequency splitting later | Measure the system before attempting to compensate it |
| Measurements | Acoustic + mechanical + electrical | Sound alone cannot explain panel vibration, power or thermal behaviour |

## 4. Component selection — exact parts remain gated

Architecture decisions are final, but **specific component models are not**. The exciter is selected first because it determines amplifier load/power, mounting and some panel constraints. No complete BOM can responsibly be frozen before that.

### 4.1 Exciters — select first

Record from the manufacturer's datasheet:
- Nominal impedance (ohms)
- Continuous/RMS power rating and test conditions
- Recommended amplifier range, if specified
- Frequency response and measurement conditions
- Dimensions and mass
- Force factor or force rating, if published
- Maximum displacement, if published
- Mounting method and required contact area
- Recommended panel type/thickness, if provided
- Thermal and mechanical limits

Choose a documented, obtainable model with a mounting method that can be implemented on the test panel.

### 4.2 Amplifier

Use a two-channel amplifier board. Verify:
- Output power at the actual exciter impedance and selected supply voltage
- Stable supported load impedance
- Gain and input sensitivity
- Analogue input compatibility with DSP
- Supply voltage/current and thermal requirements
- Heatsinking and ventilation
- Overcurrent, overtemperature and short-circuit protection

Do not select by headline wattage alone. Do not bridge channels or parallel exciters unless explicitly supported by the amplifier manufacturer.

### 4.3 DSP

Minimum requirements:
- Two analogue inputs compatible with receiver output
- At least two independent analogue outputs
- Per-output gain/mute
- Parametric EQ or equivalent filters
- High-pass/low-pass filters and crossover capability
- Limiting/dynamics processing if supported
- PC configuration software and documented programming interface
- Preset save/recall
- Published input/output voltage limits and sample-rate support

ADAU1701 is the reference learning platform. Its datasheet specifies two ADCs, four DACs, I²S-compatible serial ports and SigmaStudio graphical programming. Verify the exact evaluation/module board's connectors, USBi/programming needs, power rails and output levels before purchase.

### 4.4 Bluetooth receiver

Must support:
- Bluetooth Classic A2DP audio from common phones
- Stable pairing/reconnection
- Analogue stereo line-level output (not speaker-level output)
- Published output level, supply voltage and current
- Low noise and suitable signal-to-noise performance

Do not choose a BLE-only board for music reception. BLE control and Bluetooth Classic audio are different jobs.

### 4.5 Power

Select only after amplifier choice. Verify DC voltage, continuous current, transient capacity, polarity, connector and cable rating, regulation and safety certification. Use a certified, isolated external low-voltage supply. If the DSP/receiver requires a different voltage, use an appropriate regulated converter or separate supply. Keep exposed mains wiring out of the prototype.

### 4.6 Wiring and mechanical items

- Use connectors rated for the amplifier current.
- Keep speaker-level wiring separated from low-level analogue wiring where practical.
- Use strain relief and label both amplifier outputs.
- Secure boards on a stable nonconductive mounting plate.
- Follow the exciter manufacturer's mounting and adhesive/fastener requirements exactly.
- Power down and disconnect before changing speaker-level wiring.

---

## 5. Panel and mounting

Panel material, thickness, supports and exciter positions strongly affect output. No material is selected yet.

Compare candidates by availability, cost, mass per area, bending stiffness, damping, flatness, machining and repeatability. Record exact material, thickness, dimensions and support conditions.

Keep positions adjustable. Mark coordinates from fixed panel edges and record every configuration. Do not permanently bond or drill multiple positions until the initial placement survey is complete. Do not place soft isolation material in the intended exciter-to-panel coupling path unless the manufacturer specifies it. Treat panel support vibration separately.

A 1 m × 1 m panel does not prove full-wall bass performance. Room-scale claims require a representative wall construction and later testing.

---

## 6. Build and commissioning sequence

### Stage 0 — Datasheet and compatibility gate
1. Shortlist commercial exciters and select one candidate.
2. Check its impedance, power, mounting and mechanical limits.
3. Select a compatible two-channel amplifier.
4. Select a DSP board with analogue L/R inputs and two independent outputs.
5. Select an A2DP receiver with analogue line output.
6. Select the external supply from the amplifier and module requirements.
7. Produce the exact BOM, wiring diagram and connector list.

**Gate:** no purchase until compatibility is demonstrated on paper.

### Stage 1 — Learn/configure the audio chain on the bench
1. Install SigmaStudio if the selected DSP board is ADAU1701-based.
2. Connect the DSP board using its supported USB programming interface.
3. Build a simple pass-through design: input L/R → output L/R.
4. Compile/download and verify the signal path at low level.
5. Add a master gain control and independent output gains.
6. Save the known-good project and preset.
7. Only then connect the Bluetooth receiver and verify music reaches the DSP.

### Stage 2 — Single-exciter checkout
1. Assemble receiver, DSP, one amplifier channel and one exciter.
2. Verify supply voltage, polarity and wiring before connecting the exciter.
3. Start with low gain and a low-level test signal.
4. Check clipping, rattles, abnormal heating and mechanical contact.
5. Record electrical and acoustic baseline results.

### Stage 3 — Two-exciter integration
1. Add the matching second exciter and amplifier channel.
2. Confirm each channel independently.
3. Send the same mono-compatible programme to both outputs.
4. Compare one- and two-exciter operation at matched levels.
5. Check cancellation, uneven response, rattles and panel hot spots.

### Stage 4 — Placement and DSP experiments
1. Establish a coordinate grid.
2. Test symmetric and asymmetric positions.
3. Change one variable at a time.
4. Measure frequency response, SPL and panel acceleration at fixed points.
5. Save each DSP preset and compare against the flat baseline at matched listening level.
6. Test conservative EQ and protective filtering before experimenting with frequency splitting.

### Stage 5 — Representative panel evaluation
Document the final test panel's construction and support conditions. Later test a representative wall assembly before making claims about room-scale performance.

---

## 7. Measurement plan

**Acoustic:** frequency response at fixed and multiple microphone positions; SPL at a defined distance; matched-level listening tests; distortion/THD if suitable equipment is available. A calibrated measurement microphone is preferred; a phone microphone is not a reliable substitute for quantitative claims.

**Mechanical:** accelerometer readings at marked panel points; vibration mapping; inspection for rattles, loose fixings and unwanted support vibration; exciter and amplifier temperature.

**Electrical:** supply voltage at idle/load; current draw; amplifier clipping/output checks with suitable equipment; component temperatures after defined operating periods.

Keep panel/support, exciter mounting, amplifier/supply, test excerpt, playback level, DSP preset and microphone location constant when comparing one variable.

---

## 8. Safety

- Use a certified isolated low-voltage DC supply.
- Do not expose or improvise household mains wiring.
- Power down and disconnect before changing speaker-level wiring.
- Respect impedance, power and thermal limits.
- Start at low volume and increase gradually.
- Secure the panel and fixture.
- Stop for smoke, smell, unexpected heating, scraping, rattling, protection cycling or visible damage.
- Do not run high-level low-frequency tones continuously.
- Use sensible listening levels and hearing protection for extended tests.

---

## 9. Puck architecture definition of done

The architecture is defined. The **hardware design is ready to build** only when:
- Exact exciter model and datasheet are recorded.
- Amplifier compatibility is checked at the actual impedance and supply voltage.
- DSP board's inputs, outputs, programming interface and software are confirmed.
- Bluetooth receiver's A2DP support and analogue line output are confirmed.
- Supply is correctly rated for all modules.
- Wiring diagram, connectors and BOM are complete.
- Mounting method follows the manufacturer's instructions.
- Panel material, thickness, dimensions and supports are documented.
- Initial test procedure and safety checks are written.

No performance claims are validated until measured.
