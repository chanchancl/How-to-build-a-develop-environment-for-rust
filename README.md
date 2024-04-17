# How-to-build-a-develop-environment-for-rust

## 前情提要

由于众所周知的原因，如同golang一样，rust的官网虽然可以访问，但是速度却跟没有一样，为了快速正确的安装rust

这里将以前遇到的坑总结一下，并给出一份最简单的解决方案

## 准备步骤

1. 运行命令来下载原版rust安装脚本

```
curl --proto '=https' --tlsv1.2 https://sh.rustup.rs -sSf > rust.sh && chmod +x rust.sh
```

2. 运行命令来替换安装脚本中的下载源

```
sed -i 's/${RUSTUP_UPDATE_ROOT:-https:\/\/static.rust-lang.org\/rustup}/https:\/\/mirrors.ustc.edu.cn\/rust-static\/rustup/g' rust.sh
```

将 RUSTUP_UPDATE_ROOT 替换为 **https://mirrors.ustc.edu.cn/rust-static/rustup** 来加速

3. (optional)

如果 step1.都无法下载，可以使用如下命令来下载，并运行步骤2.

`curl --proto '=https' --tlsv1.2 https://mirrors.ustc.edu.cn/misc/rustup-install.sh -sSf > rust.sh && chmod +x rust.sh`


或者你可以选择使用这个库中提供的 [rust.sh](https://github.com/chanchancl/How-to-build-a-develop-environment-for-rust/blob/main/rust.sh)(替换过的)，但我还是推荐你自己修改一遍，以防rustup有后续更新

4. 设置环境变量

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

1. 运行命令 `cargo version`, 若出现 cargo 1.69.0 (6e9a83356 2023-04-12) (版本可能变化) 则说明安装成功

# 软件源

rust的第三方包也需要根据dependency从网络上下载，可以通过修改 ~/.cargo/env 来修改源

## 常规修改

修改 ~/.cargo/env

```
[source.crates-io]
replace-with = 'ustc'

[source.ustc]
registry = "git://mirrors.ustc.edu.cn/crates.io-index"
```

## 稀疏索引（推荐）

cargo 1.68 版本开始支持稀疏索引：不再需要完整克隆 crates.io-index 仓库，可以加快获取包的速度。如果您的 cargo 版本大于等于 1.68，可以在 $HOME/.cargo/config 中添加如下内容：

```
[source.crates-io]
replace-with = 'ustc'

[source.ustc]
registry = "sparse+https://mirrors.ustc.edu.cn/crates.io-index/"
```

如果build时出错，可以尝试 `export SSL_CERT_FILE=/etc/ssl/certs/ca-certificates.crt`

# 参考文献

更多内容请参考

* https://mirrors.ustc.edu.cn/help/rust-static.html
* https://mirrors.ustc.edu.cn/help/crates.io-index.html
