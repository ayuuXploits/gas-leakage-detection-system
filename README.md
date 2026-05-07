# MQ-2 Gas & Smoke Detection System

A robust, production-ready firmware for the MQ-2 gas sensor designed for the **ESP32** (or Arduino). This project implements industrial-standard logic for noise reduction and false-alarm prevention.

## 🚀 Features

* **Baseline Calibration:** Automatically samples "fresh air" on startup to set a dynamic baseline.
* **Signal Smoothing:** Uses a **Circular Rolling Buffer** to calculate a rolling average, filtering out ADC spikes.
* **Hysteresis & Debounce:** * **Confirmation:** Requires 3 consecutive over-threshold reads to trigger an alarm.
    * **Hysteresis:** Requires the gas level to drop well below the trigger point before clearing, preventing "buzzer chatter."
* **Non-Blocking Architecture:** Uses `millis()` for sampling, ensuring the CPU isn't halted during operation.
* **Remote Recalibration:** Supports Serial commands (`R`) to recalibrate the baseline without rebooting.

## 🛠 Hardware Configuration

| Component | Pin (ESP32) | Description |
| :--- | :--- | :--- |
| **MQ-2 Sensor** | `GPIO 34` | Analog Output (A0) |
| **Active Buzzer** | `GPIO 2` | Digital Output |
| **VCC** | `5V` | External power recommended for MQ heater |
| **GND** | `GND` | Common Ground |

## ⚙️ Configuration Constants

You can fine-tune the detection sensitivity in the top section of the code:

* `WARMUP_DELAY_MS`: Default **20s** (Essential for MQ sensor chemical stability).
* `THRESHOLD_OFFSET`: Set to **150** units above baseline for alarm trigger.
* `HYSTERESIS_BAND`: Set to **30** units to ensure the alarm clears cleanly.
* `CONFIRM_READINGS`: Number of samples required to confirm gas presence.

## 💻 Installation & Usage

1.  **Hardware Setup:** Connect your MQ-2 sensor and buzzer to the pins defined in the code.
2.  **IDE Setup:** Open the code in the Arduino IDE. Ensure your board is set (e.g., `ESP32 Dev Module`).
3.  **Baud Rate:** Open the Serial Monitor and set the baud rate to `115200`.
4.  **Calibration:** On startup, leave the sensor in clean air for the duration of the 20s warm-up and calibration phase.
5.  **Monitoring:** The Serial Monitor will output live smoothed data, current baseline, and the calculated alarm threshold.

## 🕹 Serial Commands

The system listens for commands over the Serial interface:
* `R` or `r`: Triggers a full recalibration of the sensor baseline.

## ⚠️ Disclaimer

This project is for **educational and prototyping purposes only**. MQ-series sensors require significant burn-in time (often 24-48 hours) for high accuracy. This software should not be used in life-critical safety systems or industrial environments without professional certification and secondary fail-safes.

## 📝 License

This project is open-source and available under the [MIT License](LICENSE).
