# APEX (Avionics Protocol Embedded eXperiment)

**In active Development**

I am currently designing and developing a multi-protocol embedded sensor fusion node on an ESP32, integrating environmental, inertial, and GPS sensing across I2C, SPI, and UART interfaces. Architecture includes WiFi telemetry over MQTT and a FreeRTOS task-based firmware design, targeting real-world flight computer sensor stack patterns.

## System Overview

**ESP32:** Used for central processing, WiFi/BLE capable for telemetry, dual-core for FreeRTOS task partitioning

**BME280:** Environmental Sensor (temp/humidity/pressure) communicated via I2C

**ISM330DHCX:** 6-axis accel/gyro senor communicated via SPI

**NEO-6M:** GPS module communicated via UART


## Project Status

Phase 1: Component Selection & Research - **Completed**

**Phase 2: Breadboard Bring-Up & Individual Driver Development - In Process**

Phase 3: Full System Integration - Planned

Phase 4: Custom PCB Design & Fabrication - Planned
