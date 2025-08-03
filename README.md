This is the simplest, the cheapest Ultrasonic Parametric Directional Speaker a man can build. This device uses ultrasound as a carrier for amplitude modulated audible signals and can send it in specified direction on high distance. 
The are two hardware versions - simple speaker with Audio Jack input ( you may connect to your mobile phone as audio source ) and Ultrasonic speaker with integrated MAX4466 Microphone Module that lets you to speak to the people on the higher distance. The power of this device relies on number of ultrasonic transducets used and how dense are they placed (they should be placed as a flat circular matrix) and the power of audio amplifier module (I strongly recomend experiment with H-type MOSFET bridges), 

ATTENTION! Please download and install RP2040-PWM library into your Arduino before compiling the code : https://github.com/khoih-prog/RP2040_PWM  

My design uses only : 
- RP2040-zero/RP2040-one board with Raspberry Pi Pico chip programmed in Arduino with Earle Philhower RP2040 core : https://github.com/earlephilhower/arduino-pico. I am using Linux based PC (Linux Mint / Ubuntu) for this Arduino + RP2040 core from Earle
- TPA3116D2 audio amplifier board ( XH - M543 MONO ) which I found working best with 25kHZ ultrasound transducers
- set of 10+ ultrasonic transducers for 25kHz or other (if different please modify "frequency" variable in the source code)
- 5V Voltage stabilizer : LM7805. This is to power RP2040 board from 12V, we need to provide 5V to 5V-pin of RP2040
- 2 resistors for 1:2 voltage divider at the ADC input of RP2040 chip. I am using two times 50 Ohm resistor here, but they can be different like 2x470Ohm, 2x1000Ohm.. They just need to be equal.
- couple of electrolytic capacitors for ADC stability and 5V power line stability. I am using 47uF between 5V/GND line and 330uF between 12V/GND.
- electrolytic capacitor to separate ADC input. I am using 10uF but it can be within the range 1uF-47uF. It will impact the audio signal level. The signal is AC and we are trying to make it in the range 0-3.3V

The RP2040 chip is programmed to use TWO CPU COREs at the very same time. First core is collecting information from ADC input ( converting analog voltage into digital value ) while second core is generating PWM (Pulse Width Modulated) signal to drive the audio amplifier and the set of ultrasonic transducers. I am using the "semaphore" variable to avoid collisions while reading/writing into the variable that holds ADC reading.

How it works ?
The Audio Signal gets digitized by Analog to Digital converter that is built-in into Raspberry Pico. The reading is a base to create Pulse Width Modulated signal with Duty Cycle that corresponds to ADC reading. Of course we need to convert AC signal in proper way because we need absolute value of the signal, therefore some additional calculations are made in the code. PWM signal is further send to audio amplifier TPA3116D which makes it strong enough to drive set of ultrasonic transducers. When ultrasound waves hit an obstacle, they get demodulated and audio signal becomes audible. The range is at least 15meters (I checked with only 15 transducers 25kHz)

