# ESP32 Low Power Temp Monitor

## Introduction
ESP32 Low Power Temp Monitor is an ESP32 C6 based low power Temperature and Humidity Monitor that aims to have a device that is very efficent and doen't need charging.
## General description
The device reads the temperature, relative humidity and barometric pressure  and displays it on an 4.2 inch Epaper Display, alonside the current date and time.
The device is awake just for a few seconds as it reads the tempereature and displays it and then goes to sleep. It performs a reading every minute.
One a day it connects to WiFi and checks the current time to sync up with an NTP server to account for Daylight saving time and potential drifts in the internal esp32 real time clock.

In the project files you may find ESP32-C3 instead of ESP32-C6. I named it wrong when i created the project. The issue is just the file names, the footprints are correct and use a C6.
The TPL5111 is currenly does not work properly so i emulated it in software using the esp32 deep sleep.

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

| Component           |         Image                                                                                         |         Model  |           Pcs | Price           | 
| -------------       |                                                                                        -------------  | -------------  | ------------- | --------------- |
| ESP32-C6-WROOM-1-N8  |  ![Esp32 c6](https://assets.lcsc.com/images/lcsc/900x900/20240825_Espressif-Systems-ESP32-C6-WROOM-1-N8_C5366877_blank.jpg)        | ESP32-C6-WROOM-1-N8 | 1             | € 4.91          |
|Buck Boost Coverter  | ![Screenshot ](https://github.com/user-attachments/assets/268663cf-1794-42c4-8324-cbcd939803ec)       |   TPS63020     | 1             | € 0.52          | 
| TPL5111        | ![images](https://assets.lcsc.com/images/lcsc/900x900/20230122_Texas-Instruments-TPL5111DDCR_C2870554_front.jpg)            | TPL5111         | 1             | € 0.47       |     
| BME280        | ![images](https://assets.lcsc.com/images/lcsc/900x900/20230217_Bosch-Sensortec-BME280_C92489_front.jpg)            | BME280         | 1             | € 2.78      |
| MCP7381        | ![images](https://assets.lcsc.com/images/lcsc/900x900/20180914_Microchip-Tech-MCP73812T-420I-OT_C144308_front.jpg)            | MCP7381         | 1             | € 0.84      |
| Solar Panel        | ![images](https://ae-pic-a1.aliexpress-media.com/kf/Sfc9f51dd00154d2a8cfb73cd2f78d38bW.jpg_960x960q75.jpg_.avif)            | 5V 1W Solar Panel         | 1             | € 2.23       |
| 4.2 Inch Epaper Display | ![images](https://ae-pic-a1.aliexpress-media.com/kf/Sac4ad89e401f44f3a886430887d86d69h.jpg_960x960q75.jpg_.avif)            | GDEY042Z98         | 1             | € 11.64       |


## PCB

![front_3d](https://github.com/user-attachments/assets/d808d23c-5207-4c4c-b3c6-4fadc37c807d)
![front](https://github.com/user-attachments/assets/d5231514-abba-429d-a512-3c7c42c4a00e)


## Case and 3d design

The case was make using autocad and exported to stl
The stl files can be found in the 3D Parts folder


The gerber production zip file can be found in the Production folder
## Software Design

The device wakes up, connect to wifi on the first boot, reads the time from an NTP server, and then for the next 24 hours it just reads the temperature and displays it. The time is kept in a register while the esp sleeps and increments it on every boot. Every 24 hours it connect to Wifi to check the time again for accuracy.
The program can be found in the Project_Code folder

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
![20250605_153828](https://github.com/user-attachments/assets/c3cd7984-27e2-4c95-8f2e-7cf2528b2a08)
![20250605_153837](https://github.com/user-attachments/assets/1b07f9c0-f66d-4d23-aac5-b7c0be39ec1e)

## Resources
- [ESP32Time](https://github.com/fbiego/ESP32Time)
- [ttf2gfx utility](https://github.com/Pconti31/TTF2GFX)
- [ezTime](https://github.com/ropg/ezTime)
- [GxEPD2](https://github.com/ZinggJM/GxEPD2)
