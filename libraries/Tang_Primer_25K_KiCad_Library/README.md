# Tang Primer 25K - KiCad library

Ready-to-use KiCad 8 library for the Sipeed Tang Primer 25K core module.

## Contents

- `Tang_Primer_25K.kicad_sym` - one 120-pin module symbol.
- `Tang_Primer_25K.pretty/Tang_Primer_25K_Module_2xDF40C-60DS.kicad_mod` - carrier-side footprint containing both DF40C-60DS-0.4V(51) connectors.
- `pinout.csv` - auditable J1/J2 pin table.

## Install

1. Extract this directory into your KiCad project, for example `lib/Tang_Primer_25K/`.
2. In **Preferences -> Manage Symbol Libraries -> Project Specific Libraries**, add `Tang_Primer_25K.kicad_sym` with nickname `Tang_Primer_25K`.
3. In **Preferences -> Manage Footprint Libraries -> Project Specific Libraries**, add the `Tang_Primer_25K.pretty` directory with the same nickname `Tang_Primer_25K`.
4. Place `Tang_Primer_25K:Tang_Primer_25K_Module`; its default footprint is already linked.

## Pin numbering and naming

KiCad pin/pad numbers are namespaced as `J1.1`...`J1.60` and `J2.1`...`J2.60`. This prevents the two physical 1...60 number ranges from colliding inside one module component. Pin names retain the FPGA package location and bank from Sipeed's schematics. Examples: `L9_IOT31A_B0`, `C1_JTAG_TCK_B10`, and `M0_D3_P_MIPI`.

The library intentionally does not assign project-specific functions such as `DAC_D0`, `SPI_CS`, or `SYNC`; those are net labels chosen by the carrier-board designer, not facts in Sipeed's source documents.

## Mechanical basis

The footprint is flattened from Sipeed's official KiCad reference project `Tang_Primer_25K_Footprint.kicad_pcb`:

- module outline: 23 x 18 mm (Sipeed product documentation);
- carrier connectors: 2 x Hirose `DF40C-60DS-0.4V(51)`;
- J1 and J2 have the same 180-degree rotation in the official project;
- connector center-to-center spacing: 13.97 mm;
- footprint origin: midpoint between connector centers;
- J1 center: Y = +6.985 mm; J2 center: Y = -6.985 mm.

## Mandatory orientation check before fabrication

Do not release a PCB using only the screen preview. Print the footprint 1:1 or order a low-cost connector-only coupon and place the real Tang Primer 25K module over it.

Verify all of the following against the physical module and Sipeed drawings:

1. J1 is on the `+Y` side and J2 on the `-Y` side when viewed from the carrier PCB top.
2. `J1.1`, `J1.2`, `J1.59`, `J1.60` and the corresponding J2 corner pads match the module's mating contacts.
3. The module outline and all component keepouts fit your chosen DF40 board-to-board stack height.
4. The exact Hirose receptacle variant and mating height are suitable. Sipeed names `DF40C-60DS-0.4V(51)` on the Dock schematic.

## Electrical cautions

- `J2.52/54/56/58/60` are `VDD_5V` inputs to the module. They are not GPIO.
- `J1.24/26`, `J1.29/31`, and `J2.2/4` are VCCIO supply pins. The Dock schematic ties the relevant banks to 3.3 V, but a custom carrier must deliberately implement and verify its own bank-voltage plan.
- `J2.38/40`, `J2.42/44`, and `J2.46/48/50` are module-generated 1.8 V, 2.5 V, and 3.3 V rails as shown in the Sipeed source. Do not back-drive them.
- MIPI pins are retained exactly as documented; no generic GPIO capability is inferred.
- `READY`, `DONE`, `RECONFIG`, and JTAG pins retain their documented configuration roles. Reuse only after checking the Gowin device documentation and your boot scheme.

## Sources and verification scope

Primary inputs:

- Sipeed `Tang_Primer_25K_52300_Schematic.pdf` (core module schematic).
- Sipeed `Tang_Primer_25K_Dock_60033_Schematic.pdf` (Dock schematic and J1/J2 mating view).
- Sipeed official KiCad reference project `Tang_Primer_25K_Footprint.kicad_pcb` from the Primer 25K PCB library download area.
- Sipeed Primer 25K wiki: https://wiki.sipeed.com/hardware/en/tang/tang-primer-25k/primer-25k.html

The electrical pin table was compared between the core-board and Dock schematics. Mechanical connector position and rotation come from Sipeed's official KiCad PCB, not from visual estimation of the PDFs. This library does not claim a verified 3D model or component-side height envelope.
