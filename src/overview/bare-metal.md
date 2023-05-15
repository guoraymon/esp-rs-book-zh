# 裸机开发 (`no_std`)

嵌入式 Rust 开发人员可能更熟悉使用 `no_std`；它不使用 `std`（[Rust 标准库][rust-lib-std] ）而是使用一个子集，即核心库。 [The Embedded Rust Book][embedded-rust-book] 对此有很大的[介绍][embedded-rust-book-no-std] 。

重要的是要注意，由于 `no_std` 使用的 `Rust 核心库`是 `Rust 标准库`的一个子集，因此 `no_std` crate 可以在 `std` 环境中编译，但反之则不然。因此，在创建 crate 时，如果它需要`标准库`才能运行，则值得牢记。

[embedded-rust-book]: https://docs.rust-embedded.org/
[embedded-rust-book-no-std]: https://docs.rust-embedded.org/book/intro/no-std.html
[rust-lib-core]: https://doc.rust-lang.org/core/index.html
[rust-lib-std]: https://doc.rust-lang.org/std/index.html


## 当前支持

下表涵盖了目前不同乐鑫产品对 `no_std` 的支持情况。

|          | [HAL][esp-rs/esp-hal] | [Wi-Fi/BLE/ESP-NOW][esp-rs/esp-wifi] | [Backtrace][esp-rs/esp-backtrace] | [Storage][esp-rs/esp-storage] |
| -------- | :-------------------: | :----------------------------------: | :-------------------------------: | :---------------------------: |
| ESP32    |           ✅           |                  ✅                   |                 ✅                 |               ✅               |
| ESP32-C2 |           ✅           |                  ✅                   |                 ✅                 |               ✅               |
| ESP32-C3 |           ✅           |                  ✅                   |                 ✅                 |               ✅               |
| ESP32-C6 |           ✅           |                  ✅                   |                 ✅                 |               ✅               |
| ESP32-S2 |           ✅           |                  ✅                   |                 ✅                 |               ✅               |
| ESP32-S3 |           ✅           |                  ✅                   |                 ✅                 |               ✅               |
| ESP32-H2 |           ⏳           |                  ⏳                   |                 ✅                 |               ⏳               |

> **注意**:
>
> - ✅ 在 Wi-Fi/BLE/ESP-NOW 中表示目标至少支持所列技术之一。有关详细信息，请参阅 esp-wifi 存储库的[当前支持][esp-wifi-current-support]表。
> - [ESP8266 HAL][esp-rs/esp8266-hal] 处于维护模式，不会对该芯片进行进一步开发。

[esp-wifi-current-support]: https://github.com/esp-rs/esp-wifi#current-support
### 相关的 `esp-rs` crates

| Repository             | Description                                                |
| ---------------------- | ---------------------------------------------------------- |
| [esp-rs/esp-hal]       | 硬件抽象层                                 |
| [esp-rs/esp-pacs]      | 外设访问                                   |
| [esp-rs/esp-wifi]      | Wi-Fi, BLE 和 ESP-NOW 支持                             |
| [esp-rs/esp-alloc]     | 简单的堆分配器                                      |
| [esp-rs/esp-println]   | `print!`,  `println!`                                      |
| [esp-rs/esp-backtrace] | 异常和 panic 处理                               |
| [esp-rs/esp-storage]   | 访问未加密闪存的嵌入式存储特性 |

### 当您可能想使用裸机时（`no_std`）

- 小内存占用：如果您的嵌入式系统资源有限并且需要小内存占用，您可能希望使用裸机，因为 std 功能会增加大量的最终二进制文件大小和编译时间。
- 直接硬件控制：如果您的嵌入式系统需要对硬件进行更直接的控制，例如低级设备驱动程序或访问专门的硬件功能，您可能希望使用裸机，因为 `std` 添加了抽象，这会使直接与硬件交互变得更加困难。
- 实时约束或时间关键型应用程序：如果您的嵌入式系统需要实时性能或低延迟响应时间，因为 `std` 会引入不可预测的延迟和开销，从而影响实时性能。
- 自定义要求：裸机允许对应用程序的行为进行更多自定义和细粒度控制，这在专用或非标准环境中很有用。


[esp-rs/esp-hal]: https://github.com/esp-rs/esp-hal "Hardware abstraction layer"
[esp-rs/esp8266-hal]: https://github.com/esp-rs/esp8266-hal "ESP8266 Hardware abstraction layer"
[esp-rs/esp-pacs]: https://github.com/esp-rs/esp-pacs "Peripheral access crates"
[esp-rs/esp-wifi]: https://github.com/esp-rs/esp-wifi "Wi-Fi, BLE and ESP-NOW support"
[esp-rs/esp-alloc]: https://github.com/esp-rs/esp-alloc "Simple heap allocator"
[esp-rs/esp-println]: https://github.com/esp-rs/esp-println "print!, println!"
[esp-rs/esp-backtrace]: https://github.com/esp-rs/esp-backtrace "Exception and panic handlers"
[esp-rs/esp-storage]: https://github.com/esp-rs/esp-storage "Embedded-storage traits to access unencrypted flash memory"


