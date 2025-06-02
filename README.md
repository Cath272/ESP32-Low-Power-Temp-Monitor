# ESP32 Low Power Temp Monitor

## Introduction
ESP32 Low Power Temp Monitor is an ESP32 C6 based low power Temperature and Humidity Monitor that aims to have a device that is very efficent and doen't need charging.
## General description
The device reads the temperature, relative humidity and barometric pressure  and displays it on an 4.2 inch Epaper Display, alonside the current date and time.
The device is awake just for a few seconds as it reads the tempereature and displays it and then goes to sleep. It performs a reading every minute.
One a day it connects to WiFi and checks the current time to sync up with an NTP server to account for Daylight saving time and potential drifts in the internal esp32 real time clock.
### Block diagram
![block diagram](https://github.com/user-attachments/assets/7f155fd0-e7f8-4665-8b3d-cabae4bbd585)

## Hardware Design
### ESP32 C6 Wroom 1 Module
This project is based around a ESP32 C6 Wroom 1 Module that incorporates a RISC-V single-core microprocessor with 4MB of flash, WiFi 6 and Zigbee
### TPS63020
The TPS63020 is High Efficiency Single Inductor Buck-Boost Converter, used for  converting the 5v from usb or the Li-ion battery voltage(3.0v to 4.2v) to a very stable 3.3V for the Microcontroller and the epaper display. The main advantage of this ic is its low operating quiescent current: 25 μA; and it's low cost:0.55€
### BME280
The BME280 is a digital humidity, pressure and temperature sensor.
### 4.2 Inch Epaper Display 
GDEY042Z98 Tri color (Black, White and Red) Epaper display using a SSD1683 driver chip, the main benefit of this display is its big size (4.2 inch) and very good price for an Epaper display (around 10€)
### TPL5111
The TPL5111 is a Nano Power System timer for power gating. It is used to turn on the esp32 every minute to read the temperature
### MCP7381
The MCP7381 is an integrated Li-ion cell battery controller 
### Li-ion 250MAh battery
Small 250Mah generic Li-ion cell with protection, used because of its small size
### Solar Panel
Generic 1W 5V Solar panel 

## Schematic

## Bill of Materials

| Component           |         Image                                                                                         |         Model  |           Pcs | Price           | Link          |
| -------------       |                                                                                        -------------  | -------------  | ------------- | --------------- | ------------- |
| ESP32-C6-WROOM-1-N8  |  ![Esp32 c6](https://assets.lcsc.com/images/lcsc/900x900/20240825_Espressif-Systems-ESP32-C6-WROOM-1-N8_C5366877_blank.jpg)        | ESP32-C6-WROOM-1-N8 | 1             | € 4.91          |   [LCSC](https://www.lcsc.com/product-detail/WiFi-Modules_Espressif-Systems-ESP32-C6-WROOM-1-N8_C5366877.html?s_z=n_esp32-c6-wroom)  |
|Buck Boost Coverter  | ![Screenshot ](https://github.com/user-attachments/assets/268663cf-1794-42c4-8324-cbcd939803ec)       |   TPS63020     | 1             | 3.5EU10/Pcs 0.35EUR/Pcs          | [LCSC](https://www.lcsc.com/product-detail/Timers-Counters_Texas-Instruments-TPL5111DDCR_C2870554.html?s_z=n_tpl5111)) |
| TPL5111        | ![images](https://github.com/user-attachments/assets/ea21eece-a85f-4848-8467-17d25432dda0)            | TPL5111         | 1             | € 0.47       | [LCSC](https://www.lcsc.com/product-detail/Timers-Counters_Texas-Instruments-TPL5111DDCR_C2870554.html?s_z=n_tpl5111) |       

## PCB




## Software Design

### Display
The program is based around the [GxEPD2](https://github.com/ZinggJM/GxEPD2) libary for interfacing with the Epaper display,   

### Fonts
The font used in this project is Roboto with 2 diffrent font sizes: 16pt for the time, date, altitude and barometric pressure and 48pt for the Temeprature and relative humidity
The font were converted from True Type to Adafruit's GFX Font format (also used by GxEPD2 as it is based on Adafruit GFX) using the [ttf2gfx utility](https://github.com/Pconti31/TTF2GFX)

### BME280
Adafruit_BME280 for the BME280 sensor
### Time 
For time i used 4 libraries: 
- esp_timer (esp_idf library) to get the execution time of the code 
- [ESP32Time](https://github.com/fbiego/ESP32Time) for keeeping the time and displaying it in a C style format on the display
- [ezTime](https://github.com/ropg/ezTime) for getting the time from an NTP server, the time is region based in this library and i used it to avoid the problem of keeping track of daylight savings
 


## Results

## Resources
- [ESP32Time](https://github.com/fbiego/ESP32Time)
- [ttf2gfx utility](https://github.com/Pconti31/TTF2GFX)
- [ezTime](https://github.com/ropg/ezTime)
- [GxEPD2](https://github.com/ZinggJM/GxEPD2)
