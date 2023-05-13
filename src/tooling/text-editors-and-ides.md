# 文本编辑器和 IDE

虽然这常常是一个有争议的话题，但是使用正确的开发环境可以显著提高您在某种编程语言下的生产力。以下是我们认为是最佳选项的经过精选的列表。

## Visual Studio Code

其中一个比较常见的开发环境是 Microsoft 的 Visual Studio Code 文本编辑器，以及 Rust Analyzer 扩展。

Visual StudioCode 是一个开源的跨平台图形文本编辑器，具有丰富的扩展生态系统。 [Rust Analyzer extension]提供了 Rust [language server protocol] 的实现，另外还包括自动完成、转到定义等功能。

Visual Studio Code 可以通过最流行的包管理器安装，安装程序可在官方网站上获得。 [Rust Analyzer extension]可以通过内置的扩展管理器安装在 Visual Studio Code 中。

除了 Rust Analyzer (RA)，还有其他可能非常有用的扩展：

- [Even Better TOML] 用于编辑基于 TOML 的配置文件
- [crates] 帮助管理 Rust 依赖项

[visual studio code]: https://code.visualstudio.com/
[rust analyzer]: https://rust-analyzer.github.io/
[Rust Analyzer extension]: https://marketplace.visualstudio.com/items?itemName=rust-lang.rust-analyzer
[language server protocol]: https://microsoft.github.io/language-server-protocol/
[Even Better TOML]: https://marketplace.visualstudio.com/items?itemName=tamasfe.even-better-toml
[crates]: https://marketplace.visualstudio.com/items?itemName=serayuzgur.crates

### 技巧和窍门

如果您正在为不支持 `std` 的目标进行开发，Rust Analyzer 的行为可能会很奇怪，经常会报告各种错误。这可以通过在项目中创建 `.vscode/settings.json` 文件并使用以下内容填充来解决：

```json
{
  "rust-analyzer.checkOnSave.allTargets": false
}
```

如果您正在使用自定义工具链，就像您使用 Xtensa 目标一样，您可以通过 `rust-toolchain.toml` 文件为 `cargo` 提供一些提示以改善用户体验：

```toml
[toolchain]
channel = "esp"
components = ["rustfmt", "rustc-dev"]
targets = ["xtensa-esp32-none-elf"]
```

## CLion

[CLion] 是 [JetBrains] 的 C 和 C++ 跨平台 IDE。

[CLion]: https://www.jetbrains.com/clion/
[JetBrains]: https://www.jetbrains.com/

## IntelliJ


## vim

[vim] 是一个基于 vi 的高度可配置的文本编辑器，它还支持[Rust Analyzer]。

[vim]: https://www.vim.org/
[Rust Analyzer]: https://rust-analyzer.github.io/manual.html#vimneovim
