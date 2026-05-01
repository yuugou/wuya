# Rust 开发环境搭建

## Windows 下 Rust 开发环境搭建

简单一句话，按照 [Rust 官方安装指南](https://www.rust-lang.org/zh-CN/learn/get-started) 来。

懒得看官网的，请按照以下步骤：

- 到上述官网指南，下载 ``rustup-init.exe（64位）`` 到任意目录。
- 运行 ``rustup-init.exe``。
- 会打开一个终端窗口，按照提示操作，不要关闭终端窗口，直到提示完成安装。（其间，会安装 Microsoft Visual Studio Community 及相关组件。

## 测试是否安装好 Rust 开发环境

打开终端，输入以下命令：

```PS
rustc --version
```

如果安装成功，会显示 Rust 版本号。

## Rust 开发 IDE 的配置 - 使用 Visual Studio Code

- 安装 Visual Studio Code [使用 zip 包安装](https://code.visualstudio.com/docs/setup/windows#_use-the-zip-file)
- 安装 rust-analyzer 插件，[点击](https://marketplace.visualstudio.com/items?itemName=rust-lang.rust-analyzer)
- 安装 C/C++ 插件， [点击](https://marketplace.visualstudio.com/items?itemName=ms-vscode.cpptools)

### 编写程序

- 在项目根目录下，新建文件 ``main.rs``，输入以下代码：

```rust
fn main() {
    println!("Hello, world!");
}
```

### 在 VS Code 中运行程序

- 打开 VS Code，打开项目文件夹，打开 main.rs 文件
- 点击左上角 ``Run Code`` 按钮（快捷键 ``Ctrl+Alt+N``）程序会自动编译并运行，输出结果如下：

```PS
Hello, world!
```

### 在 VS Code 中调试程序 ``TODO``
