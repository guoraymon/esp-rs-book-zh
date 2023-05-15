# QEMU

乐鑫维护了一个 QEMU 分支，位于 [espressif/QEMU]，其中包含必要的补丁，使其可以在乐鑫芯片上运行。请查看 [QEMU wiki] 以获取有关如何构建 QEMU 并使用它来模拟项目的说明。

一旦您构建了 QEMU，您应该会有 `qemu-system-xtensa` 可执行文件。

[espressif/QEMU]: https://github.com/espressif/qemu
[QEMU wiki]: https://github.com/espressif/qemu/wiki

## 使用 QEMU 运行我们的项目

> *注意*：目前只支持 ESP32，因此请确保您正在以 `xtensa-esp32-espidf` 为目标进行编译。

要在 QEMU 中运行我们的项目，我们需要一个固件/映像，其中包含合并的引导加载程序和分区表。我们可以使用 [`cargo-espflash`] 来生成它：

[`cargo-espflash`]: https://github.com/esp-rs/espflash/tree/main/cargo-espflash

```bash
cargo espflash save-image --merge ESP32 <OUTFILE> --release
```

> 如果您喜欢使用 [`espflash`]，则可以通过先构建项目，然后生成映像来实现相同的结果：
>```bash
> cargo build --release
> espflash save-image --merge ESP32 target/xtensa-esp32-espidf/release/<NAME> <OUTFILE>
>```

[`espflash`]: https://github.com/esp-rs/espflash/tree/main/espflash

现在，在 QEMU 中运行映像：
```sh
/path/to/qemu-system-xtensa -nographic -machine esp32 -drive file=<OUTFILE>,if=mtd,format=raw
```

