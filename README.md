# Home Assistant 12 Channel Dimming Controller for (high powered) LED Power Supplies 

<img src="/images/home-assistant-LED-driver-dimmer-adafuit-PWM-driver-with-TLC59711-completed-project.png">  

## PROJECT CURRENTLY UNDER REVIEW:

```diff
- I have completed testing and found what I believe
- to be the correct way to wire this circuit, with the
- help from some very patient guys over at the ESPhome 
- discord server (thank you).  As this guide is currently
- written, it works similar to using an optocoupler and 
- resistor on a PWM pin, however, as confirmed by meanwell
- that is not the right way to dim their 3-in-1 drivers, 
- and I have serious doubts it would work for a driver with
- purely analog 0-10v dimming.  Also, by using the method in
- this guide as it's currently written, you get weird behavior
- with things like drivers not lighting up LEDs, and the 
- channels acting weird when adjusting brightness.
- I've confirmed with Adafruit, Meanwell and some folks
- smtarter than myself of what appears to be the correct way
- to do it, and will be posting the updated version here. 
- unfortunately, this it involves wiring a resistor for each 
- channel, adding a 10v power supply and using a dc buck converter
- but in the end its a small price to pay for having something
- that works properly and can handle so many channels.
- 
- Update: we are getting there! the dimmer works more reliably
- and MeanWell confirmed this is the right way to do it. also
- it appears this will work with 0-10v analog dimming, but id
- lile to get my hands on one of those drivers to test.
-
- booya! stay tuned i still have to finish wiring mine up
- and update this documentation. 
```

# The information below is outdated I have a chance to update the guide:

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
- [Schematic](#schematic)
- [Assembly Instructions](#assembly-instructions)
- [Software Configuration](#software-configuration)
- [Usage](#usage)

## Intro
This comprehensive guide is designed to help you create a cost-effective solution that gives you the ability to control the brightness of 12 (or 24 and more!) lighting channels connected to LED fixtures.  This project integrates seamlessly with <a href="https://homeassistant.io">Home Assistant</a> using the <a href="http://esphome.io">ESPhome</a> addon. With this solution, you'll be able to control <a href="https://led.meanwell.com/productSeries.aspx">MEANWELL LED drivers</a> (aka power supplies or ballasts) that support external dimming using <a href="#parts-needed">off the shelf hobby electronic components</a>. **The information provided here may very well work with LED Drivers that support only 0-10v analog dimming, however I do not have a purely 0-10v analog dimming compatible LED Driver to test it out with.**

## Background
Controlling LED grow and aquarium lights is typically done using dimming knobs, controllers, or proprietary solutions from commercial manufacturers or resellers. However, by building your own dimming controller, you can increase the level of functionality, automation, and customization. With <a href="https://homeassistant.io">Home Assistant</a>, the possibilities are virtually limitless.

There seems to be limited options for affordable, functional lighting controllers. Open-source or DIY controllers for <a href="https://homeassistant.io">Home Assistant</a> are often limited in the number of channels or drivers they can control, resulting in increased costs as you need to control more drivers. <a href="https://www.youtube.com/watch?v=cEy5go2eZtY">A video is available on YouTube showing how to control a MeanWell driver using an ESP32 and optocouplers and resistors</a>, but this method may not be practical if you have multiple drivers to control, and furthermore using only optocouplers and resistors is not officially supported by MEANWELL as it uses the existing 10v on the DIM wires intended for resistive dimming (instead of the manufacturer supported method of applying an external 0-10V signal).  Using a method not supported by your LED driver manufacturer can experience unpredictable behavior and potentially void warranty.

The solution presented in this guide, developed in collaboration with <a href="https://forums.adafruit.com/">Adafruit</a>, <a href="https://esphome.io">ESPHome</a> and <a href="https://led.meanwell.com/">MEANWELL</a> is relatively simple, more versatile (and in the case of controlling many drivers, also cheaper). It eliminates the need for buying pre-made controllers while making use of the manufacturer supported method of adding a 10v signal to the dimming circuit of your LED Driver, and unlike the optocoupler method is compatible with both the cheaper ESP8266 as well as ESP32 devices.

<p align="right"><a href="#readme">...back to top</a></p>

## Concept
LED drivers (also known as power supplies or ballasts) that support external dimmers are designed to be connected to a dimming controller to manipulate the brightness of the LED lights. In the case of <a href="https://led.meanwell.com/productSeries.aspx">MEANWELL LED drivers</a>, this can be achieved through <a href="https://www.google.com/search?q=0-10v+Additive+DC+voltage+dimmer">0-10v additive DC voltage dimming</a>, <a href="https://www.google.com/search?q=10v+PWM+dimmer">PWM dimming</a>, or using a <a href="https://www.google.com/search?q=100+ohm+potentiometer">100ohm mechanical potentiometer</a>. This guide focuses the PWM dimming method mentioned in MEANWELL LED driver datasheets (although as previously mentioned, it may work with pure 0-10v dimming compatible drivers, although I have not tested this yet).

LED drivers that support external dimming will have either two wires, or two terminal points, positive (DIM+) and negative (DIM-). When the dimming wires are disconnected, or when 0v is applied to the DIM wires, no current flows through the DIM circuit and the light output is 100% brightness. When the wires are connected (and the DIM circuit is complete) the LED driver's internal DIM circuit signals the driver's DC output circuit (which is connected to the LEDs) to dim the light output to 0% brightness.  **Note: This controller is not to be connected directly to the DC circuit where your LEDs are connected!**

The controller in this guide works by providing a PWM signal which rapidly opens and closes this circuit over (1000 times a second!) resulting in a varying amount of current from 0v to 10v being passed through the driver's DIM circuit.  This is interpreted by the driver's internal circuitry which changes the output power of the DC output circuit connected LEDs.  Although you dont need to know how this works in detail to build this project, you can <a href="https://www.google.com/search?q=how+does+pwm+work">read more about how PWM works here</a> and refer to the dimming notes in your LED Driver's datasheet.  If you are unsure if your driver is supposrted, check the <a href="#compatible-led-drivers">compatible LED Drivers</a> section of this guide or contact your LED Driver manufacturer directly.

This project utilizes an Adafruit PWM LED driver board to control the dim signals sent to the Meanwell LED driver. The Adafruit board is originally designed to drive small low powered LEDs directly, but in this case, it is used to send a 0-10v signal to the dimming circuit of higher powered LED drivers. The Adafruit board is connected to a microcontroller (ESP8266 or ESP32), which is in turn connected over Wi-Fi to <a href="https://esphome.io">ESPHome</a>, which allows the dimming to be controlled and automated through <a href="https://homeassistant.io">Home Assistant</a>. This provides a simple, cost-effective, and scalable solution for controlling the brightness of LED lights.

<p align="right"><a href="#readme">...back to top</a></p>

## Warnings and considerations
- This controller **should not** be connected directly to the circuit where the LEDs are on. It is designed to be connected to the DIM circuit of your LED driver.  If your driver does not have DIM wires or terminals, this controller should not be used!
- Always follow <a href="https://www.google.com/search?q=hobby+electronics+safety">electronics safety guidelines</a> when working with circuits.
- If you are unsure about anything in this guide, or do not have the confidence to work with electronic circuits it is highly recommended to either have someone else do it for you, or seek guidance prior to doing anything. You proceed at your own risk!

<p align="right"><a href="#readme">...back to top</a></p>

## Applications & Use Cases
This controller is primarily intended for use in indoor gardens or aquariums with LED power supplies, but it can also be used for home decor or any other lighting application using <a href="#compatible-led-drivers">compatible LED drivers</a>. 

The widespread use of LED lighting in home gardens, aquariums, and decor lighting presents many opportunities for this controller. Since this controller works with DIY hobbyist lights as well as many off-the-shelf options, is is a great cost-effective expandable upgrade for many existing systems providing smart control from open-source systems such as my favorite option <a href="//homeassistant.io">Home Assistant</a> with the <a href="https://esphome.io">ESPhome</a> add-on.

Example uses cases include creating custom lighting profiles for different stages of plant growth or times of day, and automations that adjust light output based on room/water temperature or presence detection to dim to a comfortable level when a person is present, and more.  Further use cases include LED fixtures with multiple channels that can be individually controlled (i.e. multiple colors). Controlling different colors and temperatures of LEDs is common in aquarium applications and grow lighting. For example:

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

## Pre-requisites

#### Infrastructure
- <a href="https://www.google.com/search?q=what+are+the+different+ways+to+install+home+assistant%3F">Home Assistant</a> running on a NAS, Raspberry Pi, Computer/Server etc with the <a href="https://www.google.com/search?q=how+to+install+esphome+on+Home+Assistant">ESPHome Add-on Installed</a>
- Wi-fi network connected to the same network as your Home Assistant server (for the ESP device to connect to).

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

-<a href="https://www.google.com/search?q=ESP32+development+boards">ESP32</a> or <a href="https://www.google.com/search?q=ESP8266+development+boards">ESP8266</a> development board.   
_This is the brain of our controller setup. It has built-in Wi-Fi and can easily connect to your Home Assistant server using the ESPHome plugin. And the best part? You can use a cheap option like the <a href="https://www.google.com/search?q=wemos+mini+d1">Wemos D1 Mini ESP8266</a> we will be using in this project.  Most likely the ESP you buy will come with stacking pin headers but they wont be soldered on, and you can use the pin headers you purchased for this project instead. If female stacking headers are already soldered to your ESP, not to worry, you can simply use male dupont terminal for these connections instead, which should be included in your dupont crimp kit. _  
<img src="/images/esp8266%20wemos%20d1%20mini.jpg" height="150px">  
<BR CLEAR=”left” />  

-<a href="https://www.adafruit.com/product/1429">Adafruit TLC5947 **24-Channel** 12-bit PWM LED Driver</a> or <a href="https://www.adafruit.com/product/1455">Adafruit TLC59711 **12-Channel** 16-bit PWM LED Driver</a>.  
_These boards work rapidly "sinking" the voltage on the control pins.  In our case, this board will alternate the power feed from the MeanWell LED Driver DIM wires from 10v (open) to 0v (sunk) over a thousand times per second, creating a voltage value that corresponds with the brightness level (0v full brightness - 10v completely off)._  
 <img src="/images/Adafruit%2024%20channel%20PWM%20LED%20driver.jpg" height="150"> <img src="/images/Adafruit%2012%20channel%20PWM%20LED%20driver.jpg" height="150">  
<BR CLEAR=”left” />  

-2x <a href="https://www.google.com/search?q=Break-Away+0.1%22+2x20-pin+Strip+Dual+Male+Header">Break-Away Single Row Male Headers</a>.
_You will solder these terminal pin headers onto the *output pins* on the Adafruit PWM board. This is where the positive (+) DIM wire leads from your LED Driver will connect.  **Ensure you get the pin headers that have a long pin on both sides, as you need the pins to make connection from both the top of the board to connect to your LED driver (+) DIM wires as well as underneath the board to the breadboard for the resistors.**  See the <a href="#schematic">project schematic</a> for more details._  
<img src="/images/break-away-single-row-pin-headers.jpg" height="150">  
<BR CLEAR=”left” />  

-12-24x <a href="https://www.amazon.ca/s?k=1k+resistors">1k - 10k ohm resistors</a>.
_The resistors work with the Adafruit board to ensure you are properly providing an additive voltage to the LED driver's DIM circuit._  
<img src="/images/1k-ohm-resistors.jpg" height="150">  
<BR CLEAR=”left” />  

-<a href="https://www.amazon.ca/s?k=12+position+terminal+block">12 position screw terminal blocks</a>  
_I used these becuase they are cheap, and not too difficult to connect the LED driver DIM(+ and -) wires to. This project is building a 12 channel controller, so you need two of them (one for DIM+ connections and one for DIM- connections)._  
<img src="/images/12-position-power-screw-terminal-blocks.jpg" height="150px">  
<BR CLEAR=”left” />  

-<a href="https://www.amazon.ca/s?k=project+box">Project Box</a>  
_There are a lot of options available in size and function.  I usually go for the cheapest one possible that includes a clear lid. Somehow I feel better knowing that I can see into my project boxes incase something goes wrong I'll be able to notice it.  Make sure if you need to get a water sealed one. They are all pretty cheap._  
<img src="/images/water-resistant-project-box-with-clear-lid.jpg" height="150px">  
<BR CLEAR=”left” />  

-<a href="https://www.amazon.ca/s?k=breadboard">Small Breadboard</a>  
_This was used to make the assembly easier, removing the need for soldering or crimping resistors. **If you are using an ESP that is larger than a Wemos D1 Mini, you will need a bigger breadboard.**_  
<img src="/images/small-breadboard.jpg" height="150px">  
<BR CLEAR=”left” />  

-<a href="https://www.amazon.ca/s?k=20awg+jumper+wire">20awg Jumper Wire</a>  
_You could go with stranded wire for the connection between terminal blocks and the dupont connectors that will plug into the header pins and breadboard as it's easier to work with, but you will need solid core for some of the connections, so I went with solidcore for the whole thing.  If you wanted to make it really nice, you can use stranded wire for all connections using dupont connectors, and solid core for the bare-wire jumpers on the breadboard._  
<img src="/images/20awg-jumper-wire.jpg" height="150px">  
<BR CLEAR=”left” />  


- Mounting plate for project box and terminals.  
_In this project we used a piece of wood_  

<p align="right"><a href="#readme">...back to top</a></p>

## Schematic

<img src="/images/home-assistant-LED-driver-dimmer-adafuit-PWM-driver-TLC59711-TLC5947.png">  

<p align="right"><a href="#readme">...back to top</a></p>

## Assembly Instructions
_In our project we will connect 12 output pins to the terminals._  
Note: It is recommended to refer to the <a href="#Schematic">project schematic diagram</a> and <a href="#readme">final project photo</a> while building the circuit and to <a href="https://www.google.com/search?q=hobby+electronics+safety">follow proper safety precautions</a> while handling electrical components.  
#### If you are unsure about any of these steps, or run into any problems please log an issue on the GitHub repo so that I can provide guidance.

### 1. Prepare the project box and mount it with the terminals to your backplate.  
 - 1.1. Gather all tools and components.
 - 1.2. Arrange (but don't secure) the breadboard and LM2596 in the project box to ensure fit and see where you will mount it. Ensure you leave enough room for the DC barrel connector.
 - 1.3. Mount the project box and 12 position terminals to your backplate.  If you mount the terminals similar to how I did in my project photo, it is a good idea to use spacers under the terminals so that you can run wires underneath them, which makes the final product easier to work with.
 - 1.4. Make two holes in the project box, one on the side closest to the terminals with enough space for 24 of the jumper wires to fit through (2 wires for each channel), and a round hole for the DC barrel connector where the 10v power supply will plug into the box.  Insert the barrel connector to test fit, and adjust the hole as needed.

_You should now have the project box with holes made along with the 12-position terminals now mounted_

### 2. Solder the terminals onto the ESP and Adafruit boards, assemble components onto the breadboard and loosely
_Don't rush the next steps are they involve soldering. You only really get one shot at this, so make sure you follow these next steps properly.  
If you are unsure about this section, do everything except solder and fit it into the breadboard to make sure you got it right.  Once you're sure its good you can go back and solder the pins before proceeding to test._  
  
 - 2.1. Using your wire cutters (or side cutters), cut the single row terminal pins into 4x 3-pin segments for the output holes of the adafruit board, and however many you need for your ESP. Make sure they are good cuts dont use any that are broken/damaged.  
 - 2.2. **Solder** the 5-pin terminals that came with the Adafruit board **facing up** to the side terminals (labelled GND, VCC, V+, CI, DI and GND, VCC, V+, CO, DO).  You will need to connect jumper wires to these, so make sure the long end of these pins are on the top of the board! 
 - 2.3. **Solder** your 3 position pins **to the output pins (inside row) of holes on the Adafruit board**, and the headers you cut for your ESP onto your ESP.  For these make sure that the longer end of the pins facing down!
 - 2.4 **Solder** two wires to the DC barrel connector.  Although not completely necessary, it is recommended to use heatshrink to cover the terminals on the barrel connector once you're soldered them.  Once soldering complete, securely fit the barrel connector into the project box using the hole you made.
 - 2.5. Insert the Afafruit board and ESP into the breadboard as shown in the <a href="#schematic">project schematic</a> to test the fit.  If you are using an ESP larger than the Wemos D1 Mini, there might be some double checking needed here.
 - 2.6. Insert the resistors as shown in the <a href="#schematic">project schematic</a>.
 - 2.7. Place the LN2596 in the project box wire up the rest of the components according to <a href="#schematic">project schematic</a>.  Since you have spools of jumper wire, you can always make the wires to the LM2596 longer than needed and shorten them once you've permanently mounted everything in the box.

_You should now have a breadboard with everything complete except the wires that connect from the terminal to the Adafruit board and breadboard._

### 3. Make and connect the jumper wires that will go from the terminals to the project box
_It is recommended to do these wires one at a time, starting with the furthest away wires._  

- 3.1. Feed the jumper wire from the project box to the terminal to visually inspect if you have the right legnth, a little extra is good here, but not too much.  
- 3.2. Strip enough off the end of the jumper wire to go into the terminal block, but not too much, and screw it down to the first terminal. 
- 3.3. Feed the wire to the project box with a little extra slack (incase you have to re-cut the wires), and snip the end.  If you're wondering how long they need to be, the wires from the GND terminal need to go to the GND (-) rail on the breadboard, and the control wires (DIM+) need to goto the adafruit board. The good thing about this step is you will have lots of terminals and wire to redo any of them if you need to redo them.
- 3.4. Strip off a few milimeters off the end of the wire, and:
    - For the GND wires, attach a male dupont terminal to the end using the dupont crimper.


8. Heat up the glue gun and attach four M2.5 standoffs to the LM2596 board with screws.  Once your gluegun is completely heated up, dab 
9. Mount the  terminal blocks (with risers) and project box and to the mounting plate.
10. <a href="https://learn.adafruit.com/tlc5947-tlc59711-pwm-led-driver-breakout/connecting-to-the-arduino">Connect the ESP8266 microcontroller to the Adafruit PWM board.</a>
11. Connect 24 wires from the terminal blocks to the output pins on the Adafuit PWM board, and 12 wires from a terminal block to the ground rail on the breadboard.
12. Connect the DIM + wire from each MeanWell driver to the terminal posts connected to the output pins on the Adafuit board.
13. Connect the DIM - wire from each MeanWell driver to the terminal posts connected to the ground rail on the breadboard.
14. Connect the USB power cable ESP. You should get a green light on the Adafruit board.  

<p align="right"><a href="#readme">...back to top</a></p>

## Software Configuration
1. Open the ESPHome dashboard from Home Assistant and add the new ESP to ESPHome (use the "Prepare Device for Adoption" option).
2. After ESPHome has flashed the ESP, ensure you set the Wi-Fi network settings (in the ESPHome USB add-device page).
3. After the device has been added to ESPHome, navigate to ESPHome from the left Home Assistant navigation menu.
4. Find the newly added device and click "Adopt" and give it a name.
5. After the device is added to ESPHome with it's new name, navigate in Home Assistant to Settings > Devices and click "Configure" **on the newly added name** of the ESP device.  
6. Go back to the ESPHome dashboard from the left Home Assistant nav menu, and click edit under the new ESP device name and paste the code from <a href="/esp_24_channel_adafruit_pwm_driver.yaml">the yaml file</a> the github repository a few spaces below the captivate_portal line.  Ensure the indentation is correct!
7. Click save, then install over Wireless to flash the ESP with the new configuration.  Ensure that you've specified the correct pins for data_pin (DI, not DO!), clock_pin (CLK) and lat_pin (LAT). If you wired using the same pins as our example, use the following:  

        data_pin: GPIO16  
        clock_pin: GPIO14  
        lat_pin: GPIO12  
8. Test the dimmer entities from your HA dashboard.  For mine I had to change the min_power variable on the outputs in the ESP yaml. For some reason the drivers would not light up the LEDs at the low end of the slider. If you have the same issue, adjust the min_power value until you get a minimum brightness at the low end of the brightness slider.  

<p align="right"><a href="#readme">...back to top</a></p>

## Usage  
Once the code has flashed and the device has restarted, you should have the following new entities that can be used to manipulate the brightness of your LED driver(s) output.  

<p align="right"><a href="#readme">...back to top</a></p>
