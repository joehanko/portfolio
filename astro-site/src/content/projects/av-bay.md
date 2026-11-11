---
title: Avionics Bay
category: Avionics / Embedded Systems
year: "2026"
summary: Mechanical packaging, sensor integration, and bench testing for
  Dirac Delta’s avionics bay, with live telemetry and onboard video recording.
cover: /images/projects/AV Bay Profile.png
coverAlt: CAD rendering of the cylindrical Dirac Delta avionics bay with the
  electronics visible through the side.
coverCaption: CAD view of the Dirac Delta avionics bay and its internal
  electronics packaging.
highlights:
  - value: Live Telemetry
    label: Rocket-to-ground telemetry between XIAO ESP32-S3 and C3 Microcontrollers, ESP-NOW LR (2.4GHz)
  - value: Onboard Video
    label: Video recorded in custom part files and reconstructed after recovery
  - value: microSD
    label:
  - value: Mission Control
    label: Ground dashboard, rehearsals, and data review
gallery:
  - /images/projects/Mission Control.png
---

## Project overview

Dirac Delta’s avionics bay combines live ground telemetry with an onboard record of sensor data and video. For this project, I determined system requirements, designed packaging in CAD, choose the sensora and integrated the hardware, and bench tested the completed unit. I built a mission control dashboard to display live telemetry with the help of Codex.

## Avionics bay

### Fitting the electronics inside the bay

- **CAD and mechanical packaging.** Designed the cylindrical layout around the boards, sensors, and camera.
- **Electronics integration.** Integrated GPS, an accelerometer and gyroscope, a pressure sensor, a camera, and microSD storage around a XIAO ESP32-S3 Sense. A status LED provides feedback at the bay.
- **Sensor setup.** Accounted for the IMU’s installed orientation and used ground references for relative altitude and level.

## Telemetry and Mission Control

### Making live data readable on the ground

- **Embedded systems integration.** ESP-NOW carries sensor data to an ESP32-C6 receiver, which passes it to the browser dashboard over USB serial.
- **Data interpretation.** Live plots show altitude, acceleration, and link health, with gaps and stale readings marked. Recorder acknowledgments and saved-frame progress are shown separately.
- **Operator workflow.** Mission Control combines preflight checks, live instruments, and CSV review. Recorded data and OpenRocket simulations support rehearsals.

## Recording and recovery

### Saving an onboard record

- **Data logging and recovery.** The onboard recorder saves segmented video and telemetry to microSD. The recovery tools export intact frames from readable, interrupted files as playable clips.
- **Hardware assembly.** Assembled and powered up a separate Eggtimer Quantum recovery computer and checked its Wi-Fi connection. Mission Control’s read-only status view still needs an end-to-end device check.

## Validation

### Bench results and open flight checks

- **Bench testing.** The camera and microSD still-image check passed.
- **Software verification.** Recorded checks covered both firmware builds, packet handling, CSV export, and recovery from damaged test files.
- **Flight validation pending.** Field radio range, footage quality, and sensor performance under flight conditions still need data.
