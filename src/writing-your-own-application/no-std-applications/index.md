# 编写 no_std 应用程序

本章的目标是提供一个入门指南，介绍如何使用 Rust 编程语言和 [esp-hal] 在 Espressif SoCs 和模块上进行开发。

> 请注意，在每个 SoC 的 `esp-hal` 下的 `examples` 文件夹中都有涵盖特定外设使用的示例。例如 [`esp32c3-hal/examples`]。

本章展示的示例通常适用于使用 [ESP32-C3-DevKit-RUST-1] 开发板的 ESP32-C3。

您可以使用任何其他 ESP32、ESP32-C3、ESP32-S2 或 ESP32-S3 开发板，但可能需要进行较小的代码更改和配置更改。

此外，本书的本节仅涵盖本地工作。也就是说，我们将使用本地主机进行开发，而不是使用[开发容器]，因此请确保您已正确[设置开发环境]。

[esp-hal]: https://github.com/esp-rs/esp-hal
[ESP32-C3-DevKit-RUST-1]: https://github.com/esp-rs/esp-rust-board
[`esp32c3-hal/examples`]: https://github.com/esp-rs/esp-hal/tree/main/esp32c3-hal/examples
[开发容器]: ../generate-project-from-template.md#在模板中使用开发容器
[设置开发环境]: ../../installation/index.md
