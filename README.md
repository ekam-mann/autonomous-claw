# Autonomous Underwater Claw

Closed-loop underwater gripper that detects, grabs, confirms, and releases an object with no operator input. Team project for APSC 101 (Team K1), team lead. Full writeup: [ekammann.com/projects/autonomous-claw](https://ekammann.com/projects/autonomous-claw/)

## How it works

An Arduino Uno reads distance from an ultrasonic sensor, drives a 180 degree servo to actuate the claw, and runs a four state machine:

1. **SEARCHING**: waits for an object to enter grab range (within 10 cm).
2. **SETTLING**: confirms the object stays in range for a short hold window before committing, so a momentary blip does not trigger a grab.
3. **GRABBING**: closes the claw, then checks whether the measured distance rises past a lift confirmation threshold (18 cm). If it does, the grab succeeded. If not, within the retry window, it reopens and returns to searching.
4. **HOLDING**: keeps the claw closed until the object is released or lost (distance past 25 cm), then reopens.

The key design point is that it is closed loop: it does not assume a grab worked, it confirms the lift from sensor feedback and self corrects on failure. Distance readings are smoothed with a rolling average, and sampling runs at a fixed 50 ms interval using non blocking timing rather than blocking delays.

## Hardware

- Arduino Uno
- Ultrasonic distance sensor (HC-SR04 style)
- 180 degree servo
- Sheet metal frame, fabricated to plus or minus 2 mm tolerances across 12 joints
- Interference envelopes simulated in SolidWorks before fabrication, cutting material waste by 20% pre build

## Files

- `claw.ino` — Arduino sketch, runs as-is on the hardware described above.
- `cad-render.jpg` — SolidWorks assembly render.
- `assembly-drawing.jpg` — dimensioned engineering drawing (Team K1).
