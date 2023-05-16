# 故障排查

在这里，我们将列出在构建项目时可能出现的常见错误及其原因和解决方案。

## LIBCLANG_PATH 环境变量未设置

```text
thread 'main' panicked at 'Unable to find libclang: "couldn't find any valid shared libraries matching: ['libclang.so', 'libclang-*.so', 'libclang.so.*', 'libclang-*.so.*'], set the `LIBCLANG_PATH` environment variable to a path where one of these files can be found (invalid: [])"', /home/esp/.cargo/registry/src/github.com-1ecc6299db9ec823/bindgen-0.60.1/src/lib.rs:2172:31
```

我们需要 `libclang` 来为 [`bindgen`] 生成 ESP-IDF C 标头的 Rust 绑定。确保环境变量 `LIBCLANG_PATH` 已设置并指向我们的 LLVM 自定义分支：

- Unix:
  ```sh
  export $HOME/.espressif/tools/xtensa-esp32-elf-clang/<clang_version>-<host_triple>/esp-clang/lib
  ```
- Windows:
  ```powershell
  $Env:LIBCLANG_PATH="%USERPROFILE%/.espressif/tools/xtensa-esp32-elf-clang/<clang_version>-<host_triple>/esp-clang/bin/libclang.dll"
  $Env:PATH+=";%USERPROFILE%/.espressif/tools/xtensa-esp32-elf-clang/<clang_version>-<host_triple>/esp-clang/bin/"
  ```

[`bindgen`]: https://github.com/rust-lang/rust-bindgen

## 缺少 `ldproxy`

```sh
error: linker `ldproxy` not found
  |
  = note: No such file or directory (os error 2)
```

如果您尝试构建 `std` 应用程序，则必须安装 [`ldproxy。`]

```sh
cargo install ldproxy
```

[`ldproxy`]: https://github.com/esp-rs/embuild/tree/master/ldproxy


## 使用错误的 Rust 工具链

```text
$ cargo build
error: failed to run `rustc` to learn about target-specific information

Caused by:
  process didn't exit successfully: `rustc - --crate-name ___ --print=file-names --target xtensa-esp32-espidf --crate-type bin --crate-type rlib --crate-type dylib --crate-type cdylib --crate-type staticlib --crate-type proc-macro --print=sysroot --print=cfg` (exit status: 1)
  --- stderr
  error: Error loading target specification: Could not find specification for target "xtensa-esp32-espidf". Run `rustc --print target-list` for a list of built-in targets
```

如果您遇到之前的错误或类似错误，您可能没有使用正确的 Rust 工具链，请记住，对于 Xtensa 目标，您需要使用 Espressif Rust 分支工具链，有几种方法可以做到：
- 命令行上使用的[工具链覆盖]速记：`cargo +esp`。
- 将 `RUSTUP_TOOLCHAIN` 环境变量设置为 `esp`.
- 设置[目录覆盖]：`rustup override set esp`
- 将 [rust-toolchain.toml] 文件添加到您的项目中:
  ```toml
  [toolchain]
  channel = "esp"
  ```
- 将 `esp` 设置为[默认工具链]。

有关工具链覆盖的更多信息，请参阅 [The rustup book 的 Overrides 章节]。

[工具链覆盖]: https://rust-lang.github.io/rustup/overrides.html#toolchain-override-shorthand
[目录覆盖]: https://rust-lang.github.io/rustup/overrides.html#directory-overrides
[rust-toolchain.toml]: https://rust-lang.github.io/rustup/overrides.html#the-toolchain-file
[默认工具链]: https://rust-lang.github.io/rustup/overrides.html#default-toolchain
[The rustup book 的 Overrides 章节]: https://rust-lang.github.io/rustup/overrides.html#overrides

## Windows

### 长路径名

使用 Windows 时，如果使用长路径名，您可能会在构建新项目时遇到问题。请按照以下步骤替换项目的路径：
```powershell
subst r: <pathToYourProject>
cd r:\
```

### 缺少 ABI

```powershell
  Compiling cc v1.0.69
error: linker `link.exe` not found
  |
  = note: The system cannot find the file specified. (os error 2)

note: the msvc targets depend on the msvc linker but `link.exe` was not found

note: please ensure that VS 2013, VS 2015, VS 2017 or VS 2019 was installed with the Visual C++ option

error: could not compile `compiler_builtins` due to previous error
warning: build failed, waiting for other jobs to finish...
error: build failed
```

此错误的原因是我们缺少 MSVC C++，因此我们不满足[编译时要求]，请安装 [Visual Studio 2013（或更高版本）或 Visual C++ Build Tools 2019]。对于 Visual Studio，请务必检查“C++ 工具”和“Windows 10 SDK”选项。如果使用 GNU ABI，请安装 [MinGW/MSYS2 工具链]。

[编译时要求]: https://github.com/rust-lang/cc-rs#compile-time-requirements
[Visual Studio 2013（或更高版本）或 Visual C++ Build Tools 2019]: https://rust-lang.github.io/rustup/installation/windows.html
[MinGW/MSYS2 工具链]: https://www.msys2.org/

## 缺少 `libtinfo.so.5`

```text
thread 'main' panicked at 'Unable to find libclang: "the `libclang` shared library at /home/user/.espressif/tools/xtensa-esp32-elf-clang/esp-15.0.0-20221014-x86_64-unknown-linux-gnu/esp-clang/lib/libclang.so.15.0.0 could not be o
pened: libtinfo.so.5: cannot open shared object file: No such file or directory"', /home/user/.cargo/registry/src/github.com-1ecc6299db9ec823/bindgen-0.60.1/src/lib.rs:2172:31
```
一些 LLVM 15 版本，`esp-15.0.0-20220922` 和 `esp-15.0.0-20221014`，需要 `libtinfo.so.5`。此依赖项已在 `esp-15.0.0-20221201` LLVM 版本中删除。如果您使用的是任何需要它的版本，请确保安装了 `libtinf5`：
- Ubuntu/Debian: `sudo apt-get install libtinfo5`
- Fedora: `sudo dnf install ncurses-compat-libs`
- openSUSE: `sudo dnf install libncurses5`
- Arch Linux: `sudo pacman -S ncurses5-compat-libs`

## FAQ

### 我更新了我的 `sdkconfig.defaults` 文件，但它似乎没有任何效果

您必须清理您的项目并重新构建，以使 `sdkconfig.defaults` 中的更改生效：

```shell,ignore
cargo clean
cargo build
```

### 此页面上提到的 crates 的文档已过时或丢失

由于 [docs.rs] 施加的[资源限制]，在构建文档时互联网访问被阻止，因此我们无法为 `esp-idf-sys` 或依赖它的任何 crate 构建文档。

相反，我们正在构建文档并将其托管在 GitHub Pages 上：

- [`esp-idf-hal` 文档]
- [`esp-idf-svc` 文档]
- [`esp-idf-sys` 文档]

[资源限制]: https://docs.rs/about/builds#hitting-resource-limits
[docs.rs]: https://docs.rs
[`esp-idf-hal` 文档]: https://esp-rs.github.io/esp-idf-hal/esp_idf_hal/
[`esp-idf-svc` 文档]: https://esp-rs.github.io/esp-idf-svc/esp_idf_svc/
[`esp-idf-sys` 文档]: https://esp-rs.github.io/esp-idf-sys/esp_idf_sys/

### \*\*\*ERROR\*\*\* A stack overflow in task main has been detected.

如果第二阶段引导加载程序报告此错误，您可能需要增加主任务的堆栈大小。这可以通过将以下内容添加到 `sdkconfig.defaults` 文件来完成：

```ignore
CONFIG_ESP_MAIN_TASK_STACK_SIZE=7000
```

在此示例中，我们为主任务的堆栈分配 7kB。

### 我怎样才能完全禁用看门狗定时器？

添加到您的 `sdkconfig.defaults` 文件：

```ignore
CONFIG_INT_WDT=n
CONFIG_ESP_TASK_WDT=n
```

回想一下，在修改这些配置文件时，您必须在重建之前清理您的项目
