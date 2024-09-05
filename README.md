# How-to-build-a-develop-environment-for-rust

## 前情提要

由于众所周知的原因，如同golang一样，rust的官网虽然可以访问，但是速度却跟没有一样，为了快速正确的安装rust

这里将以前遇到的坑总结一下，并给出一份最简单的解决方案

## 准备步骤

1. 下载rust安装脚本

原版：

```
curl --proto '=https' --tlsv1.2 https://sh.rustup.rs -sSf > rust.sh && chmod +x rust.sh
```

USTC每天更新的(源已替换), 推荐:

```
curl --proto 'https' --tlsv1.2 https://mirrors.ustc.edu.cn/misc/rustup-install.sh -sSf > rust.sh && chmod +x rust.sh
```

2. 设置环境变量

**RUSTUP_DIST_SERVER**用于更新toolchain
**RUSTUP_UPDATE_ROOT**用于更新rustup自身

```
export RUSTUP_DIST_SERVER=https://mirrors.ustc.edu.cn/rust-static
export RUSTUP_UPDATE_ROOT=https://mirrors.ustc.edu.cn/rust-static/rustup
```

如果需要后续加速，则可以将这两个环境变量写入你的个人 profile

## 安装

1. 运行命令 `sudo bash rust.sh`, 并输入回车选择 default 安装
2. 等待安装完成

## 检测

1. 运行命令 `cargo version`, 若出现 cargo 1.80.1 (376290515 2024-07-16) (版本可能变化) 则说明安装成功

# 软件源

rust的第三方包也需要根据dependency从网络上下载，可以通过修改 ~/.cargo/config.toml 来修改源


## 稀疏索引（推荐）

cargo 1.68 版本开始支持稀疏索引：不再需要完整克隆 crates.io-index 仓库，可以加快获取包的速度。如果您的 cargo 版本大于等于 1.68，可以在 $HOME/.cargo/config。toml 中添加如下内容：

```
[source.crates-io]
replace-with = 'ustc'

[source.ustc]
registry = "sparse+https://mirrors.ustc.edu.cn/crates.io-index/"
```

## 常规修改


如果你的cargo版本小于 1.6.8，可以如下修改 ~/.cargo/config.toml

目前来说，该方法比稀疏索引慢很多，所以强烈推荐升级cargo版本

```
[source.crates-io]
replace-with = 'ustc'

[source.ustc]
registry = "git://mirrors.ustc.edu.cn/crates.io-index"
```

如果build时出错，可以尝试 `export SSL_CERT_FILE=/etc/ssl/certs/ca-certificates.crt`

# 参考文献

更多内容请参考

* https://mirrors.ustc.edu.cn/help/rust-static.html
* https://mirrors.ustc.edu.cn/help/crates.io-index.html
