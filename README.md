### pwm vs analog signals
**Pulse-width modulation (PWM) signals** are a type of digital signal that turn on and off very rapidly each second to create the illusion (simulate) a different voltages depending on the ratio of the on to off time.

**Analog signals** on the other hand are continuous and provide a smooth, unchanging voltage level.

Both PWM and analog show up on a multimeter the same, but in an oscilloscope the waveforms look different.

## 0-10v Analog Dimming controller
It appears that more devices support this type of dimming.  Some of these devices include MEANWELL LED drivers as well as other LED drivers and even HPS fixtures, air conditioners, dehumidifiers and anything else that supports 0-10v analog control.  

The example below shows using a ESP32 to generate PWM signals, and then using the converter boards to convert the PWM into Analog voltage.   ESP32's are a little expensive, if you already have an ESP8266, you can use that with a PCA9568 board to generate the PWM signals instead of a ESP32.
<img src="/images/Converting-5v-PWM-signals-from-ESP32-to-a-0-10v-Analog.png">

## 0-10v PWM Dimming controller
Originally created this 24 channel 0-10v PWM dimming controller for MEANWELL 3-in-1 LED Drivers, however since MEANWELL drivers also support 0-10v analog dimming, if you dont need 24 channels of dimming you might be better off going the 0-10v analog route.  
<img src="/images/24-Channel-TLC5947-based-LED-Driver-dimmer-for-Home-Assistant.png">
     
