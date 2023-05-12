# 简介

本书的目标是提供有关在 [乐鑫] 设备上使用 [Rust 编程语言]的综合指南。

对这些设备的 Rust 支持仍在进行中，并且进展迅速。因此，本文档的某些部分可能已过时或在阅读之间发生巨大变化。

有关 ESP 上 Rust 的相关工具和库，请参阅 GitHub 上的 [esp-rs 组织]。该组织由乐鑫员工和社区成员管理。

[rust 编程语言]: https://www.rust-lang.org/
[乐鑫]: https://espressif.com/
[esp-rs 组织]: https://github.com/esp-rs/

> **关于设备支持的说明**
>
> 本书内容仅适用于ESP32系列设备；这包括：
>
> - ESP32 系列
> - ESP32-C 系列
> - ESP32-S 系列
> - ESP32-H 系列
>
> ESP8266 系列不在本书讨论范围之内。 ESP8266 系列的 Rust 支持有限，乐鑫未提供官方支持。

## 本书适合谁

本书适用于具有一定 Rust 经验的人，并且还假定读者具有嵌入式开发和电子学的基本知识。对于那些没有经验的人，我们建议先阅读[假设和先决条件]以及[其他资源]部分以加快速度。

[假设和先决条件]: #assumptions-and-prerequisites
[其他资源]: #other-resources

### 假设和先决条件

- 您可以熟练使用 Rust 编程语言，并在桌面环境中编写和运行应用程序。
- 你应该熟悉 2021 版的习语，因为本书针对 [Rust 2021]。
- 您能够熟练地使用其他语言（例如 C 或 C++）开发嵌入式系统，并且熟悉以下概念：
  - 交叉编译
  - 常见的数字接口，如 UART、SPI、I²C 等。
  - 内存映射外设
  - 中断

[rust 2021]: https://doc.rust-lang.org/edition-guide/rust-2021/index.html

### 其他资源

如果您对上述任何内容都不熟悉或经验不足，或者您只是想了解有关本书中提到的特定主题的更多信息，您可能会发现这些资源很有帮助。

| 资源                        | 描述                                                                          |
| ------------------------------- | ------------------------------------------------------------------------------------ |
| [The Rust Programming Language] | 如果您不熟悉 Rust，我们建议您先阅读本书。          |
| [The Embedded Rust Book]        | 在这里，您可以找到 Rust 的嵌入式工作组提供的其他一些资源。 |
| [The Embedonomicon]             | 在 Rust 中进行嵌入式编程时的基本细节。                    |
| [Embedded Rust on Espressif]    | 与 [Ferrous Systems] 合作创建的培训材料。                     |

[the rust programming language]: https://doc.rust-lang.org/book/
[the embedded rust book]: https://docs.rust-embedded.org/book/index.html
[the embedonomicon]: https://docs.rust-embedded.org/embedonomicon/
[embedded rust on espressif]: https://espressif-trainings.ferrous-systems.com/
[ferrous systems]: https://ferrous-systems.com/

### 翻译

这本书目前只有英文版本。一旦本书的内容稳定下来，我们计划将其翻译成其他语言。随着翻译可用，本节将更新以包含它们。

## 如何使用本书

本书通常假定您是从前到后阅读它；如果没有前几章的上下文，后面章节中涵盖的内容可能毫无意义。

## 为本书做贡献

本书的工作在[这个存储库]中进行协调。

[这个存储库]: https://github.com/esp-rs/book

如果您在遵循本书中的说明时遇到问题，或者发现本书的某些部分不够清晰或难以理解，那么这是一个错误，应该在本书的[问题跟踪器]中报告。

[问题跟踪器]: https://github.com/esp-rs/book/issues/

非常欢迎修复拼写错误和添加新内容的拉取请求！

## 重复使用该材料

本书根据以下许可分发：

- 本书中包含的代码示例和独立的 Cargo 项目是根据 [MIT 许可证] 和 [Apache 许可证 v2.0] 的条款获得许可的。
- 本书中包含的文字、图片和图表均根据 Creative Commons [CC-BY-SA v4.0] 许可条款获得许可。

[mit 许可证]: https://opensource.org/licenses/MIT
[apache 许可证 v2.0]: http://www.apache.org/licenses/LICENSE-2.0
[cc-by-sa v4.0]: https://creativecommons.org/licenses/by-sa/4.0/legalcode

TL;DR：如果您想在您的作品中使用我们的文字或图片，您需要：

- 给予适当的信任（即在幻灯片中提及这本书，并提供相关页面的链接）
- 提供指向 [CC-BY-SA v4.0] 许可证的链接
- 说明您是否以任何方式更改了材料，并根据相同的许可对我们提供的材料进行任何更改

请让我们知道如果你寻找这本书有用！
