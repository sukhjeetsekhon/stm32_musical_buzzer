# STM32 Musical Buzzer Library

A lightweight STM32 library for driving a **passive buzzer** using PWM signals to play melodies and musical sequences.

> ⚠️ **Passive buzzers only.** Active buzzers cannot modulate frequency and are incompatible with this library. See [Troubleshooting](#troubleshooting) if you are unsure.

---

## Overview

This library generates precise musical tones on an STM32 microcontroller by leveraging hardware timers and PWM output. By controlling the timer's frequency and duty cycle, the library manages the **pitch** and **duration** of each note played through a passive buzzer.

**Key features:**
- Full range of the piano available for musical notes
- PWM-based tone generation for accurate pitch control
- Configurable BPM (tempo) for musical timing
- Optional articulation delay for natural note separation
- Compatible with any STM32 timer channel that supports PWM output

---

## Getting Started

### Prerequisites

- [STM32CubeMX](https://www.st.com/en/development-tools/stm32cubemx.html) installed
- An existing STM32CubeMX project (`.ioc` file)
- A **passive** buzzer

---

### Step 1 — STM32CubeMX Configuration

1. Open your STM32 project file (`.ioc`) in **STM32CubeMX**

2. Navigate to **Pinout & Configuration** (top-left tab)

3. Under **Categories**, expand **Timers** and select a timer (e.g., `TIM1`)

4. Under **TIM# Mode and Configuration → Mode**:
   - Set **Clock Source** → `Internal Clock`
   - Set **Channel 1** → `PWM Generation CH1`

5. Under **TIM# Mode and Configuration → Configuration → Parameter Settings**:
   - Set **Prescaler** *(Default: `4999`)*
   - Set **Counter Mode** → `Up`
   - Set **Counter Period (ARR)** *(Default: `99`)*
   - Note the **TIM#_CH# pin** shown in the Pinout view — you will need this for wiring

6. Navigate to the **Clock Configuration** tab (top-center):
   - Locate your **Timer clock** on the far right
   - Set the **Timer frequency** *(Default: `60 MHz`)*

7. **Save** your configuration and click **Generate Code**

---

### Step 2 — Library Configuration (`musical_config.h`)

Open `musical_config.h` and update the following values to match your CubeMX settings:

| Constant | Description | Default |
|---|---|---|
| `TIM_FREQ` | Timer frequency from Clock Configuration (in Hz) | `60000000` |
| `TIM_ARR` | Counter Period (AutoReload Register) | `99` |
| `BPM` | Desired tempo, defined at the quarter note | *(user-defined)* |
| `ARTICULATION_DELAY` | *(Optional)* Short rest appended to each note for separation | `5` |

---

### Step 3 — Wiring

1. Connect the **TIM#_CH# pin** → positive terminal **(+)** of the buzzer
2. Connect the **negative terminal (−)** of the buzzer → any **GND** pin on the board

---

## Troubleshooting

**The buzzer makes sound, but it all sounds the same pitch / doesn't play melodies correctly.**
> You likely have an **active buzzer**. Active buzzers use a fixed internal oscillator and cannot respond to PWM frequency changes. Replace it with a **passive buzzer**.

**No sound at all.**
> - Double-check your wiring against the **TIM#_CH# pin** noted during CubeMX setup
> - Confirm that PWM output is being started in your code (e.g., `HAL_TIM_PWM_Start(...)`)
> - Verify your Clock Configuration frequency matches `TIM_FREQ` in `musical_config.h`

**Notes sound too fast or too slow.**
> Confirm that `BPM` is set correctly and that `TIM_FREQ` exactly matches your timer clock frequency from CubeMX.
`

**My project is not building/compiling.**
> - Make sure that your CMakeLists.txt contains your include directory and source files.**
> - It should look like this:
```
   # CMakeLists.txt
   # Add sources to executable
   target_sources(${CMAKE_PROJECT_NAME} PRIVATE
       # Add user sources here
       Core/Src/musical_player.c
   )
   
   # Add include paths
   target_include_directories(${CMAKE_PROJECT_NAME} PRIVATE
       # Add user defined include paths
       Core/Inc
   )
```
