This is the simplest, the cheapest Parametric Directional Speaker. This device uses ultrasound as a carrier for amplitude modulated audible signals. 

My design uses only : 
- RP2040-zero/RP2040-one board with Raspberry Pi Pico chip programmed in Arduino with Earle Philhower RP2040 core : https://github.com/earlephilhower/arduino-pico
- TPA3116D2 audio amplifier board ( XH - M543 MONO ) which I found working best with 25kHZ ultrasound transducers
- set of 10+ ultrasonic transducers for 25kHz
- 5V Voltage stabilizer : LM7805
- 2 resistors for 1:2 voltage divider at the ADC input of RP2040 chip
- couple of electrolytic capacitors for ADC input separation as well as 5V power line stability

The RP2040 chip is programmed to use TWO CPU COREs at the very same time. First core is collecting information from ADC input ( converting analog voltage into digital value ) while second core is generating PWM (Pulse Width Modulated) signal to drive the audio amplifier nad set of ultrasonic transducers

When ultrasound waves hit an obstacle, they get demodulated and audio signal becomes audible. 

