# Ruler

A KiCad PCB ruler that doubles as a desk reference for SMD footprints, drill sizes, and trace widths—and includes a small ATtiny85 circuit to light five LEDs. Designed by [Philip McGaw](https://philipmcgaw.com).

The board is roughly **100 mm** long and combines measurement scales with populated example parts you can touch, compare, and (optionally) program.

## Features

### Measurement & reference

- **Metric** and **imperial** ruler scales (including 1/8″ and 1/10″ subdivisions)
- **Degree / protractor** scale for rough angle checks
- **Drill hole** reference sizes
- **Trace width** and **font size** guides (mm and mil)
- **SMD resistor** footprints from 0201 through 2512, with separate **hand-solder** and **machine-solder** pad variants
- **SMD LED** examples (0201, 0402, 0603, 0805, 1206)
- **Test points** labelled for common pad/hole diameters (1.0–2.5 mm)

### Electronics

| Item | Details |
|------|---------|
| MCU | ATtiny85V-10SU (SOIJ-8) |
| LEDs | Five, one per common SMD size (red 1206, green 0402, yellow 0603, orange 0805, blue 0201) |
| Power | CR2032 coin cell (Keystone 3002 holder) |
| Programming | 2×3 pin header (`J1`, labelled ICP6) for in-circuit programming |

The populated LEDs are driven by the ATtiny85. **Firmware is not included in this repository**—you will need to write and flash your own sketch (e.g. with [Arduino IDE + ATTinyCore](https://github.com/SpenceKonde/ATTinyCore) or `avrdude` via the ISP header).

> **Note:** This ruler is intended as a **reference and teaching aid**, not a calibrated measuring instrument.

## Repository layout

```
KiCAD/
├── ruler.kicad_pro      # KiCad project
├── ruler.kicad_sch      # Schematic
├── ruler.kicad_pcb      # Board layout
├── Common/              # Project symbols, footprints, design blocks
│   ├── Symbols.kicad_sym
│   ├── Footprints/      # Custom footprints (SquashedFly — https://squashedfly.eu/)
│   └── Design Blocks/
├── Libraries/           # Additional third-party symbol/footprint libraries
├── sym-lib-table
├── fp-lib-table
└── design-block-lib-table
```

Gerber output is configured to write to `KiCAD/Gerber/` when you plot from Pcbnew.

## Requirements

- **[KiCad](https://www.kicad.org/)** 9 or 10 (project files were last saved with KiCad 10)
- **PCBRulers** footprint library — the board uses footprints such as `PCBRulers:Metric_Ruler_96mm`, `PCBRulers:Inch_Ruler_3.5in_10`, `PCBRulers:Drills`, and `PCBRulers:Degree_Scale`. Install the [PCBruler](https://github.com/jbtronics/PCBruler) library (or equivalent) and add it to your KiCad footprint library path if footprints appear missing.
- Standard KiCad libraries for passives, connectors, batteries, and the ATtiny symbol (project rescue lib `ruler-rescue` is embedded in the schematic)

## Opening the project

1. Clone this repository.
2. Open `KiCAD/ruler.kicad_pro` in KiCad.
3. If prompted about missing libraries, confirm that `Common/` paths resolve (they use `${KIPRJMOD}`) and that **PCBRulers** is installed globally or added to `fp-lib-table`.

Project-local tables already point at bundled assets:

- Symbols: `Common/Symbols.kicad_sym`, Partsbox HTTP lib
- Footprints: `Common/Footprints/`

## Building the board

1. Run **Design Rules Check** (DRC) in Pcbnew and resolve any issues for your fab’s capabilities.
2. Generate manufacturing files: **File → Plot** (Gerbers) and **File → Fabrication Outputs → Drill Files**.
3. Order from your preferred PCB fab. For best readability of the silkscreen scales and contact details, **ENIG** finish and a dark soldermask are commonly used on similar rulers.

Populate the optional electronics if you want the LED demo to work; the ruler scales and footprint references are useful even unpopulated.

## Programming the ATtiny85

1. Connect an ISP programmer to `J1` (6-pin, 2.54 mm pitch, 2×3).
2. Power the board from the CR2032 cell or according to your programmer’s requirements.
3. Flash firmware with your toolchain of choice.

Pin mapping and LED connections are defined in `ruler.kicad_sch` / `ruler.kicad_pcb`.

## Credits

- **Design:** Philip McGaw — [philipmcgaw.com](https://philipmcgaw.com) · [GitHub](https://github.com/PhilipMcGaw)
- **[SquashedFly](https://squashedfly.eu/)** — title block worksheet (`KiCAD/Common/Squashed Fly.kicad_wks`) and optional PCB logo footprints (`SquashedFlySilk`, `SquashedFlyCopper`, etc.) in `KiCAD/Common/Footprints/`
- Ruler scale footprints from the **PCBRulers** / [PCBruler](https://github.com/jbtronics/PCBruler) ecosystem

## License

Hardware design files in this repository are licensed under the **[CERN Open Hardware Licence v2 — Strongly Reciprocal (CERN-OHL-S-2.0)](LICENSE)**, an open source hardware license recommended by the [Open Source Hardware Association (OSHWA)](https://oshwa.org/).

- **SPDX identifier:** `CERN-OHL-S-2.0`
- **Copyright:** [Philip McGaw](https://philipmcgaw.com/)
- **Full text:** [LICENSE](LICENSE)

You may use, study, modify, and redistribute the schematic and PCB layout under those terms. **Derivative designs must be released under the same license.** If you share the design or boards built from it, include a copy of the license.
