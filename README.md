#include <Wire.h>
#include <Adafruit_CCS811.h>

// Create sensor object
Adafruit_CCS811 ccs;

// Define pin for Plasma module (via MOSFET)
const int plasmaPin = 13;  // You can change this to any GPIO pin

// Air quality thresholds
const int CO2_THRESHOLD = 800;   // ppm
const int TVOC_THRESHOLD = 300;  // ppb

void setup() {
  Serial.begin(9600);
  delay(1000);
  
  // Initialize CCS811 sensor
  if (!ccs.begin()) {
    Serial.println("CCS811 sensor not found. Check wiring!");
    while (1);
  }

  // Wait for sensor to be ready
  while (!ccs.available());

  // Set plasma pin as output
  pinMode(plasmaPin, OUTPUT);
  digitalWrite(plasmaPin, LOW); // Turn off plasma initially
}

void loop() {
  if (ccs.available()) {
    if (!ccs.readData()) {
      uint16_t co2 = ccs.geteCO2();
      uint16_t tvoc = ccs.getTVOC();

      Serial.print("CO2: ");
      Serial.print(co2);
      Serial.print(" ppm, TVOC: ");
      Serial.print(tvoc);
      Serial.println(" ppb");

      // Control plasma purifier based on threshold
      if (co2 > CO2_THRESHOLD || tvoc > TVOC_THRESHOLD) {
        digitalWrite(plasmaPin, HIGH); // Turn on purifier
        Serial.println("Plasma purifier ON");
      } else {
        digitalWrite(plasmaPin, LOW);  // Turn off purifier
        Serial.println("Plasma purifier OFF");
      }
    } else {
      Serial.println("Error reading sensor.");
    }
  }

  delay(2000);  // Wait 2 seconds before next reading
}

chandan73-ark/chandan73-ark is a ✨ special ✨ repository because its `README.md` (this file) appears on your GitHub profile.
You can click the Preview link to take a look at your changes.
--->
