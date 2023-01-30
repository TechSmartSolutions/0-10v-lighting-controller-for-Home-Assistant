# 0-10v PWM LED Dimming Controller (includes Home Assistant Integration)

<p align="center">
```diff
! Warning !
```
</p>

This repository provides the code and instructions to build a custom LED dimming controller using 0-10v pulse-width modulation (PWM) technology including the code and setup to integrate with Home Assistant. The controller allows for precise adjustment of LED light brightness and is a cost-effective and personalized solution compared to commercially available dimming options.  Includes stl for 3D printed case.

## Pre-requisites
- [Home Assistant](https://www.home-assistant.io/) server
- LED Drivers (or Ballasts if you're in the UK) **with DIM wires that support 0-10v PWM** dimming.  
It's super important you understand what that bold text above means.  This controller is not to be connected on the same circuit as the LEDs themselves, or the 120/240V circuit. If you dont understand this, **STOP!** In our project we are using Meanwell HLG LED Drivers.

## Parts for the build
- [ESP32](https://www.google.com/search?q=ESP32+development+boards) or [ESP8266](https://www.google.com/search?q=ESP8266+development+boards) development board with [SPI pins](https://www.google.com/search?q=spi+pins)  
This is the brain of our controller setup. It has built-in Wi-Fi and can easily connect to your Home Assistant server using the ESPHome plugin. And the best part? You can use a cheap option like the [Wemos D1 Mini ESP8266](https://www.google.com/search?q=wemos+mini+d1) we will be using in this project.  
    <img src="/images/esp8266%20wemos%20d1%20mini.jpg" height="150">

- [10v AC/DC Wall Plug Adapter](https://www.digikey.ca/en/products/detail/globtek-inc/WR9HU1800LCP-F-R6B/10187591)  
For power, we'll use a AC/DC wall plug similar to this. Technically, all we need is a 10V power supply, so feel free to use a different type if you prefer.  The advantage of a power adapter with a round barrel plug like this one is that it will allow us to easily disconnect the power without having to unplug the controller entirely.  
    <img src="/images/10v%20AC-DC%20Wall%20Plug%20Adapter.jpg" height="150">

- [Adafruit TLC5947 **24-Channel** 12-bit PWM LED Driver](https://www.adafruit.com/product/1429)
or [Adafruit TLC59711 **12-Channel** 16-bit PWM LED Driver](https://www.adafruit.com/product/3995)  
These boards are capable of generating some serious PWM signals, all the way up to 30V! The PWM voltage it generates is determined by the input voltage, which is why we're using a 10V power supply.  
    <img src="/images/Adafruit%2024%20channel%20PWM%20LED%20driver.jpg" height="150"> <img src="/images/Adafruit%2012%20channel%20PWM%20LED%20driver.jpg" height="150">

- 2x [Break-Away 0.1" 2x20-pin Strip Dual Male Headers](https://www.google.com/search?q=Break-Away+0.1%22+2x20-pin+Strip+Dual+Male+Header) or 8 3x2 pin headers  
You will solder these terminal pin headers onto the PWM board. This is where the positive (+) and negative (-) DIM wire leads from your LED Driver will connect.  
    <img src="/images/Break-Away%200.1-inch%202x20-pin%20Strip%20Dual%20Male%20Header.jpg" height="150">

- [LM2596 DC to DC Voltage Regulator 4-40V to 1.5-35V Buck Converter with LED Display](https://www.google.com/search?q=LM2596+DC+to+DC+Voltage+Regulator+4-40V+to+1.5-35V+Buck+Converter+with+LED+Display)  
So we know that we're using the 10V power source to feed the PWM baord, but wouldn't it be nice if we could use that same power supply to power the ESP controller to? Well, we can! The only catch is that the ESP runs on 5V, and 10V is just too high. That's where this regulator board comes in - it lets us connect the 10V source and then dial it down to the perfect 5V for our ESP.  
    <img src="/images/LM2596-DC-to-DC-Voltage-Regulator.png" height="150">



- _wagos/wirenuts_
- _project box (need size)_

# To be added:
## Assembly Instructions
## Software Configuration
## Usage
