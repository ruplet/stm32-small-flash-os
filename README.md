# Simple operating system for Cortex-M0 STM32 microcontrollers

*This README was revised in April 2026. The rest of the files are original versions from 2019 and have not been developed since then.*

While working on the [EUCYS UAV submission](https://github.com/ruplet/mid-flight-uav-solar-charging-eucys/tree/master), we needed to use a microcontroller with very small Flash memory. We were optimizing for the smallest possible die size to keep the PCB compact, so a full RTOS was not an option - it would not fit in 8 KiB of Flash.

Because of that constraint, I started re-implementing only the operating-system-like pieces we actually needed: basic threading abstractions, a scheduler, filesystem support, and an SD card driver. After gaining experience with these modules, I glued them together into one uniform layer for STM32 microcontrollers. This repository is that result: a minimal operating system for constrained Cortex-M0 targets.

This project became the basis of my submission to the Meet IT Foundation contest, where it received 1st place in Poland. The original submission paper from 2019 is available here (in Polish): [Meet IT submission paper](./meetit-submission.pdf).

It has also led to my first industrial experience: the reward for winning the contest was an internship in a software house. I have completed it during the summer after high school.

## Overview

This repository implements STM32 abstractions that are typically associated with operating systems, but are less common in small bare-metal projects (see feature list below). It avoids ST HAL and uses CMSIS-level code only, because binary size was a critical constraint on boards with very small Flash memory (~8 KiB).

## Features

- A kernel thread abstraction + a round-robin scheduler
- SDHC card driver
- FAT32 support via a port of [ChaN's Petit FATFS](https://elm-chan.org/fsw/ff/00index_p.html)
- `O(1)` context switch in the `PendSV` IRQ handler
- SPI communication abstraction layer
- Semihosting support

## Getting Started

1. Pick any file from `example/`.
2. Rename it to `main.c`.
3. Place it in `src/`.
4. Compile with:

```bash
make
```

5. Run OpenOCD:

```bash
make con
```

6. Flash the program:

```bash
make flash
```

7. In the GDB console that opens, run:

```gdb
continue
```
