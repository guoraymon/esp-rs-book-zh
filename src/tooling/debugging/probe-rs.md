# probe-rs

probe-rs 项目是一组使用各种调试探针与嵌入式 MCU 交互的工具。它类似于 openOCD、PyOCD、Segger 工具等。支持 ARM 和 RISCV 架构以及一系列工具，包括但不限于：

- Debugger
  - GDB 支持
  - 用于交互式调试的 CLI
  - VSCode 扩展
- RTT (实时传输)
  - 类似于 IDF 的 app_trace 组件
- Flashing algorithms

有关 probe-rs 以及如何设置项目的更多信息，请访问 [probe.rs](https://probe.rs/) 网站。

## `USB-JTAG-SERIAL` peripheral for ESP32-C3

从 `probe-rs` v0.12 开始，可以使用内置的 `USB-JTAG-SERIAL` 外设对 ESP32-C3 进行烧写和调试，无需任何外部硬件调试器。有关配置接口的更多信息可以在[官方文档]中找到。

## 支持乐鑫 Espressif 芯片

`probe-rs` 目前仅支持 `ARM` & `RISC-V`，因此这限制了目前可以使用的 Espressif 芯片数量。

|   Chip   | Flashing | Debugging |
| :------: | :------: | :-------: |
| ESP32-C3 |    ✅     |     ⚠️     |

**请注意**: _标有 ⚠️ 的项目目前正在进行中，可用，但可能会出现错误。_

[官方文档]: https://docs.espressif.com/projects/esp-idf/en/latest/esp32c3/api-guides/jtag-debugging/configure-builtin-jtag.html

## 权限 - Linux

在 Linux 上，您可能会在尝试与 Espressif 交互时遇到权限问题。安装以下 `udev` 规则并重新加载应该可以解决该问题。

```udev
# Espressif dev kit FTDI
ATTRS{idVendor}=="0403", ATTRS{idProduct}=="6010", MODE="660", GROUP="plugdev", TAG+="uaccess"

# Espressif USB JTAG/serial debug unit
ATTRS{idVendor}=="303a", ATTRS{idProduct}=="1001", MODE="660", GROUP="plugdev", TAG+="uaccess"

# Espressif USB Bridge
ATTRS{idVendor}=="303a", ATTRS{idProduct}=="1002", MODE="660", GROUP="plugdev", TAG+="uaccess"
```

<!-- TODO: when probe-rs can actually debug at least a C3 with decent back traces etc, add a section here with an example config: see https://github.com/probe-rs/probe-rs/issues/877 -->
