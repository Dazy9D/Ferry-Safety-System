# ⚓ Ferry Safety System

A low-cost, Arduino-based prototype that monitors ferry load, load distribution, hull tilt and waterline depth, and automatically controls a boarding barrier when conditions become unsafe.

## Overview

Overloading and uneven load distribution are a major cause of river accidents in Bangladesh, and safety checks still depend largely on the crew's visual judgement. This project combines readings from multiple sensors into a single risk assessment and closes the boarding barrier when something looks wrong, instead of relying on one sensor threshold.

## Features

- **Sensor fusion:** Weight-based and depth-based imbalance readings are cross-checked against MPU6050 tilt data
- **Automatic barrier control:** Boarding stops and the barrier closes on overload, imbalance, excessive tilt, waterline or overspeed conditions
- **Verified actuation:** Limit switches confirm the barrier's real position rather than trusting the servo command
- **Fail-safe design:** The system defaults to a safe state on sensor disagreement, and enters a Fault state if closure isn't confirmed
- **Hold time before reopening:** The barrier only reopens after conditions stay safe for a full wait period, which prevents constant open/close cycling
- **Obstruction detection:** An ultrasonic sensor checks the barrier path before and during closing
- **Manual controls:** Emergency stop and a manual override that only works when no fault is active

## Hardware

| Component | Purpose |
|---|---|
| Arduino Mega 2560 | Main controller |
| 4× Load cell (5 kg) + HX711 | Corner-mounted weight and load position sensing |
| MPU6050 | Roll and pitch monitoring (I2C) |
| 4× Waterline sensor | Submersion depth at each corner |
| 2× HC-SR04 | Vehicle approach speed and barrier obstruction |
| SG90 servo | Boarding barrier actuator |
| 2× Micro limit switch | Barrier open/closed verification |
| LEDs, buzzer, push buttons | Status alerts, emergency stop, manual override |
| LM2596 buck converter + battery pack | Regulated 5V power |

**Estimated total cost:** about 6,268 BDT

## Libraries

- `Servo`
- `Adafruit MPU6050`
- `HX711`

## How It Works

1. **Boot:** The barrier moves to the closed position, the MPU6050 calibrates for 30 seconds, the load cells are tared and the waterline sensors record a dry baseline.
2. **Monitoring:** All 11 inputs are read every loop cycle.
3. **Unsafe condition detected:** Boarding stops and the barrier closes, then the closed limit switch confirms it.
4. **Recovery:** Once conditions stay safe for the hold time, the barrier reopens and the open limit switch confirms it.

## Results

| Feature | Status |
|---|---|
| Boot-up and calibration | ✅ Working |
| Overload detection (buzzer + LED) | ✅ Working |
| Ultrasonic speed detection | ✅ Detected correctly |
| Overspeed warning LED | ✅ Working |

## Limitations

- Resistive waterline sensors can drift and corrode after repeated wet cycles
- No backup power, and no safe shutdown on power loss
- Thresholds are tuned for the demo setup only and need recalibration for real use

## Future Work

- Solar charging
- Waterproof enclosures
- GSM (SIM800L) alerts to shore authorities
- Machine-learning-based risk grading instead of fixed thresholds

## Team

| Members |
|---|
| Anita Haque Sushma |
| Tauhidul Islam Pranto | 
| Shafi Ahmed Adib | 

Department of Computer Science and Engineering, BRAC University
