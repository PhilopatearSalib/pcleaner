# PCleaner — Autonomous Pool-Surface Cleaning Robot

A small floating robot that skims leaves, insects and other debris off the surface of a swimming pool on its own. Designed, built and tested as the course project for **MCT 382: Design of Mechatronics (2)** at Ain Shams University, Spring 2019.

![PCleaner CAD render](CAD%20-%20PCleaner%20V5/PCleanerV3/render.png)

**Demo:** [Moving water with bug for presentation.mp4](Moving%20water%20with%20bug%20for%20presentation.mp4)

---

## The problem

Owners of private villa pools want the water surface clean every day, without hiring someone to come in and do it. PCleaner was designed for a medium-sized rectangular pool: plug it in, put it in the water, press one button, and it covers the surface lane by lane, then returns to where it started.

## How it works

- **Propulsion.** Two thrusters built from 63 L/min bilge pumps, with the impellers removed and small boat propellers fitted. Off-the-shelf thrusters were too large and too expensive for a robot this size.
- **Collection.** A modified aquarium pump draws surface water through a filter that traps the debris.
- **Navigation.** The pool's length and width are set in firmware. The robot works out how many lanes it needs, uses an ultrasonic sensor to detect the end of each lane, and turns using yaw feedback from an MPU-6050 IMU (Kalman-filtered).
- **Power and control.** The robot is tethered to a waterproof control box at the poolside, which keeps the floating hull light (no batteries). The box holds the AC-DC converter, a buck converter for the 12 V and 5 V rails, a custom standalone ATmega328 board, H-bridge motor drivers, and a solid-state relay with optocoupler isolation that switches the 220 V filter pump.
- **Safety.** Fuses, sealed in-water components, a water sensor that prevents starting out of the water, and a leak sensor inside the control box.

![Autonomous operation flowchart](Flow%20Chart%20-%20Autonomous.png)

## Design highlights

| | |
|---|---|
| Size / mass | approx. 27 × 18.5 cm, approx. 1 kg |
| Buoyancy margin | floats with up to 3.54 kg of load |
| Target speed | 0.2 m/s |
| Drag at target speed | 0.32 N |
| Available thrust | approx. 0.46 N |
| Required total flow | approx. 111 L/min |
| Power budget | approx. 103 W (78 W load + 25 W losses) |
| Yaw controller | PID, Ziegler–Nichols tuned (Kp 10.15, Ki 20.19, Kd 0.446) |

- **Hull shape** is pointed at the front and square at the back to reduce drag in its single direction of travel, with draft chosen so the filter pump stays submerged and the cabling stays dry.
- **Manufacturing.** Most parts are 3D-printed in PLA, many on a printer the team built, which cut printing cost by about 25%. Parts larger than the available print beds were split and joined with male–female features.
- **Maintainability.** Motors sit in cylindrical housings held by studs, so they can be removed without cutting anything.

## Process

The project followed a design-thinking front end (customer survey, value proposition canvas, lean canvas) and the **VDI 2206** V-model for mechatronic development, over a 14-week plan. Two early features, a water-temperature sensor and a chlorine dispenser, were dropped after costing.

## Testing and lessons learned

In validation, PCleaner cleared about **2 m² of surface in under 30 seconds**.

Problems found during integration and how they were solved:

- **Leaking bolt holes** let water into the hull → sealed with silicone.
- **Fastener corrosion** → coated with clear nail polish.
- **Driver overload.** One 4 A driver could not run both thrusters (approx. 6.5 A combined) → the load was split across two dual-channel drivers instead of buying a 30 A unit.

## Cost

Total bill of materials: **3,034 EGP**. The thrusters were the largest item at 900 EGP, even after saving about 800 EGP by buying used bilge pumps. Full breakdown: [`Bill of Materials and Cost.docx`](Bill%20of%20Materials%20and%20Cost.docx), power budget: [`Power Sizing.xlsx`](Power%20Sizing.xlsx).

## Repository contents

| Path | Contents |
|---|---|
| `Final_PCleaner_Report.docx` | Full design report, including calculations and the firmware listing (appendix) |
| `Final presentation.pptx`, `After Midterm Presenattion.pptx`, `A0 poster.pptx` | Presentations and poster |
| `Market Research Survey.pptx`, `the-value-proposition-canvas.jpg`, `Tool_24_leancanvas.jpg` | Customer research and business canvases |
| `brochure.pptx`, `Brochure(1).pptx` | Product brochure drafts |
| `CAD - PCleaner V4/`, `CAD - PCleaner V5/`, `CAD - PCleaner V5 (rev b)/` | Autodesk Inventor design iterations, with FEA results |
| `3D cut parts/` | Print-ready STL files of the split parts |
| `arduino/`, `AC to DC Converter/`, `water leakage sensor/`, `Motherboard.pdsprj`, `Children.pdsprj` | Proteus circuit designs |
| `Mother SID Sketch.jpg`, `Mother BlackBox.jpg`, `MotherBoard (1).png` | System interconnection diagram and board layout |
| `ATMega328.pdf`, `SSR40DA.pdf`, `USB to TTL.pdf` | Component datasheets |

The bilge pump CAD model (`Seaflo_BilgePump1100GPH_CAD_Model`) and the water-level sensor model (`Capteur Pluie2 v3`) are third-party reference models used in the assembly; they were not designed by the team.

## Known limitations

These files are published as submitted in May 2019 (tag `v2019-original`). Reviewing them now, a few things don't hold up:

- **Firmware.** The listing in the report's appendix doesn't fully match the design described: the PID yaw loop is marked by a comment rather than implemented, the start button is hard-coded as pressed, the distance function doesn't return its reading, two thruster conditions use a comma where a logical AND was intended, and the code turns 180° per lane end while the flowchart uses two 90° turns.
- **Report errata.** The required flow rate is printed as 1.85 m³/s; it should be 1.85 × 10⁻³ m³/s (approx. 111 L/min). Kd appears as +0.446 in one section and −0.446 in another. Thruster current figures (2 A, 3.2 A, approx. 6.5 A for both) and wire gauge (24 vs 26 AWG) are inconsistent between the report and the BOM.
- **Scope of testing.** Validation covered a section of pool surface, not a full-pool run.

## Team

PCleaner was designed and built jointly by **Philopatear Salib**, **Ahmed Amr** and **Nourhan Ayman**, with the research, mechanical, electrical, firmware and testing work shared across the team.

## License

No license is granted. All rights reserved.
