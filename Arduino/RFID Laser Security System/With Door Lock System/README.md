# RFID Security System with Laser Alarm & Servo Door Lock

## Complete Setup Guide

-----

## Components You Need

### Electronics:

- 1x Arduino UNO R3
- 1x MFRC522 RFID Reader
- 1x LCD1602 I2C Display (5V)
- 1x LDR Photosensitive Sensor Module
- 1x KY-008 Laser Module (650nm, 5V)
- 1x Active Buzzer (3-pin)
- 1x SG90 Servo Motor (NEW)
- 1x Breadboard
- Jumper wires (male-male, male-female, female-female)
- 2x RFID Cards (one for alarm, one for door)

-----

## Complete Wiring Guide

### Power Rails Setup (Breadboard):

1. Connect Arduino **5V** → Breadboard **positive (+) rail** (red)
1. Connect Arduino **3.3V** → Separate row on breadboard (for RFID only)
1. Connect Arduino **GND** → Breadboard **negative (-) rail** (blue/black)

-----

### MFRC522 RFID Reader:

Pin order on module: **3.3V - RST - GND - IRQ - MISO - MOSI - SCK - SDA**

- **3.3V** → Breadboard 3.3V row (CRITICAL: NOT 5V!)
- **RST** → Arduino Pin 9
- **GND** → Breadboard GND rail (-)
- **IRQ** → Leave disconnected
- **MISO** → Arduino Pin 12
- **MOSI** → Arduino Pin 11
- **SCK** → Arduino Pin 13
- **SDA** → Arduino Pin 10

-----

### LCD1602 I2C Display:

- **GND** → Breadboard GND rail (-)
- **VCC** → Breadboard 5V rail (+)
- **SDA** → Arduino A4
- **SCL** → Arduino A5

-----

### KY-008 Laser Module:

- **S (Signal)** → Arduino Pin 3
- **+ (Middle pin)** → Breadboard 5V rail (+)
- **- (GND)** → Breadboard GND rail (-)

-----

### LDR Photosensitive Sensor:

- **VCC** → Breadboard 5V rail (+)
- **GND** → Breadboard GND rail (-)
- **AO (Analog Out)** → Arduino A0

-----

### Active Buzzer (3-pin):

- **S (Signal)** → Arduino Pin 8
- **+ (Middle pin/VCC)** → Breadboard 5V rail (+)
- **- (GND)** → Breadboard GND rail (-)

-----

### SG90 Servo Motor (NEW):

The servo has 3 wires with these colors:

- **Brown or Black (GND)** → Breadboard GND rail (-)
- **Red (VCC/Power)** → Breadboard 5V rail (+)
- **Orange or Yellow (Signal)** → Arduino Pin 6

-----

## Pin Summary Chart

|Component |Pin/Connection|Arduino Pin |
|-----------------|--------------|--------------|
|RFID SDA |→ |Pin 10 |
|RFID SCK |→ |Pin 13 |
|RFID MOSI |→ |Pin 11 |
|RFID MISO |→ |Pin 12 |
|RFID RST |→ |Pin 9 |
|RFID 3.3V |→ |Arduino 3.3V |
|RFID GND |→ |Breadboard GND|
|LCD SDA |→ |A4 |
|LCD SCL |→ |A5 |
|LCD VCC |→ |Breadboard 5V |
|LCD GND |→ |Breadboard GND|
|Laser S |→ |Pin 3 |
|Laser + |→ |Breadboard 5V |
|Laser - |→ |Breadboard GND|
|LDR AO |→ |A0 |
|LDR VCC |→ |Breadboard 5V |
|LDR GND |→ |Breadboard GND|
|Buzzer S |→ |Pin 8 |
|Buzzer + |→ |Breadboard 5V |
|Buzzer - |→ |Breadboard GND|
|Servo Signal |→ |Pin 6 |
|Servo Red |→ |Breadboard 5V |
|Servo Brown/Black|→ |Breadboard GND|

-----

## Software Setup

### Step 1: Install Arduino Libraries

1. Open Arduino IDE
1. Go to **Sketch** → **Include Library** → **Manage Libraries**
1. Install these libraries:
- **MFRC522** by GithubCommunity
- **LiquidCrystal I2C** by Frank de Brabander
- **Servo** (usually pre-installed)

-----

### Step 2: Get Your RFID Card UIDs

Before uploading the main code, you need to scan both RFID cards to get their unique IDs.

#### A. Upload RFID Test Code:

Use this simple test code to read your cards:

```cpp
#include <SPI.h>
#include <MFRC522.h>

#define RST_PIN 9
#define SS_PIN 10

MFRC522 rfid(SS_PIN, RST_PIN);

void setup() {
Serial.begin(9600);
SPI.begin();
rfid.PCD_Init();
Serial.println("Scan your RFID cards...");
}

void loop() {
if (!rfid.PICC_IsNewCardPresent() || !rfid.PICC_ReadCardSerial()) {
delay(50);
return;
}

Serial.print("byte cardUID[] = {");
for (byte i = 0; i < rfid.uid.size; i++) {
Serial.print("0x");
if (rfid.uid.uidByte[i] < 0x10) Serial.print("0");
Serial.print(rfid.uid.uidByte[i], HEX);
if (i < rfid.uid.size - 1) Serial.print(", ");
}
Serial.println("};");

rfid.PICC_HaltA();
rfid.PCD_StopCrypto1();
delay(2000);
}
```

#### B. Scan Both Cards:

1. Upload the test code above
1. Open Serial Monitor (9600 baud)
1. Scan your **first card** (for alarm system)
1. Note the UID shown, example: `byte cardUID[] = {0xC7, 0x43, 0x53, 0x24};`
1. Scan your **second card** (for door control)
1. Note this UID too, example: `byte cardUID[] = {0xAB, 0xCD, 0xEF, 0x12};`

-----

### Step 3: Update Main Code with Your Card UIDs

In the main security system code, find **lines 30-31**:

```cpp
byte alarmCardUID[] = {0xC7, 0x43, 0x53, 0x24}; // Card for alarm system
byte doorCardUID[] = {0xDE, 0xAD, 0xBE, 0xEF}; // Card for door control - CHANGE THIS!
```

Replace them with YOUR actual card UIDs:

```cpp
byte alarmCardUID[] = {0xYOUR, 0xFIRST, 0xCARD, 0xUID}; // Card for alarm system
byte doorCardUID[] = {0xYOUR, 0xSECOND, 0xCARD, 0xUID}; // Card for door control
```

-----

### Step 4: Calibrate LCD I2C Address (If Needed)

If your LCD shows backlight but no text:

1. The I2C address might be **0x3F** instead of **0x27**
1. Change **line 17** in the code:

```cpp
LiquidCrystal_I2C lcd(0x3F, 16, 2); // Try 0x3F if 0x27 doesn't work
```

-----

### Step 5: Calibrate LDR Threshold

Your LDR sensor is **inverted** (values go UP when dark, DOWN when bright).

#### To calibrate:

1. Upload the main code
1. Arm the system (scan alarm card)
1. Open Serial Monitor (9600 baud)
1. Point laser at LDR - note the LOW value (e.g., 50)
1. Block the laser - note the HIGH value (e.g., 800)
1. Calculate threshold: (50 + 800) / 2 = 425
1. Update **line 26** in the code:

```cpp
int ldrThreshold = 425; // Use your calculated value
```

-----

### Step 6: Upload Main Code

1. **Unplug any wire from Pin 0** (if there is one)
1. Copy the main code from the artifact
1. Paste into Arduino IDE
1. Click **Upload**
1. Wait for “Upload Complete”
1. Reconnect any wires you unplugged

-----

## Physical Setup

### Laser and LDR Alignment:

1. **Position laser and LDR** facing each other (5-20 cm apart)
1. **Turn on system** (it starts disarmed, laser is off)
1. **Scan alarm card** to arm - laser turns on
1. **Align the laser beam** to shine directly on the LDR sensor
1. **Check Serial Monitor** - LDR values should be LOW (under 100) when laser hits it
1. **Block the beam** - values should jump HIGH (over 500)
1. If not working, adjust laser angle and LDR position

### Servo Motor Setup:

1. **Attach servo horn** (the white plastic cross piece that comes with servo)
1. **Connect to servo** before powering on
1. **Power on system** - servo will move to 0° (closed position)
1. **Attach your “door”** to the horn:
- Cardboard flap
- 3D printed latch
- Popsicle stick
- Whatever you want the servo to move

-----

## How to Use the System

### System Features:

The system has **TWO independent functions**:

1. **Laser Alarm System** (controlled by Card 1)
1. **Servo Door Lock** (controlled by Card 2)

-----

### Using the Laser Alarm:

**To Arm:**

- Scan **Card 1** (alarm card)
- LCD shows “System Armed”
- Laser turns ON
- One beep confirmation
- System now monitors laser beam

**When Alarm Triggers:**

- Someone/something breaks the laser beam
- LCD shows “!!! ALARM !!!”
- Buzzer sounds rapidly (beep-beep-beep)
- Alarm continues until you disarm

**To Disarm:**

- Scan **Card 1** again
- LCD shows “System Disabled”
- Laser turns OFF
- Alarm stops
- Two beeps confirmation

-----

### Using the Servo Door:

**To Open Door:**

- Scan **Card 2** (door card)
- LCD shows “Opening Door…”
- Servo rotates to 180°
- LCD shows “Dr:OPEN”
- One beep confirmation

**To Close Door:**

- Scan **Card 2** again
- LCD shows “Closing Door…”
- Servo rotates to 0°
- LCD shows “Dr:SHUT”
- Two beeps confirmation

-----

### LCD Display Guide:

The LCD shows status on two lines:

**Top Line:**

```
Armed Dr:OPEN
```

or

```
Disarmed Dr:SHUT
```

**Bottom Line:**

- “Monitoring…” - when alarm is armed
- “Scan RFID Card” - when disarmed
- “Breach Detected” - when alarm triggered
- “Opening Door…” or “Closing Door…” - when door activating

-----

### Scanning Unknown Cards:

If you scan a card that’s not programmed:

- LCD shows “Access Denied!”
- One long beep
- Nothing else happens

-----

## Advanced Settings

### Changing Servo Door Angles:

The servo opens to 180° and closes to 0° by default.

**To change the OPEN angle** (line 126):

```cpp
doorServo.write(180); // Change to 90, 120, etc.
```

**To change the CLOSED angle** (line 143):

```cpp
doorServo.write(0); // Change to 30, 45, etc.
```

-----

### Changing Scan Debounce Time:

Cards are ignored for 2 seconds after each scan to prevent accidental double-scans.

**To change this** (line 27):

```cpp
unsigned long debounceDelay = 2000; // Change to 1000 (1 sec), 3000 (3 sec), etc.
```

-----

## Troubleshooting

### RFID Not Reading Cards:

**Check:**

- RFID connected to 3.3V (NOT 5V)
- All SPI pins correct (10, 11, 12, 13, 9)
- Card held within 1-3cm of reader
- Hold card steady for 2-3 seconds

**Fix:**

- Run RFID test code to verify it works
- Check firmware version shows 0x92 (not 0x0)

-----

### LCD Shows No Text:

**Check:**

- Backlight is on (power is good)
- I2C address is correct (try 0x3F instead of 0x27)
- SDA on A4, SCL on A5

**Fix:**

- Adjust contrast potentiometer on back of LCD
- Run I2C scanner to find correct address

-----

### Alarm Triggers Immediately:

**Check:**

- LDR threshold value
- Laser alignment

**Fix:**

- Check Serial Monitor for LDR values
- When laser hits LDR, values should be LOW (under 100)
- Recalculate threshold: (low + high) / 2
- Remember: your LDR is inverted!

-----

### Servo Doesn’t Move:

**Check:**

- Signal wire on Pin 6
- Power (red) on 5V
- Ground (brown/black) on GND
- Servo horn attached before powering on

**Fix:**

- Try external power (9V adapter to Arduino)
- Check servo by manually setting position in code

-----

### System Resets When Servo Moves:

**Problem:** Servo draws too much current, causing voltage drop

**Fix:**

- Use external power: 9V battery or DC adapter to Arduino barrel jack
- Or use a powered USB hub
- Add 100µF capacitor between servo VCC and GND

-----

### Wrong Card Recognized:

**Check:**

- Card UIDs in code match your actual cards
- You’re scanning the correct card

**Fix:**

- Re-run RFID test code
- Verify UIDs printed in Serial Monitor
- Update lines 30-31 with correct UIDs

-----

## Power Requirements

### Total Current Draw:

- Arduino: ~50mA
- RFID: ~25mA
- LCD: ~50mA
- Laser: ~40mA
- Buzzer: ~30mA
- Servo (idle): ~10mA
- Servo (moving): ~100-300mA
- **TOTAL: ~300-500mA**

### Power Options:

**Option 1: USB Power (Simple)**

- Computer USB port: works but may be unstable when servo moves
- USB power bank: better, provides more current

**Option 2: External Power (Recommended)**

- 9V DC adapter to Arduino barrel jack (best option)
- 7-12V battery pack to VIN pin

**If using external power:**

- You can still connect USB for Serial Monitor
- Arduino will automatically use external power
- Much more stable operation

-----

## Testing Checklist

### Initial Power-On:

- [ ] Arduino ON LED lights up (green)
- [ ] LCD backlight turns on
- [ ] LCD shows “Security System” then “Initializing…”
- [ ] LCD shows “Disarmed Dr:SHUT / Scan RFID Card”
- [ ] Servo moves to 0° position
- [ ] No alarms or beeping

### Test Alarm Card:

- [ ] Scan alarm card
- [ ] LCD shows “System Armed”
- [ ] Laser turns ON (red beam visible)
- [ ] One beep sounds
- [ ] LCD shows “Armed Dr:SHUT / Monitoring…”
- [ ] Scan again
- [ ] LCD shows “System Disabled”
- [ ] Laser turns OFF
- [ ] Two beeps sound

### Test Door Card:

- [ ] Scan door card
- [ ] LCD shows “Opening Door…”
- [ ] Servo rotates to 180°
- [ ] LCD shows “Disarmed Dr:OPEN”
- [ ] One beep sounds
- [ ] Scan again
- [ ] LCD shows “Closing Door…”
- [ ] Servo rotates to 0°
- [ ] Two beeps sound

### Test Alarm Trigger:

- [ ] Arm system (scan alarm card)
- [ ] Laser is ON
- [ ] Break laser beam with hand
- [ ] LCD shows “!!! ALARM !!!”
- [ ] Buzzer sounds rapidly
- [ ] Scan alarm card to disarm
- [ ] Alarm stops

### Test Both Together:

- [ ] Open door (scan door card)
- [ ] Arm alarm (scan alarm card)
- [ ] Close door (scan door card) - should NOT trigger alarm
- [ ] Break laser - alarm triggers
- [ ] Disarm (scan alarm card)

-----

## Safety Notes

- Do not point laser at eyes
- Laser is Class 2 (safe but avoid direct eye exposure)
- Always disconnect power before changing wiring
- RFID must use 3.3V - 5V will damage it permanently
- Servo can draw high current - use external power for stability

-----

## Support

If you encounter issues:

1. Check all wiring matches the guide exactly
1. Run individual test codes (RFID test, LCD test, etc.)
1. Check Serial Monitor for debug messages
1. Verify all libraries are installed
1. Make sure card UIDs are correct in code
1. Ensure LDR threshold is calibrated

-----

## What’s Next?

Want to expand your system? Here are ideas:

- Add more RFID cards for different users
- Add LED indicators for visual feedback
- Add keypad for PIN entry
- Add motion sensor (PIR) for area detection
- Log access attempts to SD card
- Add real-time clock for timestamps
- Control via Bluetooth or WiFi
- Add multiple servos for more doors

Let me know if you want to add any of these features!
