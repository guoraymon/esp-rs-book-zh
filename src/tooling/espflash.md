# espflash

用于 ESP 设备的串行烧写工具。支持烧写 _ESP32_、_ESP32-C2_、_ESP32-C3_、_ESP32-S2_、_ESP32-S3_ 和 _ESP8266_。

[esp-rs/espflash] 存储库包含两个 crate，`cargo-espflash` 和 `espflash`。您可以在下面各自的部分中找到有关它们的更多信息。

[esp-rs/espflash]: https://github.com/esp-rs/espflash

## cargo-espflash

为 `cargo` 提供一个子命令，用于处理交叉编译和烧写的操作。请注意，这需要使用 `cargo` 的实验性 `build-std `特性。有关这方面的更多信息，请参阅 [cargo documentation]。

安装方法:

```bash
cargo install cargo-espflash
```

这个命令必须在一个 Cargo 项目中运行，也就是包含一个 `Cargo.toml` 文件的目录中运行。例如，要在 `release` 模式下构建一个名为 `blinky` 的示例，将生成的二进制文件烧写到设备中，然后启动串行监视器，可以执行以下操作：

```bash
cargo espflash --example=blinky --release --monitor
```

如需更多信息，请参阅 [cargo-espflash README].

[cargo documentation]: https://doc.rust-lang.org/cargo/reference/unstable.html#build-std
[cargo-espflash readme]: https://github.com/esp-rs/espflash/blob/master/cargo-espflash/README.md

## espflash

提供一个独立的命令行应用程序，用于将一个 ELF 文件烧写到设备中。

安装方法:

```bash
cargo install espflash
```

假设您已经通过其他方式构建了一个 ELF 二进制文件，`espflash` 可以用于将其下载到您的设备中。例如，如果您使用 `idf.py` 从 [esp-idf] 构建了 `getting-started/blinky` 示例，则可以运行以下命令：

```bash
espflash build/blinky
```

如需更多信息，请参阅 [espflash README].

[esp-idf]: https://github.com/espressif/esp-idf
[espflash readme]: https://github.com/esp-rs/espflash/blob/master/espflash/README.md
