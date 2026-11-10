# CAN interface v2 design notes

## Schematic

The v2 schematic contains two PEAK D-sub channels, three 2-pin Nano-Fit headers and two WAGO 250-102 terminals per channel, two switched 120 Ω terminations, a two-pole channel link, a two-pole CANable channel selector, and a seventh Nano-Fit header for the module. Nano-Fit and WAGO pin 1 is CAN-H; pin 2 is CAN-L. D-sub pin 7 is CAN-H, pin 2 CAN-L, pin 3 CAN ground; pin 1 feeds that channel’s power indicator; pins 4, 5, 6, 8 and 9 are unconnected. Pin 6 is CAN ground on PCAN-USB FD but LIN ground on PCAN-USB Pro FD; do not tie it to pin 3.

The user explicitly requested permanently shared CAN ground: PEAK D-sub pin 3 on both channels and TP1/TP2 share CAN_GND. This removes channel-to-channel ground isolation and permits ground-loop or equalization currents. The two-pin CAN connectors do not provide a ground conductor; arrange a suitable bus reference externally. PEAK pinouts: https://www.peak-system.com/products/hardware/external-pc-interfaces/pcan-usb-pro-fd/ and https://www.peak-system.com/produktcd/Pdf/English/PCAN-USB-FD_UserMan_eng.pdf

SW1/SW2 are Würth 450406020024 SPDT termination switches. JP1/JP2 are parallel bypass headers: a fitted shunt enables the channel 120 Ω resistor regardless of the switch position; remove the shunt to control termination with the switch. SW3 links both lines simultaneously; SW4 selects both module lines together. The C&K JS202011CQN used for SW3/SW4 has physical switch positions 2–1 with 5–6, and 2–3 with 5–4. The two generic schematic units are drawn independently, so use these physical pairings rather than inferring the simultaneous state from the drawn contacts. Datasheet: https://www.mouser.com/datasheet/2/60/js-1141209.pdf. Switch only when buses are idle and enable 120 Ω termination only at a physical end of a bus.

## PEAK power indicators

D1/R3 and D2/R4 indicate the presence of each adapter’s optional pin-1 5 V supply, not CAN traffic or mechanical connection. PEAK ships pin 1 disabled; the relevant solder bridge must be enabled on the PCAN-USB FD or on each desired Pro FD channel. Each indicator is connected between its own pin 1 and the shared CAN_GND connected to both pin-3 terminals. Lite-On LTST-C193KRKT-2A red 0603 LEDs use the same LED footprint as BPS-Leader. Yageo RC0603FR-071K5L, 1.5 kΩ 1% 0603 resistors use its resistor footprint. With 5 V and nominal 1.9 V LED drop, current is about 2.1 mA and resistor dissipation about 6.4 mW. Brightness and rail tolerance remain to be checked on actual hardware. LED data: https://media.digikey.com/pdf/Data%20Sheets/Lite-On%20PDFs/LTST-C193KRKT-2A.pdf

## Parts

- Nano-Fit: Molex 105309-1102, Mouser 538-105309-1102. https://www.mouser.com/en/ProductDetail/Molex/105309-1102?qs=5aG0NVq1C4zzTOxEjLJqtg%3D%3D
- WAGO 250-102, four total. https://www.mouser.com/ProductDetail/WAGO/250-102
- Channel link and selector switches: C&K JS202011CQN, two total. https://www.mouser.com/en/ProductDetail/CK/JS202011CQN?qs=LgMIjt8LuD%2Fe%252BE3iTcEFYw%3D%3D
- Termination resistors: Yageo RC0603FR-07120RL, 120 Ω, 1%, 0.1 W. https://www.mouser.com/en/ProductDetail/YAGEO/RC0603FR-07120RL?qs=uM1JSSdPjOCKJQFXEGGb5w%3D%3D
- PEAK connector: Amphenol L77SDE09SA4CH3R, existing project footprint. Confirm ordering and connector mechanical fit before manufacture.

Mouser search listings showed stock for the Nano-Fit, WAGO, C&K switch and Yageo resistor during review; stock and pricing change. Switch chosen for a verified KiCad footprint and low cost; this is not an exhaustive price optimization.

- Termination switches: Würth 450406020024, two total, 2 common, 1 connects termination, 3 unused. The Würth KiCad library symbol (rev26b) incorrectly assigns common to pin 1 versus the manufacturer datasheet; the schematic uses the standard SPDT symbol, with common pin 2, and the manufacturer footprint. Datasheet: https://www.we-online.com/components/products/datasheet/450406020024.pdf
- Two-pin through-hole headers (CAN1, CAN2, JP1, JP2): Samtec TSW-102-07-G-S. Selector header J16: Samtec TSW-103-07-G-D, 2.54 mm pitch.

## CANable selector shunts

J16 is a 2×3 header, with pins 3 and 4 (CAN-H and CAN-L) forming the center row. Bridge 1–3 and 2–4 for channel 1, or 3–5 and 4–6 for channel 2. Rotate the footprint during PCB placement to present these rows as left, center, and right columns. J16 is electrically parallel to the on-on SW4; if shunts are fitted they MUST select the same channel as SW4 or the mismatched shunts will join both CAN buses. Remove both shunts to use SW4 alone. Use two narrow 2.54 mm shunts such as Samtec SNT-100-BK-G (order separately); JP1/JP2 may each use one more when desired. Fit clearance of the adjacent shunts should be checked on actual hardware. Move both shunts together with buses inactive.

## Layout remains unfinished

The PCB file is still v1 and is not synchronized to the new schematic. Do not fabricate it. J8–J11 now use a project-local WAGO 250-102 footprint converted from EasyEDA C3309175, with its original 1.3 mm drills corrected to the WAGO datasheet 1.1 mm +0.1 mm recommendation, and courtyard expanded. Pads are 3.5 mm apart; body envelope is 8.5 × 12 mm, with the pins 7.3 mm from one edge. The 3D model and full mechanical fit remain to be checked against physical hardware. Source: https://www.wago.com/us/pcb-interconnect/1-conductor-pcb-terminal-strip/p/250-102

The user confirmed the module envelope 45 × 16 × 10.5 mm, four 2.2 mm holes 12.5 mm across, with rows 9 mm and 43.5 mm from the USB-C end (34.5 mm between centers). Verify against the actual module. PEAK PCAN-USB Pro FD D-sub connector center spacing is 38.5 mm: https://www.peak-system.com/produktcd/Pdf/English/PCAN-USB-Pro-FD_UserMan_eng.pdf. PCB placement, connector engagement, mounting clearances, switching access and DRC remain to be done and checked against actual hardware.

## Indicator check, 2026-11-10

KiCad 10.0.6 ERC: 0 errors and 0 warnings. The exported KiCad netlist connects J12.1–R3.1, R3.2–D1.2, D1.1–J12.3/TP1, and the equivalent J13/R4/D2/TP2 path for channel 2. Original non-indicator nets were unchanged. D1/D2 pad 1 is cathode, matching the selected standard KiCad LED footprint convention; the Lite-On package drawing identifies its cathode marking.

The independent schematic analyzer reports two LR-001 missing-series-resistor errors for D1/D2 despite the KiCad-exported netlist above; these are analyzer false positives to investigate if its parser is updated. It also reports missing local datasheet coverage. The wiring guard flags the existing remote CAN-H/CAN-L fan-out labels and the three-point per-channel ground labels; these are same-channel distributed nets, and the two LED-to-resistor connections are direct wires. SPICE and anti-aliasing simulation were skipped: no simulator is installed; no analog filter is part of this change. Visual inspection and nominal-current arithmetic do not replace physical testing.

## Header and termination update

SW1/SW2 use the shared KiCAD-Sharepoint footprint LHRS_Special:BootSW, which matches Würth 450406020024's datasheet pad numbering and layout and includes an embedded 3D model. The separately reviewed Würth KiCad library uses the same pad geometry rotated 90 degrees. The datasheet gives a 6 V DC switching rating; suitability under the actual CAN-bus fault and transient conditions remains to be assessed. The manufacturer's own symbol in that bundle places COM on pin 1, contradicting its datasheet; this project uses the standard SPDT symbol with the datasheet's pin 2 COM.

KiCad ERC: 0 errors, 0 warnings. The exported netlist confirms each switch and shunt both bridge the same CAN-L-to-termination-resistor nodes, and that J14/J15 expose CAN-H on pin 1 and CAN-L on pin 2. J16 pin pairs and selection instructions are as above. The independent analyzer still reports the two previously documented LR-001 false positives for the LED resistors, plus a grounding warning for J16: J16 is a local signal-selection jumper, not an external interface, and intentionally carries the six specified signal pins rather than a separate ground. Datasheet coverage and SPICE limitations remain as previously noted. The PCB has not been updated.

## Layout intent, 2026-11-10

User requested shared CAN ground permanently and maximum 1 Mbit/s. Keep each channel pair compact; place the two PEAK DB9 connectors at 38.5 mm pitch, Nano-Fit latches and connector access toward the board edge, switches accessible near the edge, and pin headers behind connectors. Treat external cables and connector transitions as part of the stub: minimize on-board branches but do not treat a passive multiport star as signal-conditioned or guaranteed compatible with arbitrary external stub cables. Use a common ground plane referenced to CAN_GND if feasible; avoid split planes under a differential pair. Keep the board and termination rules based on actual topology, including at most two end terminations per bus and none at a mid-bus stub.

WAGO source: EasyEDA/JLC C3309175 candidate, converted with easyeda2kicad 1.0.1; pin pitch, body envelope and 1.1 mm drill checked against WAGO 250-102 product drawing and datasheet. The provider-generated 3D model remains provisional.

## 2026-11-10 placement draft (not ready for manufacture)

`CANInterfacePCB-v2-layout-draft.kicad_pcb` is a separate preliminary placement, not the original v1 board. It contains 34 schematic footprints, an approximately 92 x 80 mm exploratory outline, and a drawing-layer 45 x 16 mm CANable envelope using the supplied mounting-hole locations. The two PEAK DB9 footprint origins are 38.5 mm apart and face the same way. The outline, actual mating fit, module clearance, latch access and mounting hardware remain to be checked against real parts. The Nano-Fit connectors are top-entry; latch access has not been physically verified. The D-sub footprint courtyards overhang the tentative board edge by about 2.1 mm and need mechanical verification. The draft is not routed and has no ground copper plane yet. Confirm schematic-to-PCB updating in KiCad before using it as the project board; the draft's schematic paths were populated from the exported netlist but full round-trip update has not been tested.

The first mechanical pass reports no courtyard overlaps, with the above D-sub overhang and four mounting holes whose footprints lack courtyards. Connector-access checking recognizes the headers as top-entry, but it cannot prove hand clearance or switch access. The draft DRC report is `V2-layout-draft-drc.txt`: 59 unrouted connections and 28 hole-clearance errors involving the locating holes and plated pins within the seven Nano-Fit footprints. Those manufacturing constraints must be resolved against Molex's recommended land pattern and the intended fabricator's drill tolerances, not suppressed without evidence. DRC used the draft board's newly generated default project rules, which have not been matched to a specific fabricator.

The WAGO 250-102 footprint is a corrected third-party derivative: 3.5 mm pin pitch, body outline and 1.1 mm nominal drill were compared to WAGO's drawing; actual fit and 3D model alignment still require verification. The project-local C&K switch footprint is geometrically identical to KiCad's standard JS202011CQN footprint; its explicit clockwise-numbering name documents the manufacturer's intentional numbering and avoids a generic chirality check's false positive. Both units of each schematic switch have the same footprint assignment. Schematic ERC reports zero violations after the common-ground and footprint updates.

The selection switch and parallel jumper header can bridge CH1 and CH2 if the shunts disagree with the switch position: fit shunts only to the channel selected by the switch. A termination shunt enables its 120-ohm termination regardless of the corresponding switch position. Select termination only at the two ends of the complete CAN bus. The two-channel ground tie intentionally removes isolation between channel grounds; the two-wire external ports still require a suitable shared reference elsewhere.

## 2026-11-10 narrower placement revision

`CANInterfacePCB-v2-narrow.kicad_pcb` is the newer, still-unrouted placement draft. Its outline is 74 x 80 mm, reduced from 92 x 80 mm, while the PEAK connector origins remain 38.5 mm apart. WAGO terminals have moved to the sides, headers are inside, and termination switches are near the side edges. J1-J7 were rotated so the extended latch side of the vertical Nano-Fit footprint faces the upper board edge; verify the actual mating harness and latch access. The CANable envelope and tentative USB access corridor remain unverified against hardware. The two DB9 courtyards intentionally overhang the bottom outline and remain unverified mechanically. This placement remains provisional and is not an optimized or routing-ready board.

A preliminary `/CAN_GND` zone was added and filled on B.Cu, with 0.3 mm clearance and thermal pad connections. Its continuity and return paths require review after routing and refilling. The schematic-association/round-trip caution from the earlier draft still applies. PCB DRC `V2-narrow-drc.txt` reports 28 drill-to-drill clearance errors and 58 unconnected items; the latter are expected because no tracks have been routed. The default 0.25 mm drill clearance is not confirmed against a fabricator; the Molex 105309 family drawing SD-105309-100 revision H1, sheet 1, specifies 1.34 mm locator offset, 1.25 mm lateral offset, 1.20 mm contact holes, and 1.30 mm locator hole, producing a close hole web. Confirm the complete footprint and the chosen fabricator's capabilities before changing rules or ordering. Source: https://www.molex.com/content/dam/molex/molex-dot-com/products/automated/en-us/salesdrawingpdf/105/105309/1053091106_sd.pdf

## 2026-11-10 centered and labeled revision

`CANInterfacePCB-v2-labeled.kicad_pcb` is a new revision derived from the saved narrow draft; the prior draft was open in KiCad and was not overwritten. The board is 74 x 80 mm with 2 mm-radius rounded corners, centered at (148.5, 105) mm on the A4 drawing sheet. The DB9 connector origins are centered on the same X coordinate and remain 38.5 mm apart; the J1–J7 row is also centered. F.Silkscreen groups CH1, CH2, and the CANable connector; the terminal connectors have matching channel brackets. H/L markings were placed using the actual board pad nets, including the selection header and the DB9 pin-number legend. The B.Cu CAN_GND zone was refilled after the movements. Physical readability, plug fit, board edge clearances, and module interference remain unverified. DRC `V2-labeled-drc.txt` reports the same 28 Nano-Fit hole-clearance errors and 58 unconnected items; no new silkscreen violations were reported. The board remains an unrouted, non-fabrication draft.

## 2026-11-10 project PCB promotion and synchronization

The centered v2 layout is now the project board `CANInterfacePCB.kicad_pcb`; the previous v1 board is preserved at `backups/CANInterfacePCB-v1-before-v2.kicad_pcb`. KiCad's project-managed Update PCB from Schematic was applied and then run a second time for confirmation: no further actions or errors were reported. Four warnings remain because DB9 mounting-shell pads S1 and S2 have no corresponding schematic pins; they remain electrically unassigned. The board retains 34 footprints, 38.5 mm DB9 center spacing, shared `/CAN_GND`, one ground zone and no routed tracks. The fabrication and DRC caveats above still apply.

## 2026-11-10 shared Nano-Fit labels, J16 DNP, WAGO orientation

- J1–J7 use the shared `UTSVT_Connectors:Molex_Nano-Fit_105309-xx02_1x02_P2.50mm_Vertical` footprint (embedded STEP). Its pad numbering is mirrored relative to the earlier footprint, so the 14 H/L silkscreen labels above the row were swapped to match: pad 1 (square) = CAN-H, pad 2 = CAN-L.
- J16 (CANable select 2x3 header) is DNP in the schematic and on the board. The footprint and nets remain; it is simply not populated. SW4 is the populated CANable selector.
- J8–J11 (WAGO 250-102) rotated 180° about their courtyard centers so the wire-entry openings face the board edge; courtyard envelopes are unchanged. Pads shift 1.5 mm in Y because the footprint is asymmetric about its pads; the adjacent H/L labels were moved to the new pad rows.
- SW3/SW4 reference text moved beside the switches to clear the Nano-Fit outlines and CH1/CH2 group lines.
- Verification: ERC 0; DRC with `--refill-zones --schematic-parity`: 0 silk, 0 clearance, 0 parity. Still open: 28 Nano-Fit hole_clearance and 58 unconnected (unrouted). Backups in `backups/` (`*-before-J16-dnp`, `*-before-silk-dnp`, `*-before-wago-rotate`).

## 2026-11-10 J16 removed, headers relocated, termination labels

- J16 deleted from the board by the user; in the schematic it is now `on_board no` (and DNP) so Update PCB will not re-add it. Its orphaned H/L silkscreen labels were removed.
- J14 (CH1 header) and J15 (CH2 header) placed below the CANable envelope, between the bottom WAGOs J9 and J11, pins stacked vertically and centered on the bottom WAGO row (pin 1 / CAN-H at y 109.48, CAN-L at y 112.02; J14 x 132.0, J15 x 165.0). H/L labels sit on the outboard side of each header. (They briefly sat in the side gaps between the WAGO pairs; that space is not usable.)
- "TERMINATION" silkscreen added under JP1 and JP2.
- Verification: ERC 0; DRC with `--refill-zones --schematic-parity`: 0 silk, 0 clearance, 0 parity. Still open: 28 Nano-Fit hole_clearance and 52 unconnected (unrouted). Backups: `backups/CANInterfacePCB-before-J14J15-move.kicad_pcb`, `backups/CANInterfacePCB-before-J14J15-move2.kicad_pcb`.

## 2026-11-10 routing (first complete pass)

Layer plan (2-layer): F.Cu = signal layer plus a CAN_GND pour; B.Cu = CAN_GND plane, with signals allowed only as short jumps where one net must cross another. 71 stitching vias on a 7 mm grid tie the two CAN_GND pours together. Tracks are 0.3 mm; vias are 0.6/0.3 mm. CAN-H/L are routed as pairs (the router favours running L about 0.6 mm from H); at 1 Mbit/s every on-board branch is far below any stub-length concern, so the priority was short, paired runs and an intact return plane.

Placement change: SW3 was rotated 180° in place so its CH1 column (middle) and CH2 column (right) face their own channels, which removes a CH1/CH2 crossing. Nothing else moved.

Method: copper was generated by a project-local 2-layer grid router (0.125 mm grid, clearance target 0.25 mm plus half-cell margin, F.Cu preferred, vias allowed), with no copper above the Nano-Fit pad centerline. Net order: CH1 H/L, CH2 H/L, module H/L, terminations, LED/5V. Result: 257 segments (689 mm), 20 signal vias (6 per L net, mostly one per Nano-Fit tap), all on 45° geometry.

Verification: ERC 0; DRC with `--refill-zones --schematic-parity`: 0 unconnected, 0 clearance, 0 parity; copper-collision audit 0; route-shape report recorded (not graded). Still open: 28 Nano-Fit hole_clearance (fabricator capability check), and a hand tidy-up pass in KiCad is recommended (the CH2 right-side H/L run about 2.5 mm apart past the WAGOs). Track and via sizes have not been checked against a fabricator. Backup: `backups/CANInterfacePCB-before-routing.kicad_pcb`.
