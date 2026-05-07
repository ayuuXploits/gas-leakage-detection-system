# 🛡️ MQ-2 Gas & Smoke Detection System

A robust, production-ready firmware for the MQ-2 gas sensor designed for the **ESP32** (or Arduino). This project implements industrial-standard logic for noise reduction, signal stability, and false-alarm prevention.

---

## 🚀 Key Features

* **Baseline Calibration:** Automatically samples "fresh air" on startup to establish a dynamic environment-specific baseline.
* **Signal Smoothing:** Implements a **Circular Rolling Buffer** to calculate a rolling average, effectively filtering out ADC spikes and electrical noise.
* **Hysteresis & Debounce Logic:**
    * **Confirmation:** Requires multiple consecutive over-threshold reads to trigger an alarm, filtering out transient spikes.
    * **Hysteresis:** Requires gas levels to drop significantly below the trigger point before clearing, preventing "buzzer chatter."
* **Non-Blocking Architecture:** Built using `millis()` timing, ensuring the CPU can handle other tasks or Serial communication without interruption.
* **Remote Recalibration:** Supports real-time Serial commands (`R`) to reset the baseline without rebooting the hardware.

---

## 🛠 Hardware Configuration

| Component | Pin (ESP32) | Description |
| :--- | :--- | :--- |
| **MQ-2 Sensor** | `GPIO 34` | Analog Output (A0) |
| **Active Buzzer** | `GPIO 2` | Digital Output |
| **VCC** | `5V` | **Note:** MQ sensors require 5V for the internal heater |
| **GND** | `GND` | Common Ground |

> [!IMPORTANT]
> **Voltage Note:** While the ESP32 logic is 3.3V, the MQ-2 heater needs 5V to function correctly. Ensure your sensor's VCC is connected to a stable 5V rail.


 ## 💻 Installation & Usage

### 1. Clone & Setup
Open your terminal and run the following commands to get the project locally:

```bash
# Clone the repository
git clone [https://github.com/your-username/gas-detection-system.git](https://github.com/your-username/gas-detection-system.git)

# Navigate into the project directory
cd gas-detection-system
---

## ⚙️ Configuration Constants

Fine-tune the sensitivity and performance by adjusting these constants at the top of the code:

* `WARMUP_DELAY_MS`: Default **20s** (Essential for chemical stability of the MQ sensor).
* `THRESHOLD_OFFSET`: Set to **150** units above baseline for the alarm trigger.
* `HYSTERESIS_BAND`: Set to **30** units to ensure the alarm clears cleanly.
* `CONFIRM_READINGS`: Number of samples required to confirm gas presence.

---

## 💻 Installation & Usage

1.  **Hardware Setup:** Connect the MQ-2 sensor and buzzer as per the pinout table above.
2.  **IDE Setup:** Open the `.ino` file in the Arduino IDE. Set your board to `ESP32 Dev Module`.
3.  **Baud Rate:** Set your Serial Monitor to **115200 baud**.
4.  **Calibration:** On startup, leave the sensor in clean air. The system will wait 20s for the heater to stabilize before setting the baseline.
5.  **Monitoring:** The Serial Monitor will stream live smoothed data, current baseline, and the calculated alarm threshold.

---

## 🕹 Serial Commands

The system monitors the Serial interface for the following commands:
* `R` or `r`: Triggers an immediate full recalibration of the sensor baseline.

---

## ⚠️ Safety Disclaimer

This project is for **educational and prototyping purposes only**. 
* **Burn-in Time:** MQ sensors require an initial burn-in period (often 24-48 hours) for maximum accuracy.
* **Critical Safety:** This software is not certified for life-critical safety systems. Always use commercially manufactured and certified gas detectors in residential or industrial environments where combustible or toxic gases are a risk.

---

## 📝 License

Distributed under the **MIT License**. See `LICENSE` for more information.
