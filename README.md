# How-to-build-a-develop-environment-for-rust

## 前情提要

由于众所周知的原因，如同golang一样，rust的官网虽然可以访问，但是速度却跟没有一样，为了快速正确的安装rust

这里将以前遇到的坑总结一下，并给出一份最简单的解决方案

## 准备步骤

1. 运行命令 `curl -sSf https://sh.rustup.rs > rust.sh`
2. 运行命令

```
sed -i 's/${RUSTUP_UPDATE_ROOT:-https:\/\/static.rust-lang.org\/rustup}/https:\/\/mirrors.ustc.edu.cn\/rust-static\/rustup/g' rust.sh
```

将 RUSTUP_UPDATE_ROOT 替换为 **https://mirrors.ustc.edu.cn/rust-static/rustup** 来加速

(optional)

或者你可以选择使用这个库中提供的 [rust.sh](https://github.com/chanchancl/How-to-build-a-develop-environment-for-rust/blob/main/rust.sh)，但我还是推荐你自己修改一遍，以防rustup有后续更新

3. 设置环境变量

```
export RUSTUP_DIST_SERVER=https://mirrors.ustc.edu.cn/rust-static
export RUSTUP_UPDATE_ROOT=https://mirrors.ustc.edu.cn/rust-static/rustup
```

如果需要后续加速，则可以将这两个环境变量写入你的个人 profile

## 安装

1. 运行命令 `sudo bash rust.sh`, 并输入回车选择 default 安装
2. 等待安装完成

## 检测

1. 运行命令 `cargo version`, 若出现 cargo 1.56.0 (4ed5d137b 2021-10-04) (版本可能变化) 则说明安装成功
