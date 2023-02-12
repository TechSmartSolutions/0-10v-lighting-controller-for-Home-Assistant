### what is 0-10V?
0-10V is a simple low voltage, low current, low-cost, and reliable electrical signal used as a method for transmitting information and control signals between devices in control systems and building automation. 


#### do I need analog or digital (PWM)?
The choice between 0-10V analog and 0-10V PWM depends on what your system is compatible with. Dimmable MEANWELL LED drivers can dim with either PWM or analog (3-in-1 or 2-in-1 dimming).

Check the specifications of your lighting system or contact your manufacturer to determine which dimming signal(s) are compatible with your setup.  0-10V analog is a simpler and more straightforward making it more commonly seen. 

### 0-10v analog
0-10V Analog is a constant signal at a voltage between 0-10V.

### 0-10v PWM
0-10V PWM is different in that it sends a stream of upwards of 3000 pulses per second of 10V electricity.  The longer the pulses last per second the higher the voltage increases until it eventually becomes a constant 10V (analog) signal (100% brightness).  It's kind of like your television refresh rate, you can't see it's flashing because it's happening many times per second.  

## 0-10v Analog Dimming controller
It appears that more devices support this type of dimming.  Some of these devices include MEANWELL LED drivers as well as other LED drivers and even HPS fixtures, air conditioners, dehumidifiers and anything else that supports 0-10v analog control.  

The example below shows using a ESP32 to generate PWM signals, and then using the converter boards to convert the PWM into Analog voltage.   ESP32's are a little expensive, if you already have an ESP8266, you can use that with a PCA9568 board to generate the PWM signals instead of a ESP32.
<img src="/images/Converting-5v-PWM-signals-from-ESP32-to-a-0-10v-Analog.png">

## 0-10v PWM Dimming controller
Originally created this 24 channel 0-10v PWM dimming controller for MEANWELL 3-in-1 LED Drivers, however since MEANWELL drivers also support 0-10v analog dimming, if you dont need 24 channels of dimming you might be better off going the 0-10v analog route.  
<img src="/images/24-Channel-TLC5947-based-LED-Driver-dimmer-for-Home-Assistant.png">
     
