# 下载 rustup-init

访问 https://rust-lang.org/zh-CN/learn/get-started/ 并下载适合你系统架构的（建议加速） rustup-init.exe

# 配置环境变量

> RUSTUP_DIST_SERVER https://mirrors.ustc.edu.cn/rust-static

> RUSTUP_UPDATE_ROOT https://mirrors.ustc.edu.cn/rust-static/rustup

# 安装

运行 rustup-init.exe

因为 rust 在 windows 上编译依赖 [Microsoft C++ Build Tools](https://visualstudio.microsoft.com/zh-hans/visual-cpp-build-tools/)，所以你可能需要同时安装

# 检测

打开 cmd 或者 powershell，运行命令 `rustup -V` 和 `cargo -V`

期待结果如下：

```
rustup 1.28.2 (e4f3ad6f8 2025-04-28)
info: This is the version for the rustup toolchain manager, not the rustc compiler.
info: The currently active `rustc` version is `rustc 1.91.1 (ed61e7d7e 2025-11-07)`
```

```
cargo 1.91.1 (ea2d97820 2025-10-10)
```

如果显示找不到命令，那么你可能需要额外添加一些路径到 PATH，比如：

> "C:\Users\xxx\.cargo\bin"

# 配置软件源

在目录 C:\Users\xxx\.cargo\config.toml (xxx 为你的用户名) 中写入:

```
[source.crates-io]
replace-with = 'ustc'

[source.ustc]
registry = "sparse+https://mirrors.ustc.edu.cn/crates.io-index/"

[registries.ustc]
index = "sparse+https://mirrors.ustc.edu.cn/crates.io-index/"
```
