# 🔐 RFID Laser Security System  
### Complete Wiring & Setup Guide (Arduino UNO)

---

## 📦 Wiring Instructions (Using Breadboard)

### 🔌 Breadboard Power Rails Setup
- **Arduino 5V** → Breadboard **positive (+) rail** (red)
- **Arduino 3.3V** → Separate breadboard row (**for RFID only**)
- **Arduino GND** → Breadboard **negative (-) rail** (blue/black)

---

## 📡 MFRC522 RFID Reader
| RFID Pin | Arduino Pin |
|--------|-------------|
| SDA    | Pin **10** |
| SCK    | Pin **13** |
| MOSI   | Pin **11** |
| MISO   | Pin **12** |
| IRQ    | *Not connected* |
| RST    | Pin **9** |
| GND    | Breadboard **GND (-)** |
| 3.3V   | Breadboard **3.3V row** ⚠️ **(NOT 5V!)** |

---

## 🖥️ LCD1602 I2C Display
| LCD Pin | Arduino Pin |
|-------|-------------|
| GND   | Breadboard **GND (-)** |
| VCC   | Breadboard **5V (+)** |
| SDA   | **A4** |
| SCL   | **A5** |

---

## 🔴 KY-008 Laser Module
| Laser Pin | Arduino Pin |
|---------|-------------|
| S (Signal) | Pin **3** |
| + (Middle Pin) | Breadboard **5V (+)** |
| - (GND) | Breadboard **GND (-)** |

---

## 🌞 LDR Photosensitive Sensor
| LDR Pin | Arduino Pin |
|--------|-------------|
| VCC    | Breadboard **5V (+)** |
| GND    | Breadboard **GND (-)** |
| AO / DO | **A0** |

---

## 🔊 Active Buzzer
| Buzzer Pin | Arduino Pin |
|-----------|-------------|
| + / I/O   | Pin **8** |
| - / GND   | Breadboard **GND (-)** |

---

## ⚙️ Setup Instructions

### 🧩 Step 1: Install Required Libraries
In **Arduino IDE**:
1. Go to **Sketch → Include Library → Manage Libraries**
2. Install:
   - **MFRC522** by *GithubCommunity*
   - **LiquidCrystal I2C** by *Frank de Brabander*

---

### 💻 Step 2: Upload the Code
1. Connect **Arduino UNO** via USB  
2. Copy the project code  
3. Paste into **Arduino IDE**  
4. Select:
   - **Tools → Board → Arduino UNO**
   - **Tools → Port → (Your COM Port)**
5. Click **Upload**

---

### 🆔 Step 3: Get Your RFID Card UIDs
1. Open **Serial Monitor**
2. Set **Baud Rate: 9600**
3. Scan RFID cards near the reader  
4. Note UID shown in Serial Monitor  
