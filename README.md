## TASK_CREATION_SCHEDULING_FREERTOS_STM32
A simple Real-Time Operating System (RTOS) project demonstrating multi-threading on an STM32 microcontroller. The project uses FreeRTOS wrapped with the CMSIS-RTOS v2 API to independently toggle three LEDs at different frequencies using separate tasks.

Hardware Setup
Target MCU: STM32 (configured for 84 MHz SYSCLK)

LEDs: Connected to pins PA5, PA6, and PA7.

Debugger: Serial Wire (SWD).

1. Pinout & Core Configuration
System Core > RCC: High Speed Clock (HSE) set to Crystal/Ceramic Resonator.

System Core > SYS:

Debug: Serial Wire

Enable System Wake-Up

Timebase Source: SysTick

GPIO: PA5, PA6, and PA7 configured as GPIO_Output.

2. Clock Configuration
The clock tree is configured as follows:

HSE Input Frequency: 8 MHz.

Main PLL: /M = 8, *N = 168, /P = 2, /Q = 4.

System Clock Mux: PLLCLK selected.

SYSCLK: 84 MHz.

AHB Prescaler: /1 (HCLK = 84 MHz).

APB1 Prescaler: /4 (PCLK1 = 21 MHz, Timers = 42 MHz).

APB2 Prescaler: /2 (PCLK2 = 42 MHz, Timers = 84 MHz).

3. Middleware (FreeRTOS)
Interface: CMSIS v2

Config Parameters:

MINIMAL_STACK_SIZE: 256 words

TOTAL_HEAP_SIZE: 20000 bytes

Memory Management: heap_4

Advanced Settings:

USE_NEWLIB_REENTRANT: Enabled

Tasks Configuration:

TASK1: Priority osPriorityLow, Stack Size 256 Words

TASK2: Priority osPriorityNormal, Stack Size 256 Words

TASK3: Priority osPriorityHigh, Stack Size 256 Words

Important Code Modifications
⚠️ Critical FreeRTOS Configuration Step:
To prevent the system from halting unexpectedly during debugging or runtime, the configASSERT macro must be disabled in FreeRTOSConfig.h.

Navigate to Core/Inc/FreeRTOSConfig.h and comment out the following line:

C
// #define configASSERT( x ) if ((x) == 0) {taskDISABLE_INTERRUPTS(); for( ;; );}
### Task Execution Overview
The application schedules three separate tasks that run infinitely, yielding to the RTOS scheduler using osDelay():

TASK1 (Low Priority): Toggles pin PA5 every 200 ms.

TASK2 (Normal Priority): Toggles pin PA6 every 500 ms.

TASK3 (High Priority): Toggles pin PA7 every 1000 ms.

How to Build and Run
### I uploaded the zip file of this project in case there is any issue. just extract the zip file and import in the stmcube IDE for the reference.
Open the project in STM32CubeIDE.

Build the project (Project -> Build All).

Connect your STM32 board via ST-LINK.

Flash the code and start the debugging session.
