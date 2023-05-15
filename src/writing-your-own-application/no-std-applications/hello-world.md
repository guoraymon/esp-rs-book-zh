# Hello World

在上一章中，您已经将第一段代码烧录到 SoC 上并运行成功 - 虽然这已经很令人兴奋，但我们可以做得更好。

传统上，在微控制器上运行的第一件事情是闪烁灯（blinky）。

但是，在这里我们将从 _Hello World_ 开始。

## 添加依赖项

你可以通过以下任意一种方法添加依赖项：

- 编辑 `Cargo.toml` 文件。在 `[dependencies]` 部分添加以下行：

```toml
esp-println = { version = "0.3.1", features = ["esp32c3"] }
```

- 使用 [`cargo add`] 命令：

```sh
cargo add esp-println --features "esp32c3"
```

[`esp-println`] 是一个附加的 crate，调用 ROM 函数打印文本，该文本由 [`espflash`]（或任何其他串行监视器）显示。

我们需要传递 `esp32c3` 特性，因为该 crate 针对多个 SoC，需要知道它要在哪个 SoC 上运行。

> 请注意，可能在您阅读本文时会有新版本，请检查 [crates.io]。

## 打印一些内容

在 `main.rs` 的 `loop {}` 之前，加入以下这行代码：

```rust,ignore
esp_println::println!("Hello World");
```

## 查看结果

再次运行

```shell
cargo run
```

你应该能看到打印出了 _Hello World_ 这段文字！

[`espflash`]: https://github.com/esp-rs/espflash
[`esp-println`]: https://github.com/esp-rs/esp-println
[crates.io]: https://crates.io/crates/esp-println
[`cargo add`]: https://doc.rust-lang.org/cargo/commands/cargo-add.html
