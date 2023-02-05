# 12 or 24+ Channel Dimming Controller for Meanwell LED Power Supplies (and other LED Drivers with driver-source current PWM dimming)

<img src="/images/Home-Assistant-MeanWell-LED-Driver-Dimming-Controller-completed.png">  

**By using the information you agree to the following disclaimer:** The author of this information assumes no liability for the accuracy of the info, or  damage or injury caused by using these instructions or any product created using methods or instructions herein. Building and using this controller requires technical knowledge and experience with electrical circuits and components. If you are unsure about any aspect of this project it is strongly recommended that you seek assistance from a knowledgeable or licensed individual or consult relevant resources before proceeding.  Always follow <a href="https://www.google.com/search?q=hobby+electronics+safety">electronics safety guidelines</a>.

### Table of Contents
- [Intro](#intro)
- [Background](#background)
- [Applications & Use Cases](#applications-&-use-cases)
  - [Example use case 1: Tuneable 3000k-5500k grow light for all growth stages](#example-use-case-1-tuneable-3000k-5500k-grow-light-for-all-growth-stages)
  - [Example use case 2: 8-Channel aquarium light](#example-use-case-2-8-channel-aquarium-light)
- [Concept](#concept)
- [Pre-requisites](#pre-requisites)
  - [Infrastructure](#infrastructure)
  - [Skills needed](#skills-needed)
  - [Tools Required](#tools-required)
  - [Optional stuff](#optional-stuff)
- [Parts needed](#parts-needed)
- [Schematics](#schematics)
- [Assembly Instructions](#assembly-instructions)
- [Software Configuration](#software-configuration)
- [Usage](#usage)

## Intro
This comprehensive guide is designed to help you create a versatile and cost-effective light dimming solution that integrates seamlessly with Home Assistant. With this solution, you'll be able to control most <a href="https://led.meanwell.com/productSeries.aspx">Meanwell LED drivers</a> (power supplies or ballasts) using <a href="#parts-needed">a few electronic components</a>. Moreover, the information provided here is also applicable to other LED drivers that use PWM dimming, so long as the driver provides source current on the dimming circuit. You may also be able to modify the setup to work as a 0-10v DC additive dimmer, which is more widely used for LED Drivers.

## Background
Controlling LED grow and aquarium lights is typically done using dimming knobs, controllers, or proprietary solutions from commercial manufacturers or resellers. However, by building your own dimming controller, you can increase the level of functionality, automation, and customization. With </a href="https://homeassistant.io">Home Assistant</a>, the possibilities are virtually limitless. You can create custom lighting profiles for different stages of growth or times of day, adjust light output based on room/water temperature or presence detection to dim to a comfortable level when a person is present, and more.

There are limited options for an affordable, functional lighting controller. Open-source or DIY controllers for Home Assistant are often limited in the number of channels or drivers they can control, resulting in increased costs for those with multiple drivers. <a href="https://www.youtube.com/watch?v=cEy5go2eZtY">A video is available on YouTube showing how to control a MeanWell driver using an ESP32 and various components such as optocouplers and resistors</a>, but this method may not be practical if you have multiple drivers to control.

The solution presented in this guide, developed in collaboration with <a href="https://forums.adafruit.com/">Adafruit</a> and <a href="https://esphome.io">ESPHome</a>, is simpler, cheaper, and more versatile. It eliminates the need for buying pre-made controllers or working with optocouplers, resistors, and tying up multiple pins on an ESP32 (the optocoupler method is not compatible with cheaper ESP8266s). This solution uses only a few components and reduces costs.

<p align="right"><a href="#readme">...back to top</a></p>

## Applications & Use Cases
This controller is primarily intended for use in indoor gardens or aquariums with LED power supplies, but it can also be used for home decor lighting. The widespread use of LED lighting in home gardens, aquariums, and decor lighting presents many opportunities for this controller. Since this controller works with DIY hobbyist lights as well as many off-the-shelf options, is is a great cost-effective upgrade for many existing systems.

A further use case would be a LED fixture with multiple channels or colors that can be individually controlled. Controlling different colors and temperatures of LEDs is common in aquarium applications and grow lighting. For example:

### Example use case 1: Tuneable 3000k-5500k grow light for all growth stages: 
Cost-effective tuneable grow light based on white LEDs only. Using only two channels, you can adjust the overall temperature of the grow light from 5500K cool (early stage growth) to 3000K warm (late stage growth) simply by changing the output of each channel.

- Channel 1: Warm White 3000K
- Channel 2: Cool White 5500K  

### Example use case 2: 8-Channel aquarium light
Control over multiple channels to optimize the aesthetics of a reef tank or adjust the spectrum mix for optimal aquatic life.

- Channel 1: Cool White
- Channel 2: Royal Blue
- Channel 3: Blue
- Channel 4: Red
- Channel 5: Green
- Channel 6: UV
- Channel 7: Violet
- Channel 8: Warm White  

<p align="right"><a href="#readme">...back to top</a></p>

## Concept
LED drivers, also known as power supplies, are designed to be connected to a dimming controller for controlling the brightness of the LED lights. In the case of Meanwell LED drivers, this can be achieved through <a href="https://www.google.com/search?q=0-10v+Additive+DC+voltage+dimmer">0-10v additive DC voltage dimming</a>, <a href="https://www.google.com/search?q=10v+PWM+dimmer">PWM dimming</a>, or using a <a href="https://www.google.com/search?q=100+ohm+potentiometer">100ohm mechanical potentiometer</a>. This guide focuses on using PWM dimming to control LED drivers.

Meanwell LED drivers that support external dimming will have either two wires, or two terminal points, positive (DIM+) and negative (DIM-). When the dimming wires are disconnected, no current flows and the light output is 100% brightness. When the wires are connected, the LED driver creates a small current that signals the driver to dim the light output to 0% brightness. PWM works by rapidly opening and closing this circuit over 1000 times a second, resulting in a varying amount of current being allowed to pass through the driver's DIM circuit, which is interpreted by the driver to control the brightness of the LED lights.  Although you dont need to know how this works in detail to build this project, you can <a href="https://www.google.com/search?q=how+does+pwm+work">read more about how PWM works here.</a>

This project utilizes an Adafruit PWM LED driver board to control the dim signals sent to the Meanwell LED driver. The Adafruit board is originally designed to drive LEDs directly, but in this case, it is used to control the dimming of the higher powered Meanwell LED drivers. The Adafruit board is connected to a microcontroller, which is in turn connected over Wi-Fi to ESPHome, which allows the dimming to be controlled and automated through Home Assistant. This provides a simple, cost-effective, and scalable solution for controlling the brightness of LED lights.

<p align="right"><a href="#readme">...back to top</a></p>

## Pre-requisites

#### Infrastructure
- <a href="https://www.google.com/search?q=what+are+the+different+ways+to+install+home+assistant%3F">Home Assistant</a> running on a NAS, Raspberry Pi, Computer/Server etc with the <a href="https://www.google.com/search?q=how+to+install+esphome+on+Home+Assistant">ESPHome Add-on Installed</a>
- Wi-fi network for the ESP device to connect to.

#### Skills needed
- <a href="https://www.google.com/search?q=working+with+electronic+hobby+tools">Working with hobby electronic tools</a>.  
- <a href="https://www.google.com/search?q=how+to+solder+pin+headers+into+a+breakout+board" target="_blank">Soldering pin headers into a breakout board</a>.
- <a href="https://www.google.com/search?q=how+to+wire+microcontrollers+to+breakout+boards">Wiring ESP32/ESP8266 MCUs to breakout boards</a>.  

#### Tools Required
_Although having top-of-the-line tools makes the job easier, I rely on economical options for my toolkit. While working with budget tools may not always be optimal, they are adequate for small and occasional projects and have served me well._  
- [Soldering iron](https://www.amazon.ca/s?k=Soldering+iron) ([brass wool](https://www.amazon.ca/s?k=brass+wool+soldering) recommended instead of using a wet solder sponge).  
- [Wire stripper](https://www.amazon.ca/s?k=wire+stripper)  
- [Multimeter](https://www.amazon.ca/s?k=Multimeter&rh=n%3A3006902011%2Cp_36%3A12035760011&dc&ds=v1%3AVJ07OZbfZbPTVCeOY0%2FOTVw2%2FDdTF6NoXoZ9MvBP20c&qid=1675112740&rnid=12035759011&ref=sr_nr_p_36_1) (for testing connections) - _if you plan on using this a lot, it would be better to get a somewhat decent one. Cheap multimeters will have weak connectors that can break easily, or the mode selector dial might not hold up over a long time. I'm happy with the cheapest option available on Amazon, I've only had to replace mine once. I'm just careful with it._  

##### Optional stuff:
- <a href="https://www.amazon.ca/s?k=breadboard">Breadboards</a> - _I used two small breadboards for this project.  They came in a 3-pack, and you can take off the +/- power rails and then snap the main pieces together.  They came with a sticky foam on the back with a peel off sticker.  Using this I didn't need to mount or glue anything into the project box, and it lets me be able to change parts up in the future if I ever wanted to rebuild it._
- [Hot glue gun](https://www.amazon.ca/s?k=Hot+glue+gun) - _I've seen videos of people hot glueing circuit boards directly into project boxes.  I'm not a huge fan of that for most of the stuff I build, but for small projects that you don't think you'll rebuild later that you want to do quick and easy, this can shave off cost and time._  

<p align="right"><a href="#readme">...back to top</a></p>

## Parts needed
- <a href="https://led.meanwell.com/productSeries.aspx">One or more MeanWell (or other brand) LED Drivers</a> compatible with PWM dimming connected to a lighting circuit that you want to be able to control. There should be DIM + and - wires or terminals.
_Some of the smaller MeanWell LED drivers do not supporting dimming. The best place to confirm that is the datasheet for your driver.  You can find it by searching "MODEL_NUMBER pdf" (example "HLG-320H-C1400 pdf"). If you have a driver made by a manufacturer and you can't find any information on the datasheet regarding PWM dimming, contact the manufacturer directly to clarify_.  
<img src="/images/meanwell-driver-with-led-boards-and-dim-wire.png">  
<BR CLEAR=”left” />  

- <img src="/images/esp8266%20wemos%20d1%20mini.jpg" height="150px" align="right"><a href="https://www.google.com/search?q=ESP32+development+boards">ESP32</a> or <a href="https://www.google.com/search?q=ESP8266+development+boards">ESP8266</a> development board with <a href="https://www.google.com/search?q=spi+pins">SPI pins</a>   
_This is the brain of our controller setup. It has built-in Wi-Fi and can easily connect to your Home Assistant server using the ESPHome plugin. And the best part? You can use a cheap option like the <a href="https://www.google.com/search?q=wemos+mini+d1">Wemos D1 Mini ESP8266</a> we will be using in this project._  
<BR CLEAR=”left” />  

- <img src="/images/Adafruit%2024%20channel%20PWM%20LED%20driver.jpg" height="150" align="right"> <img src="/images/Adafruit%2012%20channel%20PWM%20LED%20driver.jpg" height="150" align="right"><a href="https://www.adafruit.com/product/1429">Adafruit TLC5947 **24-Channel** 12-bit PWM LED Driver</a> or <a href="https://www.adafruit.com/product/1455">Adafruit TLC59711 **12-Channel** 16-bit PWM LED Driver</a>.  
_These boards work rapidly "sinking" the voltage on the control pins.  In our case, this board will alternate the power feed from the MeanWell LED Driver DIM wires from 10v (open) to 0v (sunk) over a thousand times per second, creating a voltage value that corresponds with the brightness level (0v full brightness - 10v completely off)._  
<BR CLEAR=”left” />  

- <img src="/images/Break-Away%200.1-inch%202x20-pin%20Strip%20Dual%20Male%20Header.jpg" height="150" align="right">2x <a href="https://www.google.com/search?q=Break-Away+0.1%22+2x20-pin+Strip+Dual+Male+Header">Break-Away 0.1" 2x20-pin Strip Dual Male Headers</a> or 8 3x2 pin headers  
_You will solder these terminal pin headers onto the PWM board. This is where the positive (+) and negative (-) DIM wire leads from your LED Driver will connect._  
<BR CLEAR=”left” />  

- <img src="/images/12-position-power-screw-terminal-blocks.jpg" height="150px" align="right"><a href="https://www.amazon.ca/s?k=12+position+terminal+block">12 position screw terminal blocks</a>  
_I used these becuase they are cheap, and not too difficult to connect the driver wires to. They aren't super easy to use if you're constantly disconnecting and moving around power supplies, but for this type of application you shouldn't need to do that._  
<BR CLEAR=”left” />  

- <img src="/images/water-resistant-project-box-with-clear-lid.jpg" height="150px" align="right"><a href="https://www.amazon.ca/s?k=project+box">Project Box</a>  
_There are a lot of options available in size and function.  I usually go for the cheapest one possible that includes a clear lid. Somehow I feel better knowing that I can see into my project boxes incase something goes wrong I'll be able to notice it.  Make sure if you need to get a water sealed one. They are all pretty cheap._  
<BR CLEAR=”left” />  

- Mounting plate for project box and terminals.  
_In this project we used a piece of wood_  

<p align="right"><a href="#readme">...back to top</a></p>

## Schematics

<img src="/images/Home-Assistant-MeanWell-LED-Driver-Dimming-Controller-Schematic.png">  
<img src="/images/Home-Assistant-MeanWell-LED-Driver-Dimming-Controller-Schematic-Adafruit-PWM-Driver-Closeup.png">

<p align="right"><a href="#readme">...back to top</a></p>

## Assembly Instructions
_In our project we will connect all 24 output pins to the terminals.  You can use less if you want._  
Note: It is recommended to refer to the <a href="#Schematics">schematic diagrams</a> and <a href="#readme">final project photo</a> while building the circuit and to <a href="https://www.google.com/search?q=hobby+electronics+safety">follow proper safety precautions</a> while handling electrical components.  
#### If you are unsure about any of these steps, or run into any problems please log an issue so that I can provide guidance.

1. Gather the necessary components:
    - MeanWell LED Driver(s) connected to LEDs
    - ESP8266 microcontroller
    - Adafruit PWM board
    - Project Box
    - Connector hardware  
In our case we are using breadboards and wire.
2. Mount the  terminal blocks (with risers) and project box and to the mounting plate.
3. <a href="https://learn.adafruit.com/tlc5947-tlc59711-pwm-led-driver-breakout/connecting-to-the-arduino">Connect the ESP8266 microcontroller to the Adafruit PWM board.</a>
4. Connect 24 wires from the terminal blocks to the output pins on the Adafuit PWM board, and 12 wires from a terminal block to the ground rail on the breadboard.
5. Connect the DIM + wire from each MeanWell driver to the terminal posts connected to the output pins on the Adafuit board.
7. Connect the DIM - wire from each MeanWell driver to the terminal posts connected to the ground rail on the breadboard.
8. Connect the USB power cable ESP. You should get a green light on the Adafruit board.  

<p align="right"><a href="#readme">...back to top</a></p>

## Software Configuration
1. Open the ESPHome dashboard from Home Assistant and add the new ESP to ESPHome (use the "Prepare Device for Adoption" option).
2. After ESPHome has flashed the ESP, ensure you set the Wi-Fi network settings (in the ESPHome USB add-device page).
3. After the device has been added to ESPHome, navigate to ESPHome from the left Home Assistant navigation menu.
4. Find the newly added device and click "Adopt" and give it a name.
5. After the device is added to ESPHome with it's new name, navigate in Home Assistant to Settings > Devices and click "Configure" **on the newly added name** of the ESP device.  
6. Go back to the ESPHome dashboard from the left Home Assistant nav menu, and click edit under the new ESP device name and paste the code from the yaml file the github repository a few spaces below the captivate_portal line.  Ensure the indentation is correct!
7. Click save, then install over Wireless to flash the ESP with the new configuration.  Ensure that you've specified the correct pins for data_pin (DI, not DO!), clock_pin (CLK) and lat_pin (LAT). If you wired using the same pins as our example, use the following:  

        data_pin: GPIO16  
        clock_pin: GPIO14  
        lat_pin: GPIO12  
8. Test the dimmer entities from your HA dashboard.  For mine I had to change the min_power variable on the outputs in the ESP yaml. For some reason the drivers would not light up the LEDs at the low end of the slider. If you have the same issue, adjust the min_power value until you get a minimum brightness at the low end of the brightness slider.  

<p align="right"><a href="#readme">...back to top</a></p>

## Usage  
Once the code has flashed and the device has restarted, you should have the following new entities that can be used to manipulate the brightness of your LED driver(s) output.  

<p align="right"><a href="#readme">...back to top</a></p>
