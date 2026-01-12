# RFID Laser Security System - Complete Wiring & Setup Guide

## Wiring Instructions (Using Breadboard)

### Breadboard Power Rails Setup:
Connect Arduino 5V to breadboard positive (+) rail (red)  
Connect Arduino 3.3V to a separate row on breadboard (for RFID only)  
Connect Arduino GND to breadboard negative (-) rail (blue/black)  

### MFRC522 RFID Reader:
SDA → Arduino Pin 10 (direct connection or via breadboard)  
SCK → Arduino Pin 13 (direct connection or via breadboard)  
MOSI → Arduino Pin 11 (direct connection or via breadboard)  
MISO → Arduino Pin 12 (direct connection or via breadboard)  
IRQ → (leave unconnected)  
GND → Breadboard GND rail (-)  
RST → Arduino Pin 9 (direct connection or via breadboard)  
3.3V → Breadboard 3.3V row (IMPORTANT: Use 3.3V, NOT 5V!)  

### LCD1602 I2C Display:
GND → Breadboard GND rail (-)  
VCC → Breadboard 5V rail (+)  
SDA → Arduino A4 (direct connection or via breadboard)  
SCL → Arduino A5 (direct connection or via breadboard)  

### KY-008 Laser Module:
S (Signal) → Arduino Pin 3 (direct connection or via breadboard)  
Middle pin (+) → Breadboard 5V rail (+)  
- (GND) → Breadboard GND rail (-)  

### LDR Photosensitive Sensor:
VCC → Breadboard 5V rail (+)  
GND → Breadboard GND rail (-)  
DO or AO (Analog Out) → Arduino A0 (direct connection or via breadboard)  

### Active Buzzer:
Positive (+) or I/O → Arduino Pin 8 (direct connection or via breadboard)  
Negative (-) or GND → Breadboard GND rail (-)  

## Setup Instructions

### Step 1: Install Required Libraries
In Arduino IDE:  
Go to Sketch → Include Library → Manage Libraries  
Search and install: "MFRC522" by GithubCommunity  
Search and install: "LiquidCrystal I2C" by Frank de Brabander  

### Step 2: Upload the Code
Connect your Arduino UNO to your computer via USB  
Copy the code from the Arduino Code artifact  
Paste it into Arduino IDE  
Select Tools → Board → Arduino UNO  
Select Tools → Port → (your Arduino's COM port)  
Click Upload (arrow button)  
