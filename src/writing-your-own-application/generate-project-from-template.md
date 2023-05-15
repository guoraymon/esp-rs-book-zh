# 从模板生成项目

我们目前维护两个模板仓库：

- [esp-template] - `no_std` 模板。
- [esp-idf-template] - `std` 模板。

这两个模板都基于 [cargo-generate]，这是一个允许你基于现有模板创建新项目的工具。在我们的情况下，可以使用 [esp-idf-template] 或 [esp-template] 生成具有所有必需配置和依赖项的应用程序。

可以通过运行以下命令安装 `cargo generate`：

```shell
cargo install cargo-generate
```

当调用 `cargo generate` 子命令时，你将被提示回答关于你的应用程序目标的一系列问题。完成此过程后，你将拥有一个可构建的项目，具有所有正确的配置。

使用任何一个模板，生成的应用程序可以像正常情况下一样使用适当的工具链和目标构建，只需运行 `cargo build` 即可。

使用 `cargo run` 将编译项目，将其烧录，并打开我们的芯片的串行监视器。

## esp-idf-template

当使用 Rust 标准库 (`std`) 时，可以使用 [esp-idf-template] 模板，它看起来像这样：

```shell
$ cargo generate --git https://github.com/esp-rs/esp-idf-template cargo
🤷   Project Name : esp-rust-app
🔧   Destination: /home/alice/esp-rust-app ...
🔧   Generating template ...
✔ 🤷   MCU · esp32
✔ 🤷   Configure project to use Dev Containers (VS Code, GitHub Codespaces and Gitpod)? (beware: Dev Containers not available for esp-idf v4.3.2) · false
✔ 🤷   STD support · true
✔ 🤷   ESP-IDF native build version (v4.3.2 = previous stable, v4.4 = stable, mainline = UNSTABLE) · v4.4
[ 1/10]   Done: .cargo/config.toml
[ 2/10]   Done: .cargo
[ 3/10]   Done: .gitignore
[ 4/10]   Done: .vscode
[ 5/10]   Done: Cargo.toml
[ 6/10]   Done: build.rs
[ 7/10]   Done: rust-toolchain.toml
[ 8/10]   Done: sdkconfig.defaults
[ 9/10]   Done: src/main.rs
[10/10]   Done: src
🔧   Moving generated files into: `/home/alice/esp-rust-app`...
💡   Initializing a fresh Git repository
✨   Done! New project created /home/alice/esp-rust-app
```

有关模板项目的更多详细信息，请参见 [了解 esp-idf-template]。

## esp-template

对于裸机应用程序（`no_std`），您可以使用 [esp-template] 模板：

```shell
cargo generate --git https://github.com/esp-rs/esp-template
🤷   Project Name : esp-rust-app
🔧   Destination: /home/alice/esp-rust-app ...
🔧   Generating template ...
✔ 🤷   Which MCU to target? · esp32c3
✔ 🤷   Configure project to use Dev Containers (VS Code, GitHub Codespaces and Gitpod)? · false
✔ 🤷   Enable allocations via the esp-alloc crate? · false
[ 1/11]   Done: .cargo/config.toml
[ 2/11]   Done: .cargo
[ 3/11]   Done: .gitignore
[ 4/11]   Done: .vscode/settings.json
[ 5/11]   Done: .vscode
[ 6/11]   Done: Cargo.toml
[ 7/11]   Done: LICENSE-APACHE
[ 8/11]   Done: LICENSE-MIT
[ 9/11]   Done: rust-toolchain.toml
[10/11]   Done: src/main.rs
[11/11]   Done: src
🔧   Moving generated files into: `/home/alice/esp-rust-app`...
✨   Done! New project created /home/alice/esp-rust-app
```

有关模板项目的更多详细信息，请参见 [了解 esp-template]。

### 在模板中使用开发容器

在两个模板仓库中，都有对于开发容器支持的提示。使用开发容器可以添加对以下工具的支持：

- [VS Code 开发容器]
- [GitHub Codespaces]
- [Gitpod]

开发容器使用 `idf-rust` 容器镜像，该容器镜像在 Rust 安装章节的 [使用容器部分] 中有介绍，并且为开发乐鑫芯片的 Rust 应用程序提供了一个无需安装的开发环境。开发容器还与 [Wokwi 模拟器] 集成，可以在容器内模拟项目，并且使用 [Web Flash] 进行烧录。

更多有关开发容器的详细信息，请参阅[模板自述文件中的开发容器部分]。

[cargo-generate]: https://github.com/cargo-generate/cargo-generate
[esp-idf-template]: https://github.com/esp-rs/esp-idf-template
[esp-template]: https://github.com/esp-rs/esp-template
[VS Code 开发容器]: https://code.visualstudio.com/docs/remote/containers#_quick-start-open-an-existing-folder-in-a-container
[GitHub Codespaces]: https://docs.github.com/en/codespaces/developing-in-codespaces/creating-a-codespace
[Gitpod]: https://www.gitpod.io
[使用容器部分]: ../installation/index.md#使用容器
[Wokwi 模拟器]: https://wokwi.com/
[web flash]: https://github.com/bjoernQ/esp-web-flash-server
[模板自述文件中的开发容器部分]: https://github.com/esp-rs/esp-template/tree/main/docs#dev-containers
[了解 esp-template]: ./no-std-applications/understanding-esp-template.md
[了解 esp-idf-template]: ./std-applications/understanding-esp-idf-template.md
