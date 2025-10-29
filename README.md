#  How to Debug STM32 (or Any ARM MCU) Using ESP32JTAG + VSCode + Platforio

This guide explains how to set up **VSCode** and **Cortex-Debug** to debug an STM32 MCU using **ESP32JTAG**, a wireless JTAG interface based on ESP32 + FPGA.

---

## 1. Install Prerequisites

### Visual Studio Code
Download and install VSCode from:
👉 [https://code.visualstudio.com/](https://code.visualstudio.com/)

### Platforio Extension
In VSCode, open the Extensions view (**Ctrl+Shift+X**), search for **“Platformio”**, and install it.

---

## 3. Preparing the Code

It should be any **GNU Makefile–based** project for your target board.

Here we prepared a project for **STM32F103C8T6 (BluePill)**.
You can download and try it — it also includes the required configuration files to use with **ESP32JTAG + VSCode + Cortex-Debug**.

👉 **GitHub Repository:** [https://github.com/EZ32Inc/start-stm32](https://github.com/EZ32Inc/start-stm32)

**Code copy:**
```bash
git clone https://github.com/EZ32Inc/start-stm32.git
cd start-stm32/blinky-hal
make clean
make
```

The BluePill STM32F103C8 board is used for this project.

## LED Blink Program

### [With No Libraries](blinky-no-lib) 

This program is based on the article ***["Bare Metal STM32 Programming – LED Blink"](https://freeelectron.ro/bare-metal-stm32-led-blink/)*** without using any external libraries (except `stdint.h` which is only used to define `uint32_t`).

In order to compile and link this program we need the main program source file [`main.c`](blinky-no-lib/src/main.c), the linker script file [`linker.ld`](blinky-no-lib/src/linker.ld), and the C run-time assembly file [`crt.s`](blinky-no-lib/src/crt.s).

```c
#include <stdint.h>

// register address
#define RCC_BASE      0x40021000
#define GPIOC_BASE    0x40011000

#define RCC_APB2ENR   *(volatile uint32_t *)(RCC_BASE   + 0x18)
#define GPIOC_CRH     *(volatile uint32_t *)(GPIOC_BASE + 0x04)
#define GPIOC_ODR     *(volatile uint32_t *)(GPIOC_BASE + 0x0C)

// bit fields
#define RCC_IOPCEN   (1<<4)
#define GPIOC13      (1UL<<13)

void main(void)
{
    RCC_APB2ENR |= RCC_IOPCEN;
    GPIOC_CRH   &= 0xFF0FFFFF;
    GPIOC_CRH   |= 0x00200000;

    while(1)
    {
        GPIOC_ODR |=  GPIOC13;
        for (int i = 0; i < 500000; i++); // arbitrary delay
        GPIOC_ODR &= ~GPIOC13;
        for (int i = 0; i < 500000; i++); // arbitrary delay
    }
}
```

### [Using STM32 HAL](blinky-hal)

This program is based on the article ***["STM32 LED Blink"](https://stm32world.com/wiki/STM32_LED_Blink)*** which uses STM32 HAL to configure the microcontroller and blink the LED.

The project initialization is done by STM32CubeMX.

```c
  /* Infinite loop */
  /* USER CODE BEGIN WHILE */
  while (1)
  {

	// Toggle the LED
	HAL_GPIO_TogglePin(GPIOC, GPIO_PIN_13);

	// Wait for 500 ms
	HAL_Delay(500);

	// Rinse and repeat :)

    /* USER CODE END WHILE */

    /* USER CODE BEGIN 3 */
  }
  /* USER CODE END 3 */
```

### Boot Modes

A couple of special MCU pins has to be set-up to proper logical values to enter the bootloader. The pins are named BOOT0 and BOOT1 on the STM32 microcontroller. Boot pins can select several modes of bootloader operation:

| BOOT1  | BOOT0  | Boot Mode         | Aliasing                                    |
| ------ | ------ | ----------------- | ------------------------------------------- |
| X      | 0      | Main Flash Memory | Main flash memory is selected as boot space |
| 0      | 1      | System Memory     | System memory is selected as boot space     |
| 1      | 1      | Embedded SRAM     | Embedded SRAM is selected as boot space     |

## Pinout

![image](https://user-images.githubusercontent.com/1549028/213869634-1ede5169-8cdf-4ff9-8a94-26daba5fbd69.png)

## Schematics

![image](https://user-images.githubusercontent.com/1549028/213869613-a7071a58-811e-42a3-b75f-5759ac5d6baa.png)

## Resources

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
