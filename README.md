Project Description: SafeAir – IoT Based Gas Detection Hardware

SafeAir is a hardware system that detects hazardous gases in real time and sends alerts over the internet so users can act before a situation becomes dangerous. It is designed for homes, commercial kitchens, warehouses, biogas plants, and small industries where LPG, methane, carbon monoxide, or other combustible and toxic gases may leak. The goal is to combine reliable sensing, local alarms, and cloud connectivity in a low cost device that anyone can install.

Problem We Address
Traditional gas detectors only beep on site. If no one is present, a leak can go unnoticed until it causes fire, explosion, or poisoning. Industrial systems exist but are expensive, wired, and hard to scale. SafeAir solves this by using IoT components to provide continuous monitoring, remote alerts, and data logging at a price point suitable for households and MSMEs.

Hardware Architecture
Sensing Layer: The core uses a multi sensor array. An MQ-5 or MQ-6 sensor handles LPG and natural gas. An MQ-7 covers carbon monoxide. An MQ-135 adds air quality and ammonia detection for biogas or waste areas. Each sensor module is paired with a dedicated analog filter and calibration trimpot to reduce drift. A BME280 adds temperature, humidity, and pressure so readings can be temperature compensated.

Processing and Connectivity: An ESP32 microcontroller reads all analog values, applies a moving average, and runs a threshold engine. The ESP32 provides built in Wi-Fi and Bluetooth for setup. For sites with poor Wi-Fi, an optional SIM800L or NB-IoT module adds cellular uplink. A relay output can drive a solenoid valve to shut off gas supply, and a buzzer plus RGB LED provides local indication.

Power Design: The unit runs on 5V USB or 12V DC. A Li-ion backup cell with TP4056 charger keeps the device alive for 6 to 8 hours during outages. Deep sleep modes cut power when only periodic sampling is needed, extending backup time.

Enclosure and Safety: The PCB and sensors are housed in a flame retardant ABS enclosure with a louvered front for airflow and conformal coating on the board. The gas inlet is placed at the bottom because LPG is heavier than air, while a top vent helps for methane. The design follows basic IEC 60079 guidelines for sensor placement and avoids spark sources in the sensing chamber.

Cloud and Software
Data Flow: The ESP32 publishes JSON payloads over MQTT every 10 seconds, or immediately when a threshold is crossed. Topics are structured by device ID and location. A Node.js backend with Mosquitto broker stores time series data in InfluxDB and device metadata in PostgreSQL.

Alerting Logic: Three alert levels are used. Level 1 is early warning when gas crosses 10 percent of LEL. Level 2 is danger at 25 percent LEL and triggers the buzzer and relay. Level 3 is critical at 50 percent LEL and triggers repeated calls. Alerts go out via push notification, SMS, and automated voice call using Twilio. Users can add multiple phone numbers and set quiet hours.

Dashboard and App: A React web dashboard and a Flutter mobile app show live PPM values, historical graphs, device health, and calibration dates. Users can set custom thresholds, test the alarm, and download monthly safety reports for compliance. The app uses BLE for first time Wi-Fi provisioning so installation needs no laptop.

Calibration and Maintenance
Each device ships factory calibrated with a gas reference. The firmware tracks sensor hours and prompts for recalibration every six months. A field calibration mode lets a technician use a test gas and two point calibration through the app. The device also runs self diagnostics for heater failure and reports it as a fault code.

Deployment Use Cases
In a home kitchen, SafeAir mounts 30 cm above the floor near the cylinder and shuts off the supply if LPG exceeds 20 percent LEL. In a hostel mess, multiple units link to one gateway and notify the warden. In a biogas plant, methane and H2S levels are logged to ensure digester health and worker safety.

Roadmap
Phase 1: Wi-Fi only units for LPG and CO in residential use.  
Phase 2: Cellular variant, Modbus output for PLC integration, and solar option for remote sites.  
Phase 3: LoRaWAN version for campus wide deployment with a single gateway.

SafeAir turns gas safety from a passive beeper into an active, connected system. It gives seconds of warning when minutes matter, and keeps a record so leaks can be prevented, not just detected.
