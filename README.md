### 0-10v analog
**Works with:** 
       - MEANWELL LED Drivers supplies with 2-in-1 or 3-in-1 dimming  
0-10V analog is a type of analog electrical signaling commonly used in industrial control systems and building automation. This type of signal is a low voltage, low current signal that varies linearly between 0 and 10 volts, representing a given range of values.

It is typically used to control lighting, heating, ventilation and air conditioning (HVAC) systems, as well as a wide range of other industrial and commercial applications. For example, in lighting systems, the 0-10V signal is used to control the brightness of a light, with 10V representing full brightness and 0V representing full off. In HVAC systems, the 0-10V signal can be used to control the temperature or airflow of a system.

The 0-10V analog signal is popular because it is a simple, low-cost, and reliable method for transmitting information and control signals between devices. It can be transmitted over long distances without significant signal degradation, making it ideal for applications that require remote control and monitoring.

### 0-10v PWM
**Works with:** MEANWELL LED Drivers supplies with 2-in-1 or 3-in-1 dimming  
  
  
0-10V PWM (Pulse Width Modulation) is a variation of the 0-10V analog signal that encodes the signal as a series of pulses rather than a continuous voltage signal. In this format, the width of the pulses represents the signal value, with wider pulses indicating higher values and narrower pulses indicating lower values.

The advantage of 0-10V PWM is that it provides higher resolution compared to a simple 0-10V analog signal. This makes it suitable for applications that require fine control over the signal, such as adjusting the brightness of a light or controlling the speed of a fan.

The choice between 0-10V analog and 0-10V PWM depends on the specific requirements of the application. 0-10V analog is a simpler and more straightforward signal that is easy to implement and transmit, making it more common in many applications. 0-10V PWM is used when higher resolution and more precise control are required.

### pwm vs analog signals
0-10v Analog is a commonly used industrial control for lighting, hvac, 

**Analog signals** on the other hand are continuous and provide a smooth, unchanging voltage level.

**Pulse-width modulation (PWM) signals** are a type of digital signal that turn on and off very rapidly each second to create the illusion (simulate) a different voltages depending on the ratio of the on to off time.





## 0-10v Analog Dimming controller
It appears that more devices support this type of dimming.  Some of these devices include MEANWELL LED drivers as well as other LED drivers and even HPS fixtures, air conditioners, dehumidifiers and anything else that supports 0-10v analog control.  

The example below shows using a ESP32 to generate PWM signals, and then using the converter boards to convert the PWM into Analog voltage.   ESP32's are a little expensive, if you already have an ESP8266, you can use that with a PCA9568 board to generate the PWM signals instead of a ESP32.
<img src="/images/Converting-5v-PWM-signals-from-ESP32-to-a-0-10v-Analog.png">

## 0-10v PWM Dimming controller
Originally created this 24 channel 0-10v PWM dimming controller for MEANWELL 3-in-1 LED Drivers, however since MEANWELL drivers also support 0-10v analog dimming, if you dont need 24 channels of dimming you might be better off going the 0-10v analog route.  
<img src="/images/24-Channel-TLC5947-based-LED-Driver-dimmer-for-Home-Assistant.png">
     
