# RFID Laser Security System - Complete Wiring & Setup Guide
Wiring Instructions (Using Breadboard)
Breadboard Power Rails Setup:
Connect Arduino 5V to breadboard positive (+) rail (red)
Connect Arduino 3.3V to a separate row on breadboard (for RFID only)
Connect Arduino GND to breadboard negative (-) rail (blue/black)
MFRC522 RFID Reader:
SDA → Arduino Pin 10 (direct connection or via breadboard)
SCK → Arduino Pin 13 (direct connection or via breadboard)
MOSI → Arduino Pin 11 (direct connection or via breadboard)
MISO → Arduino Pin 12 (direct connection or via breadboard)
IRQ → (leave unconnected)
GND → Breadboard GND rail (-)
RST → Arduino Pin 9 (direct connection or via breadboard)
3.3V → Breadboard 3.3V row (IMPORTANT: Use 3.3V, NOT 5V!)
LCD1602 I2C Display:
GND → Breadboard GND rail (-)
VCC → Breadboard 5V rail (+)
SDA → Arduino A4 (direct connection or via breadboard)
SCL → Arduino A5 (direct connection or via breadboard)
KY-008 Laser Module:
S (Signal) → Arduino Pin 3 (direct connection or via breadboard)
Middle pin (+) → Breadboard 5V rail (+)
- (GND) → Breadboard GND rail (-)
LDR Photosensitive Sensor:
VCC → Breadboard 5V rail (+)
GND → Breadboard GND rail (-)
DO or AO (Analog Out) → Arduino A0 (direct connection or via breadboard)
Active Buzzer:
Positive (+) or I/O → Arduino Pin 8 (direct connection or via breadboard)
Negative (-) or GND → Breadboard GND rail (-)

Setup Instructions
Step 1: Install Required Libraries
In Arduino IDE:
Go to Sketch → Include Library → Manage Libraries
Search and install: "MFRC522" by GithubCommunity
Search and install: "LiquidCrystal I2C" by Frank de Brabander
Step 2: Upload the Code
Connect your Arduino UNO to your computer via USB
Copy the code from the Arduino Code artifact
Paste it into Arduino IDE
Select Tools → Board → Arduino UNO
Select Tools → Port → (your Arduino's COM port)
Click Upload (arrow button)
Step 3: Get Your RFID Card UIDs
Open Serial Monitor (Tools → Serial Monitor)
Set baud rate to 9600
Scan your RFID cards one at a time near the RFID reader
The system will show "Access Denied" on the LCD, but Serial Monitor will display the card UID
Note down the UID values (they look like: 0xDE, 0xAD, 0xBE, 0xEF)
Replace the dummy values in lines 25-26 of the code with your actual card UIDs
Upload the code again
Step 4: Calibrate the LDR Threshold
Position your laser module so it points directly at the LDR sensor
Open Serial Monitor (set to 9600 baud)
Watch the "LDR Value" readings:
When laser hits the sensor: value should be HIGH (typically 800-1000)
When beam is blocked: value should be LOW (typically 100-300)
Calculate a threshold between these values (e.g., if HIGH=900 and LOW=200, use 500)
Update line 23 in the code: int ldrThreshold = 500; (use your calculated value)
Upload the code again
Step 5: Physical Setup
Mount the laser and LDR sensor facing each other
Align them so the laser beam hits the LDR sensor directly
Ensure the beam crosses the area you want to protect
Test by breaking the beam with your hand

How the System Works
Initial State:
System starts DISABLED
LCD displays: "System Disabled" / "Scan RFID"
Laser is OFF
Arming the System:
Scan an authorized RFID card
You'll hear a single beep
LCD displays: "System Armed" / "Monitoring..."
Laser turns ON
Alarm Trigger:
If the laser beam is broken while armed
LCD displays: "!!! ALARM !!!" / "Breach Detected"
Buzzer sounds repeatedly (beep pattern)
Disarming the System:
Scan the same RFID card again
You'll hear two quick beeps
LCD displays: "System Disabled" / "Laser Off"
Laser turns OFF
Alarm stops
Unauthorized Card:
Scan an unauthorized card
LCD displays: "Access Denied!"
One long beep
System remains in current state

Troubleshooting
LCD shows nothing:
Check I2C address (try 0x3F instead of 0x27 in line 17 of code)
Check SDA/SCL connections to A4/A5
Test with I2C scanner sketch
RFID not reading:
Ensure RFID module is connected to 3.3V (NOT 5V)
Check all SPI connections (pins 10, 11, 12, 13)
Hold card close to reader (within 3cm)
Laser always triggering alarm:
Adjust ldrThreshold value (make it lower)
Ensure laser is aimed directly at LDR sensor
Check Serial Monitor for LDR readings
False alarms:
Increase ldrThreshold value
Shield LDR from ambient light
Ensure stable positioning of laser and LDR
Buzzer not working:
Check polarity (+ to pin 8, - to GND)
Try active buzzer instead of passive, or vice versa

Component Summary
You have all required components:
✓ Arduino UNO R3
✓ MFRC522 RFID Reader
✓ LCD1602 I2C Display
✓ Jumper wires (male-male, male-female, female-female)
✓ LDR Photosensitive Sensor
✓ KY-008 Laser Module
✓ Active Buzzer
✓ Breadboard
No additional components needed!
