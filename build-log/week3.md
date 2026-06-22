# Week 3 — Betaflight Configuration and Debugging

## Summary

Configured Betaflight on the flight controller: receiver protocol setup, motor direction/mapping, bidirectional DShot, and AUX mode assignments. This week involved real debugging — four distinct problems were hit and resolved, documented below in the order they occurred.

## Problems and Fixes

### 1. Receiver not powering on (no LED)

The FlySky FS-iA10B receiver LED was not lighting up, meaning it was not receiving power from the FC at all. Diagnosed the issue against the receiver pin schematic and found the Signal/+/- (S, +, -) wires were connected to the FC UART sensor pad in the wrong order. Reordered the wires to match the receiver schematic. Receiver powered on.

### 2. Power but no stick input

With power confirmed, the FC still was not receiving any stick input from the receiver. Root cause: the signal wire was connected to the wrong UART pad — it needed to be on the RX2 pin (servo/UART RX), not the sensor pad it was originally wired to. Since IBUS is a one-way serial protocol (receiver to FC only), TX2 was not needed and was left unused. The +(VCC) and -(GND) pins were already correctly positioned in both the sensor and servo sections — only the signal pin needed to move. Re-wired the signal line to RX2.

### 3. IBUS not available on current firmware

After the wiring fix, IBUS protocol still was not being recognized by the FC. Found that the Betaflight firmware version installed did not support IBUS. Flashed the FC back to Betaflight 4.5.4, which does support IBUS. After reflashing, stick input worked correctly and consistently.

### 4. Motors continued spinning after transmitter was powered off

During motor-spin testing, discovered that turning off the transmitter did not stop the motors — a failsafe gap. Fixed this in the transmitter UI by creating a logic function: when throttle or the arm switch input reads -100 (the value sent when the transmitter itself loses power or turns off), the system treats this as a transmitter-off condition and cuts motor output.

## Other Configuration

- Verified and corrected motor direction per motor (CW/CCW) to match prop rotation requirements.
- Verified motor output mapping matched physical motor position on the frame.
- Enabled bidirectional DShot for RPM filtering.
- Set up AUX channel modes (arm, blackbox logging trigger, PID profile switching).

## Notes

Problems 1 through 3 were a single connected fault chain — fixing the wiring order alone was not sufficient, and neither would reflashing alone have been; both were required. This is a good example of why isolating one variable at a time (wiring, then protocol, then firmware) mattered for correctly diagnosing the root cause rather than guessing.
