# BUILD-A-THON-IOT-Health-Monitoring-System-
Developed an IoT-based Health Monitoring System using ESP32 and multiple biomedical sensors to monitor Heart Rate, SpO₂, ECG, Pulse Rate, and Glucose levels in real time. The system displays data on an OLED screen and transmits it to Blynk and Google Sheets for remote monitoring, cloud storage, and future health analysis. 

Requirements:-
📋 Hardware Requirements
ESP32 Development Board
MAX30102 Heart Rate & SpO₂ Sensor
AD8232 ECG Sensor Module
Pulse Sensor
TEMT6000 Sensor
OLED Display (SSD1306, 128×64)
PCB and Power Supply Circuit
Connecting Wires
USB Cable

💻 Software Requirements
Arduino IDE
ESP32 Board Package
Blynk IoT Platform
Google Sheets (Cloud Database)
Google Apps Script
Serial Plotter
Required Arduino Libraries (WiFi, Blynk, Adafruit SSD1306, Adafruit GFX, MAX30105)

🌐 Functional Requirements
Real-time health parameter monitoring
OLED display visualization
ECG waveform plotting
WiFi connectivity
Cloud data storage

Working:-
1.The user places the sensors properly to acquire physiological signals.
2.The MAX30102 sensor measures Heart Rate and SpO₂ using the PPG (Photoplethysmography) technique.
3.The AD8232 sensor captures the electrical activity of the heart and generates the ECG waveform.
4.The Pulse Sensor detects pulse rate by sensing blood flow variations.
5.The TEMT6000 sensor is used to estimate glucose levels for demonstration purposes.
6.The ESP32 microcontroller collects data from all sensors and processes the readings.
7.The measured values are displayed in real time on the OLED display.
8.The ECG signal is sent to the Serial Plotter for waveform visualization.
9.Using the built-in WiFi of ESP32, the data is transmitted to Blynk IoT for remote monitoring.
10.The same data is also stored in Google Sheets through Google Apps Script for cloud-based logging and future analysis.
11.The system continuously updates the readings, enabling real-time health monitoring and remote access to patient data.
12.Mobile dashboard monitoring
13.Continuous sensor data acquisition

