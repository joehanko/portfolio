---
title: Tensile Tester
category: Mechanical Design / Mechatronics
year: "2026"
summary: A compact electromechanical tensile tester with stepper motion, load-cell force measurement, and a custom controller PCB.
cover: /images/projects/mack-10-controller-enclosure.jpg
coverAlt: MACK-10 controller installed in its enclosure beside the red emergency stop button.
coverCaption: The assembled MACK-10 controller installed in its enclosure.
highlights:
  - value: "~50%"
    label: Smaller controller PCB footprint from V1 to V2
  - value: Stepper-driven
    label: Linear motion control
  - value: Load cell
    label: Force measurement
  - value: Custom PCB
    label: Power, motion, sensing, and expandable I/O
gallery:
  - /images/projects/mack-10-pcb-revisions.jpg
  - /images/projects/mack-10-v2-assembly.jpg
  - /images/projects/mack-10-v2-pcb-render.png
  - /images/projects/mack-10-v1-pcb-routing.png
  - /images/projects/mack-10-v1-pcb-render.png
---

## Project overview

I designed and built the MACK-10, a compact electromechanical tensile tester. It brings together stepper-driven motion, load-cell force measurement, and embedded control on a custom PCB.

## Mechanical and electrical design

### Combining motion and force measurement

- **Mechatronics integration.** Combined controlled linear motion with load-cell sensing in a desktop test machine.
- **Embedded control.** Integrated the stepper drive, force measurement, and control electronics into the system.

## Controller revision

### About half the board footprint

- **PCB layout.** Redesigned the controller from V1 to V2, reducing the board footprint by about 50%.
- **Circuit integration.** Combined power management, motor control, sensing, I²C peripherals, and expandable I/O on the smaller board.
