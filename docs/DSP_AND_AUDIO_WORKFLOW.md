# DSP, Bluetooth and Audio Workflow

**Purpose:** Explain the audio path and the software/skills required for the first Musical Guacamole prototype.

## 1. What happens when you press play

1. A music app on the phone obtains the music.
2. The phone transmits audio over Bluetooth Classic A2DP.
3. The Bluetooth receiver decodes the stream and outputs analogue stereo left/right line-level audio.
4. The DSP samples and processes those signals, then produces two outputs.
5. The amplifier converts those low-power signals into the voltage/current needed by each exciter.
6. The exciters vibrate the panel, which radiates sound.

Bluetooth receiver ≠ DSP ≠ amplifier. Each has a separate job.

## 2. Interfaces in plain language

- **A2DP:** Bluetooth profile for ordinary wireless music streaming.
- **Line-level audio:** low-voltage signal between receiver and DSP; it cannot drive an exciter.
- **DSP:** digital signal processor for gain, EQ, filters, routing, crossover and dynamics.
- **DAC:** converts digital audio into analogue audio; commonly included on DSP boards.
- **Amplifier:** supplies the power needed by the exciter.
- **I²S:** digital audio bus between chips; not required in our first architecture.
- **BLE:** possible later control link for app commands; not the music path here.

## 3. Software

### SigmaStudio — DSP programming

For an ADAU1701-based board, use Analog Devices **SigmaStudio**. It is graphical: processing blocks are connected as a signal-flow diagram, so you do not need to write DSP algorithms from scratch for basic routing, EQ and filters.

Resources:
- [SigmaDSP and SigmaStudio overview](https://www.analog.com/en/lp/001/sigma-dsp-audio-processors.html)
- [ADAU1701 datasheet](https://www.analog.com/media/en/technical-documentation/data-sheets/adau1701.pdf)
- [ADAU1701 SigmaStudio quick start](https://wiki.analog.com/resources/tools-software/sigmastudio/gettingstarted/quickstart/adau1701)
- [ADAU1701 self-boot application note](https://www.analog.com/en/resources/app-notes/an-923.html)

The exact board must expose analogue inputs/outputs and support the required programming interface (often a USBi-compatible interface). Confirm this before purchase.

### REW — acoustic measurement

Use [Room EQ Wizard (REW)](https://www.roomeqwizard.com/) later to measure frequency response and compare panel/placement experiments. A calibrated measurement microphone is needed for meaningful quantitative results. REW measures the system; it does not program the DSP.

## 4. What to learn, in order

1. Audio signal levels: line-level versus speaker-level.
2. Frequency: Hz, bass/midrange/treble and frequency response.
3. Gain and clipping: setting levels without digital or amplifier clipping.
4. EQ: changing frequency bands; why bass boost can overload an exciter.
5. Filters: high-pass and low-pass filters.
6. Crossover: splitting bands between outputs; a later experiment.
7. Impedance and amplifier matching: ohms, power, supply voltage and heat.
8. Measurement: sweeps, SPL, microphone placement and repeatability.

## 5. First SigmaStudio exercises

Do these on a compatible DSP evaluation board **before connecting the power amplifier or exciters**.

**A — Pass-through**
- Create a project for the correct DSP hardware.
- Add stereo input and output blocks.
- Connect L to L and R to R.
- Compile/download using the board's supported programming interface.
- Verify low-level line output with suitable test equipment.

**B — Master volume**
- Insert a stereo gain/volume block.
- Compile/download and confirm the level changes smoothly.
- Save the project.

**C — Mono-compatible routing**
- Attenuate and sum L+R inside the DSP.
- Route the same mono-compatible signal to both outputs.
- Keep separate output gain blocks for the two exciter channels.
- Never physically short L and R together.

**D — EQ and filters**
- Add one parametric EQ band and observe the change.
- Experiment with high-pass/low-pass filters at low signal levels.
- Do not choose final cutoffs or bass boost until the exciter limits and panel response are measured.

**E — Save and standalone operation**
- Save the SigmaStudio project.
- If the board supports EEPROM self-boot, follow its documentation to store the design so the PC is not needed during playback.
- Record board model, project version, sample rate and routing.

## 6. How the prototype is operated

- Pair the phone with the A2DP receiver.
- Connect receiver analogue L/R line outputs to DSP analogue inputs.
- Configure the DSP from a PC using its vendor software/programming interface.
- Connect DSP outputs to amplifier inputs.
- Connect amplifier outputs to exciters only after checking impedance, wiring and power limits.

The PC is for configuring/tuning. It need not stay connected during music playback if the selected DSP board supports standalone operation and has been configured accordingly.

## 7. Phone controls later

Audio and control are separate:
- **Audio:** phone → A2DP → receiver → DSP.
- **Control:** future app → BLE → controller or DSP-supported control interface → DSP parameters.

Future controls could include master volume, bass, treble, EQ preset and per-channel gain. Verify the DSP's documented parameter-control interface before designing the app. Do not assume an ordinary Bluetooth audio receiver can change DSP settings.

## 8. Commissioning rule

Test receiver and DSP at line level first. Add the amplifier and one exciter only after interfaces are verified. Start at low volume. Never connect an amplifier speaker output to a DSP line input. The exact BOM and wiring diagram remain gated by the selected exciter and amplifier datasheets.
