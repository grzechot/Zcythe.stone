Shield: [![CC BY-NC-SA 4.0][cc-by-nc-sa-shield]][cc-by-nc-sa]

This work is licensed under a
[Creative Commons Attribution-NonCommercial-ShareAlike 4.0 International License][cc-by-nc-sa].

[![CC BY-NC-SA 4.0][cc-by-nc-sa-image]][cc-by-nc-sa]

[cc-by-nc-sa]: http://creativecommons.org/licenses/by-nc-sa/4.0/
[cc-by-nc-sa-image]: https://licensebuttons.net/l/by-nc-sa/4.0/88x31.png
[cc-by-nc-sa-shield]: https://img.shields.io/badge/License-CC%20BY--NC--SA%204.0-lightgrey.svg

# Zcythe Stone – nozzle reference sensor for 3D printers

The companion of [Zcythe](https://github.com/grzechot/Zcythe.3d-printer-bed-probe), the bed probe.
Zcythe measures the bed; **Zcythe Stone** measures the nozzle. It sits on the bed frame beside the bed, and the
printer presses the nozzle onto it. Together they let the printer find its own Z offset (`G31 Z`) before every
print – no paper, no feeler gauge, no babystepping after a nozzle change. The system that uses both is
[Zcythe Field](https://github.com/grzechot/Zcythe.field) (macros and procedure).

**The Zcythe family:**

- **Zcythe** – bed probe on the toolhead (K0) – [Zcythe.3d-printer-bed-probe](https://github.com/grzechot/Zcythe.3d-printer-bed-probe)
- **Zcythe Stone** – nozzle sensor beside the bed (K1) *(this repository)*
- **Zcythe Field** – the system: RepRapFirmware macros, calibration, tools, docs – [Zcythe.field](https://github.com/grzechot/Zcythe.field)

**Highlights:**

- Repeatability of the nozzle dives: ~0.002 mm (σ, v1), two agreeing dives within 0.006 mm
- Rounded top (the head of an M3 screw) – the nozzle does not have to be centred exactly; position is found from the curvature
- Same principle as Zcythe: an opto endstop and a lever that multiplies the motion – here a long one, 14 : 200 mm
  (≈ 1 : 14), so 0.003 mm at the tip is ≈ 0.04 mm at the optosensor
- Printable (ABS)

Status: **working prototype, work in progress** – one printer (400 × 400 CoreXY, RepRapFirmware 3.6.1). Photos and
v2-specific assembly instructions will follow; the dead-zone screw is a new, untested prototype.

## Versions

| Version | Lever | Bearings | Status |
|---|---|---|---|
| **v1** (until 26 Oct 2026) | compact, Zcythe-like | 3x7x3, 2 pcs | worked; repeatability of the nozzle dives σ ≈ 0.002 mm |
| **v2** (since 26 Oct 2026) | long and thin, along the bed edge: 14 mm (tip) / 200 mm (optosensor), ≈ 1 : 14 | **3x8x4**, 2 pcs | in testing |

A larger bearing seems to give a smoother, more repeatable lever motion – v2 may work better with an even larger
one. **Work in progress.**

## Which printers, requirements

- CoreXY with a Z bed (Voron Trident, RatRig V-Core … – tested on a 4-Z CoreXY), flying gantry (Voron 2.4),
  bed slingers (Stone on the moving bed carriage), Cartesian printers, deltas (untested) – anywhere Stone can be fixed rigidly to the
  bed or its frame.
- **The nozzle must travel about 15 mm beyond the bed edge** to reach Stone (and so must Zcythe on the toolhead).
- Firmware: RepRapFirmware (macros in Zcythe Field, tested on RRF 3.6.1); Klipper / Marlin untested – same principle,
  the macros need porting.

Measured accuracy, diagnostics (bed and hotend thermal expansion, bed sag, Z drive faults) and the full list of
printer types: see the [Zcythe Field README](https://github.com/grzechot/Zcythe.field).

## How it works

A long, thin lever runs along the edge of the bed. Its short arm (14 mm from the pivot) carries an M3 screw with a
rounded head – the tip – next to the bed corner; the long arm (200 mm) ends with a flag in the slot of an opto endstop
at the far end. The nozzle presses the screw head down; after a small travel the flag leaves the beam and the sensor
switches. The Zcythe probe on the toolhead is too light to trigger it – only the nozzle does.

Before every print the printer:

1. probes the bed next to Stone with Zcythe,
2. probes the top of the Stone screw head with Zcythe (5 dives – the apex position comes from the curvature),
3. presses the nozzle onto the same apex until Stone triggers.

From these three heights it computes the nozzle height at the moment Zcythe triggers. One constant – how far the
nozzle pushes the head before the switch (dead travel) – is calibrated once with a first-layer print.

## Video

[Zcythe Field in action – Zcythe + Stone v2](https://youtu.be/-OZQV1-SDUw) (28 s; earlier: [how Zcythe works](https://www.youtube.com/watch?v=oTdeM-AWUHY)): 0:00 Zcythe dives on the Stone tip, 0:07 close-up of the
cross of dives (the red LED is the Zcythe optosensor), 0:19 the nozzle on the Stone tip and the whole 200 mm lever.

## Files

- `stl/` – parts to print, `cad/` – STEP / CAD sources, `pic/` – photos. Videos are on YouTube (links above).

## Parts

The same as [Zcythe](https://github.com/grzechot/Zcythe.3d-printer-bed-probe#parts), except the ball:

- printable: `stl/K1Flag.stl` and `stl/K1Support.stl` (version to be confirmed: v1 / v2). Recommended material – ABS.
- optosensor 5 V (the same type as in Zcythe)
- bearings, 2 pcs: 3x7x3 (v1) or **3x8x4 (v2)**. Soak in grease before use (hot paraffin bath works).
- 5x5 mm brass heat insert with collar
- 0.5x5 mm magnet, 2 pcs
- **instead of the 3.2 mm ball: an M3 screw with a rounded head** – it is the measuring tip and the K1 height
  adjustment at the same time
- M3 bolts as in Zcythe (M3x6 for the optosensor, M3x10 for the pivot)

## Assembly instructions

For now the same as [Zcythe](https://github.com/grzechot/Zcythe.3d-printer-bed-probe#assembly-instructions)
(K1Flag = ZFlag, K1Support = ZSupport), except the tip:

1. Press both bearings into the slot in the K1Flag. Leave no space between them.
2. Screw the M3 rounded-head screw into the K1Flag (instead of heat pressing the ball). It must turn with a little
   resistance and keep its position – it is the height adjustment.
3. Heat press the magnets into both parts, as the brass insert. Mind the polarity!
4. Desolder the socket of the endstop and solder it on the same side as the optosensor.
5. Prepare the K1Support: remove the printed supports, press fit the optosensor, secure it with one M3x6 bolt.
   When preparing the cables, mind the polarity!
6. Paint the flag surfaces black or silver as in Zcythe. Cut 0.5 mm of the edge.
7. Heat press the brass insert, the narrow part stays outside.
8. Screw the K1Flag to the K1Support with an M3x10 bolt. Check the bearings: the lever must rotate without any
   resistance, and the painted surfaces must not touch the optosensor. Adjust the brass insert if needed.
9. Test the magnets and the motion many times – the assembly must withstand tens of thousands of cycles.

## Adjustments

- **Tip screw (M3, rounded head):** sets how high the tip sits relative to the bed. About 0.4–0.55 mm per turn,
  **loosening raises the tip**. Target: Stone triggers ~0.2 mm before Zcythe would touch the bed
  ("K1 margin" 0.2 ± 0.15 mm). If the nozzle triggers Stone *lower* than Zcythe touches the bed, the Zcythe
  needle gets pushed into the bed and latches closed.
- **Dead-zone screw** (new prototype, not tested yet): moves the flag in the slot – changes the trigger point, not the rest height of the tip.
  Recommended 0.05–0.15 mm. Too far and the flag sits on the beam edge: the sensor switches by itself.

After any adjustment of the bed sheet or of Stone the calibration constants have to be measured again
(see [Zcythe Field](https://github.com/grzechot/Zcythe.field/blob/main/docs/procedures.md)).

## Mounting

- Screw it to the **bed frame**, not to the bed sheet, outside the print area, with the lever along the bed edge
  and the tip next to a bed corner within reach of the nozzle and of Zcythe.
- Stone triggers only from a vertical push (v1: sideways only to a push in −Y). Do not drag the nozzle over it.
- Do not press the tip far beyond the trigger point – repeated over-travel lowers the trigger height.

## Configure (RepRapFirmware)

Stone is a second, permanent digital probe:

```gcode
M558 K1 P8 C"e0stop" H3 F60 T6000 A1
G31 K1 P500 X0 Y0 Z0
```

The nozzle dives on Stone at 30 mm/min. The calibration macros (`HesCal2`) are in [Zcythe Field](https://github.com/grzechot/Zcythe.field).

## Test

Press the tip gently with a finger: the LED must change state and the controller must report the triggered state
(`sensors.probes[1].value`). Release: back to open. Repeat many times.
