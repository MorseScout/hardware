# Morse Scout custom I/O PCB v1.2

This directory contains the current custom I/O board for Morse Scout. It is
designed for the Elecrow CrowPanel Advanced 7-inch ESP32-P4 host board (PCB
V1.1). PCB revision 1.2 was fabricated from the included Gerber package,
passed bring-up and functional testing, and is used in the current Morse Scout
build.

## Current design

- `J1`: 2x12, 2.54 mm, pin-for-pin connection to Elecrow expansion header
  `J7`. Pins 2, 4, and 6 carry 5 V on the host and are intentionally not
  connected on the custom PCB.
- `J5`-`J7`: three switched TRS Morse-key inputs. Tip and ring are independent
  active-low contacts to ground.
- `ENC1`: rotary encoder A/B signals and push switch.
- `RV1`: 10 kOhm linear volume potentiometer. `R1/C1` form the 1 kOhm/100 nF
  ADC filter.
- `SW1`: active-low sleep/wake button with the 10 kOhm `R2` pull-up.
- `U1`: DS3231 real-time-clock module with battery backup.
- `J8`: four-position HY2.0 I2C connector (`ZX-HY2.0-4PZZ`) between
  the Elecrow board and `U1`.
- `J4`: two-wire Elecrow left-speaker input. Both conductors belong to the
  bridge-tied speaker output; neither conductor is ground.
- `J2`: switched 3.5 mm audio jack.
- `J3`: two-wire internal-speaker connector. Inserting a plug in `J2` opens
  the feed to the internal speaker.

## Opening the project

Use KiCad 10 and open `morse_scout_custom_io_elecrow.kicad_pro`. The project
library tables use `${KIPRJMOD}`, so the included `Libraries` directory is
found without machine-specific paths. A normal KiCad 10 installation is also
required for the standard KiCad symbols, footprints, and 3D models.

The project-local `MorseScout` symbol and footprint libraries contain the
custom parts required to edit and fabricate the board. Their pad and drill
geometry is based on the tested PCB v1.2 design.

## Optional third-party 3D models

This repository does **not** redistribute third-party SamacSys/Component
Search Engine symbols, footprints, or 3D models. The custom symbols and
fabrication-tested footprints needed to edit the project are included. The
footprints contain optional 3D-model references through the
`MORSESCOUT_3DMODEL_DIR` KiCad path variable, but the referenced model files
are intentionally omitted from this public repository.

To enable a fully populated local 3D view, create a directory for the
downloaded models and add this entry in **KiCad > Preferences > Configure
Paths**:

| Name | Value |
| --- | --- |
| `MORSESCOUT_3DMODEL_DIR` | Absolute path to your local model directory |

Use the filenames shown below. If the variable or a model is absent, KiCad may
report a missing model and omit that component from the 3D Viewer. This does
not affect pads, drills, copper, routing, DRC geometry, Gerbers, or other
fabrication data.

Optional vendor CAD data can be obtained from
[SamacSys Component Search Engine](https://componentsearchengine.com/). Search
for the exact manufacturer part number:

| Part | Expected filename | CAD-model search |
| --- | --- | --- |
| Kycon `STX-3120-5B-701C` audio jack | `STX-3120-5B-701C.stp` | [Search](https://componentsearchengine.com/search?term=STX-3120-5B-701C) |
| Bourns `PTV09A-4030F-B103` potentiometer | `PTV09A-4030F-B103.stp` | [Search](https://componentsearchengine.com/search?term=PTV09A-4030F-B103) |
| Omron `B3F-4050` pushbutton | `B3F-4050.stp` | [Search](https://componentsearchengine.com/search?term=B3F-4050) |
| Bourns `PEC11R-4230F-S0024` encoder | `PEC11R-4230F-S0024.stp` | [Search](https://componentsearchengine.com/search?term=PEC11R-4230F-S0024) |
| DS3231 RTC module | `DS3231-RTC-V3.step` | Based on the [DS3231 RTC Module model on GrabCAD](https://grabcad.com/library/ds3231-rtc-module-1) |
| Megastar `ZX-HY2.0-4PZZ` four-position HY2.0 connector | `CONN-TH_4P-P2.00_HX20020-4A_2.step` | [LCSC C7429580](https://www.lcsc.com/product-detail/C7429580.html) |

Availability differs by part: the service may offer a direct download or a
model-request workflow, and an account may be required. Review and accept the
provider's current terms before downloading. Keep downloaded files outside
this repository unless their license expressly permits redistribution.

Do not replace the included footprints merely to obtain a 3D model: the
included pad and drill geometry is the fabrication-tested v1.2 geometry. If an
external footprint is substituted, verify its pad numbers, hole sizes,
spacing, and mechanical clearances before manufacturing.

The fitted encoder is `PEC11R-4230F-S0024`. Its PCB mounting geometry is the
same as the shorter-shaft `PEC11R-4220F-S0024`; the project and sourcing links
use the fitted 4230F manufacturer part number.

## Release files

- KiCad source: `morse_scout_custom_io_elecrow.kicad_sch`,
  `morse_scout_custom_io_elecrow.kicad_pcb`, and
  `morse_scout_custom_io_elecrow.kicad_pro`.
- Tested fabrication package:
  [`Gerbers/v1.2/morse_tutor_custom_io_elecrow-v1.2.zip`](Gerbers/v1.2/morse_tutor_custom_io_elecrow-v1.2.zip).
  The archive filename is retained unchanged because this is the exact package
  used to fabricate and validate PCB v1.2.

Manufacturer part numbers and component values are stored in the schematic
symbol properties. Use the included v1.2 fabrication package when ordering
this tested board revision.

## Validation

Native KiCad 10 validation on 2026-10-28:

- Schematic ERC: 0 errors and 0 warnings.
- PCB routing: complete, with 0 unrouted nets.
- PCB DRC: 20 accepted warnings and 0 unconnected pads. All 20 warnings are
  expected because the four 3.5 mm jack footprints deliberately overhang the
  board edge.
- A PCB fabricated from the v1.2 Gerbers passed the current Morse Scout
  bring-up and functional tests.
