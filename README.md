# 0-10v PWM LED Dimming Controller (includes Home Assistant Integration)

This repository provides the code and instructions to build a custom LED dimming controller using 0-10v pulse-width modulation (PWM) technology including the code and setup to integrate with Home Assistant. The controller allows for precise adjustment of LED light brightness and is a cost-effective and personalized solution compared to commercially available dimming options.  Includes stl for 3D printed case.

## Parts Needed
- [ESP32](https://www.google.com/search?q=ESP32+development+boards) or [ESP8266](https://www.google.com/search?q=ESP8266+development+boards) development board with [SPI pins](https://www.google.com/search?q=spi+pins)  
This is the main brain for the controller. It has built in Wi-Fi and will connect to your Home Assistant server via the ESPHome plugin.  For this project we will use a cheap [Wemos D1 Mini ESP8266](https://www.google.com/search?q=wemos+mini+d1)  
    <img src="/images/esp8266%20wemos%20d1%20mini.jpg" height="150">

- [10v AC/DC Wall Plug Adapter](https://www.digikey.ca/en/products/detail/globtek-inc/WR9HU1800LCP-F-R6B/10187591)  
This is the type of plug we will use to power our version of this project.  In theroy, all you need is a 10v power supply, so it could be a different style than this one.  We will use a wire terminal to splice this to both the Adafruit PWM board and the LM2596 voltage regulator board.  
    <img src="/images/10v%20AC-DC%20Wall%20Plug%20Adapter.jpg" height="150">

- [Adafruit TLC5947 **24-Channel** 12-bit PWM LED Driver](https://www.adafruit.com/product/1429)
or [Adafruit TLC59711 **12-Channel** 16-bit PWM LED Driver](https://www.adafruit.com/product/3995)  
These boards are capable of generating PWM signals up to 30v! The PWM voltage that the board produces is based on the voltage you feed to it (hence the 10v power supply).  
    <img src="/images/Adafruit%2024%20channel%20PWM%20LED%20driver.jpg" height="150"> <img src="/images/Adafruit%2012%20channel%20PWM%20LED%20driver.jpg" height="150">

- [LM2596 DC to DC Voltage Regulator 4-40V to 1.5-35V Buck Converter with LED Display](https://www.google.com/search?q=LM2596+DC+to+DC+Voltage+Regulator+4-40V+to+1.5-35V+Buck+Converter+with+LED+Display)  
Since we already have a power supply for the PWM board, it seems to make the most sense that we use that to power the ESP, only problem is that the ESP is powered by 5v (usually the USB power cable).  By using this regulator board, we can connect a 10v line to it and down-regulate it to 5v so that we can safely power the ESP.  
    <img src="/images/LM2596-DC-to-DC-Voltage-Regulator.png" height="150">

- 2x [Break-Away 0.1" 2x20-pin Strip Dual Male Headers](https://www.google.com/search?q=Break-Away+0.1%22+2x20-pin+Strip+Dual+Male+Header) or 8 3x2 pin headers  
We will cut these down to 3x2 segments and solder them onto the PWM board.  
    <img src="/images/Break-Away%200.1-inch%202x20-pin%20Strip%20Dual%20Male%20Header.jpg" height="150">


- _wagos/wirenuts_
- _project box (need size)_

# To be added:
## Assembly Instructions
## Software Configuration
## Usage
