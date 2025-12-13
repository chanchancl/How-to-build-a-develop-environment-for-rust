# How-to-build-a-develop-environment-for-rust

## 前情提要

由于众所周知的原因，如同 golang 一样，rust 的官网虽然可以访问，但是速度却跟没有一样，为了快速正确的安装 rust

这里将以前遇到的坑总结一下，并给出一份最简单的解决方案

[Windows 版本](./README-window.md)

## 基础知识

rust 语言的基础工具有 2 个，`rustup` 与 `cargo`

rustup：Rust 官方的工具链管理器，用于安装、更新、切换不同版本的 Rust 编译器（rustc）、标准库和工具以及跨平台目标。

cargo：Rust 官方的构建系统和包管理器，负责编译代码、管理依赖、运行测试、生成文档和发布包。

## 准备步骤

1. 下载 rust 安装脚本

运行命令:

```
curl --proto 'https' --tlsv1.2 https://mirrors.ustc.edu.cn/misc/rustup-install.sh -sSf > rust.sh && chmod +x rust.sh
```

2. 设置环境变量

> **RUSTUP_UPDATE_ROOT**用于更新 rustup 自身 `rustup self update`

> **RUSTUP_DIST_SERVER**用于更新 toolchain `rustup update`

```
export RUSTUP_DIST_SERVER=https://mirrors.ustc.edu.cn/rust-static
export RUSTUP_UPDATE_ROOT=https://mirrors.ustc.edu.cn/rust-static/rustup
```

如果需要后续加速，则可以将这两个环境变量写入你的个人 profile

## 安装

1. 运行命令 `sudo bash rust.sh`, 并输入回车选择 default 安装
2. 等待安装完成

## 检测

### 检测 rustup

运行命令 `rustup -V`, 输出应该类似下面：

```
rustup 1.28.2 (e4f3ad6f8 2025-04-28)
info: This is the version for the rustup toolchain manager, not the rustc compiler.
info: The currently active `rustc` version is `rustc 1.91.1 (ed61e7d7e 2025-11-07)`
```

### 检测 cargo

运行命令 `cargo -V`, 输出应类似：

```
cargo 1.91.1 (ea2d97820 2025-10-10)
```

# 软件源

成功安装 rustup 与 cargo 之后，我们要解决软件源的问题（类似 python 的 pip）

rust 的第三方包也需要根据 dependency 从网络上下载，可以通过修改 ~/.cargo/config.toml 来修改源

## 稀疏索引（推荐）

cargo 1.68 版本开始支持稀疏索引：不再需要完整克隆 crates.io-index 仓库，可以加快获取包的速度。如果您的 cargo 版本大于等于 1.68，可以在 $CARGO_HOME/config.toml 中添加如下内容：

> $CARGO_HOME 默认为 ~/.cargo

```
[source.crates-io]
replace-with = 'ustc'

[source.ustc]
registry = "sparse+https://mirrors.ustc.edu.cn/crates.io-index/"

[registries.ustc]
index = "sparse+https://mirrors.ustc.edu.cn/crates.io-index/"
```

如果 build 时出错，可以尝试 `export SSL_CERT_FILE=/etc/ssl/certs/ca-certificates.crt`

## 尝试构建

通过 `cargo new hello_world && cd hello_world` 创建项目并进入

运行 `cargo run` 检测输出是否正确

```
>> cargo run
   Compiling hello_world v0.1.0 (xxx\hello_world)
    Finished `dev` profile [unoptimized + debuginfo] target(s) in 0.23s
     Running `target\debug\hello_world.exe`
Hello, world!
```

## 参考文献

更多内容请参考

- https://mirrors.ustc.edu.cn/help/rust-static.html
- https://mirrors.ustc.edu.cn/help/crates.io-index.html
- https://course.rs/first-try/installation.html
- https://course.rs/first-try/slowly-downloading.html
