# 了解 esp-template

现在我们已经知道了如何[生成 no_std 项目]，让我们检查生成的项目包含的内容并尝试了解它的每个部分。

## 检查生成的项目

使用以下选项从 [esp-template] 创建项目：

- MCU: `esp32c3`
- Devcontainer 支持: `false`
- `esp-alloc` crate 支持: `false`

将生成如下文件结构：

```text
├── .cargo
│   └── config.toml
├── src
│   └── main.rs
├── .vscode
│   └── settings.json
├── .gitignore
├── Cargo.toml
├── LICENSE-APACHE
├── LICENSE-MIT
└── rust-toolchain.toml
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
    - 这定义了几个选项以正确构建项目
    - 还包含 `runner = "espflash --monitor"` - 这意味着您可以使用 `cargo run` 来烧录和监视您的代码
- .vscode/settings.json
    - Visual Studio Code 的设置 - 如果您不使用 VSCode，可以删除整个文件夹
- src/main.rs
    - 新创建项目的主源文件
    - 我们将在下一节中分析它的内容
    
## `main.rs`

```rust,ignore
#![no_std]
#![no_main]

use esp32c3_hal::{clock::ClockControl, pac::Peripherals, prelude::*, timer::TimerGroup, Rtc};
use esp_backtrace as _;

#[riscv_rt::entry]
fn main() -> ! {
    let peripherals = Peripherals::take().unwrap();
    let system = peripherals.SYSTEM.split();
    let clocks = ClockControl::boot_defaults(system.clock_control).freeze();

    // Disable the RTC and TIMG watchdog timers
    let mut rtc = Rtc::new(peripherals.RTC_CNTL);
    let timer_group0 = TimerGroup::new(peripherals.TIMG0, &clocks);
    let mut wdt0 = timer_group0.wdt;
    let timer_group1 = TimerGroup::new(peripherals.TIMG1, &clocks);
    let mut wdt1 = timer_group1.wdt;

    rtc.swd.disable();
    rtc.rwdt.disable();
    wdt0.disable();
    wdt1.disable();

    loop {}
}
```

这是相当多的代码。让我们看看它有什么用。

- `#![no_std]`
    - 这告诉Rust编译器，这段代码不使用 `libstd`
- `#![no_main]`
    - `no_main` 属性表示该程序不使用标准的 main 接口，该接口专为接收参数的命令行应用程序设计。我们将使用`riscv-rt` 创建的 entry 属性来定义自定义入口点，而不是标准的 main。在此程序中，我们将入口点命名为 `main`，但是也可以使用任何其他名称。入口点函数必须是 [diverging function]。即具有签名 `fn foo() -> !`；此类型表示该函数永远不会返回，这意味着程序永远不会终止。
- `use esp32c3_hal:{...}`
    - 我们需要引入一些要使用的类型
    - 这些来自 `esp-hal`
- `use esp_backtrace as _;`
    - 由于我们处于裸机环境中，如果在代码中发生 panic，我们需要运行一个 panic-handler
    - 有几种不同的 crate 可用（例如 `panic-halt`），但 `esp-backtrace` 提供了一个实现，该实现打印回溯的地址 - 与 `espflash`/`espmonitor` 一起，这些地址可以解码为源代码位置
- `let peripherals = Peripherals::take().unwrap();`
    - HAL 驱动程序通常接管通过 PAC 访问的外设的所有权
    - 在这里，我们从 PAC 中获取所有外设，以便稍后将它们传递给 HAL 驱动程序
- `let system = peripherals.SYSTEM.split();`
    - 有时一个外设（这里是 System 外设）是粗粒度的，并且不完全适合 HAL 驱动程序 - 因此，在这里，我们将 System 外设分成更小的部分，这些部分将传递给驱动程序
- `let clocks = ClockControl::boot_defaults(system.clock_control).freeze();`
    - 在这里，我们配置系统时钟 - 在本例中，我们使用默认值
    - 我们冻结时钟，这意味着我们以后无法更改它们
    - 一些驱动程序需要时钟的引用，以了解如何计算速率和持续时间
- 接下来的代码块实例化了一些外设（即 RTC 和两个定时器组）以禁用启动后已启用的看门狗
    - 如果没有该代码，SoC 将在一段时间后重新启动
    - 还有另一种方法可以防止重新启动：[feeding](https://docs.rs/esp32c3-hal/0.2.0/esp32c3_hal/prelude/trait._embedded_hal_watchdog_Watchdog.html#tymethod.feed) 看门狗
- `loop {}`
    - 因为我们的函数永远不会返回值，所以我们在一个循环中“什么也不做”。

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
MAC address:       60:55:f9:c0:0e:ec
App/part. size:    198752/4128768 bytes, 4.81%
[00:00:00] ########################################      12/12      segment 0x0
[00:00:00] ########################################       1/1       segment 0x8000
[00:00:01] ########################################      57/57      segment 0x10000
Flashing has completed!
Commands:
    CTRL+R    Reset chip
    CTRL+C    Exit

ESP-ROM:esp32c3-api1-20210207
Build:Feb  7 2021
rst:0x15 (USB_UART_CHIP_RESET),boot:0xc (SPI_FAST_FLASH_BOOT)
Saved PC:0x4004c72e
0x4004c72e - _stack_start
    at ??:??
SPIWP:0xee
mode:DIO, clock div:1
load:0x3fcd6100,len:0x172c
load:0x403ce000,len:0x928
0x403ce000 - _erwtext
    at ??:??
load:0x403d0000,len:0x2ce0
0x403d0000 - _erwtext
    at ??:??
entry 0x403ce000
0x403ce000 - _erwtext
    at ??:??
I (24) boot: ESP-IDF v4.4-dev-2825-gb63ec47238 2nd stage bootloader
I (24) boot: compile time 12:10:40
I (25) boot: chip revision: 3
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
I (99) esp_image: segment 0: paddr=00010020 vaddr=3c030020 size=04a6ch ( 19052) map
I (110) esp_image: segment 1: paddr=00014a94 vaddr=40380000 size=00910h (  2320) load
I (116) esp_image: segment 2: paddr=000153ac vaddr=00000000 size=0ac6ch ( 44140)
I (131) esp_image: segment 3: paddr=00020020 vaddr=42000020 size=2081ch (133148) map
I (152) boot: Loaded app from partition at offset 0x10000

```

这里你看到的是来自第一阶段和第二阶段引导程序的消息，然后...什么都没有了。

这正是代码所做的。

您可以使用 `CTRL+R` 重新启动，或使用 `CTRL+C` 退出。

在下一章中，我们将添加一些更有趣的输出。

[生成 no_std 项目]: ../generate-project-from-template.md#esp-template
[esp-template]: https://github.com/esp-rs/esp-template
[.gitignore]: https://git-scm.com/docs/gitignore
[Cargo.toml]: https://doc.rust-lang.org/cargo/reference/manifest.html
[rust-toolchain.toml]: https://rust-lang.github.io/rustup/overrides.html#the-toolchain-file
[.cargo/config.toml]: https://doc.rust-lang.org/cargo/reference/config.html
[`espflash`]: https://github.com/esp-rs/espflash/tree/main/espflash
[`runner` 配置]: https://doc.rust-lang.org/cargo/reference/config.html#targettriplerunner
[diverging function]: https://doc.rust-lang.org/beta/rust-by-example/fn/diverging.html
