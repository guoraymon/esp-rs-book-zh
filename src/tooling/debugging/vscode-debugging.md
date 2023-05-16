# 在 Visual Studio Code 上调试

您可以直接在 Visual Studio Code 中使用图形输出进行调试。

## ESP32

### 硬件设置

ESP32 没有内置 JTAG 接口，需要外接 JTAG 转接器到 ESP32 开发板，例如可以使用 [ESP-Prog](https://docs.espressif.com/projects/espressif-esp-iot-solution/en/latest/hw-reference/ESP-Prog_guide.html)。

|  ESP32 Pin  | JTAG Signal |
| :---------: | :---------: |
| MTDO/GPIO15 |     TDO     |
| MTDI/GPIO12 |     TDI     |
| MTCK/GPIO13 |     TCK     |
| MTMS/GPIO14 |     TMS     |
|     3V3     |    VJTAG    |
|     GND     |     GND     |

**注意**：在 Windows 上 `USB 串行转换器 A 0403 6010 00` 驱动程序应该是 WinUSB。

### VSCode 设置

1. 为 VScode 安装 [Cortex-Debug](https://marketplace.visualstudio.com/items?itemName=marus25.cortex-debug) 扩展。
2. 在要调试的项目中创建 `.vscode/launch.json` 文件。可以使用[这个](https://github.com/esp-rs/esp32-hal/blob/master/.vscode/launch.json)模板文件。
3. 修改 **executable**，**svdFile**，**serverpath** 路径和 **toolchainPrefix** 字段。

```jsonc
{
  // Use IntelliSense to learn about possible attributes.
  // Hover to view descriptions of existing attributes.
  // For more information, visit: https://go.microsoft.com/fwlink/?linkid=830387
  "version": "0.2.0",
  "configurations": [
    {
      // more info at: https://github.com/Marus/cortex-debug/blob/master/package.json
      "name": "Attach",
      "type": "cortex-debug",
      "request": "attach", // attach instead of launch, because otherwise flash write is attempted, but fails
      "cwd": "${workspaceRoot}",
      "executable": "target/xtensa-esp32-none-elf/debug/.....",
      "servertype": "openocd",
      "interface": "jtag",
      "svdFile": "../../esp-pacs/esp32/svd/esp32.svd",
      "toolchainPrefix": "xtensa-esp32-elf",
      "openOCDPreConfigLaunchCommands": ["set ESP_RTOS none"],
      "serverpath": "C:/Espressif/tools/openocd-esp32/v0.11.0-esp32-20220411/openocd-esp32/bin/openocd.exe",
      "configFiles": ["board/esp32-wrover-kit-3.3v.cfg"],
      "overrideAttachCommands": [
        "set remote hardware-watchpoint-limit 2",
        "mon halt",
        "flushregs"
      ],
      "overrideRestartCommands": ["mon reset halt", "flushregs", "c"]
    }
  ]
}
```

## ESP32-C3

**revision < 3** **没有**内置 JTAG 接口。

**revision 3** **具有**内置 JTAG 接口，您无需连接外部设备即可进行调试。要获取芯片版本，请运行 `cargo espflash board-info` 命令。

### 硬件设置

如果您的 ESP32-C3 的版本小于 3，请按照这些说明进行操作，如果您的版本为 3，则可以跳转到[**VSCode 设置**](#vscode-设置-1)。

ESP32-C3 **版本 1** 和**版本 2** 没有内置 JTAG 接口，因此您必须将外部 JTAG 适配器连接到 ESP32-C3 板，例如，可以使用 [ESP-Prog](https://docs.espressif.com/projects/espressif-esp-iot-solution/en/latest/hw-reference/ESP-Prog_guide.html)。

| ESP32-C3 Pin | JTAG Signal |
| :----------: | :---------: |
|  MTDO/GPIO7  |     TDO     |
|  MTDI/GPIO5  |     TDI     |
|  MTCK/GPIO6  |     TCK     |
|  MTMS/GPIO4  |     TMS     |
|     3V3      |    VJTAG    |
|     GND      |     GND     |

**注意**：在 Windows 上 `USB 串行转换器 A 0403 6010 00` 驱动程序应该是 WinUSB。

### VSCode 设置

1. 为 VScode 安装 [Cortex-Debug](https://marketplace.visualstudio.com/items?itemName=marus25.cortex-debug) 扩展。
2. 在要调试的项目中创建 `.vscode/launch.json` 文件。可以使用[这个](https://github.com/esp-rs/esp32-hal/blob/master/.vscode/launch.json)模板文件。
3. 修改 **executable**，**svdFile**，**serverpath** 路径和 **toolchainPrefix** 字段。

```jsonc
{
  // Use IntelliSense to learn about possible attributes.
  // Hover to view descriptions of existing attributes.
  // For more information, visit: https://go.microsoft.com/fwlink/?linkid=830387
  "version": "0.2.0",
  "configurations": [
    {
      // more info at: https://github.com/Marus/cortex-debug/blob/master/package.json
      "name": "Attach",
      "type": "cortex-debug",
      "request": "attach", // attach instead of launch, because otherwise flash write is attempted, but fails
      "cwd": "${workspaceRoot}",
      "executable": "target/riscv32imc-unknown-none-elf/debug/examples/usb_serial_jtag", //
      "servertype": "openocd",
      "interface": "jtag",
      "svdFile": "../../esp-pacs/esp32c3/svd/esp32c3.svd",
      "toolchainPrefix": "riscv32-esp-elf",
      "openOCDPreConfigLaunchCommands": ["set ESP_RTOS none"],
      "serverpath": "C:/Espressif/tools/openocd-esp32/v0.11.0-esp32-20220411/openocd-esp32/bin/openocd.exe",
      "configFiles": ["board/esp32c3-builtin.cfg"],
      "overrideAttachCommands": [
        "set remote hardware-watchpoint-limit 2",
        "mon halt",
        "flushregs"
      ],
      "overrideRestartCommands": ["mon reset halt", "flushregs", "c"]
    }
  ]
}
```
