#  How to Debug STM32 (or Any ARM MCU) Using ESP32JTAG + VSCode + Platformio

This guide explains how to set up **VSCode** and **Platformio** to debug an STM32 MCU using **ESP32JTAG**, a wireless JTAG interface based on ESP32 + FPGA.

For more info on ESP32JTAG, please visit:
👉 [https://www.crowdsupply.com/ez32/esp32jtag](https://www.crowdsupply.com/ez32/esp32jtag)

---

## 1. Install Prerequisites

### Visual Studio Code
Download and install VSCode from:
👉 [https://code.visualstudio.com/](https://code.visualstudio.com/)

### Platforio Extension
In VSCode, open the Extensions view (**Ctrl+Shift+X**), search for **“Platformio”**, and install it.

---

## 2. Preparing the Code

It should be any **GNU Makefile–based** project for your target board.

Here we prepared a project for **STM32F103C8T6 (BluePill)**.
You can download and try it — it also includes the required configuration files to use with **ESP32JTAG + VSCode + Cortex-Debug**.

👉 **GitHub Repository:** [https://github.com/EZ32Inc/stm32f103c8t6test](https://github.com/EZ32Inc/stm32f103c8t6test)

**Code copy:**
```bash
git clone https://github.com/EZ32Inc/stm32f103c8t6test.git
```

The BluePill STM32F103C8 board is used for this project.

---

## 3. Board Info

### Boot Modes

A couple of special MCU pins has to be set-up to proper logical values to enter the bootloader. The pins are named BOOT0 and BOOT1 on the STM32 microcontroller. Boot pins can select several modes of bootloader operation:

| BOOT1  | BOOT0  | Boot Mode         | Aliasing                                    |
| ------ | ------ | ----------------- | ------------------------------------------- |
| X      | 0      | Main Flash Memory | Main flash memory is selected as boot space |
| 0      | 1      | System Memory     | System memory is selected as boot space     |
| 1      | 1      | Embedded SRAM     | Embedded SRAM is selected as boot space     |

### Pinout

![image](https://user-images.githubusercontent.com/1549028/213869634-1ede5169-8cdf-4ff9-8a94-26daba5fbd69.png)

### Schematics

![image](https://user-images.githubusercontent.com/1549028/213869613-a7071a58-811e-42a3-b75f-5759ac5d6baa.png)

### Resources

- [https://www.crowdsupply.com/ez32/esp32jtag](https://www.crowdsupply.com/ez32/esp32jtag)
- [https://code.visualstudio.com/](https://code.visualstudio.com/)
- [https://github.com/m3y54m/start-stm32](https://github.com/m3y54m/start-stm32)
- [Setting-up cross compiler and build tools for STM32](https://freeelectron.ro/arm-cross-compiler-tutorial-stm32/)
- [Bare Metal STM32 Programming – LED Blink](https://freeelectron.ro/bare-metal-stm32-led-blink/)
- [Bare Metal - From zero to blink](https://www.linuxembedded.fr/2021/02/bare-metal-from-zero-to-blink)
- [STM32 toolchain for Windows](https://embeddedgeek.net/posts/STM32-toolchain-for-windows/)
- [STM32 toolchain for Windows - Part 1 (CubeMX, GCC, Make and OpenOCD)](https://youtu.be/PxQw5_7yI8Q)
- [Visual Studio Code for STM32 development and debugging - Part 2](https://youtu.be/xaC5oWwzOt0)
- [STM32 Startup code, Bare metal - Part 3](https://youtu.be/7stymN3eYw0)
- [stm32-for-vscode](https://marketplace.visualstudio.com/items?itemName=bmd.stm32-for-vscode)
- [Program STM32F4 with UART](http://stm32f4-discovery.net/2014/09/program-stm32f4-with-uart/)
- [STM32 LED Blink](https://stm32world.com/wiki/STM32_LED_Blink)
- [Blinking LED with STM32CubeMX and HAL](https://wiki.st.com/stm32mcu/wiki/STM32StepByStep:Step2_Blink_LED)
- [UART and new board introduction](https://wiki.st.com/stm32mcu/wiki/STM32StepByStep:Step3_Introduction_to_the_UART)
- [Using the STM32 UART interface with HAL ](https://visualgdb.com/tutorials/arm/stm32/uart/hal/)
- [st-flash tool](https://github.com/stlink-org/stlink)

## 4. Credit goes to:
[https://github.com/m3y54m/start-stm32](https://github.com/m3y54m/start-stm32)

Thank you!
