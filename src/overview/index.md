# 生态概述

在乐鑫芯片上使用 Rust 有以下几种方法：

- 使用 std 库，又名标准库。
- 使用核心库 (no_std)，也就是裸机开发。

这两种方法各有利弊，因此您应该根据项目的需要做出决定。本章包含对这两种方法的概述：

- [使用标准库 (std)][rust-esp-book-std]
- [在裸机上开发 (no_std)][rust-esp-book-no-std]

[rust-esp-book-std]: ./using-the-standard-library.md
[rust-esp-book-no-std]: ./bare-metal.md

另请参阅 [The Embedded Rust Book][embedded-rust-book-intro-std] 中不同运行时的比较。

[embedded-rust-book-intro-std]: https://docs.rust-embedded.org/book/intro/no-std.html#a-no_std-rust-environment

GitHub 上的 [esp-rs 组织] 拥有许多与在乐鑫芯片上运行 Rust 相关的存储库。大多数所需的 crate 都在这里托管了它们的源代码。

> 关于存储库命名约定的注释
>
> 在 [esp-rs 组织]中，我们使用以下写法：
>
> - 以 `esp-idf-` 开头的存储库专注于 `std` 方法。例如：`esp-idf-hal`
> - 以 `esp-` 开头的存储库专注于 `no_std` 方法。例如：`esp-hal`
>
> 很容易记住如下：
>
>- `no_std` 在裸机之上工作，所以 `esp-` 是乐鑫芯片
>- `std` 除了裸机，还需要[附加层](https://github.com/espressif/esp-idf), 则是 `esp-idf-`

[esp-rs 组织]: https://github.com/esp-rs/

## 支持的乐鑫产品

> **笔记**:
>
> - ✅ - 该功能已实施或支持
> - ⏳ - 该功能正在开发中
> - ❌ - 不支持该功能

| 芯片     | `std` | `no_std` |
| -------- | :---: | :------: |
| ESP32    |   ✅   |    ✅     |
| ESP32-C2 |   ⏳   |    ✅     |
| ESP32-C3 |   ✅   |    ✅     |
| ESP32-C6 |   ⏳   |    ✅     |
| ESP32-S2 |   ✅   |    ✅     |
| ESP32-S3 |   ✅   |    ✅     |
| ESP32-H2 |   ⏳   |    ⏳     |
| ESP8266  |   ❌   |    ✅     |

在某些情况下支持的产品将在整本书中称为支持的 _乐鑫产品_。

截至目前，esp-idf 框架支持的乐鑫产品是支持 Rust `std` 开发的产品。不同版本的esp-idf以及对乐鑫芯片的支持情况见[此表][esp-idf-release-compatibility]。

[esp-idf-release-compatibility]: https://github.com/espressif/esp-idf#esp-idf-release-and-soc-compatibility
