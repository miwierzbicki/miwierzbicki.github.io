---
layout: ../../layouts/MarkdownLayout.astro
title: STM32 μSD Datalogger
author: Mirosław Wierzbicki
date: 2024-05-04 10:01
status: finished ✅
repo: https://github.com/miwierzbicki/stm_datalogger
---

**Project Objective:**

The primary objective of this project was to develop a configurable, compact measurement data logger, strictly tailored to specific user requirements.

## Key Features:

- Data logging capability to a microSD card.
- Configurable sensor type and quantity.
- Configurable reporting parameters (frequency and status/state).
- Temperature measurement functionality.
- Voltage measurement functionality.
- User interface implemented via an OLED display and a rotary encoder.
- Designed as a shield form factor for the STM32 Black Pill board.

## Components:

- **Microcontroller:** STM32F411CEU6 (Black Pill development board).
- **Temperature Sensor:** Maxim DS18B20.
- **ADC Converter:** ADS1115 (External).
- **Internal ADC:** Utilizes the built-in ADC of the STM32F411CEU6 microcontroller.

### Main Project Files in the Repository

```
\Core\src*.c
\Core\inc*.h
```

## PCB Design

![PCB Design](/imgs/PCB.png)

## Completed Datalogger

![Completed device](/imgs/PCB_soldered.jpg)

## Module Breakdown

- adc.c – Handles operations related to the Analog-to-Digital Converters (ADCs).
- ds18b20.c – Handles communication with the DS18B20 temperature sensor.
- encoder.c – Handles the rotary encoder input.
- main.c – The main program file.
- menu.c – Manages the menu system and user interface logic.
- save.c – Handles the saving of measurement data.
- sd.c – Handles SD card operations.

## User Interface Schema

![UI Schema](/imgs/UI_schema.jpg)

### Editing the User Interface

```c
menu.h

struct Menu {
    void (*function)(Menu *menu);
    int entry_count;
    MenuEntry entries[8];
};

typedef struct MenuEntry {
    Screen entry;
    const char *entry_string;
} MenuEntry;

typedef enum {
    MAIN_MENU = 0,
    SENSOR_CONFIG,
    SENSOR_CONFIG_ADC_EXT,
    ...
} Screen;
```

The Menu structure consists of the following fields:

`void (*function)(Menu *menu)` – a pointer to a function that takes a pointer to the Menu structure as an argument.

`int entry_count` – the number of available options in the menu.

`MenuEntry entries[8]` – an array of available options; each option is a MenuEntry structure.

The MenuEntry structure consists of the following fields:

`Screen entry` – this field represents a specific view in the menu, of type Screen; values of this type are defined in an enumerated type (enum).

`const char *entry_string` – a pointer to a character string, the actual name of the item displayed on the screen.

An enumerated type (enum) named `Screen` has been created, containing a list of screens.

## UI Screen Flow
Menu navigation logic in the context of sensor configuration:
![UI screens](/imgs/Menu_options.jpg)

Menu navigation logic in the context of microSD card configuration:
![SD Configuration](/imgs/sd_config.png)

Error message displayed in the SD card configuration menu when the card is not detected or there is a file system-related error:
![SD Error](/imgs/sd_err.png)

Menu for previewing values from the external analog-to-digital converter:
![ADC Values real-time](/imgs/adc_values.png)

Menu navigation logic in the context of enabling or disabling measurements:
![ADC Values real-time](/imgs/change_state.png)


## Measurement Logic

Interrupts are utilized to define the timing for sensor readings. Four channels are defined, each with a different sampling rate. Interrupts are generated using the microcontroller's built-in timer in Output Compare No Output mode (counting up to the value in the CCRx register). A callback function is invoked within the interrupt service routine (ISR).

```c
void HAL_TIM_OC_DelayElapsedCallback(TIM_HandleTypeDef *htim) {
  uint32_t pulse;
  if(htim->Channel == HAL_TIM_ACTIVE_CHANNEL_1) {
    pulse = HAL_TIM_ReadCapturedValue(htim, TIM_CHANNEL_1);
    ch1przerwanie=1;
      __HAL_TIM_SET_COMPARE(htim, TIM_CHANNEL_1, (pulse + 50000));
  }
  ...
}
  ```

## <br>Saving to microSD Card

The FatFS file system library is used via the SPI interface, utilizing the "cubeide-sd-card" library by kiwih.

Data is written through a circular buffer (100 entries) and managed by the FatFS controller to a .csv file on the microSD card.

Each line in the file contains all recorded sensor data along with a timestamp from the Real-Time Clock (RTC).

```csv
<NEW_MEASURE_BEGIN>
timestamp,adc_ext_ch0,adc_ext_ch1,adc_ext_ch2,adc_ext_ch3,adc_int_ch0,adc_int_ch1,adc_int_ch2,adc_int_ch3,ds18b20_1,ds18b20_2,ds18b20_3
01:01:14 06/12/23,3.287618,,0.902696,,0.500317,,,,,,
01:01:14 06/12/23,3.287618,,0.901949,,0.500317,,,,,,
01:01:14 06/12/23,3.287618,,0.902136,,0.499512,,,,,,
01:01:14 06/12/23,3.287618,,0.903629,,0.498706,,,,,,
```

## <br>Debugging

Debugging messages related to the SD card controller are implemented and output via the COM port (using a UART<->USB bridge). 

```
sd status: SD ERR
<DEVICE ERROR>
sd status: FR_OK
<DEVICE READY>
```

## <br>Measurements / Testing

###   ADC linearity measurements.

Measurements of the ADC were made using a custom voltage stabilizer, Keysight EDU36311A power supply and a Keithley 2000 multimeter. Measurements were made in 100 mV increments (starting from 3.3 V down to 0 V).
* Voltage measurements compared against a reference standard show that the converter's response closely matches the ideal characteristic after linear fitting. This confirms good linearity across the full measurement range.
![ADC linearity measurement](/imgs/ADC_measure.jpg)
###  Gain error measurement.
* The difference between the values obtained from the analog-to-digital converter and the reference standard was calculated. This discrepancy was observed to decrease as the measured voltage decreased. Consequently, this allows for the potential development of correction functions in future enhancements to achieve better converter calibration within the desired measurement range.
![ADC linearity measurement](/imgs/ADC_measure_delta.jpg)
###   Operating time measurement under typical usage conditions.
* Assuming a worst-case average current consumption scenario of 35 mA, the average operating time of the device was estimated when powered by a portable battery bank (power bank) with a nominal capacity of 10 Ah at a battery voltage of 3.7 V. Due to the necessary voltage conversion from the cells' 3.7 V to the required 5 V level, this power bank would have an effective capacity of approximately 7.4 Ah.

* Based on these figures, it was calculated that the theoretical operating time of the device powered by the aforementioned power bank would be 102 days. This represents an ideal value and does not account for conversion losses or fluctuations in the datalogger's current draw.