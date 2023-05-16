# 了解 esp-idf-template

现在我们已经知道了如何[生成 std 项目]，让我们检查生成的项目包含的内容并尝试了解它的每个部分。

## 检查生成的项目

使用以下选项从 [esp-idf-template] 创建项目：

- MCU: `esp32c3`
- ESP-IDF version: `v4.4`
- STD support: `true`
- Devcontainer support: `false`

将生成如下文件结构：

```text
├── build.rs
├── .cargo
│   └── config.toml
├── Cargo.toml
├── .gitignore
├── rust-toolchain.toml
├── sdkconfig.defaults
└── src
    └── main.rs
```

在继续之前，让我们看看这些文件的作用。

- [.gitignore]
    - 告诉 `git` 忽略哪些文件和文件夹
- [Cargo.toml]
    - Cargo 元数据和项目依赖声明清单
- `LICENSE-APACHE`，`LICENSE-MIT`
    - 这些是 Rust 生态系统中使用最广泛的许可证
    - 如果您想应用不同的许可证，可以删除这些文件并在 `Cargo.toml` 中更改许可证。
- [rust-toolchain.toml]
    - 定义要使用的 Rust 工具链
    - 根据您的目标，这将使用 `nightly` 或 `esp`
- [.cargo/config.toml]
    - Cargo 配置
    - 包含我们的目标
    - 包含 `runner = "espflash --monitor"` - 这意味着您可以使用 `cargo run` 来烧录和监视您的代码
    - 包含要使用的链接器，在我们的例子中是 [`ldproxy`]
    - 包含启用的 unstable `build-std` cargo feature。
    - 包含 `ESP-IDF-VERSION` 环境变量，该变量告诉 [`esp-idf-sys`] 将使用哪个 ESP-IDF 版本。
- src/main.rs
    - 新创建项目的主源文件
    - 我们将在下一节中分析它的内容
- [build.rs]
    - 传播 `ldproxy` 的链接器参数。
- [sdkconfig.defaults]
    - 包含来自 ESP-IDF 默认值的覆盖值。

## `main.rs`

```rust,ignore
use esp_idf_sys as _; // If using the `binstart` feature of `esp-idf-sys`, always keep this module imported

fn main() {
    esp_idf_sys::link_patches();
    println!("Hello, world!");
}

```

第一行是一个导入，它定义了 esp-idf 入口点，当根 crate 是一个定义了 main 函数的二进制 crate 时。

然后，我们有一个通常的 main 函数，上面有两行：

- 调用 `esp_idf_sys::link_patches` 函数，确保在 Rust 中实现的 ESP-IDF 的一些补丁链接到最终的可执行文件。
- 我们在控制台中打印著名的“Hello World!”。

## 运行代码

构建和运行代码非常简单：

```shell
cargo run
```

这将根据配置构建代码并执行 [`espflash`] 将代码刷入开发板。

由于我们的 [`runner` 配置] 还将 `--monitor` 参数传递给 [`espflash`]，因此我们可以看到代码输出的内容。

> 确保你已经安装了 [`espflash`]，否则这一步会失败。安装 [`espflash`]：
> `cargo install espflash`

你应该会看到类似以下的输出：

```text
Connecting...

Chip type:         ESP32-C3 (revision 3)
Crystal frequency: 40MHz
Flash size:        4MB
Features:          WiFi
MAC address:       60:55:f9:c0:39:7c
App/part. size:    409728/4128768 bytes, 9.92%
[00:00:00] ########################################      12/12      segment 0x0
[00:00:00] ########################################       1/1       segment 0x8000
[00:00:04] ########################################     210/210     segment 0x10000
Flashing has completed!
Commands:
    CTRL+R    Reset chip
    CTRL+C    Exit

ESP-ROM:esp32c3-api1-20210207
Build:Feb  7 2021
rst:0x15 (USB_UART_CHIP_RESET),boot:0xc (SPI_FAST_FLASH_BOOT)
Saved PC:0x4004c97e
0x4004c97e - chip726_phyrom_version_num
    at ??:??
SPIWP:0xee
mode:DIO, clock div:1
load:0x3fcd6100,len:0x172c
load:0x403ce000,len:0x928
0x403ce000 - _iram_text_end
    at ??:??
load:0x403d0000,len:0x2ce0
0x403d0000 - _iram_text_end
    at ??:??
entry 0x403ce000
0x403ce000 - _iram_text_end
    at ??:??
I (24) boot: ESP-IDF v4.4-dev-2825-gb63ec47238 2nd stage bootloader
I (24) boot: compile time 12:10:40
I (24) boot: chip revision: 3
I (28) boot_comm: chip revision: 3, min. bootloader chip revision: 0
I (35) boot.esp32c3: SPI Speed      : 80MHz
I (39) boot.esp32c3: SPI Mode       : DIO
I (44) boot.esp32c3: SPI Flash Size : 4MB
I (49) boot: Enabling RNG early entropy source...
I (54) boot: Partition Table:
I (58) boot: ## Label            Usage          Type ST Offset   Length
I (65) boot:  0 nvs              WiFi data        01 02 00009000 00006000
I (73) boot:  1 phy_init         RF data          01 01 0000f000 00001000
I (80) boot:  2 factory          factory app      00 00 00010000 003f0000
I (88) boot: End of partition table
I (92) boot_comm: chip revision: 3, min. application chip revision: 0
I (99) esp_image: segment 0: paddr=00010020 vaddr=3c050020 size=17640h ( 95808) map
I (122) esp_image: segment 1: paddr=00027668 vaddr=3fc89c00 size=0146ch (  5228) load
I (123) esp_image: segment 2: paddr=00028adc vaddr=40380000 size=0753ch ( 30012) load
I (133) esp_image: segment 3: paddr=00030020 vaddr=42000020 size=419d8h (268760) map
I (176) esp_image: segment 4: paddr=00071a00 vaddr=4038753c size=02644h (  9796) load
I (178) esp_image: segment 5: paddr=0007404c vaddr=50000010 size=00010h (    16) load
I (185) boot: Loaded app from partition at offset 0x10000
I (188) boot: Disabling RNG early entropy source...
I (205) cpu_start: Pro cpu up.
I (213) cpu_start: Pro cpu start user code
I (213) cpu_start: cpu freq: 160000000
I (213) cpu_start: Application information:
I (216) cpu_start: Project name:     libespidf
I (221) cpu_start: App version:      1
I (226) cpu_start: Compile time:     Nov  3 2022 13:16:23
I (232) cpu_start: ELF file SHA256:  0000000000000000...
I (238) cpu_start: ESP-IDF:          755ce10-dirty
I (243) heap_init: Initializing. RAM available for dynamic allocation:
I (250) heap_init: At 3FC8BF90 len 00050780 (321 KiB): DRAM
I (257) heap_init: At 3FCDC710 len 00002950 (10 KiB): STACK/DRAM
I (263) heap_init: At 50000020 len 00001FE0 (7 KiB): RTCRAM
I (270) spi_flash: detected chip: generic
I (274) spi_flash: flash io: dio
I (279) sleep: Configure to isolate all GPIO pins in sleep state
I (285) sleep: Enable automatic switching of GPIO sleep configuration
I (292) cpu_start: Starting scheduler.
Hello, world!
```

如您所见，有来自第一阶段和第二阶段引导加载程序的消息，然后是我们的打印的“Hello, world!”。

您可以使用 `CTRL+R` 重新启动，或使用 `CTRL+C` 退出。

[.gitignore]: https://git-scm.com/docs/gitignore
[Cargo.toml]: https://doc.rust-lang.org/cargo/reference/manifest.html
[rust-toolchain.toml]: https://rust-lang.github.io/rustup/overrides.html#the-toolchain-file
[.cargo/config.toml]: https://doc.rust-lang.org/cargo/reference/config.html
[生成 std 项目]: ../generate-project-from-template.md#esp-idf-template
[esp-idf-template]: https://github.com/esp-rs/esp-idf-template
[`esp-idf-sys`]: https://github.com/esp-rs/esp-idf-sys
[`ldproxy`]: https://github.com/esp-rs/embuild/tree/master/ldproxy
[build.rs]: https://doc.rust-lang.org/cargo/reference/build-scripts.html
[sdkconfig.defaults]: https://docs.espressif.com/projects/esp-idf/en/latest/esp32/api-guides/build-system.html#custom-sdkconfig-defaults
[`espflash`]: https://github.com/esp-rs/espflash/tree/main/espflash
[`runner` 配置]: https://doc.rust-lang.org/cargo/reference/config.html#targettriplerunner
