# 🌱 Smart Greenhouse Monitoring & Control System

An embedded systems project based on the **ATmega32** microcontroller that automatically monitors the temperature and light level of a greenhouse and controls a **cooling fan** and a **grow light** accordingly. Live readings and device status are shown on a **16x2 LCD**.

The project is written in **Embedded C (AVR-GCC / Microchip Studio)** and simulated in **Proteus**.

---

## 📌 Features

- 🌡️ Real-time temperature measurement using an **LM35** sensor
- 💡 Ambient light detection using an **LDR (Torch LDR)**
- 🌀 Automatic **fan (DC motor)** control through an **L293D** motor driver when the temperature reaches the target value
- 🔆 Automatic **grow light (LED)** control when the light level is low
- 📟 **16x2 LCD** (4-bit mode) showing temperature, target temperature, and fan/light status
- ⏱️ Display refreshes every 500 ms

---

## 🧰 Components Used

| Component | Description |
|-----------|-------------|
| ATmega32 (U1) | Main microcontroller, 16 MHz |
| LM35 (U2) | Analog temperature sensor |
| LDR + 10kΩ resistor (R2) | Light sensor (voltage divider) |
| LM016L (LCD1) | 16x2 character LCD |
| L293D (U3) | Motor driver IC |
| DC Motor | Cooling fan |
| LED-YELLOW (D1) + 330Ω (R1) | Grow light indicator |
| 4x4 Keypad | Connected to PORTB (reserved for future use) |
| RV1 (1kΩ pot) | LCD contrast control |
| R3 (1kΩ) + Push button | Reset circuit |

---

## 🔌 Pin Configuration

| ATmega32 Pin | Connected To | Function |
|--------------|--------------|----------|
| PA0 (ADC0) | LM35 output | Temperature input |
| PA1 (ADC1) | LDR divider | Light level input |
| PC0 | LCD RS | LCD register select |
| PC1 | LCD EN | LCD enable |
| PC4 – PC7 | LCD D4 – D7 | LCD data lines (4-bit mode) |
| PD4 | LED (via 330Ω) | Grow light |
| PD5 | L293D IN1 | Fan control |
| PD6 | L293D IN2 | Fan control |
| PB0 – PB7 | 4x4 Keypad | Reserved (not used in current code) |

---

## ⚙️ How It Works

1. **ADC** is initialized with AVCC as the reference and a prescaler of 128.
2. **Temperature** is read from the LM35 (10 mV/°C):

   ```
   Temp (°C) = (ADC × 5.0 × 100) / 1024
   ```
3. **Fan control:** if `temperature >= target_temp` (default **25 °C**), the L293D is driven (IN1 = HIGH, IN2 = LOW) and the fan turns **ON**; otherwise the fan is **OFF**.
4. **Light control:** if the LDR ADC value is **below 500** (dark), the LED turns **ON**; otherwise it turns **OFF**. The threshold can be adjusted for daylight conditions.
5. The **LCD** shows:

   ```
   Row 1:  T:31 C  Tgt:25C
   Row 2:  Fan:ON  Lt:OFF
   ```

📂 Project Structure
 ```
Smart-Greenhouse/
├── greenhouse.atsln    # Source code
├── schematic.png       # Proteus circuit diagram
├── Automated greenhouse.pdsprj  # Proteus simulation file
└── README.md
```
## 🚀 How to Run

1. Open the project in **Microchip Studio (Atmel Studio)** or any AVR-GCC toolchain.
2. Build the code and generate the `.hex` file (MCU: `ATmega32`, `F_CPU = 16000000UL`).
3. Open the circuit in **Proteus**.
4. Double-click the ATmega32, load the `.hex` file into **Program File**, and set the clock frequency to **16 MHz**.
5. Run the simulation. Change the LM35 value and LDR (torch) intensity to see the fan and light respond.
---
🖼️ Circuit Diagram
```
<img width="1105" height="742" alt="image" src="https://github.com/user-attachments/assets/0a237a58-6e95-47ed-a04d-cc8ea7997d5f" />

```

## 🔮 Future Improvements

- Use the **4x4 keypad** to set the target temperature at runtime
- Add soil moisture sensing with automatic water pump control
- Use PWM for variable fan speed
- Add hysteresis to avoid rapid fan ON/OFF switching near the threshold
- Add data logging or IoT (ESP8266/ESP32) monitoring

---

This project is for educational purposes. Feel free to use and modify it.
