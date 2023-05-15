# 使用中断检测按钮按下

[中断]提供了处理器处理异步事件和致命错误的机制。

让我们添加 [`critical-section`] crate [(请参阅如何添加依赖项的说明)]，并将 `main.rs` 更改为以下内容：

```rust,ignore
#![no_std]
#![no_main]

use core::cell::RefCell;
use critical_section::Mutex;
use esp32c3_hal::{
    clock::ClockControl,
    gpio::Gpio9,
    gpio_types::{Event, Input, Pin, PullUp},
    interrupt,
    pac::{self, Peripherals},
    prelude::*,
    timer::TimerGroup,
    Rtc, IO,
};
use esp_backtrace as _;

static BUTTON: Mutex<RefCell<Option<Gpio9<Input<PullUp>>>>> = Mutex::new(RefCell::new(None));

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

    // Set GPIO9 as input
    let io = IO::new(peripherals.GPIO, peripherals.IO_MUX);
    let mut button = io.pins.gpio9.into_pull_up_input();
    button.listen(Event::FallingEdge); // raise interrupt on falling edge

    critical_section::with(|cs| BUTTON.borrow_ref_mut(cs).replace(button));

    interrupt::enable(pac::Interrupt::GPIO, interrupt::Priority::Priority3).unwrap();

    loop {}
}

#[interrupt]
fn GPIO() {
    critical_section::with(|cs| {
        esp_println::println!("GPIO interrupt");
        BUTTON
            .borrow_ref_mut(cs)
            .as_mut()
            .unwrap()
            .clear_interrupt();
    });
}
```

这里有很多新东西。

首先是 `static BUTTON`。我们需要它，因为在中断处理程序中我们必须清除 button 上挂起的中断，并且我们需要以某种方式将 button 从 main 传递到中断处理程序。

由于中断处理程序不能有参数，我们需要一个静态变量来将 button 放入中断处理程序。

我们需要 `Mutex` 来安全地访问 button 。

> 请注意，这不是您可能从 `libstd` 中了解到的 Mutex，而是来自 [`critical-section`] 的 Mutex（这就是我们需要将其添加为依赖项的原因）。

然后我们需要在输出引脚上调用 `listen` 以配置外设以引发中断。我们可以为不同的事件引发中断——这里我们想在 FallingEdge 引发中断。

在下一行中，我们将 button 移入到 `static BUTTON` 中，以便中断处理程序获取它。

我们需要做的最后一件事实际上是启用中断。

这里的第一个参数是我们想要的中断类型。有几种[可能的中断]。

第二个参数是中断的优先级。

中断处理程序是通过 `#[interrupt]` 宏定义的。这里函数的名称必须与中断匹配。

[中断]: https://docs.rust-embedded.org/book/start/interrupts.html
[`critical-section`]: https://crates.io/crates/critical-section
[(请参阅如何添加依赖项的说明)]: ./hello-world.md#添加依赖项
[可能的中断]: https://docs.rs/esp32c3/0.5.1/esp32c3/enum.Interrupt.html
