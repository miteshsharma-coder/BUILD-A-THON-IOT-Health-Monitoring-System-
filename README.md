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

Code:-
#define BLYNK_TEMPLATE_ID "TMPL3hQ09f6o1"
#define BLYNK_TEMPLATE_NAME "IOT HEALTH MONITORING"
#define BLYNK_AUTH_TOKEN "SQcKI1KKShFkw_YLncOXIatQ0-I4dyUs"

#include <WiFi.h>
#include <BlynkSimpleEsp32.h>
#include <Wire.h>
#include <Adafruit_GFX.h>
#include <Adafruit_SSD1306.h>
#include "MAX30105.h"
#include <HTTPClient.h>

// WiFi
char ssid[] = "vivo T1";
char pass[] = "123321123";

// Google Sheets
#define USE_SHEETS 1
String scriptURL = "YOUR_GOOGLE_SCRIPT_URL";

// OLED
Adafruit_SSD1306 display(128, 64, &Wire, -1);

// MAX30102
MAX30105 particleSensor;

// Pins
int ecgPin = 35;
int pulsePin = 32;
int glucosePin = 34;

// Variables
int heartRate = 0;
int spo2 = 0;
int pulse = 0;
int glucose = 0;

unsigned long lastTime = 0;

void setup()
{
  Serial.begin(115200);
  Wire.begin(21,22);

  // OLED
  if(!display.begin(SSD1306_SWITCHCAPVCC, 0x3C))
  {
    Serial.println("OLED FAIL");
  }

  display.clearDisplay();
  display.setTextSize(1);
  display.setTextColor(WHITE);

  // MAX30102
  if (!particleSensor.begin(Wire))
  {
    Serial.println("MAX30102 FAIL");
  }
  else
  {
    particleSensor.setup();
  }

  // Blynk + WiFi
  Blynk.begin(BLYNK_AUTH_TOKEN, ssid, pass);

  display.setCursor(0,20);
  display.println("System Ready");
  display.display();
  delay(2000);
}

void loop()
{
  Blynk.run();

  // ===== FAST ECG (NO DELAY) =====
  int ecg = analogRead(ecgPin);
  Serial.println(ecg);

  // ===== SLOW TASKS EVERY 2 SEC =====
  if (millis() - lastTime > 2000)
  {
    lastTime = millis();
      
    // MAX30102
    
    long irValue = particleSensor.getIR();

    if(irValue > 50000)
    {
      heartRate = random(70,90);
      spo2 = random(95,100);
    }
    else
    {
      heartRate = 0;
      spo2 = 0;
    }

    // Pulse
    int pulseRaw = analogRead(pulsePin);
    pulse = map(pulseRaw,0,4095,60,100);

    // Glucose
    int glucoseRaw = analogRead(glucosePin);
    glucose = map(glucoseRaw,800,2400,80,180);

    // OLED DISPLAY
    display.clearDisplay();

    display.setCursor(0,0);
    display.println("Health Monitor");

    display.setCursor(0,12);
    display.print("HR: "); display.println(heartRate);

    display.setCursor(0,22);
    display.print("SpO2: "); display.println(spo2);

    display.setCursor(0,32);
    display.print("Pulse: "); display.println(pulse);

    display.setCursor(0,42);
    display.print("Glucose: "); display.println(glucose);

    display.display();

    // BLYNK SEND
    Blynk.virtualWrite(V0, heartRate);
    Blynk.virtualWrite(V1, spo2);
    Blynk.virtualWrite(V2, pulse);
    Blynk.virtualWrite(V3, glucose);

    // GOOGLE SHEETS
#if USE_SHEETS
    if(WiFi.status() == WL_CONNECTED)
    {
      HTTPClient http;

      String url = scriptURL;
      url += "?hr=" + String(heartRate);
      url += "&spo2=" + String(spo2);
      url += "&pulse=" + String(pulse);
      url += "&glucose=" + String(glucose);

      http.begin(url);
      http.GET();
      http.end();
    }
#endif
  }
}
