# Intro to controlling 0-10v PWM and 0-10v Analog devices from Home Assistant
This repo was originally created with designs for a 0-10v PWM controller that works with MEANWELL 3-in-1 LED drivers.  It's not a bad option if you have a lot of drivers to control, however after some digging it looks like using a PWM generator (an ESP32 or an ESP8266 with a PCA9685 for example) along with PWM to Analog 0-10v voltage converter boards might make more sense, since they work with the MEANWELLS and other 0-10v devices that support 0-10v analog control like HPS lights, some dehumidifiers, some air conditioners etc.

## 0-10v Analog Dimming controller
Check out <a href="https://github.com/TechSmartSolutions/12-or-24-Channel-Home-Assistant-LED-Driver-dimmer-for-High-Powered-LED-Drivers/discussions/2">the 0-10v Analog Dimming discussion thread</a> for more info.  

Most all LED drivers with dimming wires/terminals support 0-10v analog dimming (MEANWELL drivers included).  As such this might be the best option.



## 0-10v PWM Dimming controller
This was the controller I originally built, but it seems that the 0-10v Analog option is better if you can get the parts cheap.  
Here is the schematic for the version that has been tested and working with MEANWELL 3-in-1 LED drivers and Home Assistant 
<img src="/images/24-Channel-TLC5947-based-LED-Driver-dimmer-for-Home-Assistant.png">
     
