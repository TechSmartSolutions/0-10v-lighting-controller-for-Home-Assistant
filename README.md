### what is 0-10V?
0-10V is a simple low voltage, low current, low-cost, and reliable electrical signal used as a method for transmitting information and control signals between devices in control systems and building automation. 


#### how do I know which 0-10V I need, analog or PWM?
The choice between 0-10V analog and 0-10V PWM depends on what your system is compatible with. Dimmable MEANWELL LED drivers can dim with either PWM or analog (3-in-1 or 2-in-1 dimming).

Check the specifications of your lighting system or contact your manufacturer to determine which dimming signal(s) are compatible with your setup.  

### 0-10V analog
0-10V analog is a simpler and more straightforward making it more commonly seen.  It's found in commercial lighting, ventliation systems, motor control etc.  An Analog signal is constant and at a voltage between 0-10V.

### 0-10V PWM
0-10V PWM sends a stream of upwards 3000 pulses of 10V per second.  The longer the pulses last the higher the voltage until it eventually becomes a constant 10V (analog) signal.  

## 0-5V PWM
This is voltage we usually find on the microcontrollers that connect to <a href="https://esphome.io">ESPhome</a> like the <a href="https://www.google.com/search?q=ESP32">ESP32</a> and expansion boards that work with the cheaper ESP8266 like the <a href="https://www.google.com/search?q=PCA9568">PCA9568 i2c to 16-channel pwm board</a>.

# Two examples

## 2 channel 0-10v Analog dimmer (up to 16 channels per ESP32)
The example below shows using an ESP32 to generate PWM signals connected into <a href="https://www.amazon.ca/s?k=PWM+to+0-10V+analog">PWM to 0-10V analog converter</a> boards.   ESP32's are a little expensive, if you already have an ESP8266, you can use a <a href="https://www.google.com/search?q=PCA9568">16-channel PCA9568 board</a>  with it to generate the PWM signals instead of a ESP32.  **There are bad reviews about some of these online! Watch out**
<img src="/images/Converting-5v-PWM-signals-from-ESP32-to-a-0-10v-Analog.png">

## 24 channel 0-10v PWM dimmer (Works with MEANWELL LED drivers)
<img src="/images/24-Channel-TLC5947-based-LED-Driver-dimmer-for-Home-Assistant.png">
     
