```diff
- By continuing to read this repo you agree to the following disclaimer -
```
**Disclaimer:** The author of this repository assumes no liability for any damage or injury caused by following these instructions or using the finished product. Building and using this controller requires technical knowledge and experience with electrical circuits and components. If you are unsure about any aspect of this project, it is strongly recommended that you seek assistance from a knowledgeable individual or consult relevant resources before proceeding. The author does not guarantee the accuracy or reliability of the information provided and assumes no responsibility for any resulting damages or injuries.

# 0-10v PWM LED Dimming Controller (includes Home Assistant Integration)

This repository provides the code and instructions to build a custom LED dimming controller using 0-10v pulse-width modulation (PWM) technology including the code and setup to integrate with Home Assistant. The controller allows for precise adjustment of LED light brightness and is a cost-effective and personalized solution compared to commercially available dimming options.  Includes stl for 3D printed case.

## Pre-requisites
- [Home Assistant](https://www.home-assistant.io/) server with [ESPHome](https://esphome.io/) installed
- LED Drivers (or Ballasts if you're in the UK) **with DIM wires that support 0-10v PWM** dimming.

## Tools Required
Although having top-of-the-line tools makes the job easier, I mostly rely on economical options for my toolkit. While working with budget tools may not always be optimal, they are adequate for small and occasional projects and have served me well.
- [Soldering iron](https://www.amazon.ca/s?k=Soldering+iron) ([brass wool](https://www.amazon.ca/s?k=brass+wool+soldering) recommended instead of using a wet solder sponge).
- [Wire stripper](https://www.amazon.ca/s?k=wire+stripper)
- [Multimeter](https://www.amazon.ca/s?k=Multimeter&rh=n%3A3006902011%2Cp_36%3A12035760011&dc&ds=v1%3AVJ07OZbfZbPTVCeOY0%2FOTVw2%2FDdTF6NoXoZ9MvBP20c&qid=1675112740&rnid=12035759011&ref=sr_nr_p_36_1) (for testing connections)
- [Hot glue gun](https://www.amazon.ca/s?k=Hot+glue+gun) (for securing components in case)

## Parts for the build
- [ESP32](https://www.google.com/search?q=ESP32+development+boards) or [ESP8266](https://www.google.com/search?q=ESP8266+development+boards) development board with [SPI pins](https://www.google.com/search?q=spi+pins)  
This is the brain of our controller setup. It has built-in Wi-Fi and can easily connect to your Home Assistant server using the ESPHome plugin. And the best part? You can use a cheap option like the [Wemos D1 Mini ESP8266](https://www.google.com/search?q=wemos+mini+d1) we will be using in this project.  
    <img src="/images/esp8266%20wemos%20d1%20mini.jpg" height="150">

- [10v AC/DC Wall Plug Adapter](https://www.digikey.ca/en/products/detail/globtek-inc/WR9HU1800LCP-F-R6B/10187591)  
For power, we'll use a AC/DC wall plug similar to this. Technically, all we need is a 10V power supply, so feel free to use a different type if you prefer.  The advantage of a power adapter with a round barrel plug like this one is that it will allow us to easily disconnect the power from the controller without having to unplug the power adapter from the wall.  
    <img src="/images/10v%20AC-DC%20Wall%20Plug%20Adapter.jpg" height="150">

- [5.5mm x 2.5mm Panel Mount Female DC Power Supply Barrel Connector](https://www.google.com/search?q=DC+Power+Extension+Cable+5.5+mm+x+2.5+mm+Male+to+Female+Connector%2C+DC+Power+Cord+Extension+Cable+for+Power&oq=DC+Power+Extension+Cable+5.5+mm+x+2.5+mm+Male+to+Female+Connector%2C+DC+Power+Cord+Extension+Cable+for+Power)  
This connector provides a port to plug in the power adapter. The choice of a connector like this has benefits such as: 1) Improved safety - If the power cord is pulled hard, the cable will simply disconnect from the controller, preventing damage to both you and the controller. 2) Convenient power control - To connect or disconnect the LED Drivers to the dimming controller, it is necessary to power off the controller and drivers. With this type of connector, you can easily turn off the power without having to unplug it from the wall. 
    <img src="/images/5.5mm%20x%202.5mm%20Panel%20Mount%20Female%20DC%20Power%20Supply%20Barrel%20Connector.jpg" height="150">

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
