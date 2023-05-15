# Wokwi

[Wokwi] 是一个在线模拟器，支持在 ESP 芯片中模拟 Rust 项目（包括 `std` 和 `no_std`），请访问 [wokwi.com/rust] 查看示例列表和开始新项目的方法。

Wokwi 提供了 WiFi 模拟、虚拟逻辑分析仪和 [GDB 调试]等众多功能，有关更多详细信息，请参阅 [Wokwi 文档]。对于 ESP 芯片，有一张[表格]列出了当前支持的模拟功能。

## 使用 wokwi-server

[wokwi-server] 是一个命令行工具，用于启动您项目的 Wokwi 模拟。也就是说，它允许您在您的计算机或容器中构建项目并模拟生成的二进制文件。

[wokwi-server] 还允许您在其他 Wokwi 项目上模拟您的生成二进制文件，其中包括比芯片本身更多的硬件部件。有关详细说明，请参阅 [wokwi-server 自述文件的相应部分]。

[Wokwi]: https://wokwi.com/
[wokwi.com/rust]: https://wokwi.com/rust
[GDB 调试]: https://docs.wokwi.com/gdb-debugging
[Wokwi 文档]: https://docs.wokwi.com/
[表格]: https://docs.wokwi.com/guides/esp32#simulation-features
[wokwi-server]: https://github.com/MabezDev/wokwi-server
[wokwi-server 自述文件的相应部分]: https://github.com/MabezDev/wokwi-server#simulating-your-binary-on-a-custom-wokwi-project
