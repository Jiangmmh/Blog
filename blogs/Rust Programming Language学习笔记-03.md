# Rust programming language 学习笔记-02

## 1. Variable

在rust中最常用的定义变量的方法是使用 `let`关键字来定义变量：

```rust
let x = 10;
```

出于安全性的考虑，使用 `let`定义的变量默认值不可修改，也就是说定义时所附的初值将不可更改。

```rust
let x = 10;
x = 20;  // Fail
```

使用 `rustc`编译这两行代码会得到错误提示，其告诉你 `immutable`变量是不可更改的。

为了定义可变的变量还需加上 `mut`关键字：

```rust
let mut x = 10;
x = 20  // OK
```

注意，在使用let定义变量时，我们是没有指定变量类型的，在rust中还可以使用 `const`定义类型确定的常量：

```rust
const THREE_HOUSES_IN_SECOND : u32 = 60 * 60 * 3;
```

常量命名的惯例是使用大写字母并用下划线分隔，在 `:`后规定类型，初始化常量时，必须用算数表达式，不可使用函数的返回值来初始化。

下面介绍 `shadowing`，通过shadowing你用 `immutable`变量的名字再次定义新的变量，并且变量类型可以和之前不同。

```rust
let spaces = "    ";
let spaces = spaces.len();
```

可见，第一行先定义了一个字符串变量 `spaces`，第二行再次使用 `spaces`定义了整型变量。

为了保证安全性，rust不允许对 `mutable`变量进行隐式类型转换，例如下述代码无法通过编译器的检查。

```rust
let mut spaces = "    ";
spaces = spaces.len();
```

## 2. Data Type

rust是一个静态类型语言，其类型分为标量（Scalar）类型和符合（Compound）类型。

**标量类型**：

- 整型：`ixx`表示xx位的带符号整数，`uxx`表示xx位的无符号整数，可以使用 `_`来分隔数字，`_`本身无意义。
  - 默认整型为 `i32`
  - `0x`前缀：十六进制
  - `0o`前缀：八进制
  - `0b`前缀：二进制
  - `b'x'`：x的ascii编码
- 浮点型：默认为 `f64`，可选 `f32`。
- 布尔型：`false`, `true`。
- 字符型：`char`表示字符型。

整型和浮点型的运算操作有：

```rust
// addition
    let sum = 5 + 10;

    // subtraction
    let difference = 95.5 - 4.3;

    // multiplication
    let product = 4 * 30;

    // division
    let quotient = 56.7 / 32.2;
    let floored = 2 / 3; // Results in 0

    // remainder
    let remainder = 43 % 5;
```

**复合类型：**

元组

```rust
let tup: (i32, f64, u8) = (500, 6.4, 1); // 元组初始化
let (x, y, z) = tup;	// 元组赋值

println!("The value of x is: {}", tup.0); // 使用下标访问元组元素
```

元组不可变长，能容纳不同类型的元素，元素可变性取决于否使用mut关键字，利用 `.+index`来访问元素。

数组

```
let a: [i32; 5] = [1, 2, 3, 4, 5];
let a = [1, 2, 3, 4, 5];
println!("The value of x is: {}", a[0]); // 使用下标访问元组元素
```

数组不可变长，只能容纳相同类型的元素，元素的可变性取决于是否使用mut关键字，利用 `[index]`来访问元素。

示例

```rust
use std::io;

fn main() {
    let a = [1, 2, 3, 4, 5];

    println!("Please enter an array index.");

    let mut index = String::new();

    io::stdin()
        .read_line(&mut index)
        .expect("Failed to read line");

    let index: usize = index
        .trim()
        .parse()
        .expect("Index entered was not a number");

    let element = a[index];

    println!("The value of the element at index {index} is: {element}");
}
```

当输入index进行数组元素索引时，rust会检查index是否小于数组长度，防止访问越界。

## 3. Functions

rust使用 `fn`来定义函数，并以蛇形法来对函数命名，函数定义中参数和返回值需要明确给出类型。

```rust
fn main() {
    let x = plus_one(5);

    println!("The value of x is: {x}");
}

fn plus_one(x: i32) -> i32 {
    x + 1  // 无 ';'， 是一个表达式，返回x+1
}
```

语句（Statement）和表达式（Expression）的区别：

语句是一条指令，用来进行运算、初始化变量等操作，无返回值；而表达式则用来计算创造返回值。

例如：

```rust
fn main() {
    let y = {
        let x = 3;
        x + 1  // 注意，这行指令没有 ';', 表示是一个表达式，会返回计算结果作为右值
    };

    println!("The value of y is: {y}");
}
```

## 4. Comment

rust使用 `//`来进行注释。

```rust
let x = 1 // 初始化变量x为1
```

## 5. Control flow

**if语句**：

rust比C语言更加严格，rust中的条件表达式必须为 `bool`类型，否则编译器会报错。

```rust
fn main() {
    let number = 3;

    if number < 5 {
        println!("condition was true");
    } else {
        println!("condition was false");
    }
}
```

关于多层条件语句的嵌套：

```rust
fn main() {
    let number = 6;

    if number % 4 == 0 {
        println!("number is divisible by 4");
    } else if number % 3 == 0 {
        println!("number is divisible by 3");
    } else if number % 2 == 0 {
        println!("number is divisible by 2");
    } else {
        println!("number is not divisible by 4, 3, or 2");
    }
}
```

可以在let中使用条件语句，但必须注意if和else中的返回值必须是相同类型的数据。

```rust
fn main() {
    let condition = true;
    let number = if condition { 5 } else { 6 };

    println!("The value of number is: {number}");
}
```

**Loop语句：**

```rust
fn main() {
    let mut counter = 0;

    let result = loop {
        counter += 1;

        if counter == 10 {
            break counter * 2;
        }
    };

    println!("The result is {result}");
}
```

```rust
fn main() {
    let mut count = 0;
    'counting_up: loop {
        println!("count = {count}");
        let mut remaining = 10;

        loop {
            println!("remaining = {remaining}");
            if remaining == 9 {
                break;
            }
            if count == 2 {
                break 'counting_up;
            }
            remaining -= 1;
        }

        count += 1;
    }
    println!("End count = {count}");
}
```


**While语句:**

```rust
fn main() {
    let a = [10, 20, 30, 40, 50];
    let mut index = 0;

    while index < 5 {
        println!("the value is: {}", a[index]);

        index += 1;
    }
}
```


**for语句：**

相当于foreach语句，其中使用 `(a..b)`生成从[a,...,b)的范围元素，左开右闭。

```rust
fn main() {
    let a = [10, 20, 30, 40, 50];

    for element in a {
        println!("the value is: {element}");
    }
}
```

```rust
fn main() {
    for number in (1..4).rev() {
        println!("{number}!");
    }
    println!("LIFTOFF!!!");
}
```
