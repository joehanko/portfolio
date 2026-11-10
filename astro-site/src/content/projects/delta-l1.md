---
title: Dirac Delta L1 Rocketry
category: Avionics / Embedded Systems
year: "2026"
summary: A compact rocket avionics bay with onboard sensing and video, an ESP-NOW ground receiver, and a browser dashboard called Mission Control.
cover: /images/projects/AV Bay Profile.png
coverAlt: CAD rendering of the cylindrical avionics bay with circuit boards mounted inside.
coverCaption: CAD view of the Dirac Delta avionics bay and its internal electronics packaging.
highlights:
  - value: XIAO ESP32-S3
    label: Onboard sensing and recording
  - value: ESP-NOW
    label: Radio link to the ground receiver
  - value: microSD
    label: Onboard video and telemetry logs
  - value: Mission Control
    label: Ground dashboard and post-flight review
---

## The challenge

Dirac Delta is a high-power rocketry project. My work focused on the avionics bay and Mission Control. I wanted to fit sensors, a camera, and a radio inside a compact bay, see what the rocket was reporting from the ground, and keep an onboard record for later review. That meant designing for two kinds of data access: a live link when the receiver is in contact, and files on the rocket that can be recovered afterward.

## Avionics bay

The CAD model packages the electronics inside a cylindrical bay. The rocket unit uses a XIAO ESP32-S3 Sense with a NEO-7M GPS receiver, an MPU6050 accelerometer and gyroscope, and a BMP280 pressure sensor. The barometer provides pressure and an estimate of altitude relative to a ground reference. The camera and microSD card handle local recording, and an external status LED gives a quick indication of the avionics state without a screen.

The sensor placement also affected the software. The IMU is mounted in a fixed orientation, so the dashboard’s attitude display needs to use the bay’s installed axes and establish a level reference. That reference is useful for setup. Accelerometer-based tilt becomes unreliable under thrust, and the configured ±2 g range can saturate during rocket acceleration.

## From rocket to ground

The S3 sends telemetry over ESP-NOW to a XIAO ESP32-C6 ground receiver. The receiver connects to a laptop by USB, where Mission Control reads the serial stream. This keeps the dashboard usable with a direct receiver connection at the launch site.

Packet freshness matters as much as the numbers themselves. Mission Control dims stale readings and marks missing or invalid values. Recorded CSV data can be reopened in a labeled review mode, so a saved session is distinguishable from a live stream.

## Mission Control

I built Mission Control around three tasks an operator performs before, during, and after a session.

- **Before:** Check radio and sensor readiness, establish the ground references, choose a log destination, and review the preflight checklist. The checklist distinguishes live device evidence from steps the operator acknowledges manually.
- **During:** Watch GPS, relative altitude, acceleration, and link status. The dashboard provides recording controls and shows the state reported by the onboard recorder. The countdown is a dashboard clock. Ignition and recovery are handled separately.
- **After:** Export or reopen telemetry CSV, review events, and recover footage from the onboard microSD card. Rehearsal and simulation comparison provide ways to exercise the dashboard and interpret planned events separately from measured data.

The Eggtimer Quantum is a separate recovery computer. Mission Control has a read-only view of reported Quantum settings. Hardware compatibility for that integration still needs bench verification.

## Recording and recovery

The rocket writes video in short segments and logs telemetry to microSD. I used that structure so that an interrupted recording can still contain complete frames. A recovery tool reads closed segments and intact frames from interrupted files and converts them to a playable video. The ground CSV provides a second record of telemetry that made it through the radio link.

This design keeps the two records useful for different reasons. The ground log shows what the operator could see during the session. The onboard record can retain data captured while the ground link was unavailable, provided the files are recoverable.

## What I verified

The camera and microSD still-image check passed on the bench. Both firmware targets compiled in the recorded validation runs. Software checks exercised packet handling, recorder command acknowledgments, dashboard CSV export, and recovery from deliberately truncated or damaged recording files. Those recovery checks used test recordings. The project records don’t establish in-flight radio range, footage quality, or a completed flight result.
