# APEX (Avionics Protocol Embedded eXperiment)

**In active Development**

I am currently designing and developing a multi-protocol embedded sensor fusion node on an ESP32, integrating environmental, inertial, and GPS sensing across I2C, SPI, and UART interfaces. Architecture includes WiFi telemetry over MQTT and a FreeRTOS task-based firmware design, targeting real-world flight computer sensor stack patterns.

##System Overview

**ESP32** Used for central processing, WiFi/BLE capable for telemetry, dual-core for FreeRTOS task partitioning

**BME280** Environmental Sensor (temp/humidity/pressure) communicated via I2C

**ISM330DHCX** 6-axis accel/gyro senor communicated via SPI

**NEO-6M** GPS module communicated via UART


## Status

-Commponent research and selection
**-Compoenent testing on breadboard and testing proliminary embedded code.**
-Complete system intergation
-Create custom PCB using best practices.
