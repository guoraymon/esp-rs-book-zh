# 编写 std 应用程序

如果您想学习如何开发 `std` 应用程序，可以与 [Ferrous Systems] 一起开发培训：

- [培训书籍]
- [培训代码库]

培训基于 [ESP32-C3-DevKit-RUST-1]。您可以使用任何其他 ESP32、ESP32-C3、ESP32-S2 或 ESP32-S3 开发板，但可能需要更改代码和配置。

培训分为两部分：

* [入门示例]:
   * 基本硬件检查 ([源码](https://github.com/ferrous-systems/espressif-trainings/tree/main/intro/hardware-check))
   * HTTP 客户端 ([源码](https://github.com/ferrous-systems/espressif-trainings/tree/main/intro/http-client))
   * HTTP 服务器 ([源码](https://github.com/ferrous-systems/espressif-trainings/tree/main/intro/http-server))
   * MQTT 客户端 ([源码](https://github.com/ferrous-systems/espressif-trainings/tree/main/intro/mqtt))
* [高级示例]:
   * 底层 GPIO
   * 通用中断
   * I2C 驱动程序 ([源码](https://github.com/ferrous-systems/espressif-trainings/tree/main/advanced/i2c-driver))
   * I2C 传感器读取 ([源码](https://github.com/ferrous-systems/espressif-trainings/tree/main/advanced/i2c-sensor-reading))
   * GPIO/Button 中断 ([源码](https://github.com/ferrous-systems/espressif-trainings/tree/main/advanced/button-interrupt))
   * 驱动 RGB LED

> 请注意，在 `esp-idf-hal` 的示例文件夹下有几个示例涵盖了特定外设的使用。例如 [`esp32-idf-hal/examples`]。

[Ferrous Systems]: https://ferrous-systems.com/
[培训书籍]: https://espressif-trainings.ferrous-systems.com/
[培训代码库]: https://github.com/ferrous-systems/espressif-trainings
[ESP32-C3-DevKit-RUST-1]: https://github.com/esp-rs/esp-rust-board
[入门示例]: https://github.com/ferrous-systems/espressif-trainings/tree/main/intro
[高级示例]: https://github.com/ferrous-systems/espressif-trainings/tree/main/advanced
[`esp32-idf-hal/examples`]: https://github.com/esp-rs/esp-idf-hal/tree/master/examples
