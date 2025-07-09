# 🚗 Smart Parking System using Arduino + IR Remote + LCD

A smart and compact system to monitor and manage parking slots using an **IR Remote**, **LED indicators**, and a **16x2 LCD display**. Perfect for beginner-to-intermediate level Arduino enthusiasts. This project simulates a **real-time smart parking lot** that shows occupied/free slots with visual feedback and remote control simulation.

[🔗 View TinkerCAD Simulation](https://www.tinkercad.com/things/INSERT-YOUR-LINK-HERE)

---

## 🧭 Table of Contents

- [📷 Project Screenshots](#-project-screenshots)
- [🔧 Components Used](#-components-used)
- [💡 Working Principle](#-working-principle)
- [🧠 Code Explanation](#-code-explanation)
- [📦 Applications](#-applications)
- [🧑‍💻 Full Arduino Code](#-full-arduino-code)
- [📌 Credits & Notes](#-credits--notes)

---

## 📷 Project Screenshots

| State                  | Screenshot                        |
|-----------------------|-----------------------------------|
| 🔌 Circuit Layout      | ![Circuit](./circuit_layout.png) |
| ✅ All Slots Free      | ![All Free](./all_free.png)      |
| ❌ All Slots Full      | ![All Full](./all_full.png)      |
| 🔁 Half Slots Filled   | ![Half Full](./half_full.png)    |

---

## 🔧 Components Used

- Arduino UNO
- IR Receiver + Remote
- 6 Push Buttons (Simulating vehicle presence)
- 6 Red LEDs (For 6 slots)
- 16x2 LCD Display (with potentiometer)
- Breadboard + Jumper Wires
- USB Cable

---

## 💡 Working Principle

The project simulates 6 parking slots. Each slot is toggled using an IR remote. Pressing button `1-6` changes the slot status between **Free** and **Occupied**:

- 🔴 **LED ON** → Slot Occupied
- ⚪ **LED OFF** → Slot Free
- LCD shows the number and list of free slots in real-time.
  
---

## 🧠 Code Explanation

- Uses **IRremote library** to decode signals from an IR remote.
- Each button press toggles the slot status using `slotOccupied[]` array.
- LEDs are updated accordingly.
- LCD displays:
  - List of available slots (e.g., `Pno: 1,3,4`)
  - Number of free slots (e.g., `Free Slots: 3`)

---

## 📦 Applications

This smart parking system concept can be applied or adapted in:

### 🚘 Real-Time Parking Management Systems:
- Mall, hospitals, smart cities, or corporate campuses.

### 🏭 Embedded Systems & IoT Projects:
- Can be integrated with **NodeMCU**, **WiFi/Bluetooth**, and **Mobile Apps**.

### 🧪 VLSI / Chip Design Domain:
- Can be prototyped in **Verilog/SystemVerilog** for chip-level simulation of control logic and FSM-based occupancy tracker.

### 🌐 Smart City Infrastructure:
- Integrate with **cloud dashboards**, **mobile apps**, or **camera modules** for real-time traffic + parking guidance.

---

## 💻 Full Arduino Code

```cpp
#include <IRremote.h>
#include <LiquidCrystal.h>

#define IR_RECEIVE_PIN 7

LiquidCrystal lcd(12, 11, 5, 4, 3, 2);

int ledPins[6] = {A0, A1, A2, A3, A4, A5};
bool slotOccupied[6] = {false, false, false, false, false, false};

unsigned long buttonCodes[6] = {
  0xEF10BF00, 0xEE11BF00, 0xED12BF00,
  0xEB14BF00, 0xEA15BF00, 0xE916BF00
};

void setup() {
  Serial.begin(9600);
  IrReceiver.begin(IR_RECEIVE_PIN, ENABLE_LED_FEEDBACK);
  lcd.begin(16, 2);
  for (int i = 0; i < 6; i++) pinMode(ledPins[i], OUTPUT);

  lcd.setCursor(0, 0);
  lcd.print("Smart Parking");
  delay(2000);
  lcd.clear();
}

void loop() {
  if (IrReceiver.decode()) {
    unsigned long code = IrReceiver.decodedIRData.decodedRawData;
    Serial.print("Received: ");
    Serial.println(code, HEX);
    for (int i = 0; i < 6; i++) {
      if (code == buttonCodes[i]) {
        slotOccupied[i] = !slotOccupied[i];
        delay(300);
        break;
      }
    }
    IrReceiver.resume();
  }

  int freeCount = 0;
  String freeSlots = "Pno: ";
  for (int i = 0; i < 6; i++) {
    digitalWrite(ledPins[i], slotOccupied[i] ? HIGH : LOW);
    if (!slotOccupied[i]) {
      freeCount++;
      freeSlots += String(i + 1) + ",";
    }
  }

  if (freeCount > 0) freeSlots.remove(freeSlots.length() - 1);
  else freeSlots = "FULL";

  lcd.clear();
  lcd.setCursor(0, 0);
  lcd.print(freeSlots);
  lcd.setCursor(0, 1);
  lcd.print("Free Slots: ");
  lcd.print(freeCount);

  delay(500);
}
````

---

## 📌 Credits & Notes

* Developed by **Shaili Sahu** 👩‍💻
* Built in [TinkerCAD Circuits](https://www.tinkercad.com/)
* Beginner-friendly IR + LCD + Arduino project
* Can be extended with sensors or mobile integration

---
