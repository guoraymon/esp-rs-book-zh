
# OpenOCD

与 [probe-rs](./probe-rs.md) 类似，OpenOCD 不支持 Xtensa 架构。然而，乐鑫在 [espressif/openocd-esp32](https://github.com/espressif/openocd-esp32) 下维护了一个 OpenOCD 的分支，它支持 Espressif 的芯片。

有关如何为您的平台安装 `openocd-esp32` 的说明，请参阅 [Espressif 文档](https://docs.espressif.com/projects/esp-idf/en/latest/esp32c3/api-guides/jtag-debugging/index.html#setup-of-openocd)。

## Espressif 芯片设置

<!-- how to choose interface & chip -->

安装后，就像使用正确的脚本运行 `openocd` 一样简单。对于带有内置 USB JTAG 的芯片，通常有一个开箱即用的配置，例如在 ESP32-C3 上：

```ignore
openocd -f board/esp32c3-builtin.cfg
```

对于其他配置，可能需要分别指定芯片和接口，例如，带有 J-Link 的 ESP32：

```ignore
openocd -f interface/jlink.cfg -f target/esp32.cfg
```
