# RP2350 A2 / Pico 2 on Gusman Logic Analyzer V6

## Status

**Tested working.** This document describes the RP2350 A2 / Raspberry Pi Pico 2 configuration that produced multiple successful logic-analyzer captures on the Gusman Logic Analyzer V6 hardware.

The working source state is preserved in this fork as:

- Commit: `956a9e407c15a65295af6915305577f53d47479e`
- Tag: `rp2350a2-working-v1`

This is a hardware-tested fork-specific configuration. It is not presented as proof of the underlying RP2350 silicon failure mechanism, and it has not been submitted upstream from this fork.

## Tested firmware binary

Prebuilt firmware for the tested configuration:

`pico_sdk_sigrok/release/pico2_rp2350a2_gusman_logic_analyzer_v6_baseline_v1.uf2`

SHA-256:

`c62048c2b2a5ee76e11a46fb2ac9e0f208f3e75fc932579b77b79ccca8128405`

File size: 127488 bytes.

## Source changes in the working configuration

The working configuration differs from the upstream `main` state in three source files.

### 1. Build for Pico 2 / RP2350

In `pico_sdk_sigrok/CMakeLists.txt`:

- Pico SDK `2.1.0` -> `2.3.1`
- picotool `2.1.0` -> `2.3.1`
- `PICO_BOARD pico` -> `PICO_BOARD pico2`

### 2. Use baseline analyzer mode

In `pico_sdk_sigrok/sr_device.h`:

- `PICO_MODE 2` -> `PICO_MODE 0`

Baseline mode exposes 21 digital inputs and 3 analog inputs.

### 3. Do not assign GP1 to UART RX

The upstream baseline initialization configures GP0 and GP1 for UART even though the source comment already states that UART RX is not used. On the tested RP2350 A2 hardware, the working configuration keeps UART TX on GP0 but does not connect GP1 to the UART peripheral.

Instead, GP1 is initialized as a normal input with a pull-up:

```c
#if (UART_EN == 1)
  uart_set_format(uart0, 8, 1, 0);
  uart_init(uart0, UART_BAUD);
  gpio_set_function(0, GPIO_FUNC_UART);

  // UART RX is unused. Do not connect GP1 to UART on RP2350 A2.
  // Keep it at a defined level so noise cannot continuously fill the UART RX FIFO.
  gpio_init(1);
  gpio_set_dir(1, GPIO_IN);
  gpio_pull_up(1);
#endif
```

The important observation is empirical: after GP1 was removed from the UART RX function in this build, the analyzer completed successful captures. The exact low-level cause has not been isolated independently from the rest of this tested configuration.

## Build with VS Code

The tested firmware was built with the Raspberry Pi Pico tooling in VS Code.

1. Open `pico_sdk_sigrok` as the project.
2. Use Raspberry Pi Pico SDK 2.3.1.
3. Build for board `pico2`.
4. Build the `pico_sdk_sigrok` target.
5. Flash the generated UF2 to the Pico 2 in BOOTSEL mode.

The source in tag `rp2350a2-working-v1` already contains the required board and mode settings.

## Verify with sigrok

Use a current sigrok build with the `raspberrypi-pico` driver. Replace `COMx` with the serial port assigned by the operating system:

```text
sigrok-cli -d raspberrypi-pico:conn=COMx:serialcomm=115200/flow=0 --scan
```

A successful scan should enumerate the baseline channels rather than timing out during the serial identify exchange.

## Hardware scope

This build is intended for the Pico 2 / RP2350 A2 configuration used on the Gusman Logic Analyzer V6 hardware. Treat it as a tested hardware-specific build rather than a universal RP2350 fix until it has been reproduced on additional boards.

## Related upstream discussion

- sigrok-pico RP2350 Rev A deadlock issue: https://github.com/pico-coder/sigrok-pico/issues/55
- Upstream sigrok-pico project: https://github.com/pico-coder/sigrok-pico

## License

This fork remains under the upstream project's GNU General Public License v3.0.
