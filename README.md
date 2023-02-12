0-10V is a simple low voltage, low current, low-cost, and reliable electrical signal used as a method for transmitting information and control signals between devices in control systems and building automation. 

Generally speaking, MEANWELL LED drivers with dimming leads are either 3-in-1 or 2-in-1 compatible, which is their way of saying it takes either:  
  1) 0-10V PWM  
  2) 0-10V analog  
  (and many also take a 100ohm mechanical potentiometer)  

Check the specifications of your lighting system or contact your manufacturer to determine which dimming signal(s) are compatible with your setup.

analog is more susceptible to signal loss over distance, and PWM more sensative to signal noise.

### 0-10v analog
The 0-10V analog signal is popular because it can be transmitted over long distances without significant signal degradation, making it ideal for longer

0-10V analog is typically used to control: **lighting**, **heating**, **ventilation and air conditioning (HVAC) systems**, as well as a **wide range of other industrial and commercial applications**. For example

 - **in lighting systems:** the 0-10V signal is used to control the brightness of a light, with 10V representing full brightness and 0V representing full off. **In

 - HVAC systems:** the 0-10V signal can be used to control the temperature or airflow of a system.

### 0-10v PWM
Unlike 0-10V Analog which sends a continuous signal of any voltage between 0V and 10V, 0-10V PWM controls a (digital) stream of upwards of 3000 pulses per second of  electricity at a full 10V.  The longer the pulses last per second the higher the voltage, until the pulses don't have time to turn off between eachother and the signal becomes a continuous 10V (analog) signal (100% brightness).

0-10V PWM is that you get finer control or "higher resolution" compared to 0-10V analog.  This makes it good for things like controlling brightness of a light or controlling the speed of a fan.

The choice between 0-10V analog and 0-10V PWM depends on the specific requirements of the application. 0-10V analog is a simpler and more straightforward signal that is easy to implement and transmit, making it more common in many applications. 0-10V PWM is used when higher resolution and more precise control are required.

## 0-10v Analog Dimming controller
It appears that more devices support this type of dimming.  Some of these devices include MEANWELL LED drivers as well as other LED drivers and even HPS fixtures, air conditioners, dehumidifiers and anything else that supports 0-10v analog control.  

The example below shows using a ESP32 to generate PWM signals, and then using the converter boards to convert the PWM into Analog voltage.   ESP32's are a little expensive, if you already have an ESP8266, you can use that with a PCA9568 board to generate the PWM signals instead of a ESP32.
<img src="/images/Converting-5v-PWM-signals-from-ESP32-to-a-0-10v-Analog.png">

## 0-10v PWM Dimming controller
Originally created this 24 channel 0-10v PWM dimming controller for MEANWELL 3-in-1 LED Drivers, however since MEANWELL drivers also support 0-10v analog dimming, if you dont need 24 channels of dimming you might be better off going the 0-10v analog route.  
<img src="/images/24-Channel-TLC5947-based-LED-Driver-dimmer-for-Home-Assistant.png">
     
