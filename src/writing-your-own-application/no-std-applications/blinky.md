# Blinky

让我们看看如何创建标志性的 _Blinky_。

将 `main.rs` 中的代码改成这样

```rust,ignore
#![no_std]
#![no_main]

use esp32c3_hal::{
    clock::ClockControl, pac::Peripherals, prelude::*, timer::TimerGroup, Delay, Rtc, IO,
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

    esp_println::println!("Hello World");

    // Set GPIO7 as an output, and set its state high initially.
    let io = IO::new(peripherals.GPIO, peripherals.IO_MUX);
    let mut led = io.pins.gpio7.into_push_pull_output();

    led.set_high().unwrap();

    // Initialize the Delay peripheral, and use it to toggle the LED state in a
    // loop.
    let mut delay = Delay::new(&clocks);

    loop {
        led.toggle().unwrap();
        delay.delay_ms(500u32);
    }
}
```

我们需要两个新类型：[`IO`] 和 [`Delay`]。

在 [ESP32-C3-DevKit-RUST-1] 上，有一个常规的 [LED 连接到 GPIO 7]。如果你使用其他开发板，请参考其数据手册。

> 请注意，Espressif 大多数开发板现在使用一种工作方式不同的可寻址 LED，它超出了本书的范围。在这种情况下，您也可以将常规 LED 连接到某些空闲引脚（不要忘记加电阻）。

在这里，我们可以将引脚设置为高电平、低电平或切换它。

我们还看到 HAL 提供了一种延迟执行的方法。

[ESP32-C3-DevKit-RUST-1]:  https://github.com/esp-rs/esp-rust-board
[LED 连接到 GPIO 7]: https://github.com/esp-rs/esp-rust-board#pin-layout
[`IO`]: https://docs.rs/esp32c3-hal/0.2.0/esp32c3_hal/gpio/struct.IO.html
[`Delay`]: https://docs.rs/esp32c3-hal/0.2.0/esp32c3_hal/struct.Delay.html
