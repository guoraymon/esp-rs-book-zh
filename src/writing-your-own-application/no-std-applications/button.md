# 检测按钮按下

大多数开发板都有一个按钮，在我们的例子中，我们将使用 [`BOOT` on `GPIO9`] 的按钮。让我们看看如何检查按钮的状态。

```rust,ignore
#![no_std]
#![no_main]

use esp32c3_hal::{
    clock::ClockControl, pac::Peripherals, prelude::*, timer::TimerGroup, Rtc, IO,
};
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

    // Set GPIO7 as an output, GPIO9 as input
    let io = IO::new(peripherals.GPIO, peripherals.IO_MUX);
    let mut led = io.pins.gpio7.into_push_pull_output();
    let button = io.pins.gpio9.into_pull_up_input();

    loop {
        if button.is_high().unwrap() {
            led.set_high().unwrap();
        } else {
            led.set_low().unwrap();
        }
    }
}
```

现在，如果未按下按钮，则 LED 会亮起。如果按下按钮，则 LED 熄灭。

类似于将 `GPIO` 转换为 `output`，我们可以将其转换为 `input`。然后，我们可以使用 `is_high` 等函数获取 `input` 引脚的当前状态。

[`BOOT` on `GPIO9`]: https://github.com/esp-rs/esp-rust-board#ios
