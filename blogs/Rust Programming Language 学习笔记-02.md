# Rust programming language 学习笔记-01

通过一个猜数字的游戏来对rust有一个大致的认识。

## 1. 使用Cargo来管理项目

`Cargo`是rust的构建（Build）系统和包（Package）管理器，使用Cargo可以帮我们节省很多时间。

首先用Cargo来创建一个项目 `guess_number`：

```shell
Cargo new guess_name
```

我们得到了如下的目录结构：

> guessing_game/
> ├── Cargo.lock
> ├── Cargo.toml
> └── src
>     └── main.rs

其中，`Cargo.toml`是Cargo项目的配置文件。

```shell
[package]
name = "guessing_game"
version = "0.1.0"
edition = "2021"

# See more keys and their definitions at https://doc.rust-lang.org/cargo/reference/manife
st.html

[dependencies]
rand = "0.8.3"
```

这里，为了生成待猜测的随机数，我们引入了rand这个外部crate，因此将rand添加到 `dependencies`中。

而 `Cargo.lock`文件是在第一次构建时产生，里面记录了构建时的依赖关系和版本，以便于下一次构建时不会因为crate的更新而使构建失败，该文件不需要要程序员修改。


## 2. 编写程序

Cargo项目的源代码都存放在 `项目跟目录/`src/下，打开 `main.rs`写入下述代码：

```rust
use std::io;
use rand::Rng;
use std::cmp::Ordering;

fn main() {
        println!("Guess the number!");

        let secrete_number = rand::thread_rng().gen_range(1..=100);
        println!("The secrete number is: {secrete_number}");

        loop {
                println!("Please input your guess.");

                let mut guess = String::new();

                io::stdin()
                        .read_line(&mut guess)
                        .expect("Failed to read line.");

                let guess:u32 = match guess.trim().parse() {
                        Ok(num) => num,
                        Err(_) => continue,
                };

                println!("Your guess is {}", guess);

                match guess.cmp(&secrete_number) {
                        Ordering::Less => println!("Too small!"),
                        Ordering::Greater => println!("Too big!"),
                        Ordering::Equal => {
                                println!("You win!");
                                break;
                        }
                }
        }

}
```

## 3. 构建并运行系统

使用Cargo构建项目：

```shell
cargo build // 默认 用于debug
cargo build --release  // 用于发布，对代码进行优化
```

只利用rust编译器来检查代码是否有错误而不真正编译：

```shell
cargo check
```

当构建完项目后会生成一个 `target`文件夹，构建生成的文件就放在其中：

> target/
> ├── CACHEDIR.TAG
> ├── debug
> │   ├── build
> │   │   ├── libc-0f1f7ee0722358c0
> │   │   │   ├── invoked.timestamp
> │   │   │   ├── out
> │   │   │   ├── output
> │   │   │   ├── root-output
> │   │   │   └── stderr
> │   │   └── libc-fc149cee1b11b1a6
> │   │       ├── build-script-build
> │   │       ├── build_script_build-fc149cee1b11b1a6
> │   │       └── build_script_build-fc149cee1b11b1a6.d
> │   ├── deps
> │   │   ├── cfg_if-90e3626e3c41eec3.d
> │   │   ├── getrandom-c3642deab3e8be33.d
> │   │   ├── guessing_game-ab543b1bbec599e4
> │   │   ├── guessing_game-ab543b1bbec599e4.d
> │   │   ├── libc-594f629dad712175.d
> │   │   ├── libcfg_if-90e3626e3c41eec3.rlib
> │   │   ├── libcfg_if-90e3626e3c41eec3.rmeta
> │   │   ├── libgetrandom-c3642deab3e8be33.rlib
> │   │   ├── libgetrandom-c3642deab3e8be33.rmeta
> │   │   ├── liblibc-594f629dad712175.rlib
> │   │   ├── liblibc-594f629dad712175.rmeta
> │   │   ├── libppv_lite86-5e6fdfabf1edbf66.rlib
> │   │   ├── libppv_lite86-5e6fdfabf1edbf66.rmeta
> │   │   ├── librand-753ae093aaf3ad1e.rlib
> │   │   ├── librand-753ae093aaf3ad1e.rmeta
> │   │   ├── librand_chacha-e266332e74ef65a2.rlib
> │   │   ├── librand_chacha-e266332e74ef65a2.rmeta
> │   │   ├── librand_core-076aaa22e3069422.rlib
> │   │   ├── librand_core-076aaa22e3069422.rmeta
> │   │   ├── ppv_lite86-5e6fdfabf1edbf66.d
> │   │   ├── rand-753ae093aaf3ad1e.d
> │   │   ├── rand_chacha-e266332e74ef65a2.d
> │   │   └── rand_core-076aaa22e3069422.d
> │   ├── examples
> │   ├── guessing_game
> │   ├── guessing_game.d
> │   └── incremental
> │       └── guessing_game-b5c9v91ywym5
> │           ├── s-gc1fa55dju-1mnpwsl-1kxv1wjcxtamx
> │           │   ├── 1huxl4jgb1j902e6.o
> │           │   ├── 1lr0zh6rw5fsougk.o
> │           │   ├── 1neb4o3mud8fw2cx.o
> │           │   ├── 225wvlig22nkgk1m.o
> │           │   ├── 26boyjj9yrjngc19.o
> │           │   ├── 2c3zsnc1z3bdzmij.o
> │           │   ├── 2g7bc9th957b1gxr.o
> │           │   ├── 2hc9oz349lpyvte2.o
> │           │   ├── 2jy8ajxinny4fpr4.o
> │           │   ├── 2kwvc285hay0d2a1.o
> │           │   ├── 2lhqfft0xp5j34ck.o
> │           │   ├── 32bucaoedk1m1omg.o
> │           │   ├── 382qj1ccvj321htz.o
> │           │   ├── 3awg1iiyc31uh5op.o
> │           │   ├── 3idffrsyjlrs6pfn.o
> │           │   ├── 43nx1eflvj2ziobo.o
> │           │   ├── 45p3kyyto6x3n2r4.o
> │           │   ├── 47o6t5jcrb7zxmd8.o
> │           │   ├── 49mf15s8yfk30a32.o
> │           │   ├── 4hvu0itn2jq8ezq4.o
> │           │   ├── 4n9l45srlw97pgwl.o
> │           │   ├── 4vdg2kxplrnlaz9j.o
> │           │   ├── 4w2wvufsbvphddkq.o
> │           │   ├── 4wj2vwukl9o4schw.o
> │           │   ├── 4x7zgj7y48c3dkmy.o
> │           │   ├── 55ou1guu12y11llm.o
> │           │   ├── 56lxm4eogeto1jlw.o
> │           │   ├── 98gdt82v0dn6t38.o
> │           │   ├── dep-graph.bin
> │           │   ├── nltthqg0czesdor.o
> │           │   ├── on412p4kpxd7zj1.o
> │           │   ├── qnmvivi14e9flkv.o
> │           │   ├── qu1745fnj7ubzd1.o
> │           │   ├── query-cache.bin
> │           │   ├── work-products.bin
> │           │   └── z83ivoteohqwbwg.o
> │           └── s-gc1fa55dju-1mnpwsl.lock
> └── release
>     ├── build
>     │   ├── libc-032cc29a04ecf5a5
>     │   │   ├── build-script-build
>     │   │   ├── build_script_build-032cc29a04ecf5a5
>     │   │   └── build_script_build-032cc29a04ecf5a5.d
>     │   └── libc-484a335dcdc620f3
>     │       ├── invoked.timestamp
>     │       ├── out
>     │       ├── output
>     │       ├── root-output
>     │       └── stderr
>     ├── deps
>     │   ├── cfg_if-af4ed11818f18246.d
>     │   ├── getrandom-87c1c4945d742c71.d
>     │   ├── guessing_game-b5e2b8d79a9e92d2
>     │   ├── guessing_game-b5e2b8d79a9e92d2.d
>     │   ├── libc-1619e1b6943fc8dd.d
>     │   ├── libcfg_if-af4ed11818f18246.rlib
>     │   ├── libcfg_if-af4ed11818f18246.rmeta
>     │   ├── libgetrandom-87c1c4945d742c71.rlib
>     │   ├── libgetrandom-87c1c4945d742c71.rmeta
>     │   ├── liblibc-1619e1b6943fc8dd.rlib
>     │   ├── liblibc-1619e1b6943fc8dd.rmeta
>     │   ├── libppv_lite86-bf7dadb5db4d146d.rlib
>     │   ├── libppv_lite86-bf7dadb5db4d146d.rmeta
>     │   ├── librand-70cfd29691238001.rlib
>     │   ├── librand-70cfd29691238001.rmeta
>     │   ├── librand_chacha-cb054c1788c832c5.rlib
>     │   ├── librand_chacha-cb054c1788c832c5.rmeta
>     │   ├── librand_core-14a5400eebde10f4.rlib
>     │   ├── librand_core-14a5400eebde10f4.rmeta
>     │   ├── ppv_lite86-bf7dadb5db4d146d.d
>     │   ├── rand-70cfd29691238001.d
>     │   ├── rand_chacha-cb054c1788c832c5.d
>     │   └── rand_core-14a5400eebde10f4.d
>     ├── examples
>     ├── guessing_game
>     ├── guessing_game.d
>     └── incremental

最后利用Cargo来运行项目：

```shell
cargo run
```

通过这个小游戏，我们可以看到利用Cargo来创建、管理、构建和运行一个rust项目的全貌。
