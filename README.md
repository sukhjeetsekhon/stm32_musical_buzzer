# STM32 Library for a musical buzzer
An STM32 library for using a passive buzzer and PWM signals to make music.

## Overview


## Getting Started



### STM32CubeMX

*Make sure you have the STM32CubeMX software installed*

*This is also assuming you already have a project created*

1. Open your STM32 project file (.ioc) in STM32CubeMX

2. Go to Pinout & Configuration at the top-left

3. Under Categories, open Timers, choose your own or choose TIM1

4. Under TIM# Mode and Configuration, go to Mode

   a. Set Clock Source to Internal Clock

   b. Set Channel1 to PWM Generation CH1

5. Under TIM# Mode and Configuration, Go to Configuration

   a. Go to Parameter Settings

   b. Set Prescaler (Default: 4999)

   c. Make sure Counter Mode = Up

   d. Set Counter Period (ARR) (Default: 99)

   e. Remember the TIM#_CH# pin on the right Pinout view for wiring later

6. Go to Clock Configuration at the top-middle

   a. Find your Timer clock on the far-right

   b. Set the Timer frequency (Default: 60 MHz)

7. Save your configuration

8. Generate Code

### Configuration

1. Set TIM_FREQ to your Timer Frequency from the Clock Configuration (Default: 60000000)

2. Set TIM_ARR to your Timer Counter Period (AutoReload Register/ARR) (Default: 99)

3. Set BPM to your desired tempo at the quarter note

4. [Optional] Set the Articulation Delay to add an extremely short rest at the end of each note (Default: 5)

## Wiring Instructions

1. Wire the TIM#_CH# pin to the positive terminal of the buzzer

2. Wire the negative terminal of the buzzer to G/GND/Ground

## Troubleshooting

- If your buzzer is working but doesn't sound right, it may be an active buzzer which cannot play different PWM frequencies. You need a passive buzzer.