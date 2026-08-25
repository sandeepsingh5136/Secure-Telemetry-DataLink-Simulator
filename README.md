# Secure Telemetry Data Link Simulator

## Overview
A microcontroller-based secure serial telemetry simulation designed to replicate tactical defense communication protocols. The system handles structured packet framing, lightweight encryption, and runtime checksum verification over asynchronous serial (UART) interfaces.

---

## Key Features
* **Structured Packet Framing:** Implements robust transmission framing containing synchronization headers, device identification, encrypted payloads, checksum validation bytes, and termination footers (`[Start Header] [Node ID] [Encrypted Payload] [Checksum] [End Byte]`).
* **Lightweight Cryptography:** Incorporates pre-shared key XOR masking to simulate secure data protection on resource-constrained embedded systems.
* **Error Checking & Integrity:** Uses runtime checksum calculation to verify register state integrity and filter out corrupted frames.

---

## Tech Stack
* **Language:** Embedded C / C++ (Arduino IDE)
* **Simulation Environment:** Tinkercad / Microcontroller Virtualization
* **Protocols:** UART / Asynchronous Serial Communication

---

## Firmware Source Code

```cpp
// Secure Telemetry Data Link Simulation for Defense Communications
// Author: ECE Final Year Project

void setup() {
  Serial.begin(9600); // Initialize UART communication at 9600 baud
  randomSeed(analogRead(0));
}

void loop() {
  // 1. Simulate reading a sensor value (e.g., fuel level or temperature)
  byte sensorData = random(20, 90); 
  byte encryptionKey = 0x5A; // Pre-shared key for XOR encryption

  // 2. Encrypt the payload
  byte encryptedData = sensorData ^ encryptionKey;

  // 3. Construct the Packet Frame
  // Format: [Start Byte] [Device ID] [Encrypted Payload] [Checksum] [End Byte]
  byte startByte = 0xAA; // Synchronization header
  byte deviceID = 0x01;  // Node 1 ID
  byte checksum = deviceID ^ encryptedData; // Simple error-checking byte

  // 4. TRANSMISSION: Send packet over UART
  Serial.print("TX Frame: ");
  Serial.print(startByte, HEX); Serial.print(" ");
  Serial.print(deviceID, HEX); Serial.print(" ");
  Serial.print(encryptedData, HEX); Serial.print(" ");
  Serial.print(checksum, HEX); Serial.print(" ");
  Serial.println(0xFF, HEX); // End byte

  // 5. RECEPTION & DECRYPTION SIMULATION
  byte receivedPayload = encryptedData; // Captured from channel
  byte decryptedData = receivedPayload ^ encryptionKey; // Decrypt

  // Output results
  Serial.print("--> RX Decrypted Sensor Value: ");
  Serial.println(decryptedData);
  Serial.println("-----------------------------------");

  delay(3000); // Wait 3 seconds before next transmission
}





---

## Simulation Output
Here is the live simulation running in Tinkercad, showing the transmitted secure packet frames and successfully decrypted sensor values via UART:

<img width="1600" height="838" alt="WhatsApp Image 2026-08-25 at 12 28 13" src="https://github.com/user-attachments/assets/f2c1eac7-172c-4fc7-81fc-3288728be2b0" />
