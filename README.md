# 12 or 24+ Channel Dimming Controller for Meanwell LED Power Supplies (works with many other manufacturers aswell)

## Disclaimer
**By using the information you agree to the following disclaimer:** The author of this information assumes no liability for the accuracy of the info, or  damage or injury caused by using these instructions or any product created using methods or instructions herein. Building and using this controller requires technical knowledge and experience with electrical circuits and components. If you are unsure about any aspect of this project it is strongly recommended that you seek assistance from a knowledgeable or licensed individual or consult relevant resources before proceeding.  Always follow <a href="https://www.google.com/search?q=hobby+electronics+safety">electronics safety guidelines</a>.

## Intro
This guide provides information to build a <a href="https://www.home-assistant.io/">Home Assistant</a> compatible light dimmer that will work with most <a href="https://led.meanwell.com/productSeries.aspx">Meanwell LED drivers (aka LED power supplies or ballasts)</a> using just a few electronic parts.  This should work with other manufacturer LED drivers which use PWM dimming, so long as the driver provides source current on the dim circuit.  I think this can also be modified to work with 0-10v DC additive dimming by connecting a 10v power source to the signal board, but I haven't tried it.  Feel free to reach out to me if you have any questions about that.

## Applications & Background
The most likely use for this controller are indoor gardens or aquarium (reef) tanks that use LED power supplies, although it can be used for decor lighting applications.  The advancement of LED lighting has led to many of these types of systems being put into use.  From do-it-yourself hobbyist lighting to the relative market explosion of off-the-shelf options, this controller is a nice upgrade to many of these systems already in use.

Most LED grow and aquarium lights offer some form of dimming, either with dimming knobs, controllers, or the option to connect a dimming knob or controller (usually promoted by the manufacturer/reseller of the light).  Just like many other products, having your own easy-to-build and cost-effective option that works with Home Assistant offers a huge improvement in functionality, as you can for example create lighting profiles for different stages of growth or times of day, and build automatic rules to adjust your lights depending on things such as room/water temperatures, reducing light output if a person is detected in the room and more.  With Home Assistant the limit to how you automate your control is mostly limited to your imagination.

Over the years I've searched for lighting controllers, only to find expensive products that are limited in their functionality. The options that are available for use with Home Assistant or other SmartHome systems usually lack the ability to control a large number channels or drivers, so the cost can quickly rise depending on how complex the lighting system is.  This controller being able to control up to 24 or more _channels_ means you can have a large number of LED drivers being controlled by a single, cheap solution.

## Concept
LED drivers (aka power supplies) with dimming wires are designed to be connected to a dimming controller. In the case of Meanwell LED drivers with 3-in-1 dimming, this could be either a <a href="https://www.google.com/search?q=0-10v+Additive+DC+voltage+dimmer">0-10v Additive DC voltage dimmer</a>, <a href="https://www.google.com/search?q=10v+PWM+dimmer">10v PWM dimmer</a> or just a basic <a href="https://www.google.com/search?q=100+ohm+potentiometer">100ohm mechanical potentiometer.</a>  This project makes use of PWM dimming.

A MeanWell power supply that supports external dimming will have two wires (or two terminal points) where a positive (DIM +) and negative (DIM -) wire can be connected to a dimming controller.  In the case of MeanWell PWM dimming (which is the focus of this project), the LED driver creates a small current which runs through the wires. When the wires are disconected from eachother, no current flows and the light output will be 100% brightness.  When the wires are connected, the current flows through and signals the driver to dim the light output to 0% brightness.  PWM works off the principle of opening and closing this circuit very fast (over 1000 times a second), creating the effect of either more or less current to be allowed to pass through the power supply's DIM circuit.  Although you dont need to know how this works in detail to build this project, you can <a href="https://www.google.com/search?q=how+does+pwm+work">read more about how PWM works here.</a>

This project uses an Adafruit PWM LED Driver board to control the dim signals we sent to a MeanWell LED driver.  The original use for the Adafruit board is to drive LEDs directly, but the LEDs we are utilizing require much higher powered drivers (the MeanWell LED Drivers), hence we can use the Adafruit board to send the DIM signals to the higher powered driver.  It is a little confusing that we are using a low-powered LED Driver board to send DIM signals to a higher powered one, it was only coincidence that the Adafruit board is also an LED driver, you can ignore that.

<img src="/images/Home-Assistant-MeanWell-LED-Driver-Dimming-Controller-Schematic.png">  
<img src="/images/Home-Assistant-MeanWell-LED-Driver-Dimming-Controller-Schematic-Adafruit-PWM-Driver-Closeup.png">
<img src="/images/Home-Assistant-MeanWell-LED-Driver-Dimming-Controller-completed.png">

## Pre-requisites
- [Home Assistant](https://www.home-assistant.io/) server with [ESPHome](https://esphome.io/) installed
- LED power supply(s) (aka drivers or ballasts) that support **0-10v PWM** dimming such as MeanWell's line of high powered LED drivers.  If you arent sure if your LED driver supports **0-10v PWM** dimming, the best place(s) to check are the driver's datasheet, or if that isn't available, contact the manufacturer of your LED driver directly by phone or email and ask them "does my grow light / LED driver support 0-10v PWM dimming?".  Manufacturers are often quite helpful if you give them a chance.   **Ensure you are clear that you are looking for or asking about 0-10v PWM dimming, not 0-10v analog dimming.**  
- Ability to solder pin headers onto a printed circuit board.  
- Basic ability to work with wire strippers, pliers and glur guns.  
- Basic experience working with and connecting ESP32/ESP8266 devices to breakout boards.  

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

- [Adafruit TLC5947 **24-Channel** 12-bit PWM LED Driver](https://www.adafruit.com/product/1429)  
or [Adafruit TLC59711 **12-Channel** 16-bit PWM LED Driver](https://www.adafruit.com/product/3995)   
These boards are capable of generating some serious PWM signals, all the way up to 30V! The PWM voltage it generates is determined by the input voltage, which is why we're using a 10V power supply.  
    <img src="/images/Adafruit%2024%20channel%20PWM%20LED%20driver.jpg" height="150"> <img src="/images/Adafruit%2012%20channel%20PWM%20LED%20driver.jpg" height="150">  

- 2x [Break-Away 0.1" 2x20-pin Strip Dual Male Headers](https://www.google.com/search?q=Break-Away+0.1%22+2x20-pin+Strip+Dual+Male+Header) or 8 3x2 pin headers  
You will solder these terminal pin headers onto the PWM board. This is where the positive (+) and negative (-) DIM wire leads from your LED Driver will connect.  
    <img src="/images/Break-Away%200.1-inch%202x20-pin%20Strip%20Dual%20Male%20Header.jpg" height="150">  
    
- _terminal blocks_
- _project box_

# To be added:
## Assembly Instructions
## Software Configuration
## Usage
