# AVR Real-Time Clock

## Project Overview

This project implements a **software-based real-time clock system using an AVR microcontroller**.

The system uses **Timer0 overflow interrupts** to generate periodic timing events and maintain the current time in **hours, minutes, and seconds**. Users can manually set the initial time through a **4×4 keypad**, while an LCD provides the user interface and displays the current time.

The project also uses a **multiplexed 7-segment display** to display the seconds and is structured using a layered embedded software architecture consisting of **MCAL, HAL, and LIB** modules.

## Main Features

* AVR Timer0-based timekeeping
* Timer overflow interrupt and callback mechanism
* Hours, minutes, and seconds tracking
* 24-hour clock format
* Manual time configuration using keypad
* Input validation for hours, minutes, and seconds
* Automatic seconds-to-minutes rollover
* Automatic minutes-to-hours rollover
* 24-hour rollover to `00:00:00`
* LCD-based user interface
* Multiplexed 7-segment display for seconds
* Custom DIO driver
* Custom Timer driver
* Custom LCD driver
* Custom Keypad driver
* Layered MCAL/HAL/LIB architecture

## System Architecture

```text
                    +----------------------+
                    |      AVR MCU         |
                    |                      |
                    |      Timer0          |
                    +----------+-----------+
                               |
                        Overflow Interrupt
                               |
                               v
                    +----------------------+
                    |    Timer Callback    |
                    |    Timer_action()    |
                    +----------+-----------+
                               |
                               v
                    +----------------------+
                    | Time Variables       |
                    | Hours / Minutes /    |
                    | Seconds              |
                    +----------+-----------+
                               |
                 +-------------+-------------+
                 |                           |
                 v                           v
        +----------------+          +----------------+
        |      LCD       |          |  7-Segment     |
        | Time / Setup   |          | Seconds Display |
        +----------------+          +----------------+
                 ^
                 |
        +----------------+
        |    Keypad      |
        |  Time Setting  |
        +----------------+
```

## Timekeeping Mechanism

The system initializes Timer0 and enables its overflow interrupt.

The callback function:

```c
void Timer_action(void)
{
    L_u8Seconds++;
}
```

is used to update the seconds counter.

The main loop then handles the time rollover:

```text
60 seconds
    ↓
1 minute + 0 seconds

60 minutes
    ↓
1 hour + 0 minutes

24 hours
    ↓
00:00:00
```

This provides the basic digital clock functionality without requiring a dedicated RTC IC.

## Time Setting

The user can press **`1`** to enter the time-setting mode.

The system then requests:

1. Hours
2. Minutes
3. Seconds

The keypad input is validated according to the allowed clock ranges.

For example:

* Hours: `00–23`
* Minutes: `00–59`
* Seconds: `00–59`

After the complete time is entered, the system resumes normal timekeeping.

## Display System

The project uses two display mechanisms.

### LCD

The LCD provides:

* Time-setting instructions
* Current time
* User interaction feedback

### 7-Segment Display

The seconds are displayed using a multiplexed seven-segment configuration.

A lookup table converts decimal digits into seven-segment patterns:

```c
static u8 R_u8SsdData[] = {
    0x3f, 0x06, 0x5b, 0x4f, 0x66,
    0x6d, 0x7d, 0x07, 0x7f, 0x6f
};
```

## Software Architecture

The project follows a layered embedded software structure:

```text
Application Layer
│
└── GccApplication2.c
       │
       ├── Timekeeping Logic
       ├── User Interface
       └── Display Control
       │
       ├───────────────┐
       ↓               ↓
     HAL             MCAL
       │               │
       ├── LCD         ├── DIO
       └── Keypad      └── Timer
       │
       └───────────────
               ↓
             LIB
       ├── BIT_MATH
       └── STD_TYPES
```

## Project Structure

```text
AVR-Real-Time-Clock/
│
├── GccApplication2.c
├── GccApplication2.cproj
│
├── HAL/
│   ├── KeyPad/
│   │   ├── KP_conf.h
│   │   ├── KP_int.h
│   │   ├── KP_private.h
│   │   └── KP_prog.c
│   │
│   └── LCD/
│       ├── LCD_conf.h
│       ├── LCD_int.h
│       ├── LCD_private.c
│       ├── LCD_private.h
│       └── LCD_prog.c
│
├── MCAL/
│   ├── DIO/
│   └── Timer/
│
├── LIB/
│   ├── BIT_MATH.h
│   └── STD_TYPES.h
│
└── .vscode/
```

## Technologies

* AVR Microcontroller
* Embedded C
* Timer0
* Interrupts
* GPIO / DIO
* LCD
* Keypad
* 7-Segment Display
* Modular Embedded Software Architecture
* MCAL / HAL / LIB

## Roadmap

* [x] Implement software-based real-time clock using AVR Timer0
* [x] Implement Timer0 overflow interrupt
* [x] Implement one-second time update
* [x] Implement hours, minutes, and seconds tracking
* [x] Implement 24-hour time format
* [x] Implement manual time setting through keypad
* [x] Add input validation for hours, minutes, and seconds
* [x] Implement seconds-to-minutes rollover
* [x] Implement minutes-to-hours rollover
* [x] Implement 24-hour rollover to `00:00:00`
* [x] Display time on LCD
* [x] Display seconds using multiplexed 7-segment output
* [x] Implement LCD driver
* [x] Implement keypad driver
* [x] Implement DIO driver
* [x] Implement configurable Timer driver
* [x] Organize firmware using MCAL, HAL, and LIB layers
* [ ] Improve time-display formatting with leading zeros
* [ ] Improve timer accuracy and calibration
* [ ] Improve keypad input handling and user interface
* [ ] Add alarm functionality
* [ ] Add date and calendar functionality
* [ ] Add dedicated RTC hardware support such as DS1307/DS3231



## My Role

**Embedded Systems Developer**

* Developed the AVR firmware for the digital clock.
* Implemented Timer0-based timekeeping.
* Developed and integrated LCD and keypad interfaces.
* Implemented time-setting and input validation logic.
* Developed modular DIO, Timer, LCD, and Keypad drivers.
* Implemented multiplexed seven-segment display control.
