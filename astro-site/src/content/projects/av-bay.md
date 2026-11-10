---
title: Avionics Bay
category: Avionics / Embedded Systems
year: "2026"
summary: I designed the avionics bay for a high-power rocket and built the
  firmware, ground receiver, and Mission Control dashboard around it.
cover: /images/projects/AV Bay Profile.png
coverAlt: CAD rendering of the cylindrical Dirac Delta avionics bay with the
  electronics visible through the side.
coverCaption: CAD view of the Dirac Delta avionics bay and its internal
  electronics packaging.
highlights:
  - value: XIAO ESP32-S3
    label: Onboard sensing and recording
  - value: ESP-NOW
    label: Telemetry and recorder commands between the rocket and ground receiver
  - value: microSD
    label: Segmented video and onboard telemetry logs
  - value: Mission Control
    label: Ground dashboard, rehearsals, and data review
gallery:
  - /images/projects/Mission Control.png
---
## Project overview

My work on Dirac Delta covered the mechanical packaging of the avionics bay, the onboard and ground firmware, and Mission Control, the browser app I built to operate and review the system. I wanted to see what the rocket was reporting from the ground and keep a separate record on the rocket for later review. That gave me two data paths to design around.

- **Live telemetry** travels from the ESP32-S3 in the rocket to an ESP32-C6 receiver over ESP-NOW. The receiver passes it to Mission Control over USB serial.
- **Onboard recording** keeps video and telemetry on microSD. Once the recorder starts, it can keep collecting data if the ground link drops, subject to its time, storage, and error limits.

## Avionics bay and sensors

I designed the CAD layout around a cylindrical bay that holds the onboard electronics. The XIAO ESP32-S3 Sense handles sensing, radio communication, and recording. The NEO-7M GPS supplies location, the MPU6050 supplies acceleration and angular rate, and the BMP280 measures pressure and temperature. The firmware uses pressure to estimate height relative to a ground reference. GPS altitude is displayed separately because it has a different reference.

I replaced the onboard OLED with a status LED and moved detailed diagnostics to Mission Control. The LED still gives a local indication of startup, readiness, and faults, with a distinct cue before recording. The electronics also support a physical still-photo button. In bench mode, a captured JPEG can be saved to the card and sent back in chunks, with integrity checks before the receiver accepts it.

The fixed IMU mounting orientation shaped the dashboard’s attitude view. I accounted for the installed sensor axes and added a way to capture a level reference. That helps during setup. The configured ±2 g range can saturate under rocket acceleration, and acceleration-based tilt becomes unreliable under thrust.

## Telemetry and recorder commands

I wrote firmware for both boards. The ground receiver forwards sensor and recorder telemetry to the laptop and tracks signal strength and packet gaps. Mission Control shows when the last packet arrived, dims stale readings, and leaves missing readings blank. It keeps gaps visible in the plots so an outage does not look like a continuous measurement.

The link also carries recorder start, stop, and settings requests. I separated a command acknowledgment from evidence that it took effect. An accepted start request only confirms the command. Fresh telemetry showing frames written confirms recording progress. Settings are shown as confirmed when the rocket acknowledges the request and reports back the applied values. Retries use the same request ID so a lost acknowledgment does not start another recording.

## Mission Control

I organized the dashboard around what I would need at the bench, at the pad, and after recovery.

### Preflight and live flight

The preflight view brings together radio, sensor, camera, and logging status with a checklist. It distinguishes checks backed by fresh telemetry from steps the operator marks manually. I added controls to capture ground altitude and level references, plus an operator-controlled countdown with hold and cancel. The countdown tracks the plan in the dashboard. Motor ignition and parachute deployment are handled separately.

The live view brings the instruments, link health, and recent trends forward. It shows barometric height, an estimate of vertical speed, acceleration, GPS position, and recorder state. It also keeps the last valid GPS fix and its age available for recovery. Observed peaks are based on received samples, so a gap in reception can hide the true peak.

I assembled and powered up the separate Eggtimer Quantum recovery computer and connected to its Wi-Fi on the bench. I added a read-only settings and status view to Mission Control so the operator can review what the Quantum reports alongside the preflight checklist. Arming stays on the Quantum’s own interface. The dashboard’s read-only integration still needs an end-to-end check with the device.

### Rehearsal, logging, and review

I added a timed replay of recorded CSV sessions and a deliberate link-gap drill to practice how the dashboard behaves when packets stop arriving. I also added a separate OpenRocket-based simulation for rehearsing the planned flight timeline without the rocket connected. The dashboard labels these modes and disables live recorder commands during rehearsal.

Mission Control can save incoming telemetry and operator events to a ground CSV and reopen saved files for review. It keeps the planned flight timeline separate from measured data. I also integrated a battery planning tab for estimating power use across mission phases. Its estimates are based on the configured devices and assumptions.

## Onboard video and recovery

I moved from bench stills to a segmented recorder using the Sense camera and microSD card. The rocket stores JPEG frames in short files and writes a local telemetry CSV. One storage worker handles the camera and card. It gives video writes priority and counts dropped log samples when storage cannot keep up.

The file format stores each frame with a timestamp and checksum. I built a Python recovery tool and a Mission Control Video view that can read intact frames from an interrupted file and turn them into playable AVI clips. The goal is to recover whatever frames survived an interruption. A card or filesystem failure can still make the files unreadable.

## Validation and next checks

The camera and microSD still-image check passed on the bench. Both firmware targets compiled in the recorded builds. Software checks covered packet handling, recorder acknowledgments, CSV logging and review, and video recovery from deliberately truncated or damaged test files. I also exercised dashboard behavior for stale links and rehearsals with simulated or recorded inputs.

The remaining checks are physical. I want to measure recording performance and power stability inside the assembled bay, check the radio link under field conditions, and verify the Quantum readout with the hardware connected. Flight data is still needed to establish usable radio range, footage quality, and how well the sensor estimates hold up in flight.