# STM32-bare-metal-gpio

Register-level GPIO programming on an STM32 NUCLEO-F401RE with **no HAL**, built from scratch to understand how the hardware works.

**Status:** Working (LED blink). Extending to further GPIO and peripherals.

## What it does

Blinks the on-board green LED (LD2, pin **PA5**) by writing directly to memory-mapped registers.

- Enables the GPIOA peripheral clock through `RCC_AHB1ENR` (bit 0)
- Configures PA5 as a general-purpose output using `GPIOA_MODER` (bits 11:10)
- Toggles the LED by writing to the output data register `GPIOA_ODR` (bit 5)
- For loop delay method

Register addresses and bit fields come from the STM32F401 reference manual (RM0368).

| Register | Address |
|---|---|
| `RCC_AHB1ENR` | `0x40023830` |
| `GPIOA_MODER` | `0x40020000` |
| `GPIOA_ODR`   | `0x40020014` |

## Reusable helpers

Small C helpers built on `volatile` memory-mapped register definitions:

- `set_bit` / `clear_bit` / `toggle_bit`
- masked read-modify-write for multi-bit register fields (e.g. the 2-bit `MODER` field per pin)

## Hardware

- Board: ST NUCLEO-F401RE (STM32F401RE, Arm Cortex-M4)
- Programmer/debugger: on-board ST-LINK

## Build and flash

1. Open the project in STM32CubeIDE.
2. Build (hammer icon).
3. Connect the board over USB and click Run/Debug to flash with ST-LINK.

## What I learned

- A peripheral's clock must be enabled before its registers do anything.
- Why register definitions need `volatile`.
- Changing one field in a register means masking and writing only those bits, so I don't effect the others.


## Next steps

- Decide on a more complex use of the STM32 NUCLEO-F401RE for the project.
