# Panic!

当 Rust 中出现严重错误时，可能会发生[panic]。

让我们看看它对我们来说是什么样子。

在 `main.rs` 中将此行放在某处，例如在我们的 `println` 之前：

```rust,ignore
panic!("This is a panic");
```

再次运行代码。

您应该会看到类似于这样的内容：

```text


!! A panic occured in 'src\main.rs', at line 25, column 5

PanicInfo {
    payload: Any { .. },
    message: Some(
        This is a panic,
    ),
    location: Location {
        file: "src\\main.rs",
        line: 25,
        col: 5,
    },
    can_unwind: true,
}

Backtrace:

0x420019aa
0x420019aa - main
    at C:\tmp\getting-started\src\main.rs:25
0x4200014c
0x4200014c - _start_rust
    at ...\.cargo\registry\src\github.com-1ecc6299db9ec823\riscv-rt-0.9.0\src\lib.rs:389
```

我们看到了 panic 发生的位置，甚至看到了回溯信息！

虽然在这个例子中事情是显而易见的，但这在更复杂的代码中会派上用场。

现在尝试使用 release 模式编译并运行代码：
```shell
cargo run --release
```

现在情况不太好看：
```text

!! A panic occured in 'src\main.rs', at line 25, column 5

PanicInfo {
    payload: Any { .. },
    message: Some(
        This is a panic,
    ),
    location: Location {
        file: "src\\main.rs",
        line: 25,
        col: 5,
    },
    can_unwind: true,
}

Backtrace:

0x42000140
0x42000140 - _start_rust
    at ??:??
```

我们仍然可以看到 panic 发生的位置，但是回溯信息不太有用了。

这是因为编译器省略了调试信息并优化了代码。

但是你可能已经注意到了烧录二进制文件大小的差异。

它从 199056 字节减少到了 86896 字节！

请注意，对于我们所得到的东西来说，这仍然很大。有许多选项可以使二进制文件更小，这超出了本书的范围。
<!--(TODO：我们应该添加一个有关二进制大小的部分吗？) -->

在继续之前，请删除引起明确 panic 的行。

[panic]: https://doc.rust-lang.org/book/ch09-01-unrecoverable-errors-with-panic.html
