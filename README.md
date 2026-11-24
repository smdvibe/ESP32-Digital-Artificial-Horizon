# ESP32 Digital Artificial Horizon

A DIY digital aircraft-style artificial horizon built with an **ESP32-C3**, a **1.28-inch 240×240 GC9A01A round TFT display**, and an **MPU6050 accelerometer/gyroscope module**.

The display shows bank (roll) and pitch attitude using a moving blue/brown or blue/navy horizon, a pitch ladder, a curved aircraft reference symbol, a roll scale, and live numeric readings.

## Preview

### Video Demo

<video src="videos/artificial-horizon-demo.mp4"
       controls
       width="320">
</video>

### Mode 1: Classic Display — Blue sky / brown ground

![Mode 1](images/mode-1-blue-brown.jpg)

### Mode 2: Night Display — Blue sky / dark ground

![Mode 2](images/mode-2-blue-navy.jpg)

## Hardware

- ESP32-C3 development board
- GC9A01A round TFT display, 240×240
- MPU6050 IMU module
- Momentary push button
- Prototype PCB and jumper wires

## Wiring

### GC9A01A display → ESP32-C3

| Display pin | ESP32-C3 GPIO |
|---|---:|
| SCLK / SCL | GPIO 4 |
| MOSI / SDA | GPIO 6 |
| CS | GPIO 7 |
| DC | GPIO 3 |
| RST / RES | GPIO 10 |
| VCC | 3.3V (according to the display module requirements) |
| GND | GND |

### MPU6050 → ESP32-C3

| MPU6050 pin | ESP32-C3 GPIO |
|---|---:|
| SDA | GPIO 0 |
| SCL | GPIO 1 |
| VCC | 3.3V (according to the module requirements) |
| GND | GND |

The sketch uses the MPU6050 I²C address `0x68`.

### Button → ESP32-C3

| Button contact | Connection |
|---|---|
| One contact | GPIO 5 |
| Other contact | GND |

The firmware enables `INPUT_PULLUP`, so no external pull-up resistor is required for a basic momentary button.

## Button functions

- **Short press:** switches between the two display colour themes.
- **Hold for 2 seconds:** calibrates the current position as the zero reference. Keep the device level and stationary while calibrating.

## Firmware behaviour

- Reads acceleration from the MPU6050 over I²C.
- Calculates roll and pitch from the accelerometer.
- Applies a simple low-pass filter to the displayed angles.
- Limits displayed roll to ±60° and pitch to ±45°.
- Renders the artificial horizon on a 240×240 RGB565 framebuffer and transfers it to the GC9A01A display.

## Arduino IDE setup

Install these libraries through Arduino IDE Library Manager:

- **Adafruit GFX Library**
- **Adafruit GC9A01A**
- **Adafruit MPU6050**
- **Adafruit Unified Sensor**

The `Wire`, `SPI`, and `math` headers are included with the Arduino/ESP32 environment.

Select an ESP32-C3 board in Arduino IDE, connect the board, and upload `ESP32_Digital_Artificial_Horizon.ino`.

## Repository contents

```text
ESP32-Digital-Artificial-Horizon/
├── ESP32_Digital_Artificial_Horizon.ino
├── README.md
└── images/
    ├── mode-1-blue-brown.jpg
    └── mode-2-blue-navy.jpg
```

## Notes

- This is a DIY educational electronics project, not a certified aviation instrument.
- The displayed attitude is based on the MPU6050 accelerometer and can be affected by vibration and linear acceleration.
- The sketch is provided as the working project code supplied with this repository. Hardware behaviour may vary with display and sensor modules.

## Author

**SMD VIBE**

YouTube: https://www.youtube.com/@smdvibe  
TikTok: https://www.tiktok.com/@smdvibe  
Instagram: https://www.instagram.com/smd_vibe/
