# TimerPulseIn

Arduino Library for Measuring Positive Impulses in nanoseconds resolution.

## Description

Class for measuring positive impulses in nanoseconds using the Timer 1 capture unit on an Arduino board. It provides a convenient way to capture the duration of a positive pulse and can be useful for various applications involving pulse width measurements.

The class offers the following settings in the constructor:
- pin: The Arduino input pin with the input capture function from Timer 1. This is the pin where the PWM signal is connected.
- prescaler: The prescaler for Timer 1. The prescaler determines the frequency at which the counter increments. It is an enumeration of the `Prescaler` type, which provides adjustable values. The available options are `PS_1`, `PS_8`, `PS_64`, `PS_256`, and `PS_1024`.
- measurements: The number of measurements to be made and then averaged. This parameter determines how many measurements will be taken to calculate an average pulse duration.
- timeout: The number of microseconds to wait for the start of a measurement. If the input pulse doesn't start within this timeout period, the measurement will be considered unsuccessful and 0 will be returned.

## Usage

1. Include the `TimerPulseIn.h` header file in your Arduino sketch.
2. Create an instance of the `TimerPulseIn` class, specifying the necessary parameters in the constructor.
3. Call the `begin()` method to set up the Timer 1 and the input pin.
4. Use the `pulseIn()` method to measure the duration of the positive pulse in nanoseconds.

## Formulas

The `TimerPulseIn` class utilizes the following formulas for calculating the resolution of the measurement and the time in seconds:

- Resolution of measurement:
  $`R_{t,IMP} = N_{Prescaler} / f_{Microcontroller}`$

- Time calculation:
  $`t_IMP = ((k_{Overflow} * 65536) + k_{Counter}) * t_{Tick}`$

Where:
- $`t_{Takt}`$ : Duration of a counter clock cycle
- $`R_{t,IMP}`$ : Resolution of pulse duration measurement
- $`N_{Prescaler}`$ : Counter prescaler
- $`f_{Mikrocontroller}`$ in Hz: Operating frequency of the microcontroller (16MHz)
- $`k_{Counter}`$ : Value of the counter
- $`k_{Overflow}`$ : Number of counter overflows

## Example

Here's an example usage of the TimerPulseIn class to measure the duration of a positive pulses with a resolution of 500ns and where 5 meassurements are made:

```cpp
#include <TimerPulseIn.h>

// Create an instance of TimerPulseIn in pin 4 with a prescaler of PS_8 and 5 meassurenments for averaging
TimerPulseIn timer(4, TimerPulseIn::PS_8, 5, 1000);

void setup() {
  timer.begin();
  Serial.begin(9600);
}

void loop() {
  unsigned long pulseDuration = timer.pulseIn();
  Serial.print("Pulse duration: ");
  Serial.print(pulseDuration);
  Serial.println(" ns");

  delay(1000);
}
```

## Author
This class was created by Alejandro Miguel Kölbl.

## License
This project is licensed under the MIT License. Feel free to modify and adapt the class as needed for your project.
