# 创建、编译和运行项目

## 用 Cargo 创建、编译和运行项目

- 创建一个新的 Rust 项目：`cargo new`

```PS
$ cargo new hello_rust
    Creating binary (application) `hello_rust` package
note: see more `Cargo.toml` keys and their definitions at https://doc.rust-lang.org/cargo/reference/manifest.html

# 查看创建的项目目录
$ cd hello_rust
(project-root)$ tree /f
...
.                                                             
│  Cargo.toml
│
├─src
│      main.rs

```

- 编译项目：`cargo build`

```PS
(project-root)$ cargo build
    Compiling hello_rust v0.1.0 ((project-root)\hello_rust)
    Finished `dev` profile [unoptimized + debuginfo] target(s) in 0.80s

# 查看编译后的项目目录
(project-root)$ tree /f
...
.                                                             
D:.
│  Cargo.lock
│  Cargo.toml
│  
├─src
│      main.rs
│
└─target
    │  .rustc_info.json
    │  CACHEDIR.TAG
    │
    └─debug
        │  .cargo-lock
        │  hello_rust.d
        │  hello_rust.exe
        │  hello_rust.pdb
        │
        ├─.fingerprint
        │  └─hello_rust-6e540d7df9638980
        │          bin-hello_rust
        │          bin-hello_rust.json
        │          dep-bin-hello_rust
        │          invoked.timestamp
        │
        ├─build
        ├─deps
        │      hello_rust.d
        │      hello_rust.exe
        │      hello_rust.pdb
        │
        ├─examples
        └─incremental
            └─hello_rust-0x5oft8jbxabj
                │  s-h8x43jzd84-02y43f5.lock
                │
                └─s-h8x43jzd84-02y43f5-50gedgo0w78w2ccdtejswr9wg
                        1cq3fbqn7fpnx4j5aas33t45r.o
                        286lugwb8t97c9ovvmizzolbd.o
                        2ayg5ehph2nn13oyh7ege4y1s.o
                        6vbl9vmralyct17e7cnltbg90.o
                        9zo35cywd4ds9yb3lipzp7s14.o
                        c3izb8h4s3due1xyajgyr1ozb.o
                        dep-graph.bin
                        query-cache.bin
                        work-products.bin
```

- 编译并运行项目：`cargo run`

=== "先执行 cargo build"

    ```PS
    (project-root)$ cargo run
        Finished `dev` profile [unoptimized + debuginfo] target(s) in 0.01s
        Running `target\debug\hello_rust.exe`
    Hello, world!
    ```

=== "不执行 cargo build"

    ```PS
    (project-root)$ cargo run
        Compiling hello_rust v0.1.0 ((project-root)\hello_rust)
        Finished `dev` profile [unoptimized + debuginfo] target(s) in 0.82s
        Running `target\debug\hello_rust.exe`
    Hello, world!
    ```

- 清除项目编译产生的文档：`cargo clean`

```PS
(project-root)$ cargo clean
     Removed 23 files, 2.9MiB total

# 清理后的文档目录
(project-root)$ tree /f
...
D:.
│  Cargo.lock
│  Cargo.toml
│
└─src
        main.rs
```

## 手动创建、编译和运行项目

- 在项目根目录下（本文档用`project-root`指代项目根目录)，新建文件 ``main.rs``，输入以下代码：

```rust
fn main() {
    println!("Hello, world!");
}
```

- 打开终端，输入以下命令：

```PS

// 编译代码
$ rustc main.rs

// 运行程序
$ .\main.exe

// 正确的输出结果
Hello, world!
```
