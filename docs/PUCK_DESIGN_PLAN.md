# Puck and Prototype Engineering Plan

**Project:** Musical Guacamole  
**Document status:** Planning baseline — Rev 0.1  
**Scope:** Two-exciter bench prototype using commercially available modules  
**Not in scope:** Residential installation, custom actuator, custom enclosure, custom PCB, integrated battery, NFC, production app.

---

## 1. Product concept

A phone streams music to a Bluetooth receiver. A digital signal processor (DSP) prepares the audio. Two separate amplifier channels drive two electromagnetic surface exciters mounted to a large test panel. The panel vibrates and radiates sound.

The first prototype is intended to answer three questions:

1. Can the selected exciter/panel combination produce useful sound?
2. Can it reproduce bass with sufficient output and without unacceptable distortion or mechanical noise?
3. Can it reproduce clear midrange and treble?

The prototype is an experiment, not proof that a finished building wall will work.

## 2. Agreed requirements

| ID | Requirement | Current definition | Status |
|---|---|---|---|
| R-01 | Exciter type | Existing commercial surface exciter | Agreed |
| R-02 | Prototype actuator count | Two exciters | Agreed |
| R-03 | Placement | Experimental; positions remain adjustable | Agreed |
| R-04 | Audio source | Phone streaming over Bluetooth Classic/A2DP | Agreed |
| R-05 | DSP | Included from first prototype | Agreed |
| R-06 | Amplification | Two independent amplifier channels | Agreed |
| R-07 | Power | External regulated low-voltage DC supply | Agreed |
| R-08 | Electronics integration | Modular/hybrid off-the-shelf modules | Recommended; user asked assistant to decide |
| R-09 | Phone controls (eventual) | Master volume, bass, treble, EQ profiles, per-channel level | Desired; control transport not yet selected |
| R-10 | Main test priorities | Sound, bass, clarity | Agreed |
| R-11 | Test board | Large board; approximately 1 m × 1 m as a provisional target | Provisional |
| R-12 | Panel material | To be selected through engineering comparison | Open |
| R-13 | Exciter mounting | Follow selected manufacturer's mounting instructions | Default rule |
| R-14 | Bass strategy | Test full-range first, then experiment with DSP frequency splitting | Agreed |
| R-15 | Measurements | Acoustic + mechanical + electrical | Agreed |
| R-16 | Custom enclosure | Not for this prototype | Out of scope |
| R-17 | Custom PCB | Not necessary; use modules | Out of scope |

## 3. Recommended system architecture

### 3.1 Audio path

Phone (music streaming app)  
→ Bluetooth Classic receiver with A2DP audio output  
→ DSP with at least two independent outputs  
→ amplifier channel 1 → exciter 1  
→ amplifier channel 2 → exciter 2  
→ test panel

The receiver output type (analogue line-level, I²S or another digital interface) must match the DSP input. The DSP output type and voltage must match the amplifier inputs.

### 3.2 Baseline DSP routing

For initial testing, configure a **mono-compatible baseline**:

- Receive left and right programme channels.
- Sum them correctly in the DSP (do not short L and R together electrically).
- Send the same mono programme to both outputs.
- Keep separate output gain for each exciter.
- Begin with EQ flat, no bass boost, no crossover and no artificial effects.
- Save a known baseline preset before experimenting.

Why: two exciters on one panel are not automatically a conventional stereo speaker pair. Panel vibration modes and placement determine what listeners hear. A stable common signal makes comparisons repeatable. Later experiments can apply different EQ, gain, delay or frequency bands per exciter.

### 3.3 Bluetooth and phone-control distinction

Bluetooth Classic A2DP is the normal transport for music from streaming applications. It does not, by itself, provide a custom control channel for a phone app to change DSP parameters.

For the bench prototype:
- Use Bluetooth Classic for music.
- Use the DSP manufacturer's USB/PC configuration tool or documented control interface for tuning.
- Do not build a custom phone app yet.

For a later phone-controlled product, add a documented control path such as BLE (or another supported connection) from phone to a controller/DSP. Keep audio streaming on A2DP. Confirm that the chosen DSP exposes a usable control interface before buying it.

## 4. Component selection — buy compatible modules

Do not select parts by headline wattage or marketplace descriptions alone. Obtain manufacturer datasheets and record the exact model numbers.

### 4.1 Exciters (select first)

Required datasheet fields:
- Nominal impedance (ohms)
- Continuous/RMS power rating and test conditions
- Recommended amplifier range, if specified
- Frequency response and measurement conditions
- Moving mass / total mass and dimensions
- Force factor or force rating, if published
- Maximum displacement, if published
- Mounting method and required contact area
- Recommended panel type/thickness, if provided
- Thermal and mechanical limits

Selection principle: choose a readily obtainable, documented exciter whose mounting method and electrical requirements suit the test panel. Buy one for initial checkout; buy the matching second unit after verifying the first.

### 4.2 Amplifiers

Use two independent channels. They may be on one stereo amplifier board or two mono boards, provided each channel can drive one exciter safely.

Verify:
- Output power at the actual exciter impedance and supply voltage
- Stable supported load impedance
- Gain and input sensitivity
- Analogue/digital input compatibility with DSP
- Power-supply voltage/current requirements
- Thermal limits, heatsinking and ventilation
- Protection features (overcurrent, overtemperature, short circuit, DC fault as applicable)
- Whether the board requires a particular power supply or filtering

Do not bridge channels or parallel exciters unless the amplifier manufacturer explicitly supports that configuration.

### 4.3 DSP

Minimum useful capabilities:
- At least two independently configurable outputs
- Inputs compatible with the selected Bluetooth receiver
- Input/output level controls
- Parametric EQ or equivalent filters
- High-pass/low-pass filters and crossover capability for experiments
- Per-output mute and gain
- Documented configuration software and control interface
- Ability to save/recall presets
- Published audio voltage levels and sample-rate support

Prefer a DSP that can be configured over USB/PC for the first prototype. Phone control is a later integration requirement.

### 4.4 Bluetooth receiver

Must support:
- Bluetooth Classic A2DP source audio from common phones
- Stable reconnection/pairing
- An audio output compatible with DSP input
- Published supply voltage and current
- Low noise and suitable line-level output, if analogue

Do not assume a BLE-only module can receive ordinary music-streaming audio.

### 4.5 Power supply

Select only after amplifier choice. Verify:
- Correct DC voltage and polarity
- Continuous current capacity with reasonable headroom
- Connector and cable current rating
- Regulated output and appropriate safety certifications
- Compatibility with amplifier transient/current demands

Use a certified, isolated external low-voltage DC supply. Keep exposed mains wiring out of the prototype. Add correctly rated fusing/current protection where appropriate. Never hot-plug wiring with the system powered.

### 4.6 Wiring and mechanical items

- Use connectors with current ratings appropriate to the amplifier output.
- Keep speaker-level wiring separate from low-level audio wiring where practical.
- Use strain relief so cable movement cannot pull on exciter terminals.
- Secure all modules on a nonconductive, stable test surface.
- Keep conductive debris and loose hardware away from exposed circuit boards.
- Follow the exciter manufacturer's mounting and adhesive/fastener requirements exactly.

## 5. Recommended modular/hybrid integration

Use separate functional modules where independent replacement or testing matters:
- Bluetooth receiver
- DSP board
- Two-channel amplifier (or two compatible mono amplifier modules)
- External DC supply
- Optional microcontroller only if required for a defined control/protection task

Avoid adding a microcontroller just because the eventual product may need one. The first prototype can be controlled by the DSP's own configuration interface. Add a controller when a specific requirement exists, such as phone-to-DSP control, coordinated power sequencing or sensor-based protection.

No custom PCB is needed. Use documented modules, short labelled wiring, screw terminals/connectors and a simple nonconductive mounting plate. Keep a wiring diagram and label both amplifier outputs.

## 6. Panel and exciter mounting

Panel material, thickness, support conditions and mounting locations strongly influence acoustic output. No panel material is selected yet.

Compare candidate materials using:
- Availability and cost
- Mass per area
- Bending stiffness
- Internal damping
- Flatness and consistency
- Ease of machining and secure mounting
- Repeatability between samples

Do not assume MDF, plywood, aluminium or a sandwich panel is inherently best. Choose a manageable baseline panel and document its exact material, thickness, dimensions and support conditions.

Mount each exciter using the selected model's specified method. The intended vibration path from exciter to panel must be secure and consistent. Do not insert soft foam or rubber into that intended coupling path unless the manufacturer specifically calls for it. Manage unwanted vibration at the panel support separately.

Keep exciter positions adjustable at first. Mark coordinates from fixed panel edges and record each test configuration. Do not permanently bond or drill multiple positions until an initial placement survey is complete.

## 7. Prototype build sequence

### Stage 0 — Datasheet and compatibility review
1. Shortlist commercial exciters.
2. Select one candidate and verify its datasheet.
3. Select an amplifier channel compatible with the exciter.
4. Select a DSP with compatible inputs/outputs and two independent outputs.
5. Select Bluetooth receiver and power supply based on the chosen signal and power interfaces.
6. Produce a signed-off bill of materials and wiring diagram.

**Gate:** No purchase until electrical compatibility is demonstrated on paper.

### Stage 1 — Single-exciter electrical checkout
1. Assemble receiver, DSP, one amplifier channel and one exciter.
2. Verify polarity, wiring, supply voltage and current limit before connecting the exciter.
3. Start with low gain and a low-level test signal.
4. Check for clipping, rattles, abnormal heating or mechanical contact.
5. Record baseline electrical and acoustic results.

### Stage 2 — Two-exciter integration
1. Add the second matched exciter and independent amplifier channel.
2. Confirm each channel works independently.
3. Run identical mono programme to both outputs.
4. Compare one-exciter and two-exciter performance under repeatable conditions.
5. Check for cancellation, uneven response, rattles and panel hot spots.

### Stage 3 — Panel placement experiments
1. Establish a coordinate grid on the panel.
2. Test a small set of symmetric and asymmetric positions.
3. Change one variable at a time.
4. Record frequency response, SPL and panel acceleration at fixed measurement locations.
5. Select positions based on measured coverage and sound quality, not appearance.

### Stage 4 — DSP experiments
1. Save the flat baseline preset.
2. Test conservative EQ.
3. Test high-pass protection to avoid excessive low-frequency excursion.
4. Explore frequency splitting only after measuring each exciter/panel response.
5. Compare every change against the baseline at matched listening level.

### Stage 5 — Larger/representative panel evaluation
A roughly 1 m × 1 m board is a provisional test target, not a guarantee of full-wall behaviour. Record its actual support and boundary conditions. Later, test a panel with the intended wall construction and dimensions before making claims about room-scale bass.

## 8. Measurement and test plan

### Acoustic
- Frequency response at a fixed microphone position and multiple positions
- SPL at a defined distance
- Listening tests using the same tracks and matched levels
- Distortion/THD if suitable measurement software and equipment are available
- Repeat tests at more than one location to identify uneven coverage

A calibrated measurement microphone is preferred. A phone microphone is not a reliable substitute for quantitative frequency-response or SPL claims.

### Mechanical
- Accelerometer measurement at marked panel points
- Vibration mapping across the panel
- Inspection for rattles, loose fixings and unwanted support vibration
- Temperature checks on exciter body and amplifier heatsink

### Electrical
- Supply voltage under idle and load
- Current draw and estimated input power
- Amplifier output/clipping observation using suitable test equipment
- Component temperature after defined operating periods

### Repeatability rules
For comparisons, keep constant:
- Panel and support arrangement
- Exciter model and mounting method
- Amplifier and supply
- Test signal/music excerpt
- DSP preset unless it is the variable being tested
- Microphone location, distance and orientation
- Playback level

Record date, configuration ID, test files, observations and measurements in a test log.

## 9. Safety

- Use only a certified isolated low-voltage DC supply for this bench build.
- Do not expose or improvise household mains wiring.
- Power down and disconnect the supply before changing speaker-level wiring.
- Respect exciter and amplifier impedance, power and thermal limits.
- Start at low volume; increase gradually.
- Secure the panel and test fixture so vibration cannot cause it to fall or walk.
- Keep fingers, cables and loose objects clear of moving parts.
- Stop if there is smoke, smell, unexpected heating, scraping, rattling, amplifier protection cycling or visible damage.
- Do not run high-level low-frequency test tones continuously; they can overheat or over-excurse an exciter.
- Use hearing protection and sensible listening levels during extended tests.

## 10. Definition of done for the puck prototype

The puck/prototype design is ready for a first build when:
- Exact exciter model and datasheet are recorded.
- Amplifier compatibility is checked at the selected impedance and supply voltage.
- DSP input/output interfaces and two-channel routing are confirmed.
- Bluetooth receiver output is compatible with DSP input.
- External power supply is correctly rated.
- Wiring diagram and connector list are complete.
- Mounting method follows the exciter manufacturer's instructions.
- Panel material, thickness, dimensions and support are documented.
- Initial test procedure and safety checks are written.
- All unknowns are explicitly labelled; no performance claims are presented as validated before measurement.

## 11. Open decisions

See [DECISION_LOG.md](DECISION_LOG.md). The highest-priority open decision is selecting the actual exciter. That choice drives amplifier, power, mounting and some panel requirements.
