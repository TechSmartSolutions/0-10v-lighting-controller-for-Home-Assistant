# Controlling 0-10v PWM and 0-10v Analog devices from Home Assistant
Originally this repo contained instructions to build a 0-10v PWM dimming controller that works with MEANWELL power supplies.  MEANWELL LED power supplies support their own proprietary 3-in-1 dimming protocol which works with PWM and Analog signal 0-10v signals.  It turns out however, that 0-10v Analog signals are more commonly used for controlling lighting (including HPS and commercial lighting) as well as even some dehumidifiers, air conditioners, etc..    

This repo still has the PWM controller method for information sake, and may be a good option if you have a lot of drivers to control, but if you can get the parts cheap, or dont have a ton of devicecs to control, it might make more sense to build a 0-10v Analog dimmer instead.

## 0-10v Analog Dimming controller
Check out <a href="https://github.com/TechSmartSolutions/12-or-24-Channel-Home-Assistant-LED-Driver-dimmer-for-High-Powered-LED-Drivers/discussions/2">the 0-10v Analog Dimming discussion thread</a> for more info.  

<img src="/images/Converting-5v-PWM-signals-from-ESP32-to-a-0-10v-Analog.png">

## 0-10v PWM Dimming controller
Here is the schematic for the version that I built that has been tested and working with MEANWELL 3-in-1 LED drivers and Home Assistant 
<img src="/images/24-Channel-TLC5947-based-LED-Driver-dimmer-for-Home-Assistant.png">
     
