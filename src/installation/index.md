# 设置开发环境

目前，乐鑫 SoC 基于两种不同的架构：`RISC-V` 和 `Xtensa`。两种架构都支持 `std` 和 `no_std` 方法。

要设置开发环境，请执行以下操作：

1. [安装 Rust][install-rust]
2. 根据您的目标安装
    - [仅限 `RISC-V` 目标][risc-v-targets]
    - [`RISC-V` 和 `Xtensa`][rics-v-xtensa-targets]

正如下面安装过程中提到的，对于 `std` 开发不要忘记安装 [`std` 开发要求][rust-esp-book-std-requirements].

请注意，您可以在[容器][use-containers]中托管开发环境。


[install-rust]: #install-rust
[risc-v-targets]: #risc-v-targets-only
[rics-v-xtensa-targets]: #risc-v-and-xtensa-targets
[use-containers]: #using-containers


## 安装 Rust

确保安装了 [Rust][rust-lang-org]。如果没有，请参阅 [rustup][rustup.rs-website] 网站上的说明。

另请参阅。 [替代安装方法][rust-alt-installation].

> **注**: 如果您在主机上运行 Windows，请确保您已经安装了下面列出的 ABI 之一。有关更多详细信息，请参阅 The rustup 一书中的 [Windows][rustup-book-windows] 一章。
>
> - **MSVC**: 推荐的 ABI，包含在 `rustup` 默认要求列表中。将其用于与 Visual Studio 生成的软件的互操作性。
> - **GNU**: GCC 工具链使用的 ABI。自行安装，以便与使用 MinGW/MSYS2 工具链构建的软件进行互操作。


[rustup.rs-website]: https://rustup.rs/
[rust-alt-installation]: https://rust-lang.github.io/rustup/installation/other.html
[rustup-book-windows]: https://rust-lang.github.io/rustup/installation/windows.html
[rust-lang-org]: https://www.rust-lang.org/


## 仅限 RISC-V 目标

要基于 `RISC-V` 架构为乐鑫芯片构建 Rust 应用程序，请执行以下操作：

1. 使用 `rust-src` [组件][rustup-book-components] 安装 [`nightly 工具链`][rustup-book-channel-nightly]:

    ```bash
    rustup toolchain install nightly --component rust-src
    ```

[rustup-book-channel-nightly]: https://rust-lang.github.io/rustup/concepts/channels.html#working-with-nightly-rust
[rustup-book-components]: https://rust-lang.github.io/rustup/concepts/components.html


2. 设定目标：
    - 对于 `no_std`（裸机）应用程序，运行：

      ```bash
      rustup target add riscv32imc-unknown-none-elf # 适用于 ESP32-C2 和 ESP32-C3
      rustup target add riscv32imac-unknown-none-elf # 适用于 ESP32-C6 和 ESP32-H2
      ```

      这个目标目前是 [Tier 2][rust-lang-book--platform-support-tier2]，请注意 Rust 中 `riscv32` 目标的不同风格，涵盖不同的 [`RISC-V` 扩展][wiki-riscv-standard-extensions]。

    - 对于 `std` 应用程序，使用 `riscv32imc-esp-espidf`。

      由于此目标当前是 [Tier 3][rust-lang-book--platform-support-tier3]，因此它没有通过 `rustup` 分发的预构建对象，并且 **不需要** 作为 `no_std` 目标安装.


[rust-lang-book--platform-support-tier2]: https://doc.rust-lang.org/nightly/rustc/platform-support.html#tier-2
[wiki-riscv-standard-extensions]: https://en.wikichip.org/wiki/risc-v/standard_extensions
[rust-lang-book--platform-support-tier3]: https://doc.rust-lang.org/nightly/rustc/platform-support.html#tier-3


3. 要构建 `std` 项目，您还需要安装：
    - [`LLVM`][llvm-website] 编译器基础设施 
    - 其他 [`std` 开发要求][rust-esp-book-std-requirements]
    - 在项目文件 `.cargo/config.toml`, 添加不稳定的 Cargo [feature][cargo-book-unstable-features] `-Z build-std`. 本书后面讨论的[模板项目][rust-esp-book-write-app-generate-project]已经包含了它。


[llvm-website]: https://llvm.org/
[rust-esp-book-std-requirements]: #std-development-requirements
[cargo-book-unstable-features]: https://doc.rust-lang.org/cargo/reference/unstable.html
[rust-esp-book-write-app-generate-project]: ../writing-your-own-application/generate-project-from-template.md


现在您应该能够在乐鑫的 `RISC-V` 芯片上构建和运行项目。


## RISC-V 和 Xtensa 目标

[`espup`][espup-github] 是一个工具，可简化为 `Xtensa` 和 `RISC-V` 架构开发 Rust 应用程序所需组件的安装和维护。

[espup-github]: https://github.com/esp-rs/espup

要安装 `espup`, 请运行:
```sh
cargo install espup
```

您也可以直接下载预编译的[发布二进制文件]或使用 [cargo-binstall].

[release binaries]: https://github.com/esp-rs/espup/releases
[cargo-binstall]: https://github.com/cargo-bins/cargo-binstall

安装 `espup` 后，执行以下操作：

1. 安装所有必要的工具，通过运行以下命令为所有支持的乐鑫目标开发 Rust 应用程序：
    ```sh
    espup install
    ```

    `espup` 将创建一个导出文件，其中包含构建项目所需的一些环境变量：

    - Unix 系统 - `$HOME/export-esp.sh`
    - Windows - `%USERPROFILE%\export-esp.ps1`

2. 在 Unix 系统上，确保在构建任何应用程序之前在每个终端中 source 此文件：`. $HOME/export-esp.sh`
    > 在 Windows 系统上，无需 source 该文件。它只是为了显示修改后的环境变量而创建的。

ß
运行 `espup install` 后：

- `no_std` （裸机）应用程序应该开箱即用
- `std` 应用程序需要额外的 [`std` 开发要求][rust-esp-book-std-requirements]

### 什么是 espup 安装

为了启用对乐鑫目标的支持, `espup` 安装了以下工具：

- 支持乐鑫目标的乐鑫 Rust 分叉
- 支持 `RISC-V` 目标的`nightly` 工具链
- 支持 `Xtensa` 目标的 `LLVM` [分叉][llvm-github-fork]
- 链接最终二进制文件的 [GCC 工具链][gcc-toolchain-github-fork]

分叉编译器可以与标准 Rust 编译器共存，允许两者都安装在您的系统上。使用任何可用的[覆盖方法][rustup-overrides]时，将调用这个分支编译器。

> **注**: 我们正在努力将我们的分支合并到上游项目中。
> 1. `LLVM` 分支的变化。已经在进行中，请查看此[问题跟踪][llvm-github-fork-upstream issue]中的状态。
> 2. Rust 编译器分叉。如果 `LLVM` 的更改被接受，我们将继续进行 Rust 编译器的修改。


[llvm-github-fork]: https://github.com/espressif/llvm-project
[gcc-toolchain-github-fork]: https://github.com/espressif/crosstool-NG/
[rustup-overrides]: https://rust-lang.github.io/rustup/overrides.html
[llvm-github-fork-upstream issue]: https://github.com/espressif/llvm-project/issues/4


### Xtensa 目标的其他安装方法

- 使用 [esp-rs/rust-build] 安装脚本。这是过去推荐的方式，但现在安装脚本已冻结功能，所有新功能只会包含在 espup 中。有关说明，请参阅存储库自述文件。
- 使用来自源代码的 `Xtensa` 支持构建 Rust 编译器。此过程的计算成本很高，可能需要一个或多个小时才能完成，具体取决于您的系统。除非有采用这种方法的主要原因，否则不推荐这样做。这是从源代码构建它的存储库： [esp-rs/rust 存储库]。

[esp-rs/rust-build]: https://github.com/esp-rs/rust-build#download-installer-in-bash
[esp-rs/rust repository]: https://github.com/esp-rs/rust


## `std` 开发要求

无论目标架构如何，请确保您安装了以下构建 [`std`][rust-esp-book-overview-std] 应用程序所需的工具：

- [`python`][python-website-download]: ESP-IDF 要求
- [`git`][git-website-download]: ESP-IDF 要求
- [`ldproxy`][embuild-github-ldproxy]: 一种将链接器参数转发给实际链接器的工具，该链接器也作为参数提供给 `ldproxy`。安装方法：
    ```sh
    cargo install ldproxy
    ```

std 运行时使用 [ESP-IDF][esp-idf-github] (乐鑫 IoT 开发框架) 作为托管环境，但用户不需要安装它。ESP-IDF 由[esp-idf-sys][esp-idf-sys-github] 自动下载和安装, 这是一个所有 std 项目在构建 std 应用程序时都需要使用的 crate。


[rust-esp-book-overview-std]: ../overview/using-the-standard-library.md
[python-website-download]: https://www.python.org/downloads/
[git-website-download]: https://git-scm.com/downloads
[embuild-github-ldproxy]: https://github.com/esp-rs/embuild/tree/master/ldproxy
[esp-idf-sys-github]: https://github.com/esp-rs/esp-idf-sys
[esp-idf-github]: https://github.com/espressif/esp-idf


## 使用容器

您可以在容器中托管开发环境，而不是直接安装在您的本地系统上。乐鑫提供 [idf-rust] 镜像，支持 `RISC-V` 和 `Xtensa` 目标架构，支持 `std` 和 `no_std` 开发。

您可以找到许多适用于 `linux/arm64` 和 `linux/amd64` 平台的标签。

对于每个 Rust 版本，我们使用以下命名约定生成标签：

- `<chip>_<rust-toolchain-version>`
  - 例如，`esp32_1.64.0.0` 包含用于使用 `1.64.0.0` Xtensa Rust 工具链为 ESP32 开发 `std` 和 `no_std` 应用程序的生态系统。

有特殊情况

- `<chip>` 可以是 `all`，表示与所有乐鑫目标兼容
- `<rust-toolchain-version>` 可以是 `latest`，表示 `Xtensa Rust` 工具链的最新版本

根据您的操作系统，您可以选择任何容器运行时，例如 [Docker]、[Podman] 或 [Lima]。


[Docker]: https://www.docker.com/
[Podman]: https://podman.io/
[Lima]: https://github.com/lima-vm/lima
[idf-rust]: https://hub.docker.com/r/espressif/idf-rust/tags
