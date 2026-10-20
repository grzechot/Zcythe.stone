<!-- License: TODO – proposed CC BY-NC-SA 4.0, the same as Zcythe (shield and LICENCE file to be added) -->

# Zcythe Stone – nozzle reference sensor for 3D printers

The companion of [Zcythe](https://github.com/grzechot/Zcythe.3d-printer-bed-probe), the bed probe.
Zcythe measures the bed; **Zcythe Stone** measures the nozzle. It sits on the bed frame beside the bed, and the
printer presses the nozzle onto it. Together they let the printer find its own Z offset (`G31 Z`) before every
print – no paper, no feeler gauge, no babystepping after a nozzle change. The system that uses both is
**Zcythe Field** (macros and procedure, separate repository).

- Repeatability of the nozzle dives: ~0.002 mm (σ), two agreeing dives within 0.006 mm
- Rounded top (the head of an M3 screw) – the nozzle does not have to be centred exactly; position is found from the curvature
- Same principle as Zcythe: an opto endstop and a lever that multiplies the motion
- Printable (ABS)

Status: **working prototype** on one printer (400 × 400 CoreXY, RepRapFirmware 3.6.1). Photos, a video and assembly
instructions will follow; the dead-zone adjustment is going to be redesigned.

## How it works

An M3 screw with a rounded head sits on the short arm of a lever, the long arm carries a flag in the slot of an
opto endstop. The nozzle presses the screw head down; after a small travel the flag leaves the beam and the sensor switches. The Zcythe probe on
the toolhead is too light to trigger it – only the nozzle does.

Before every print the printer:

1. probes the bed next to Stone with Zcythe,
2. probes the top of the Stone screw head with Zcythe (5 dives – the apex position comes from the curvature),
3. presses the nozzle onto the same apex until Stone triggers.

From these three heights it computes the nozzle height at the moment Zcythe triggers. One constant – how far the
nozzle pushes the head before the switch (dead travel) – is calibrated once with a first-layer print.

## Files

- `stl/` – parts to print, `cad/` – STEP / CAD sources, `pic/` – photos.
- Videos: links here (YouTube), not stored in the repository.

## Parts

The same as [Zcythe](https://github.com/grzechot/Zcythe.3d-printer-bed-probe#parts), except the ball:

- printable: `stl/K1Flag.stl` and `stl/K1Support.stl`. Recommended material – ABS.
- optosensor 5 V (the same type as in Zcythe)
- 3x7x3 bearing, 2 pcs. Soak in grease before use (hot paraffin bath works).
- 5x5 mm brass heat insert with collar
- 0.5x5 mm magnet, 2 pcs
- **instead of the 3.2 mm ball: an M3 screw with a rounded head** – it is the measuring tip and the K1 height
  adjustment at the same time
- M3 bolts as in Zcythe (M3x6 for the optosensor, M3x10 for the pivot)

## Adjustments

- **Tip screw (M3, rounded head):** sets how high the tip sits relative to the bed. About 0.4–0.55 mm per turn,
  **loosening raises the tip**. Target: Stone triggers ~0.2 mm before Zcythe would touch the bed
  ("K1 margin" 0.2 ± 0.15 mm). If the nozzle triggers Stone *lower* than Zcythe touches the bed, the Zcythe
  needle gets pushed into the bed and latches closed.
- **Dead-zone screw:** moves the flag in the slot – changes the trigger point, not the rest height of the tip.
  Recommended 0.05–0.15 mm. Too far and the flag sits on the beam edge: the sensor switches by itself.

After any adjustment of the bed sheet or of Stone the calibration constants have to be measured again
(see Zcythe Field).

## Mounting

- Screw it to the **bed frame**, not to the bed sheet, outside the print area (author: beside the right edge,
  apex at about X425 Y322 in probe coordinates).
- Stone triggers only from a vertical push; sideways it reacts only to a push in −Y. Do not drag the nozzle over it.
- Do not press the tip far beyond the trigger point – repeated over-travel lowers the trigger height.

## Configure (RepRapFirmware)

Stone is a second, permanent digital probe:

```gcode
M558 K1 P8 C"e0stop" H3 F60 T6000 A1
G31 K1 P500 X0 Y0 Z0
```

The nozzle dives on Stone at 30 mm/min. The calibration macros (`HesCal2`) are in Zcythe Field.

## Test

Press the tip gently with a finger: the LED must change state and the controller must report the triggered state
(`sensors.probes[1].value`). Release: back to open. Repeat many times.
