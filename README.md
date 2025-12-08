# V 语言文档

（查看 https://modules.vlang.io/ 了解 V 语言标准库的文档）
（另请参阅 https://docs.vlang.io/introduction.html，该网站包含与此文档相同的信息，
但按章节分为不同页面，便于在移动设备上阅读）

## 简介

V 是一种静态类型的编译型编程语言，旨在构建可维护的软件。

它类似于 Go，其设计也受到 Oberon、Rust、Swift、
Kotlin 和 Python 的影响。

V 是一门非常简单的语言。通读本文档大约需要一个周末的时间，
读完时您基本上就掌握了整个语言。

该语言提倡编写简单清晰的代码，尽量减少抽象。

尽管简单，V 为开发者提供了强大的功能。
您在其他语言中能做的任何事情，在 V 中也能做到。

## 从源码安装 V

获取最新最好的 V 的最佳方式是从源码安装。
这很简单，只需要几秒钟：
```bash
git clone --depth=1 https://github.com/vlang/v
cd v
make
```

注意：如果您在 Windows 上（WSL 之外），请在 CMD 命令行中运行 `make.bat` 而不是 `make`。
注意：在 Ubuntu/Debian 上，您可能需要先运行 `sudo apt install git build-essential make`。

更多详细信息，请参阅
README.md 中的
[安装 V](https://github.com/vlang/v/blob/master/README.md#installing-v-from-source)
部分。

## 升级 V 到最新版本

如果机器上已经安装了 V，可以使用 V 的内置自动更新器
将其升级到最新版本。
为此，请运行命令 `v up`。

## 为分发打包 V
请参阅[如何为 V 准备包的说明](packaging_v_for_distributions.md)。

## 快速开始

您可以让 V 自动为您设置项目的基本结构，
只需在终端中使用以下任一命令：

* `v init` → 在当前文件夹中添加必要的文件，使其成为 V 项目
* `v new abc` → 在新文件夹 `abc` 中创建一个新项目，默认为 "hello world" 项目。
* `v new --web abcd` → 在新文件夹 `abcd` 中创建一个新项目，使用 vweb 模板。

## 目录

<table>
<tr><td width=33% valign=top>

* [Hello world](#hello-world)
* [运行包含多个文件的项目文件夹](#running-a-project-folder-with-several-files)
* [注释](#comments)
* [函数](#functions)
    * [提升](#hoisting)
    * [返回多个值](#returning-multiple-values)
* [符号可见性](#symbol-visibility)
* [变量](#variables)
    * [可变变量](#mutable-variables)
    * [初始化与赋值](#initialization-vs-assignment)
    * [警告和声明错误](#warnings-and-declaration-errors)
* [V 类型](#v-types)
    * [基本类型](#primitive-types)
    * [字符串](#strings)
    * [字符](#runes)
    * [数字](#numbers)
    * [数组](#arrays)
        * [多维数组](#multidimensional-arrays)
        * [数组方法](#array-methods)
        * [数组切片](#array-slices)
    * [固定大小数组](#fixed-size-arrays)
    * [映射](#maps)
        * [映射更新语法](#map-update-syntax)

</td><td width=33% valign=top>

* [模块导入](#module-imports)
    * [选择性导入](#selective-imports)
    * [模块层次结构](#module-hierarchy)
    * [模块导入别名](#module-import-aliasing)
* [语句和表达式](#statements--expressions)
    * [If](#if)
        * [`If` 表达式](#if-expressions)
        * [`If` 解包](#if-unwrapping)
    * [Match](#match)
    * [In 运算符](#in-operator)
    * [For 循环](#for-loop)
    * [Defer](#defer)
    * [Goto](#goto)
* [结构体](#structs)
    * [堆结构体](#heap-structs)
    * [默认字段值](#default-field-values)
    * [必需字段](#required-fields)
    * [简短结构体字面量语法](#short-struct-literal-syntax)
    * [结构体更新语法](#struct-update-syntax)
    * [尾随结构体字面量参数](#trailing-struct-literal-arguments)
    * [访问修饰符](#access-modifiers)
    * [匿名结构体](#anonymous-structs)
    * [静态类型方法](#static-type-methods)
    * [[noinit] 结构体](#noinit-structs)
    * [方法](#methods)
    * [嵌入结构体](#embedded-structs)
* [联合体](#unions)
    * [为什么使用联合体？](#why-use-unions)
    * [嵌入](#embedding)

</td><td valign=top>

* [函数 2](#functions-2)
    * [默认不可变函数参数](#immutable-function-args-by-default)
    * [可变参数](#mutable-arguments)
    * [可变数量参数](#variable-number-of-arguments)
    * [匿名函数和高阶函数](#anonymous--higher-order-functions)
    * [Lambda 表达式](#lambda-expressions)
    * [闭包](#closures)
    * [参数求值顺序](#parameter-evaluation-order)
* [引用](#references)
* [常量](#constants)
    * [必需的模块前缀](#required-module-prefix)
* [内置函数](#builtin-functions)
    * [println](#println)
    * [打印自定义类型](#printing-custom-types)
    * [运行时转储表达式](#dumping-expressions-at-runtime)
* [模块](#modules)
    * [创建模块](#create-modules)
    * [项目文件夹的特殊注意事项](#special-considerations-for-project-folders)
    * [init 函数](#init-functions)
    * [cleanup 函数](#cleanup-functions)

</td></tr>
<tr><td width=33% valign=top>

* [类型声明](#type-declarations)
    * [类型别名](#type-aliases)
    * [枚举](#enums)
    * [函数类型](#function-types)
    * [接口](#interfaces)
    * [和类型](#sum-types)
    * [Option/Result 类型和错误处理](#optionresult-types-and-error-handling)
        * [处理 options/results](#handling-optionsresults)
    * [自定义错误类型](#custom-error-types)
    * [泛型](#generics)
* [并发](#concurrency)
    * [生成并发任务](#spawning-concurrent-tasks)
    * [通道](#channels)
    * [共享对象](#shared-objects)
* [JSON](#json)
    * [解码 JSON](#decoding-json)
    * [编码 JSON](#encoding-json)
* [测试](#testing)
    * [断言](#asserts)
    * [带额外消息的断言](#asserts-with-an-extra-message)
    * [不会中止程序的断言](#asserts-that-do-not-abort-your-program)
    * [测试文件](#test-files)
    * [运行测试](#running-tests)
* [内存管理](#memory-management)
    * [控制](#control)
    * [栈和堆](#stack-and-heap)
* [ORM](#orm)
* [编写文档](#writing-documentation)
    * [文档注释中的换行](#newlines-in-documentation-comments)

</td><td width=33% valign=top>

* [工具](#tools)
    * [v fmt](#v-fmt)
    * [v shader](#v-shader)
    * [性能分析](#profiling)
* [包管理](#package-management)
    * [包命令](#package-commands)
    * [发布包](#publish-package)
* [高级主题](#advanced-topics)
    * [属性](#attributes)
    * [条件编译](#conditional-compilation)
        * [编译时伪变量](#compile-time-pseudo-variables)
        * [编译时反射](#compile-time-reflection)
        * [编译时代码](#compile-time-code)
        * [编译时类型](#compile-time-types)
        * [环境特定文件](#environment-specific-files)
	* [调试器](#debugger)
 		* [调用栈](#call-stack)
   		* [跟踪](#trace)
    * [内存不安全代码](#memory-unsafe-code)
    * [带引用字段的结构体](#structs-with-reference-fields)
    * [sizeof 和 __offsetof](#sizeof-and-__offsetof)
    * [有限运算符重载](#limited-operator-overloading)
    * [性能调优](#performance-tuning)
    * [原子操作](#atomics)
    * [全局变量](#global-variables)
    * [静态变量](#static-variables)
    * [交叉编译](#cross-compilation)
    * [调试](#debugging)
        * [C 后端二进制文件（默认）](#c-backend-binaries-default)
        * [原生后端二进制文件](#native-backend-binaries)
        * [Javascript 后端](#javascript-backend)

</td><td valign=top>

* [V 和 C](#v-and-c)
    * [从 V 调用 C](#calling-c-from-v)
    * [从 C 调用 V](#calling-v-from-c)
    * [传递 C 编译标志](#passing-c-compilation-flags)
    * [#pkgconfig](#pkgconfig)
    * [包含 C 代码](#including-c-code)
    * [C 类型](#c-types)
    * [C 声明](#c-declarations)
    * [导出到共享库](#export-to-shared-library)
    * [将 C 转换为 V](#translating-c-to-v)
    * [解决 C 问题](#working-around-c-issues)
* [其他 V 特性](#other-v-features)
    * [内联汇编](#inline-assembly)
    * [热代码重载](#hot-code-reloading)
    * [V 中的跨平台 shell 脚本](#cross-platform-shell-scripts-in-v)
    * [无扩展名的 Vsh 脚本](#vsh-scripts-with-no-extension)
* [附录](#appendices)
    * [关键字](#appendix-i-keywords)
    * [运算符](#appendix-ii-operators)
    * [其他在线资源](#other-online-resources)

</td></tr>
</table>

<!--
Note: There are several special keywords, which you can put after the code fences for v:
compile, cgen, live, ignore, failcompile, okfmt, oksyntax, badsyntax, wip, nofmt
For more details, do: `v check-md`
-->

## Hello World

```v
fn main() {
	println('hello world')
}
```

将此代码片段保存到名为 `hello.v` 的文件中。然后运行：`v run hello.v`。

> 这假设您已经使用 `v symlink` 创建了 V 的符号链接，如
[此处](https://github.com/vlang/v/blob/master/README.md#symlinking)所述。
> 如果还没有，您需要手动输入 V 的路径。

恭喜 - 您刚刚编写并执行了第一个 V 程序！

您可以使用 `v hello.v` 编译程序而不执行。
运行 `v help` 查看所有支持的命令。

从上面的示例中，您可以看到函数使用 `fn` 关键字声明。
返回类型在函数名之后指定。
在这种情况下，`main` 不返回任何内容，因此没有返回类型。

与许多其他语言（如 C、Go 和 Rust）一样，`main` 是程序的入口点。

[`println`](#println) 是少数几个[内置函数](#builtin-functions)之一。
它将传递给它的值打印到标准输出。

在单文件程序中可以省略 `fn main()` 声明。
这在编写小程序、"脚本"或学习语言时很有用。
为简洁起见，本教程中将省略 `fn main()`。

这意味着 V 中的 "hello world" 程序可以简单到

```v
println('hello world')
```

> [!NOTE]
> 如果您不显式使用 `fn main() {}`，您需要确保所有
> 声明都在任何变量赋值语句或顶级函数调用之前，
> 因为 V 会将第一个赋值/函数调用之后的所有内容视为
> 隐式 main 函数的一部分。

## 运行包含多个文件的项目文件夹

假设您有一个包含多个 .v 文件的文件夹，其中一个是
您的 `main()` 函数，其他文件包含其他辅助
函数。它们可能按主题组织，但*尚未*结构化
到足以成为独立的可重用模块，您希望将它们
全部编译成一个程序。

在其他语言中，您必须使用 include 或构建系统
来枚举所有文件，将它们分别编译为目标文件，
然后将它们链接成一个最终的可执行文件。

但在 V 中，您可以一起编译和运行整个 .v 文件文件夹，
只需使用 `v run .`。传递参数也可以，所以您可以
这样做：`v run . --yourparam some_other_stuff`

上述命令将首先将您的文件编译成单个程序（以
您的文件夹/项目命名），然后执行该程序，并将
`--yourparam some_other_stuff` 作为 CLI 参数传递给它。

然后您的程序可以这样使用 CLI 参数：

```v
import os

println(os.args)
```

> [!NOTE]
> 成功运行后，V 将删除生成的可执行文件。
> 如果您想保留它，请使用 `v -keepc run .`，或者使用
> `v .` 手动编译。

> [!NOTE]
> 任何 V 编译器标志都应在 `run` 命令*之前*传递。
> 源文件/文件夹之后的所有内容将按原样传递给程序
> - 不会被 V 处理。

## 注释

```v
// 这是单行注释。
/*
这是多行注释。
   /* 可以嵌套。 */
*/
```

## 函数

```v
fn main() {
	println(add(77, 33))
	println(sub(100, 50))
}

fn add(x int, y int) int {
	return x + y
}

fn sub(x int, y int) int {
	return x - y
}
```

同样，类型在参数名之后。

就像在 Go 和 C 中一样，函数不能重载。
这简化了代码，提高了可维护性和可读性。

### 提升（Hoisting）

函数可以在声明之前使用：
`add` 和 `sub` 在 `main` 之后声明，但仍可以从 `main` 调用。
这对 V 中的所有声明都适用，消除了对头文件的需求
或考虑文件和声明顺序的需要。

### 返回多个值

```v
fn foo() (int, int) {
	return 2, 3
}

a, b := foo()
println(a) // 2
println(b) // 3
c, _ := foo() // 使用 `_` 忽略值
```

## 符号可见性

```v
pub fn public_function() {
}

fn private_function() {
}
```

函数默认是私有的（不导出）。
要允许其他[模块](#module-imports)使用它们，请在前面加上 `pub`。这同样适用于
[结构体](#structs)、[常量](#constants)和[类型](#type-declarations)。

> [!NOTE]
> `pub` 只能在命名模块中使用。
> 有关创建模块的信息，请参阅[模块](#modules)。

## 变量

```v
name := 'Bob'
age := 20
large_number := i64(9999999999)
println(name)
println(age)
println(large_number)
```

变量使用 `:=` 声明和初始化。这是 V 中
声明变量的唯一方式。这意味着变量总是有一个初始
值。

变量的类型从右侧的值推断。
要选择不同的类型，请使用类型转换：
表达式 `T(v)` 将值 `v` 转换为
类型 `T`。

与大多数其他语言不同，V 只允许在函数中定义变量。
默认情况下，V 不允许**全局变量**。更多[详细信息](#global-variables)。

为了在不同代码库之间保持一致，所有变量和函数名
必须使用 `snake_case` 风格，而类型名必须使用 `PascalCase`。

### 可变变量

```v
mut age := 20
println(age)
age = 21
println(age)
```

要更改变量的值，请使用 `=`。在 V 中，变量默认是
不可变的。
要能够更改变量的值，您必须使用 `mut` 声明它。

尝试从第一行删除 `mut` 后编译上面的程序。

### 初始化与赋值

注意 `:=` 和 `=` 之间的（重要）区别。
`:=` 用于声明和初始化，`=` 用于赋值。

```v failcompile
fn main() {
	age = 21
}
```

此代码不会编译，因为变量 `age` 未声明。
V 中的所有变量都需要声明。

```v
fn main() {
	age := 21
}
```

可以在同一行更改多个变量的值。
这样，可以在没有中间变量的情况下交换它们的值。

```v
mut a := 0
mut b := 1
println('${a}, ${b}') // 0, 1
a, b = b, a
println('${a}, ${b}') // 1, 0
```

### 警告和声明错误

在开发模式下，编译器会警告您未使用变量
（您会收到"未使用变量"警告）。
在生产模式下（通过向 v 传递 `-prod` 标志启用 – `v -prod foo.v`）
它根本不会编译（就像在 Go 中一样）。
```v
fn main() {
	a := 10
	// 警告：未使用的变量 `a`
}
```

要忽略函数返回的值，可以使用 `_`
```v
fn foo() (int, int) {
	return 2, 3
}

fn main() {
	c, _ := foo()
	print(c)
	// 不会警告 foo 返回的未使用变量。
}
```

与大多数语言不同，不允许变量遮蔽。使用已在父作用域中使用的名称
声明变量将导致编译错误。
```v failcompile nofmt
fn main() {
	a := 10
	{
		a := 20 // 错误：`a` 的重复定义
	}
}
```
虽然不允许变量遮蔽，但允许字段遮蔽。
```v
pub struct Dimension {
	width  int = -1
	height int = -1
}

pub struct Test {
	Dimension
	width int = 100
	// height int
}

fn main() {
	test := Test{}
	println('${test.width} ${test.height} ${test.Dimension.width}') // 100 -1 -1
}
```
## V 类型

### 基本类型

```v ignore
bool

string

i8    i16  int  i64      i128 (即将推出)
u8    u16  u32  u64      u128 (即将推出)

rune // 表示一个 Unicode 码点

f32 f64

isize, usize // 平台相关，大小是引用内存中任何位置所需的字节数

voidptr // 主要用于 [C 互操作](#v-and-c)
```

> [!NOTE]
> 与 C 和 Go 不同，`int` 始终是 32 位整数。

V 中所有运算符两侧的值必须具有相同类型的规则有一个例外。
如果一侧的小型基本类型完全适合另一侧类型的数据范围，
则可以自动提升。
以下是允许的可能性：

```v ignore
   i8 → i16 → int → i64
                  ↘     ↘
                    f32 → f64
                  ↗     ↗
   u8 → u16 → u32 → u64 ⬎
      ↘     ↘     ↘      ptr
   i8 → i16 → int → i64 ⬏
```

例如，`int` 值可以自动提升为 `f64`
或 `i64`，但不能提升为 `u32`。（`u32` 会导致
负值丢失符号）。
但是，从 `int` 到 `f32` 的提升目前是自动完成的
（但对于大值可能导致精度损失）。

像 `123` 或 `4.56` 这样的字面量以特殊方式处理。它们
不会导致类型提升，但在必须决定其类型时，
它们分别默认为 `int` 和 `f64`：

```v nofmt
u := u16(12)
v := 13 + u    // v 的类型是 `u16` - 无提升
x := f32(45.6)
y := x + 3.14  // y 的类型是 `f32` - 无提升
a := 75        // a 的类型是 `int` - 整数字面量的默认类型
b := 14.7      // b 的类型是 `f64` - 浮点数字面量的默认类型
c := u + a     // c 的类型是 `int` - `u` 的值自动提升
d := b + x     // d 的类型是 `f64` - `x` 的值自动提升
```

### 字符串

在 V 中，字符串以 UTF-8 编码，默认是不可变的（只读）：

```v
s := 'hello 🌎' // `world` 表情符号占 4 个字节，字符串长度以字节为单位报告
assert s.len == 10

arr := s.bytes() // 将 `string` 转换为 `[]u8`
assert arr.len == 10

s2 := arr.bytestr() // 将 `[]u8` 转换为 `string`
assert s2 == s

name := 'Bob'
assert name.len == 3
// 索引给出一个字节，u8(66) == `B`
assert name[0] == u8(66)
// 切片给出字符串 'ob'
assert name[1..3] == 'ob'

// 转义码
// 像 C 中一样转义特殊字符
windows_newline := '\r\n'
assert windows_newline.len == 2

// 可以使用 `\x##` 表示法直接指定任意字节，其中 `#` 是
// 十六进制数字
aardvark_str := '\x61ardvark'
assert aardvark_str == 'aardvark'
assert '\xc0'[0] == u8(0xc0)

// 或使用八进制转义 `\###` 表示法，其中 `#` 是八进制数字
aardvark_str2 := '\141ardvark'
assert aardvark_str2 == 'aardvark'

// Unicode 可以直接指定为 `\u####`，其中 # 是十六进制数字
// 并将在内部转换为 UTF-8 表示
star_str := '\u2605' // ★
assert star_str == '★'
// UTF-8 也可以这样指定，作为单个字节。
assert star_str == '\xe2\x98\x85'
```

由于字符串是不可变的，您不能直接更改字符串中的字符：

```v failcompile
mut s := 'hello 🌎'
s[0] = `H` // 不允许
```

> 错误：无法赋值给 `s[i]`，因为 V 字符串是不可变的

请注意，索引字符串通常会产生 `u8`（字节），而不是 `rune` 或另一个 `string`。
索引对应于字符串中的_字节_，而不是 Unicode 码点。
如果您想将 `u8` 转换为 `string`，请在 `u8` 上使用 `.ascii_str()` 方法：

```v
country := 'Netherlands'
println(country[0]) // Output: 78
println(country[0].ascii_str()) // Output: N
```

但是，您可以使用 `runes()` 方法轻松获取字符串的 runes，该方法将返回
字符串中的 UTF-8 字符数组。然后您可以索引此数组。请注意，
如果字符串中有任何非 ASCII 字符，`rune` 数组上可用的索引
可能少于字符串中的字节数。

```v
mut s := 'hello 🌎'
// 字符串中有 10 个字节（如前所示），但只有 7 个 runes，因为 `world` 表情符号
// 只算作一个 `rune`（一个 Unicode 字符）
assert s.runes().len == 7
println(s.runes()[6])
```

如果您想从特定的 `string` 索引获取码点或其他更高级的 UTF-8 处理
和转换，请参阅
[vlib/encoding/utf8](https://modules.vlang.io/encoding.utf8.html) 模块。

单引号和双引号都可以用来表示字符串。为了一致性，`vfmt` 将双引号
转换为单引号，除非字符串包含单引号字符。

在原始字符串前加上 `r`。不处理转义，因此您将得到您输入的确切内容：

```v
s := r'hello\nworld' // `\n` 将保留为两个字符
println(s) // "hello\nworld"
```

字符串可以轻松转换为整数：

```v
s := '42'
n := s.int() // 42

// 支持所有 int 字面量
assert '0xc3'.int() == 195
assert '0o10'.int() == 8
assert '0b1111_0000_1010'.int() == 3850
assert '-0b1111_0000_1010'.int() == -3850
```

有关更高级的 `string` 处理和转换，请参阅
[vlib/strconv](https://modules.vlang.io/strconv.html) 模块。

#### 字符串插值

基本插值语法非常简单 - 在变量名前使用 `${`，在变量名后使用 `}`。
变量将被转换为字符串并嵌入到字面量中：

```v
name := 'Bob'
println('Hello, ${name}!') // Hello, Bob!
```

它也适用于字段：`'age = ${user.age}'`。您也可以使用更复杂的表达式：
`'can register = ${user.age > 13}'`。

还支持类似于 C 的 `printf()` 的格式说明符。`f`、`g`、`x`、`o`、`b` 等
是可选的，用于指定输出格式。编译器会处理存储大小，因此
没有 `hd` 或 `llu`。

要使用格式说明符，请遵循以下模式：

`${varname:[flags][width][.precision][type]}`

- flags（标志）：可以是以下零个或多个：`-` 用于在字段内左对齐输出，`0` 用于使用
  `0` 作为填充字符，而不是默认的 `space` 字符。
  > **注意**
  >
  > V 目前不支持使用 `'` 或 `#` 作为格式标志，V 支持但
  > 不需要 `+` 来右对齐，因为这是默认值。
- width（宽度）：可以是描述要输出的总字段最小宽度的整数值。
- precision（精度）：前面带有 `.` 的整数值将保证小数点后
  有那么多位数字，不包含任何无意义的尾随零。如果需要显示无意义的零，
  请在精度值后附加 `f` 说明符（见下面的示例）。仅适用于浮点
  变量，对整数变量忽略。
- type（类型）：`f` 和 `F` 指定输入是浮点数并应如此渲染，`e` 和 `E` 指定
  输入是浮点数并应渲染为指数（部分损坏），`g` 和 `G` 指定
  输入是浮点数——渲染器将对小值使用浮点表示法，对大值使用指数
  表示法，`d` 指定输入是整数并应以 10 进制
  数字渲染，`x` 和 `X` 要求整数并将其渲染为十六进制数字，`o` 要求
  整数并将其渲染为八进制数字，`b` 要求整数并将其渲染为二进制
  数字，`s` 要求字符串（几乎从不使用）。

  > **注意**
  >
  > 当数字类型可以渲染字母字符时，例如十六进制字符串或特殊值
  > 如 `infinity`，类型的小写版本强制使用小写字母，
  > 大写版本强制使用大写字母。

  > **注意**
  >
  > 在大多数情况下，最好将格式类型留空。浮点数将默认
  > 渲染为 `g`，整数将默认渲染为 `d`，`s` 几乎总是多余的。
  > 仅在以下三种情况下建议指定类型：

- 格式字符串在编译时解析，因此指定类型可以帮助在编译时检测错误
- 格式字符串默认对十六进制数字和指数中的 `e` 使用小写字母。使用
  大写类型强制使用大写十六进制数字和大写 `E` 作为指数。
- 格式字符串是从整数获取十六进制、二进制或八进制字符串的最便捷方式。

更多信息请参阅
[格式占位符规范](https://en.wikipedia.org/wiki/Printf_format_string#Format_placeholder_specification)。

```v
x := 123.4567
println('[${x:.2}]') // 四舍五入到两位小数 => [123.46]
println('[${x:10}]') // 右对齐，左侧填充空格 => [   123.457]
println('[${int(x):-10}]') // 左对齐，右侧填充空格 => [123       ]
println('[${int(x):010}]') // 左侧用零填充 => [0000000123]
println('[${int(x):b}]') // 以二进制输出 => [1111011]
println('[${int(x):o}]') // 以八进制输出 => [173]
println('[${int(x):X}]') // 以大写十六进制输出 => [7B]

println('[${10.0000:.2}]') // 移除末尾无效的0 => [10]
println('[${10.0000:.2f}]') // 显示末尾的0，即使它们不改变数值 => [10.00]
```

V语言还提供了 r 和 R 开关选项，用于将字符串重复指定的次数。

```v
println('[${'abc':3r}]') // [abcabcabc]
println('[${'abc':3R}]') // [ABCABCABC]
```

#### 字符串操作符

```v
name := 'Bob'
bobby := name + 'by' // + 用于连接字符串
println(bobby) // "Bobby"
mut s := 'hello '
s += 'world' // `+=` 用于追加到字符串
println(s) // "hello world"
```

V 中的所有操作符两侧的值必须具有相同的类型。您不能将整数
连接到字符串：

```v failcompile
age := 10
println('age = ' + age) // 不允许
```

> 错误：中缀表达式：不能将 `int`（右侧表达式）用作 `string`

我们必须将 `age` 转换为 `string`：

```v
age := 11
println('age = ' + age.str())
```

或使用字符串插值（推荐）：

```v
age := 12
println('age = ${age}')
```

请参阅 [string](https://modules.vlang.io/index.html#string) 的所有方法
以及相关模块 [strings](https://modules.vlang.io/strings.html)、
[strconv](https://modules.vlang.io/strconv.html)。

### Runes（字符）

`rune` 表示单个 UTF-32 编码的 Unicode 字符，是 `u32` 的别名。
要表示它们，请使用 <code>`</code>（反引号）：

```v
rocket := `🚀`
```

`rune` 可以使用 `.str()` 方法转换为 UTF-8 字符串。

```v
rocket := `🚀`
assert rocket.str() == '🚀'
```

`rune` 可以使用 `.bytes()` 方法转换为 UTF-8 字节。

```v
rocket := `🚀`
assert rocket.bytes() == [u8(0xf0), 0x9f, 0x9a, 0x80]
```

十六进制、Unicode 和八进制转义序列在 `rune` 字面量中也有效：

```v
assert `\x61` == `a`
assert `\141` == `a`
assert `\u0061` == `a`

// 多字节字面量也可以
assert `\u2605` == `★`
assert `\u2605`.bytes() == [u8(0xe2), 0x98, 0x85]
assert `\xe2\x98\x85`.bytes() == [u8(0xe2), 0x98, 0x85]
assert `\342\230\205`.bytes() == [u8(0xe2), 0x98, 0x85]
```

请注意，`rune` 字面量使用与字符串相同的转义语法，但它们只能包含一个 Unicode
字符。因此，如果您的代码没有指定单个 Unicode 字符，您将在编译时收到
错误。

还要记住，字符串按字节索引，而不是 runes，所以请注意：

```v
rocket_string := '🚀'
assert rocket_string[0] != `🚀`
assert 'aloha!'[0] == `a`
```

字符串可以通过 `.runes()` 方法转换为 runes。

```v
hello := 'Hello World 👋'
hello_runes := hello.runes() // [`H`, `e`, `l`, `l`, `o`, ` `, `W`, `o`, `r`, `l`, `d`, ` `, `👋`]
assert hello_runes.string() == hello
```

### 数字

```v
a := 123
```

这会将值 123 赋给 `a`。默认情况下，`a` 的类型为
`int`。

您也可以使用十六进制、二进制或八进制表示法来表示整数字面量：

```v
a := 0x7B
b := 0b01111011
c := 0o173
```

所有这些都将被赋值为 123。无论您使用什么表示法，它们都将是
`int` 类型。

V 还支持使用 `_` 作为分隔符来书写数字：

```v
num := 1_000_000 // 与 1000000 相同
three := 0b0_11 // 与 0b11 相同
float_num := 3_122.55 // 与 3122.55 相同
hexa := 0xF_F // 与 255 相同
oct := 0o17_3 // 与 0o173 相同
```

如果您想要不同类型的整数，可以使用类型转换：

```v
a := i64(123)
b := u8(42)
c := i16(12345)
```

浮点数的赋值方式相同：

```v
f := 1.0
f1 := f64(3.14)
f2 := f32(3.14)
```

如果您不显式指定类型，默认情况下浮点字面量
的类型为 `f64`。

浮点字面量也可以声明为十的幂：

```v
f0 := 42e1 // 420
f1 := 123e-2 // 1.23
f2 := 456e+2 // 45600
```

### 数组

数组是相同类型的数据元素的集合。数组字面量是
用方括号包围的表达式列表。可以使用
*索引*表达式访问单个元素。索引从 `0` 开始。

```v
mut nums := [10, 20, 30]
println(nums) // `[10, 20, 30]`
println(nums[0]) // `10`
println(nums[1]) // `20`

nums[1] = 5
println(nums) // `[10, 5, 30]`
```

<a id='array-operations'></a>

可以使用推送操作符 `<<` 将元素追加到数组的末尾。
它也可以追加整个数组。

```v
mut nums := [1, 2, 3]
nums << 4
println(nums) // "[1, 2, 3, 4]"

// 追加数组
nums << [5, 6, 7]
println(nums) // "[1, 2, 3, 4, 5, 6, 7]"
```

```v
mut names := ['John']
names << 'Peter'
names << 'Sam'
// names << 10  <-- 这不会编译。`names` 是字符串数组。
```

如果数组包含 `val`，则 `val in array` 返回 true。请参阅 [`in` 操作符](#in-operator)。

```v
names := ['John', 'Peter', 'Sam']
println('Alex' in names) // "false"
```

#### 数组字段

有两个字段控制数组的"大小"：

* `len`：*长度* - 数组中预分配和初始化的元素数量
* `cap`：*容量* - 为元素保留但未初始化或计为元素的内存空间量。
  数组可以增长到此大小而无需
  重新分配。通常，V 会自动处理此字段，但在某些
  情况下，用户可能希望进行手动优化（请参阅[下文](#array-initialization)）。

```v
mut nums := [1, 2, 3]
println(nums.len) // "3"
println(nums.cap) // "3" 或更大
nums = [] // 数组现在为空
println(nums.len) // "0"
```

`data` 是一个字段（类型为 `voidptr`），包含第一个
元素的地址。这用于低级 [`unsafe`](#memory-unsafe-code) 代码。

> [!NOTE]
> 字段是只读的，用户无法修改。

#### 数组初始化

数组的类型由第一个元素决定：

* `[1, 2, 3]` 是整数数组（`[]int`）。
* `['a', 'b']` 是字符串数组（`[]string`）。

用户可以显式指定第一个元素的类型：`[u8(16), 32, 64, 128]`。
V 数组是同质的（所有元素必须具有相同的类型）。
这意味着像 `[1, 'a']` 这样的代码不会编译。

对于少量已知元素，上述语法很好，但对于非常大或空的
数组，有第二种初始化语法：

```v
mut a := []int{len: 10000, cap: 30000, init: 3}
```

这将创建一个包含 10000 个 `int` 元素的数组，所有元素都初始化为 `3`。为
30000 个元素保留了内存空间。参数 `len`、`cap` 和 `init` 是可选的；
`len` 默认为 `0`，`init` 默认为元素类型的默认初始化（数值类型为 `0`，
`string` 为 `''` 等）。运行时系统确保
容量不小于 `len`（即使显式指定了较小的值）：

```v
arr := []int{len: 5, init: -1}
// `arr == [-1, -1, -1, -1, -1]`, arr.cap == 5

// 声明空数组：
users := []int{}
```

设置容量可以提高向数组推送元素的性能，
因为可以避免重新分配：

```v
mut numbers := []int{cap: 1000}
println(numbers.len) // 0
// 现在追加元素不会重新分配
for i in 0 .. 1000 {
	numbers << i
}
```

> [!NOTE]
> 上面的代码使用了[范围 `for`](#range-for) 语句。

您可以通过访问 `index` 变量来初始化数组，该变量给出
索引，如下所示：

```v
count := []int{len: 4, init: index}
assert count == [0, 1, 2, 3]

mut square := []int{len: 6, init: index * index}
// square == [0, 1, 4, 9, 16, 25]
```

#### 数组类型

数组可以是以下类型：

| 类型         | 示例定义                           |
|--------------|--------------------------------------|
| 数字         | `[]int,[]i64`                        |
| 字符串       | `[]string`                           |
| Rune         | `[]rune`                             |
| 布尔值       | `[]bool`                             |
| 数组         | `[][]int`                            |
| 结构体       | `[]MyStructName`                     |
| 通道         | `[]chan f64`                         |
| 函数         | `[]MyFunctionType` `[]fn (int) bool` |
| 接口         | `[]MyInterfaceName`                  |
| 和类型       | `[]MySumTypeName`                    |
| 泛型类型     | `[]T`                                |
| 映射         | `[]map[string]f64`                   |
| 枚举         | `[]MyEnumType`                       |
| 别名         | `[]MyAliasTypeName`                  |
| 线程         | `[]thread int`                       |
| 引用         | `[]&f64`                             |
| 共享         | `[]shared MyStructType`              |
| 选项         | `[]?f64`                          |

**示例代码：**

此示例使用[结构体](#structs)和[和类型](#sum-types)创建一个
可以处理不同类型（例如 Points、Lines）数据元素的数组。

```v
struct Point {
	x int
	y int
}

struct Line {
	p1 Point
	p2 Point
}

type ObjectSumType = Line | Point

mut object_list := []ObjectSumType{}
object_list << Point{1, 1}
object_list << Line{
	p1: Point{3, 3}
	p2: Point{4, 4}
}
dump(object_list)
/*
object_list: [ObjectSumType(Point{
    x: 1
    y: 1
}), ObjectSumType(Line{
    p1: Point{
        x: 3
        y: 3
    }
    p2: Point{
        x: 4
        y: 4
    }
})]
*/
```

#### 多维数组

数组可以有多个维度。

二维数组示例：

```v
mut a := [][]int{len: 2, init: []int{len: 3}}
a[0][1] = 2
println(a) // [[0, 2, 0], [0, 0, 0]]
```

三维数组示例：

```v
mut a := [][][]int{len: 2, init: [][]int{len: 3, init: []int{len: 2}}}
a[0][1][1] = 2
println(a) // [[[0, 0], [0, 2], [0, 0]], [[0, 0], [0, 0], [0, 0]]]
```

#### 数组方法

所有数组都可以使用 `println(arr)` 轻松打印，并使用
`s := arr.str()` 转换为字符串。

使用 `.clone()` 复制数组数据：

```v
nums := [1, 2, 3]
nums_copy := nums.clone()
```

可以使用 `.filter()` 和
`.map()` 方法高效地过滤和映射数组：

```v
nums := [1, 2, 3, 4, 5, 6]
even := nums.filter(it % 2 == 0)
println(even) // [2, 4, 6]
// filter 可以接受匿名函数
even_fn := nums.filter(fn (x int) bool {
	return x % 2 == 0
})
println(even_fn)
```

```v
words := ['hello', 'world']
upper := words.map(it.to_upper())
println(upper) // ['HELLO', 'WORLD']
// map 也可以接受匿名函数
upper_fn := words.map(fn (w string) string {
	return w.to_upper()
})
println(upper_fn) // ['HELLO', 'WORLD']
```

`it` 是一个内置变量，指代当前在 filter/map 方法中
正在处理的元素。

此外，`.any()` 和 `.all()` 可以方便地测试
满足条件的元素。

```v
nums := [1, 2, 3]
println(nums.any(it == 2)) // true
println(nums.all(it >= 2)) // false
```

数组还有更多内置方法：

* `a.repeat(n)` 将数组元素连接 `n` 次
* `a.insert(i, val)` 在索引 `i` 处插入新元素 `val` 并
  将所有后续元素向右移动
* `a.insert(i, [3, 4, 5])` 插入多个元素
* `a.prepend(val)` 在开头插入值，等同于 `a.insert(0, val)`
* `a.prepend(arr)` 在开头插入数组 `arr` 的元素
* `a.trim(new_len)` 截断长度（如果 `new_length < a.len`，否则不执行任何操作）
* `a.clear()` 清空数组而不更改 `cap`（等同于 `a.trim(0)`）
* `a.delete_many(start, size)` 从索引 `start` 开始删除 `size` 个连续元素
  &ndash; 触发重新分配
* `a.delete(index)` 等同于 `a.delete_many(index, 1)`
* `a.delete_last()` 删除最后一个元素
* `a.first()` 等同于 `a[0]`
* `a.last()` 等同于 `a[a.len - 1]`
* `a.pop()` 删除最后一个元素并返回它
* `a.reverse()` 创建一个新数组，其中包含 `a` 的元素，顺序相反
* `a.reverse_in_place()` 反转 `a` 中元素的顺序
* `a.join(joiner)` 使用 `joiner` 字符串作为分隔符将字符串数组连接成一个字符串

请参阅 [array](https://modules.vlang.io/index.html#array) 的所有方法

另请参阅 [vlib/arrays](https://modules.vlang.io/arrays.html)。

##### 排序数组

对所有类型的数组进行排序都非常简单直观。在提供自定义排序条件时
使用特殊变量 `a` 和 `b`。

```v
mut numbers := [1, 3, 2]
numbers.sort() // 1, 2, 3
numbers.sort(a > b) // 3, 2, 1
```

```v
struct User {
	age  int
	name string
}

mut users := [User{21, 'Bob'}, User{20, 'Zarkon'}, User{25, 'Alice'}]
users.sort(a.age < b.age) // 按 User.age int 字段排序
users.sort(a.name > b.name) // 按 User.name string 字段反向排序
```

V 还通过 `sort_with_compare` 数组方法支持自定义排序。
它需要一个比较函数来定义排序顺序。
对于按自定义排序规则同时对多个字段进行排序很有用。
下面的代码按 `name` 升序和 `age` 降序对数组进行排序。

```v
struct User {
	age  int
	name string
}

mut users := [User{21, 'Bob'}, User{65, 'Bob'}, User{25, 'Alice'}]

custom_sort_fn := fn (a &User, b &User) int {
	// return -1 when a comes before b
	// return 0, when both are in same order
	// return 1 when b comes before a
	if a.name == b.name {
		if a.age < b.age {
			return 1
		}
		if a.age > b.age {
			return -1
		}
		return 0
	}
	if a.name < b.name {
		return -1
	} else if a.name > b.name {
		return 1
	}
	return 0
}
users.sort_with_compare(custom_sort_fn)
```

#### 数组切片

切片是父数组的一部分。最初它引用由 `..` 操作符分隔的
两个索引之间的元素。右侧索引必须
大于或等于左侧索引。

如果缺少右侧索引，则假定为数组长度。如果
缺少左侧索引，则假定为 0。

```v
nums := [0, 10, 20, 30, 40]
println(nums[1..4]) // [10, 20, 30]
println(nums[..4]) // [0, 10, 20, 30]
println(nums[1..]) // [10, 20, 30, 40]
```

在 V 中，切片本身就是数组（它们不是不同的类型）。因此
可以对它们执行所有数组操作。例如，可以将它们推送到
相同类型的数组：

```v
array_1 := [3, 5, 4, 7, 6]
mut array_2 := [0, 1]
array_2 << array_1[..3]
println(array_2) // `[0, 1, 3, 5, 4]`
```

切片总是以尽可能小的容量 `cap == len` 创建（请参阅
[上面的 `cap`](#array-initialization)），无论父数组的容量或长度
是多少。因此，当大小增加时，它会立即重新分配并复制到另一个
内存位置，从而独立于
父数组（*增长时复制*）。特别是向切片推送元素
不会改变父数组：

```v
mut a := [0, 1, 2, 3, 4, 5]

// 创建一个切片，最初重用与父数组*相同的内存*，而不进行新的分配：
mut b := unsafe { a[2..4] } // `b` 的内容重用 `a` 的内容使用的内存。

b[0] = 7 // 注意 `b[0]` 和 `a[2]` 指向内存中的*同一个元素*。
println(a) // `[0, 1, 7, 3, 4, 5]` - 上面更改 `b[0]` 也更改了 `a[2]`。

// `b` 的内容将被重新分配，以便为 `9` 元素腾出空间：
b << 9
// `b` 的内容现在已重新分配，并且完全独立于 `a` 的内容。

println(a) // `[0, 1, 7, 3, 4, 5]` - 没有变化，因为在追加之前，`b` 的内容被重新分配到一个更大的块。

println(b) // `[7, 3, 9]` - 重新分配并追加 `9` 后，`b` 的内容。
```

追加到父数组可能会也可能不会使其独立于其子切片。
行为取决于*父数组的容量*并且是可预测的：

```v
mut a := []int{len: 5, cap: 6, init: 2}
mut b := unsafe { a[1..4] } // `b` 的内容复用了 `a` 的一部分内存

a << 3
// 这里仍不会重新分配 `a`，因为 `a.len` 仍在 `a.cap` 范围内
b[2] = 13 // 通过切片 `b` 修改了 `a[3]`

a << 4
// 此时 `a` 的内容已被重新分配，且与 `b` 独立（`len` 超过了 `cap`）
b[1] = 3 // `a` 不会再变化

println(a) // `[2, 2, 2, 13, 2, 3, 4]`
println(b) // `[2, 3, 13]`
```

如果您*确实*想要立即拥有一个独立的副本，可以在切片上调用 .clone()：

```v
mut a := [0, 1, 2, 3, 4, 5]
mut b := a[2..4].clone()
b[0] = 7 // Note: `b[0]` is NOT referring to `a[2]`, as it would have been, without the `.clone()`
println(a) // [0, 1, 2, 3, 4, 5]
println(b) // [7, 3]
```

##### 负索引切片

V 支持带负索引的数组和字符串切片。
负索引从数组末尾向开头计数，
例如 `-3` 等于 `array.len - 3`。
负索引切片与普通切片有不同的语法，即您需要
在数组名和方括号之间添加一个 `gate`：`a#[..-3]`。
`gate` 指定这是一种不同类型的切片，并记住
结果被"锁定"在数组内。
返回的切片始终是有效数组，尽管它可能为空：

```v
a := [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]
println(a#[-3..]) // [7, 8, 9]
println(a#[-20..]) // [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]
println(a#[-20..-8]) // [0, 1]
println(a#[..-3]) // [0, 1, 2, 3, 4, 5, 6]

// 空数组
println(a#[-20..-10]) // []
println(a#[20..10]) // []
println(a#[20..30]) // []
```

#### 数组方法链

您可以链接数组方法的调用，如 `.filter()` 和 `.map()`，并使用
`it` 内置变量来实现经典的 `map/filter` 函数式范式：

```v
// 使用 filter、map 和负索引数组切片
files := ['pippo.jpg', '01.bmp', '_v.txt', 'img_02.jpg', 'img_01.JPG']
filtered := files.filter(it#[-4..].to_lower() == '.jpg').map(it.to_upper())
// ['PIPPO.JPG', 'IMG_02.JPG', 'IMG_01.JPG']
```

### 固定大小数组

V 还支持固定大小的数组。与普通数组不同，它们的
长度是常量。您不能向它们追加元素，也不能缩小它们。
您只能就地修改它们的元素。

但是，访问固定大小数组的元素更高效，
它们比普通数组需要更少的内存，并且与普通数组不同，
它们的数据在栈上，所以如果您
不想额外的堆分配，您可能想将它们用作缓冲区。

大多数方法定义为在普通数组上工作，而不是在固定大小数组上。
您可以通过切片将固定大小数组转换为普通数组：

```v
mut fnums := [3]int{} // fnums 是一个包含 3 个元素的固定大小数组。
fnums[0] = 1
fnums[1] = 10
fnums[2] = 100
println(fnums) // => [1, 10, 100]
println(typeof(fnums).name) // => [3]int

fnums2 := [1, 10, 100]! // 执行相同操作的简短初始化语法（语法可能会更改）

anums := fnums[..] // 与 `anums := fnums[0..fnums.len]` 相同
println(anums) // => [1, 10, 100]
println(typeof(anums).name) // => []int
```

请注意，切片将导致固定大小数组的数据被复制到
新创建的普通数组。

### 映射（Maps）

```v
mut m := map[string]int{} // 一个键为 `string` 类型、值为 `int` 类型的映射
m['one'] = 1
m['two'] = 2
println(m['one']) // "1"
println(m['bad_key']) // "0"
println('bad_key' in m) // 使用 `in` 检测是否存在这样的键
println(m.keys()) // ['one', 'two']
m.delete('two')
```

映射的键可以是 string、rune、integer、float 或 voidptr 类型。

可以使用此简短语法初始化整个映射：

```v
numbers := {
	'one': 1
	'two': 2
}
println(numbers)
```

如果找不到键，默认返回零值：

```v
sm := {
	'abc': 'xyz'
}
val := sm['bad_key']
println(val) // ''
```

```v
intm := {
	1: 1234
	2: 5678
}
s := intm[3]
println(s) // 0
```

也可以使用 `or {}` 块来处理缺失的键：

```v
mm := map[string]int{}
val := mm['bad_key'] or { panic('key not found') }
```

你也可以在一次操作中同时检查键是否存在，并在存在时获取其值：

```v
m := {
	'abc': 'def'
}
if v := m['abc'] {
println('该键对应的 map 值是: ${v}')
}
```

同样的 option 检查也适用于数组：

```v
arr := [1, 2, 3]
large_index := 999
val := arr[large_index] or { panic('out of bounds') }
println(val)
// 如果你想 *向上抛出* 访问错误，也可以这样写：
val2 := arr[333]!
println(val2)
```

V 也支持嵌套 map：

```v
mut m := map[string]map[string]int{}
m['greet'] = {
	'Hello': 1
}
m['place'] = {
	'world': 2
}
m['code']['orange'] = 123
print(m)
```

映射按插入顺序排序，就像 Python 中的字典一样。顺序是
一个保证的语言特性。这在未来可能会改变。

查看
[map](https://modules.vlang.io/builtin.html#map)
和
[maps](https://modules.vlang.io/maps.html)
的所有方法。

### 映射更新语法

与结构体一样，V 允许您在另一个映射的基础上初始化一个映射并应用更新：

```v
const base_map = {
	'a': 4
	'b': 5
}

foo := {
	...base_map
	'b': 88
	'c': 99
}

println(foo) // {'a': 4, 'b': 88, 'c': 99}
```

这在功能上等同于克隆映射并更新它，只是您
不必声明可变变量：

```v failcompile
// same as above (except mutable)
mut foo := base_map.clone()
foo['b'] = 88
foo['c'] = 99
```

## 模块导入

有关创建模块的信息，请参阅[模块](#modules)。

可以使用 `import` 关键字导入模块：

```v
import os

fn main() {
	// 从标准输入读取文本
	name := os.input('Enter your name: ')
	println('Hello, ${name}!')
}
```

此程序可以使用 `os` 模块中的任何公共定义，例如
`input` 函数。请参阅[标准库](https://modules.vlang.io/)
文档以获取常见模块及其公共符号的列表。

默认情况下，每次调用外部函数时都必须指定模块前缀。
这起初可能看起来冗长，但它使代码更具可读性
且更容易理解 - 总是清楚哪个函数来自
哪个模块。这在大型代码库中特别有用。

不允许循环模块导入，就像在 Go 中一样。

### 选择性导入

您也可以直接从模块导入特定函数和类型：

```v
import os { input }

fn main() {
	// 从标准输入读取文本
	name := input('Enter your name: ')
	println('Hello, ${name}!')
}
```

> [!NOTE]
> 这也会导入模块。此外，这不适用于
> 常量 - 它们必须始终带前缀。

您可以一次导入多个特定符号：

```v
import os { input, user_os }

name := input('Enter your name: ')
println('Name: ${name}')
current_os := user_os()
println('Your OS is ${current_os}.')
```
### 模块层次结构

> [!NOTE]
> 当 .v 文件不在项目根目录中时，此部分有效。

.v 文件中的模块名称必须与其目录名称匹配。

.v 文件 `./abc/source.v` 必须以 `module abc` 开头。此目录中的所有 .v 文件
都属于同一模块 `abc`。它们也应该以 `module abc` 开头。

如果您有 `abc/def/`，并且两个文件夹中都有 .v 文件，您可以 `import abc`，但您也需要
`import abc.def` 才能访问子文件夹中的符号。它们是独立的。

在 `module name` 语句中，名称从不重复目录的层次结构，而只是其目录。
所以在 `abc/def/source.v` 中，第一行将是 `module def`，而不是 `module abc.def`。

`import module_name` 语句必须遵循文件层次结构，您不能 `import def`，只能
`import abc.def`

引用模块符号（如函数或常量）时，只需要模块名作为前缀：

```v ignore
module def

// func 是一个虚拟示例函数。
pub fn func() {
	println('func')
}
```

可以这样调用：

```v ignore
module main

import def

fn main() {
	def.func()
}
```

位于 `abc/def/source.v` 中的函数使用 `def.func()` 调用，而不是 `abc.def.func()`

这总是意味着*单个前缀*，无论子模块深度如何。此行为扁平化
模块/子模块层次结构。如果您在不同目录中有两个同名模块，
那么您应该使用模块导入别名（见下文）。


### 模块导入别名

任何已导入的模块名都可以用 `as` 关键字起别名：

> [!NOTE]
> 若未创建 `mymod/sha256/somename.v`，此示例将无法编译
> （子模块名由其路径决定，而不是由其中 .v 文件的文件名决定）。

```v failcompile
import crypto.sha256
import mymod.sha256 as mysha256

fn main() {
	v_hash := sha256.sum('hi'.bytes()).hex()
	my_hash := mysha256.sum('hi'.bytes()).hex()
	assert my_hash == v_hash
}
```

您不能为导入的函数或类型创建别名。
但是，您*可以*重新声明一个类型。

```v
import time
import math

type MyTime = time.Time

fn (mut t MyTime) century() int {
	return int(1.0 + math.trunc(f64(t.year) * 0.009999794661191))
}

fn main() {
	mut my_time := MyTime{
		year:  2020
		month: 12
		day:   25
	}
	println(time.new(my_time).utc_string())
	println('Century: ${my_time.century()}')
}
```

## 语句和表达式

### If（条件语句）

```v
a := 10
b := 20
if a < b {
	println('${a} < ${b}')
} else if a > b {
	println('${a} > ${b}')
} else {
	println('${a} == ${b}')
}
```

`if` 语句非常直接，与大多数其他语言类似。
与其他类 C 语言不同，
条件周围没有括号，并且总是需要大括号。

#### `If` 表达式
与 C 不同，V 没有三元操作符，它允许您执行：`x = c ? 1 : 2`。
相反，它有一个稍微冗长但更清晰易读的功能，可以将 `if` 用作
表达式。假设 `c` 是布尔条件，上述三元构造在 V 中的直接翻译
将是：`x = if c { 1 } else { 2 }`。

这是另一个示例：
```v
num := 777
s := if num % 2 == 0 { 'even' } else { 'odd' }
println(s)
// "odd"
```

您可以在 `if` 表达式的每个分支中使用多个语句，后跟一个最终
值，当它采用该分支时，该值将成为整个 `if` 表达式的值：
```v
n := arguments().len
x := if n > 2 {
	dump(arguments())
	42
} else {
	println('something else')
	100
}
dump(x)
```

#### `If` 解包
在任何可以使用 `or {}` 的地方，您也可以使用"if 解包"。这会将表达式的解包值
绑定到变量，当该表达式既不是 none 也不是错误时。

```v
m := {
	'foo': 'bar'
}

// 处理缺失的键
if v := m['foo'] {
	println(v) // bar
} else {
	println('not found')
}
```

```v
fn res() !int {
	return 42
}

// 返回结果类型的函数
if v := res() {
	println(v)
}
```

```v
struct User {
	name string
}

arr := [User{'John'}]

// 使用变量赋值的 if 解包
u_name := if v := arr[0] {
	v.name
} else {
	'Unnamed'
}
println(u_name) // John
```

#### 类型检查和转换

你可以使用 `is` 以及它的否定形式 `!is` 检查和判断 sum type 的当前具体类型。

可以在 `if` 中这么做：

```v cgen
struct Abc {
	val string
}

struct Xyz {
	foo string
}

type Alphabet = Abc | Xyz

x := Alphabet(Abc{'test'}) // sum type
if x is Abc {
	// 这里 x 会被自动转换为 Abc，可直接使用
	println(x)
}
if x !is Abc {
	println('Not Abc')
}
```

或者使用 `match`：

```v oksyntax
match x {
	Abc {
		// 这里 x 会被自动转换为 Abc，可直接使用
		println(x)
	}
	Xyz {
		// 这里 x 会被自动转换为 Xyz，可直接使用
		println(x)
	}
}
```

这同样适用于结构体字段：

```v
struct MyStruct {
	x int
}

struct MyStruct2 {
	y string
}

type MySumType = MyStruct | MyStruct2

struct Abc {
	bar MySumType
}

x := Abc{
	bar: MyStruct{123} // MyStruct 会被自动转换为 MySumType
}
if x.bar is MyStruct {
	// x.bar 会被自动转换
	println(x.bar)
} else if x.bar is MyStruct2 {
	new_var := x.bar as MyStruct2
	// ... 或者使用 `as` 手动创建类型转换别名：
	println(new_var)
}
match x.bar {
	MyStruct {
		// x.bar 会被自动转换
		println(x.bar)
	}
	else {}
}
```

可变变量可以改变，进行类型转换将是不安全的。
但是，有时尽管可变，进行类型转换仍然很有用。
在这种情况下，开发人员必须用 `mut` 关键字标记表达式，
以告诉编译器他们知道自己在做什么。

It works like this:

```v oksyntax
mut x := MySumType(MyStruct{123})
if mut x is MyStruct {
	// x is cast to MyStruct even if it's mutable
	// without the mut keyword that wouldn't work
	println(x)
}
// same with match
match mut x {
	MyStruct {
		// x is cast to MyStruct even if it's mutable
		// without the mut keyword that wouldn't work
		println(x)
	}
}
```

### Match（匹配）

```v
os := 'windows'
print('V is running on ')
match os {
	'darwin' { println('macOS.') }
	'linux' { println('Linux.') }
	else { println(os) }
}
```

match 语句是编写一系列 `if - else` 语句的简短方式。
当找到匹配分支时，将运行后续的语句块。
当没有其他分支匹配时，将运行 else 分支。

```v
number := 2
s := match number {
	1 { 'one' }
	2 { 'two' }
	else { 'many' }
}
```

A match statement can also to be used as an `if - else if - else` alternative:

```v
match true {
	2 > 4 { println('if') }
	3 == 4 { println('else if') }
	2 == 2 { println('else if2') }
	else { println('else') }
}
// 'else if2' should be printed
```

or as an `unless` alternative: [unless Ruby](https://www.tutorialspoint.com/ruby/ruby_if_else.htm)

```v
match false {
	2 > 4 { println('if') }
	3 == 4 { println('else if') }
	2 == 2 { println('else if2') }
	else { println('else') }
}
// 'if' should be printed
```

A match expression returns the value of the final expression from the matching branch.

```v
enum Color {
	red
	blue
	green
}

fn is_red_or_blue(c Color) bool {
	return match c {
		.red, .blue { true } // comma can be used to test multiple values
		.green { false }
	}
}
```

A match statement can also be used to branch on the variants of an `enum`
by using the shorthand `.variant_here` syntax. An `else` branch is not allowed
when all the branches are exhaustive.

```v
c := `v`
typ := match c {
	`0`...`9` { 'digit' }
	`A`...`Z` { 'uppercase' }
	`a`...`z` { 'lowercase' }
	else { 'other' }
}
println(typ)
// 'lowercase'
```

match 语句也可以匹配 `sumtype` 的变体类型。请注意
在这种情况下，匹配是详尽的，因为所有变体类型都
被显式提及，所以不需要 `else{}` 分支。

```v nofmt
struct Dog {}
struct Cat {}
struct Veasel {}
type Animal = Dog | Cat | Veasel
a := Animal(Veasel{})
match a {
	Dog { println('Bay') }
	Cat { println('Meow') }
	Veasel { println('Vrrrrr-eeee') } // 参见：https://www.youtube.com/watch?v=qTJEDyj2N0Q
}
```

您也可以使用范围作为 `match` 模式。如果值落在分支的范围内，
将执行该分支。

请注意，范围使用 `...`（三个点）而不是 `..`（两个点）。这是
因为范围*包含*最后一个元素，而不是排他的
（如 `..` 范围）。在 match 分支中使用 `..` 将抛出错误。

```v
const start = 1

const end = 10

c := 2
num := match c {
	start...end {
		1000
	}
	else {
		0
	}
}
println(num)
// 1000
```

常量也可以用于范围分支表达式。

> [!NOTE]
> `match` 作为表达式不能在 `for` 循环和 `if` 语句中使用。

### In 操作符

`in` 允许检查数组或映射是否包含元素。
要执行相反操作，请使用 `!in`。

```v
nums := [1, 2, 3]
println(1 in nums) // true
println(4 !in nums) // true
```

> [!NOTE]
> `in` 检查映射是否包含键，而不是值。

```v
m := {
	'one': 1
	'two': 2
}

println('one' in m) // true
println('three' !in m) // true
```

它对于编写更清晰、更紧凑的布尔表达式也很有用：

```v
enum Token {
	plus
	minus
	div
	mult
}

struct Parser {
	token Token
}

parser := Parser{}
if parser.token == .plus || parser.token == .minus || parser.token == .div || parser.token == .mult {
	// ...
}
if parser.token in [.plus, .minus, .div, .mult] {
	// ...
}
```

V 优化了此类表达式，
因此上面的两个 `if` 语句产生相同的机器代码，并且不会创建数组。

### For 循环

V 只有一个循环关键字：`for`，有几种形式。

#### `for`/`in`

这是最常见的形式。您可以将其与数组、映射或
数字范围一起使用。

##### 数组 `for`

```v
numbers := [1, 2, 3, 4, 5]
for num in numbers {
	println(num)
}
names := ['Sam', 'Peter']
for i, name in names {
	println('${i}) ${name}')
	// 输出：0) Sam
	//         1) Peter
}
```

`for value in arr` 形式用于遍历数组的元素。
如果需要索引，可以使用替代形式 `for index, value in arr`。

请注意，值是只读的。
如果需要在循环时修改数组，需要将元素声明为可变：

```v
mut numbers := [0, 1, 2]
for mut num in numbers {
	num++
}
println(numbers) // [1, 2, 3]
```

默认情况下，数组元素按值获取，如果您需要按
引用获取元素，请在要迭代的数组上使用 `&`：

```v
struct User {
	name string
}

users := [User{
	name: 'someuserwow99'
}, User{
	name: 'visgod'
}]
// 注意 `&users`，这是如何接收数组元素引用的
for user in &users {
	// 对 `user` 进行一些操作
}
```

这同样适用于映射。

当标识符只是单个下划线时，它会被忽略。

##### 自定义迭代器

实现返回 `Option` 的 `next` 方法的类型可以使用
`for` 循环进行迭代。

```v
struct SquareIterator {
	arr []int
mut:
	idx int
}

fn (mut iter SquareIterator) next() ?int {
	if iter.idx >= iter.arr.len {
		return none
	}
	defer {
		iter.idx++
	}
	return iter.arr[iter.idx] * iter.arr[iter.idx]
}

nums := [1, 2, 3, 4, 5]
iter := SquareIterator{
	arr: nums
}
for squared in iter {
	println(squared)
}
```

上面的代码打印：

```
1
4
9
16
25
```

##### 映射 `for`

```v
m := {
	'one': 1
	'two': 2
}
for key, value in m {
	println('${key} -> ${value}')
	// 输出：one -> 1
	//         two -> 2
}
```

可以通过使用单个下划线作为标识符来忽略键或值。

```v
m := {
	'one': 1
	'two': 2
}
// 遍历键
for key, _ in m {
	println(key)
	// 输出：one
	//         two
}
// 遍历值
for _, value in m {
	println(value)
	// 输出：1
	//         2
}
```

##### 范围 `for`

```v
// 打印 '01234'
for i in 0 .. 5 {
	print(i)
}
```

`low..high` 表示一个*排他*范围，它表示从 `low` 到*但不包括* `high` 的所有值。

> [!NOTE]
> 这种排他范围表示法和基于零的索引遵循
逻辑一致性和错误减少的原则。正如 Edsger W. Dijkstra 在
"为什么编号应该从零开始"
([EWD831](https://www.cs.utexas.edu/users/EWD/transcriptions/EWD08xx/EWD831.html)) 中概述的那样，
基于零的索引使索引与序列中的前导元素对齐，
简化处理并最小化错误，特别是对于相邻子序列。
这种逻辑和高效的方法塑造了我们的语言设计，强调清晰性
并减少编程中的混淆。

#### 条件 `for`

```v
mut sum := 0
mut i := 0
for i <= 100 {
	sum += i
	i++
}
println(sum) // "5050"
```

这种循环形式类似于其他语言中的 `while` 循环。
一旦布尔条件评估为 false，循环将停止迭代。
同样，条件周围没有括号，并且总是需要大括号。

#### 裸 `for`

```v
mut num := 0
for {
	num += 2
	if num >= 10 {
		break
	}
}
println(num) // "10"
```

可以省略条件，导致无限循环。

#### C 风格 `for`

```v
for i := 0; i < 10; i += 2 {
	// 不打印 6
	if i == 6 {
		continue
	}
	println(i)
}
```

最后，还有传统的 C 风格 `for` 循环。它比 `while` 形式更安全
因为使用后者很容易忘记更新计数器并陷入
无限循环。

这里 `i` 不需要用 `mut` 声明，因为它根据定义总是可变的。

#### 带标签的 break 和 continue

`break` 和 `continue` 默认控制最内层的 `for` 循环。
您也可以使用 `break` 和 `continue` 后跟标签名称来引用外层的 `for`
循环：

```v
outer: for i := 4; true; i++ {
	println(i)
	for {
		if i < 7 {
			continue outer
		} else {
			break outer
		}
	}
}
```

标签必须紧接在外层循环之前。
上面的代码打印：

```
4
5
6
7
```

### Defer（延迟执行）

`defer {}` 语句将语句块的执行延迟到
defer 的周围作用域结束。这是一个方便的功能，
允许您将相关操作（获取资源访问权限
并在完成后清理/释放它）紧密地组合在一起，而不是
将它们分散在多个可能非常遥远的代码行中。

```v
import os

fn read_log() ! {
	mut ok := false
	mut f := os.open('log.txt')!
	defer { f.close() }
	// ...
	if !ok {
		// ...
		// defer 语句将在这里被调用，文件将被关闭
		return
	}
	// ...
	// defer 语句也会在这里被调用，文件将被关闭
}
```

如果函数返回值，`defer` 块将在返回
表达式评估*之后*执行：

```v
import os

enum State {
	normal
	write_log
	return_error
}

// 写入日志文件并返回写入的字节数

fn write_log(s State) !int {
	mut f := os.create('log.txt')!
	defer {
		f.close()
	}
	if s == .write_log {
		// `f.close()` 将在 `f.write()` 已经
		// 执行之后被调用，但在 `write_log()` 最终返回
		// 写入的字节数给 `main()` 之前
		return f.writeln('This is a log file')
	} else if s == .return_error {
		// 文件将在 `error()` 函数
		// 返回之后被关闭 - 因此错误消息仍会报告
		// 它是打开的
		return error('nothing written; file open: ${f.is_opened}')
	}
	// 文件也会在这里被关闭
	return 0
}

fn main() {
	n := write_log(.return_error) or {
		println('Error: ${err}')
		0
	}
	println('${n} bytes written')
}
```

要在 `defer` 块内访问函数的结果，可以使用 `$res()` 表达式。
`$res()` 仅在返回单个值时使用，而在多返回值时 `$res(idx)`
是参数化的。

```v ignore
fn (mut app App) auth_middleware() bool {
	defer {
		if !$res() {
			app.response.status_code = 401
			app.response.body = 'Unauthorized'
		}
	}
	header := app.get_header('Authorization')
	if header == '' {
		return false
	}
	return true
}

fn (mut app App) auth_with_user_middleware() (bool, string) {
	defer {
		if !$res(0) {
			app.response.status_code = 401
			app.response.body = 'Unauthorized'
		} else {
			app.user = $res(1)
		}
	}
	header := app.get_header('Authorization')
	if header == '' {
		return false, ''
	}
	return true, 'TestUser'
}
```

#### defer in loop scopes:
Defer 也可以在循环内使用，延迟语句将在每次
迭代时执行一次。您还可以在同一作用域中有多个 defer 语句，在这种情况下，它们
将按照在源代码中出现的相反顺序执行：
```v
fn main() {
	defer { println('Program finish.') }
	println('Loop start.')
	for i in 1 .. 4 {
		defer { println('Deferred execution for ${i}. Defer 1.') }
		defer { println('Deferred execution for ${i}. Defer 2.') }
		defer { println('Deferred execution for ${i}. Defer 3.') }
		println('Loop iteration: ${i}')
	}
	println('Loop done.')
}
```

该示例将打印以下内容：
```txt
循环开始。
循环迭代：1
延迟执行 1. Defer 3。
延迟执行 1. Defer 2。
延迟执行 1. Defer 1。
循环迭代：2
延迟执行 2. Defer 3。
延迟执行 2. Defer 2。
延迟执行 2. Defer 1。
循环迭代：3
延迟执行 3. Defer 3。
延迟执行 3. Defer 2。
延迟执行 3. Defer 1。
循环完成。
程序结束。
```

#### defer(fn) {}

请注意，在大多数上面的示例中，`defer{}` 语句直接在
函数作用域内，因此它在函数本身返回时执行。有时，您
需要延迟一个语句在函数结束时执行（如上面所示），即使
您在内部作用域内（在 `if` 或 `for` 的深层）。

对于这些更罕见的情况，您可以使用：`defer(fn) {}` 而不是仅仅 `defer {}`。

### Goto

V 允许使用 `goto` 无条件跳转到标签。标签名称必须包含在
与 `goto` 语句相同的函数中。程序可以 `goto` 到当前作用域之外
或更深的标签。`goto` 允许跳过变量初始化或
跳回到访问已释放内存的代码，因此它需要
`unsafe`。

```v ignore
if x {
	// ...
	if y {
		unsafe {
			goto my_label
		}
	}
	// ...
}
my_label:
```

应该避免使用 `goto`，特别是当可以使用 `for` 代替时。
[带标签的 break/continue](#labelled-break--continue) 可用于跳出
嵌套循环，并且这些不会冒违反内存安全的风险。

## 结构体（Structs）

```v
struct Point {
	x int
	y int
}

mut p := Point{
	x: 10
	y: 20
}
println(p.x) // 使用点号访问结构体字段
// 替代字面量语法
p = Point{10, 20}
assert p.x == 10
```

结构体字段可以重用保留关键字：

```v
struct Employee {
	type string
	name string
}

employee := Employee{
	type: 'FTE'
	name: 'John Doe'
}
println(employee.type)
```

### 堆结构体

结构体在栈上分配。要在堆上分配结构体
并获取它的[引用](#references)，请使用 `&` 前缀：

```v
struct Point {
	x int
	y int
}

p := &Point{10, 10}
// 引用使用相同的语法访问字段
println(p.x)
```

`p` 的类型是 `&Point`。它是 `Point` 的[引用](#references)。
引用类似于 Go 指针和 C++ 引用。

```v
struct Foo {
mut:
	x int
}

fa := Foo{1}
mut a := fa
a.x = 2
assert fa.x == 1
assert a.x == 2

// fb := Foo{ 1 }
// mut b := &fb  // 错误：`fb` 是不可变的，不能有可变引用
// b.x = 2

mut fc := Foo{1}
mut c := &fc
c.x = 2
assert fc.x == 2
assert c.x == 2
println(fc) // Foo{ x: 2 }
println(c) // &Foo{ x: 2 } // 注意 `&` 前缀。
```

另请参阅[栈和堆](#stack-and-heap)

### 默认字段值

```v
struct Foo {
	n   int    // n 默认为 0
	s   string // s 默认为 ''
	a   []int  // a 默认为 `[]int{}`
	pos int = -1 // 自定义默认值
}
```

在创建结构体时，默认情况下所有结构体字段都被清零。
数组和映射字段被分配。
对于引用值，请参阅[此处](#structs-with-reference-fields)。

也可以定义自定义默认值。

### 必需字段

```v
struct Foo {
	n int @[required]
}
```

您可以使用 `[required]` [属性](#attributes)标记结构体字段，以告诉 V
在创建该结构体的实例时必须初始化该字段。

此示例不会编译，因为字段 `n` 未显式初始化：

```v failcompile
_ = Foo{}
```

<a id='short-struct-initialization-syntax'></a>

### 简短结构体字面量语法

```v
struct Point {
	x int
	y int
}

mut p := Point{
	x: 10
	y: 20
}
p = Point{
	x: 30
	y: 4
}
assert p.y == 4
//
// 数组：第一个元素定义数组类型
points := [Point{10, 20}, Point{20, 30}, Point{40, 50}]
println(points) // [Point{x: 10, y: 20}, Point{x: 20, y: 30}, Point{x: 40,y: 50}]
```

省略结构体名称也适用于返回结构体字面量或将其
作为函数参数传递。

### 结构体更新语法

V 使返回对象的修改版本变得容易：

```v
struct User {
	name          string
	age           int
	is_registered bool
}

fn register(u User) User {
	return User{
		...u
		is_registered: true
	}
}

mut user := User{
	name: 'abc'
	age:  23
}
user = register(user)
println(user)
```

### 尾随结构体字面量参数

V 没有默认函数参数或命名参数，为此可以使用尾随结构体
字面量语法代替：

```v
@[params]
struct ButtonConfig {
	text        string
	is_disabled bool
	width       int = 70
	height      int = 20
}

struct Button {
	text   string
	width  int
	height int
}

fn new_button(c ButtonConfig) &Button {
	return &Button{
		width:  c.width
		height: c.height
		text:   c.text
	}
}

button := new_button(text: 'Click me', width: 100)
// height 未设置，所以它是默认值
assert button.height == 20
```

如您所见，可以省略结构体名称和大括号，而不是：

```v oksyntax nofmt
new_button(ButtonConfig{text:'Click me', width:100})
```

这仅适用于最后一个参数是结构体的函数。

> [!NOTE]
> 请注意，`[params]` 标签用于告诉 V，尾随结构体参数
> 可以*完全*省略，因此您可以编写 `button := new_button()`。
> 没有它，您必须指定*至少*一个字段名称，即使它
> 有默认值，否则当您调用不带参数的函数时，编译器将产生此错误消息：
> `error: expected 1 arguments, but got 0`。

### 访问修饰符

结构体字段默认是私有且不可变的（也使结构体不可变）。
可以使用 `pub` 和 `mut` 更改它们的访问修饰符。
总共有 5 种可能的选项：

```v
struct Foo {
	a int // 私有不可变（默认）
mut:
	b int // 私有可变
	c int // （您可以列出具有相同访问修饰符的多个字段）
pub:
	d int // 公共不可变（只读）
pub mut:
	e int // 公共，但仅在父模块中可变
__global:
	// （不推荐使用，这就是为什么 'global' 关键字以 __ 开头）
	f int // 在父模块内部和外部都是公共且可变的
}
```

私有字段仅在同一个[模块](#modules)内可用，任何
从另一个模块直接访问它们的尝试都会在编译时导致错误。
公共不可变字段在任何地方都是只读的。

### 匿名结构体

V 支持匿名结构体：不需要单独声明
结构体名称的结构体。

```v
struct Book {
	author struct {
		name string
		age  int
	}

	title string
}

book := Book{
	author: struct {
		name: 'Samantha Black'
		age:  24
	}
}
assert book.author.name == 'Samantha Black'
assert book.author.age == 24
```

### 静态类型方法

V 现在支持静态类型方法，如 `User.new()`。这些通过
`fn [Type name].[function name]` 在结构体上定义，并允许组织与结构体相关的所有函数：

```v oksyntax
struct User {}

fn User.new() User {
	return User{}
}

user := User.new()
```

这是工厂函数（如 `fn new_user() User {}`）的替代方案，应该使用
它。

> [!NOTE]
> 请注意，这些不是构造函数，而是简单函数。V 没有构造函数或
> 类。

### `[noinit]` 结构体

V 支持 `[noinit]` 结构体，这些结构体不能在定义它们的模块
外部初始化。它们要么用于内部，要么可以通过
_工厂函数_在外部使用。

例如，考虑 `sample` 目录中的以下源代码：

```v oksyntax
module sample

@[noinit]
pub struct Information {
pub:
	data string
}

pub fn new_information(data string) !Information {
	if data.len == 0 || data.len > 100 {
		return error('data must be between 1 and 100 characters')
	}
	return Information{
		data: data
	}
}
```

请注意，`new_information` 是一个_工厂_函数。现在当我们要在模块外部使用此结构体时：

```v okfmt
import sample

fn main() {
	// 当存在 [noinit] 属性时，这不起作用：
	// info := sample.Information{
	// 	data: 'Sample information.'
	// }

	// 改用这个：
	info := sample.new_information('Sample information.')!

	println(info)
}
```

### 方法

```v
struct User {
	age int
}

fn (u User) can_register() bool {
	return u.age > 16
}

user := User{
	age: 10
}
println(user.can_register()) // "false"
user2 := User{
	age: 20
}
println(user2.can_register()) // "true"
```

V 没有类，但您可以在类型上定义方法。
方法是具有特殊接收器参数的函数。
接收器出现在 `fn` 关键字和方法名之间的自己的参数列表中。
方法必须与接收器类型在同一个模块中。

在此示例中，`can_register` 方法有一个名为 `u` 的 `User` 类型接收器。
约定是不使用像 `self` 或 `this` 这样的接收器名称，
而是使用短的，最好是一个字母长的名称。

### 嵌入结构体

V 支持嵌入结构体。

```v
struct Size {
mut:
	width  int
	height int
}

fn (s &Size) area() int {
	return s.width * s.height
}

struct Button {
	Size
	title string
}
```

通过嵌入，结构体 `Button` 将自动获得结构体 `Size` 的所有字段和方法，
这允许您执行：

```v oksyntax
mut button := Button{
	title:  'Click me'
	height: 2
}

button.width = 3
assert button.area() == 6
assert button.Size.area() == 6
print(button)
```

输出：

```
Button{
    Size: Size{
        width: 3
        height: 2
    }
    title: 'Click me'
}
```

与继承不同，您不能在结构体和嵌入结构体之间进行类型转换
（嵌入结构体也可以有自己的字段，并且可以嵌入多个结构体）。

如果您需要直接访问嵌入的结构体，请使用显式引用，如 `button.Size`。

从概念上讲，嵌入结构体类似于 OOP 中的 [mixin](https://en.wikipedia.org/wiki/Mixin)，
*不是*基类。

您也可以初始化嵌入的结构体：

```v oksyntax
mut button := Button{
	Size: Size{
		width:  3
		height: 2
	}
}
```

或赋值：

```v oksyntax
button.Size = Size{
	width:  4
	height: 5
}
```

如果多个嵌入结构体具有同名的方法或字段，或者如果结构体中定义了
同名的方法或字段，您可以像 `button.Size.area()` 那样调用方法或赋值给
嵌入结构体中的变量。
当您不指定嵌入结构体名称时，将定位最外层结构体的方法。

## 联合体（Unions）

联合体是一种特殊类型的结构体，允许在同一内存位置
存储不同的数据类型。您可以定义具有许多成员的联合体，但根据成员的数据类型，
任何时候只有一个成员可能包含有效值。联合体提供了一种高效的方式，可以
将同一内存位置用于多种目的。

联合体的所有成员共享同一内存位置。这意味着修改一个成员
会自动修改所有其他成员。最大的联合体成员定义联合体的大小。

### 为什么使用联合体？

一个原因，如上所述，是为了使用更少的内存来存储东西。由于联合体的大小
是其中最大字段的大小，而结构体的大小是结构体中所有字段
的总和，联合体在降低内存使用方面肯定更好。只要您
在任何时候只需要其中一个字段有效，联合体就胜出。

另一个原因是允许更容易地访问字段的部分。例如，不使用联合体，
如果您想分别查看 32 位整数的每个字节，您需要按位
`右移`和 `AND` 操作。使用联合体，您可以直接访问各个字节。
```v
union ThirtyTwo {
	a u32
	b [4]u8
}
```
由于 `ThirtyTwo.a` 和 `ThirtyTwo.b` 共享相同的内存位置，您可以通过引用 `b[byte_offset]` 直接访问 `a` 的每个字节。

### 嵌入

联合体也支持嵌入，与结构体相同。

```v
struct Rgba32_Component {
	r u8
	g u8
	b u8
	a u8
}

union Rgba32 {
	Rgba32_Component
	value u32
}

clr1 := Rgba32{
	value: 0x008811FF
}

clr2 := Rgba32{
	Rgba32_Component: Rgba32_Component{
		a: 128
	}
}

sz := sizeof(Rgba32)
unsafe {
	println('Size: ${sz}B,clr1.b: ${clr1.b},clr2.b: ${clr2.b}')
}
```

输出：`Size: 4B, clr1.b: 136, clr2.b: 0`

联合体成员访问必须在 `unsafe` 块中执行。

> [!NOTE]
> 嵌入的结构体参数不一定按列出的顺序存储。

## 函数 2

### 默认不可变的函数参数

在 V 中，函数参数默认是不可变的，可变参数必须在
调用时标记。

由于也没有全局变量，这意味着函数的返回值
仅是其参数的函数，它们的评估没有副作用
（除非函数使用 I/O）。

函数参数默认是不可变的，即使传递了[引用](#references)。

> [!NOTE]
> 但是，V 不是纯函数式语言。

有一个编译器标志来启用全局变量（`-enable-globals`），但这
适用于内核和驱动程序等低级应用程序。

### 可变参数

可以通过使用关键字 `mut` 声明函数参数来修改它们：

```v
struct User {
	name string
mut:
	is_registered bool
}

fn (mut u User) register() {
	u.is_registered = true
}

mut user := User{}
println(user.is_registered) // "false"
user.register()
println(user.is_registered) // "true"
```

在这个示例中，接收者（只是第一个参数）被显式标记为可变的，
因此 `register()` 可以更改用户对象。对于非接收者参数也是如此：

```v
fn multiply_by_2(mut arr []int) {
	for i in 0 .. arr.len {
		arr[i] *= 2
	}
}

mut nums := [1, 2, 3]
multiply_by_2(mut nums)
println(nums)
// "[2, 4, 6]"
```

请注意，在调用此函数时必须在 `nums` 之前添加 `mut`。这使
调用该函数将修改值这一点变得清晰。

最好返回值而不是修改参数，
例如 `user = register(user)`（或 `user.register()`）而不是 `register(mut user)`。
修改参数应该只在应用程序的性能关键部分进行，
以减少分配和复制。

因此，V 不允许修改原始类型（例如整数）的参数。
只有更复杂的类型（如数组和映射）可以被修改。

### 可变参数数量
V 支持接收任意数量参数的函数，用
`...` 前缀表示。
下面，`a ...int` 指的是将被收集
到名为 `a` 的数组中的任意数量的参数。

```v
fn sum(a ...int) int {
	mut total := 0
	for x in a {
		total += x
	}
	return total
}

println(sum()) // 0
println(sum(1)) // 1
println(sum(2, 3)) // 5
// using array decomposition
a := [2, 3, 4]
println(sum(...a)) // <-- using prefix ... here. output: 9
b := [5, 6, 7]
println(sum(...b)) // output: 18
```

### 匿名函数和高阶函数

```v
fn sqr(n int) int {
	return n * n
}

fn cube(n int) int {
	return n * n * n
}

fn run(value int, op fn (int) int) int {
	return op(value)
}

fn main() {
	// Functions can be passed to other functions
	println(run(5, sqr)) // "25"
	// Anonymous functions can be declared inside other functions:
	double_fn := fn (n int) int {
		return n + n
	}
	println(run(5, double_fn)) // "10"
	// Functions can be passed around without assigning them to variables:
	res := run(5, fn (n int) int {
		return n + n
	})
	println(res) // "10"
	// You can even have an array/map of functions:
	fns := [sqr, cube]
	println(fns[0](10)) // "100"
	fns_map := {
		'sqr':  sqr
		'cube': cube
	}
	println(fns_map['cube'](2)) // "8"
}
```

### Lambda 表达式

V 中的 Lambda 表达式是小的匿名函数，使用
`|variables| expression` 语法定义。注意：此语法仅在调用高阶
函数时有效。

Here are some examples:
```v
mut a := [1, 2, 3]
a.sort(|x, y| x > y) // sorts the array, defining the comparator with a lambda expression
println(a.map(|x| x * 10)) // prints [30, 20, 10]
```

```v
// Lambda function can be used as callback
fn f(cb fn (a int) int) int {
	return cb(10)
}

println(f(|x| x + 4)) // prints 14
```

### 闭包

V 也支持闭包。
这意味着匿名函数可以从创建它们的作用域继承变量。
它们必须通过列出所有继承的变量来显式地这样做。

```v oksyntax
my_int := 1
my_closure := fn [my_int] () {
	println(my_int)
}
my_closure() // prints 1
```

继承的变量在创建匿名函数时被复制。
这意味着如果在创建函数后修改了原始变量，
修改不会反映在函数中。

```v oksyntax
mut i := 1
func := fn [i] () int {
	return i
}
println(func() == 1) // true
i = 123
println(func() == 1) // still true
```

但是，可以在匿名函数内修改变量。
更改不会反映在外部，但会在后续的函数调用中反映。

```v oksyntax
fn new_counter() fn () int {
	mut i := 0
	return fn [mut i] () int {
		i++
		return i
	}
}

c := new_counter()
println(c()) // 1
println(c()) // 2
println(c()) // 3
```

如果您需要在函数外部修改值，请使用引用。

```v oksyntax
mut i := 0
mut ref := &i
print_counter := fn [ref] () {
	println(*ref)
}

print_counter() // 0
i = 10
print_counter() // 10
```

### 参数求值顺序

函数调用参数的求值顺序*不*保证。
以以下程序为例：

```v
fn f(a1 int, a2 int, a3 int) {
	dump(a1 + a2 + a3)
}

fn main() {
	f(dump(100), dump(200), dump(300))
}
```

V 目前不保证它将按该顺序打印 100、200、300。
唯一的保证是 600（来自 `f` 的主体）将在它们全部之后打印。

这在 V 1.0 中*可能*会改变。

## 引用

```v
struct Foo {}

fn (foo Foo) bar_method() {
	// ...
}

fn bar_function(foo Foo) {
	// ...
}
```

如果函数参数是不可变的（如上面示例中的 `foo`），
V 可以按值或按引用传递它。编译器将决定，
开发人员不需要考虑它。

您不再需要记住是否应该按值或按引用传递结构体。

您可以通过添加 `&` 来确保结构体始终按引用传递：

```v
struct Foo {
	abc int
}

fn (foo &Foo) bar() {
	println(foo.abc)
}
```

`foo` 仍然是不可变的，不能更改。为此，
必须使用 `(mut foo Foo)`。

一般来说，V 的引用类似于 Go 指针和 C++ 引用。
例如，通用树结构定义将如下所示：

```v
struct Node[T] {
	val   T
	left  &Node[T]
	right &Node[T]
}
```

要解引用引用，请使用 `*` 运算符，就像在 C 中一样。

## 常量

```v
const pi = 3.14
const world = '世界'

println(pi)
println(world)
```

常量使用 `const` 声明。它们只能在
模块级别（函数外部）定义。
常量值永远不能更改。您也可以单独声明一个
常量：

```v
const e = 2.71828
```

V 常量比大多数语言更灵活。您可以分配更复杂的值：

```v
struct Color {
	r int
	g int
	b int
}

fn rgb(r int, g int, b int) Color {
	return Color{
		r: r
		g: g
		b: b
	}
}

const numbers = [1, 2, 3]
const red = Color{
	r: 255
	g: 0
	b: 0
}
// evaluate function call at compile time*
const blue = rgb(0, 0, 255)

println(numbers)
println(red)
println(blue)
```

\* 进行中 - 目前函数调用在程序启动时求值

通常不允许全局变量，所以这可能真的很有用。

**Modules**

Constants can be made public with `pub const`:

```v oksyntax
module mymodule

pub const golden_ratio = 1.61803

fn calc() {
	println(golden_ratio)
}
```

`pub` 关键字只允许在 `const` 关键字之前，不能在
`const ( )` 块内使用。

在模块 main 之外，所有常量都需要以模块名作为前缀。

### 必需的模块前缀

命名常量时，必须使用 `snake_case`。为了区分常量
和局部变量，必须指定常量的完整路径。例如，
要访问 PI 常量，在 `math` 模块外部和内部都必须使用完整的 `math.pi` 名称。
此限制仅对 `main` 模块
（包含您的 `fn main()` 的模块）放宽，您可以在那里使用
在那里定义的常量的非限定名称，即 `numbers`，而不是 `main.numbers`。

vfmt 会处理此规则，因此您可以在 `math` 模块内键入 `println(pi)`，
vfmt 会自动将其更新为 `println(math.pi)`。

<!--
Many people prefer all caps consts: `TOP_CITIES`. This wouldn't work
well in V, because consts are a lot more powerful than in other languages.
They can represent complex structures, and this is used quite often since there
are no globals:

```v oksyntax
println('Top cities: ${top_cities.filter(.usa)}')
```
-->

## 内置函数

一些函数是内置的，如 `println`。以下是完整列表：

```v ignore
fn print(s string) // prints anything on stdout
fn println(s string) // prints anything and a newline on stdout

fn eprint(s string) // same as print(), but uses stderr
fn eprintln(s string) // same as println(), but uses stderr

fn exit(code int) // terminates the program with a custom error code
fn panic(s string) // prints a message and backtraces on stderr, and terminates the program with error code 1
fn print_backtrace() // prints backtraces on stderr
```

> [!NOTE]
> Although the `print` functions take a string, V accepts other printable types too.
> See below for details.

还有一个名为 [`dump`](#dumping-expressions-at-runtime) 的特殊内置函数。

### println

`println` 是一个简单而强大的内置函数，可以打印任何内容：
字符串、数字、数组、映射、结构体。

```v
struct User {
	name string
	age  int
}

println(1) // "1"
println('hi') // "hi"
println([1, 2, 3]) // "[1, 2, 3]"
println(User{ name: 'Bob', age: 20 }) // "User{name:'Bob', age:20}"
```

另请参阅 [String interpolation](#string-interpolation)。

<a id='custom-print-of-types'></a>

### 打印自定义类型

如果您想为类型定义自定义打印值，只需定义一个
`str() string` 方法：

```v
struct Color {
	r int
	g int
	b int
}

pub fn (c Color) str() string {
	return '{${c.r}, ${c.g}, ${c.b}}'
}

red := Color{
	r: 255
	g: 0
	b: 0
}
println(red)
```

### 在运行时转储表达式

您可以使用 `dump(expr)` 转储/跟踪任何 V 表达式的值。
例如，将此代码示例保存为 `factorial.v`，然后使用
`v run factorial.v` 运行它：

```v
fn factorial(n u32) u32 {
	if dump(n <= 1) {
		return dump(1)
	}
	return dump(n * factorial(n - 1))
}

fn main() {
	println(factorial(5))
}
```

您将得到：

```
[factorial.v:2] n <= 1: false
[factorial.v:2] n <= 1: false
[factorial.v:2] n <= 1: false
[factorial.v:2] n <= 1: false
[factorial.v:2] n <= 1: true
[factorial.v:3] 1: 1
[factorial.v:5] n * factorial(n - 1): 2
[factorial.v:5] n * factorial(n - 1): 6
[factorial.v:5] n * factorial(n - 1): 24
[factorial.v:5] n * factorial(n - 1): 120
120
```

请注意，`dump(expr)` 将跟踪源位置、
表达式本身和表达式值。

## 模块

文件夹根目录中的每个文件都是同一模块的一部分。
简单程序不需要指定模块名称，在这种情况下它默认为 'main'。

请参阅 [symbol visibility](#symbol-visibility)、[Access modifiers](#access-modifiers)。

### 创建模块

V 是一种非常模块化的语言。鼓励创建可重用模块，并且
很容易做到。
要创建新模块，请创建一个以模块名称命名的目录，其中包含
带有代码的 .v 文件：

```shell
cd ~/code/modules
mkdir mymodule
vim mymodule/myfile.v
```

```v failcompile
// myfile.v
module mymodule

// To export a function we have to use `pub`
pub fn say_hi() {
	println('hello from mymodule!')
}
```
模块内的所有项都可以在模块的文件之间使用，无论它们是否
以 `pub` 关键字开头。
```v failcompile
// myfile2.v
module mymodule

pub fn say_hi_and_bye() {
	say_hi() // from myfile.v
	println('goodbye from mymodule')
}
```

您现在可以在代码中使用 `mymodule`：

```v failcompile
import mymodule

fn main() {
	mymodule.say_hi()
	mymodule.say_hi_and_bye()
}
```

* 模块名称应该简短，少于 10 个字符。
* 模块名称必须使用 `snake_case`。
* 不允许循环导入。
* 您可以在模块中拥有任意数量的 .v 文件。
* 您可以在任何地方创建模块。
* 所有模块都静态编译到单个可执行文件中。

### 项目文件夹的特殊考虑

对于顶级项目文件夹（使用 `v .` 编译的文件夹），并且*仅*
该文件夹，您可以拥有几个 .v 文件，它们可能提到不同的模块
，如 `module main`、`module abc` 等

这是为了简化该文件夹中的原型工作流程：
- 您可以使用单个 .v 文件开始开发新项目
- 根据需要将功能拆分到同一文件夹中的不同 .v 文件
- 当进一步组织在逻辑上合理时，将它们放入自己的目录模块中。

请注意，在普通模块中，所有 .v 文件都必须以 `module name_of_folder` 开头。

### `init` 函数

如果您希望模块在导入时自动调用一些设置/初始化代码，
您可以定义一个模块 `init` 函数：

```v
fn init() {
	// your setup code here ...
}
```

`init` 函数不能是公共的 - 它将被 V 自动调用，*仅一次*，无论
模块在您的程序中导入多少次。此功能对于
初始化 C 库特别有用。

### `cleanup` 函数

如果您希望模块在程序结束时自动调用一些清理/反初始化代码，
您可以定义一个模块 `cleanup` 函数：

```v
fn cleanup() {
	// your deinitialisation code here ...
}
```

就像 `init` 函数一样，模块的 `cleanup` 函数不能是公共的 - 它将在
程序结束时自动调用，每个模块一次，即使模块被其他模块
多次传递导入，按 init 调用的相反顺序。

## 类型声明

### 类型别名

要将新类型 `NewType` 定义为 `ExistingType` 的别名，
请使用 `type NewType = ExistingType`。<br/>
这是[和类型](#sum-types)声明的特殊情况。

### 枚举（Enums）

枚举是一组常量整数值，每个值都有自己的名称，
值从 0 开始，每个列出的名称递增 1。
例如：
```v
enum Color as u8 {
	red   // the default start value is 0
	green // the value is automatically incremented to 1
	blue  // the final value is now 2
}

mut color := Color.red
// V knows that `color` is a `Color`. No need to use `color = Color.green` here.
color = .green
println(color) // "green"
match color {
	.red { println('the color was red') }
	.green { println('the color was green') }
	.blue { println('the color was blue') }
}
println(int(color)) // prints 1
```

枚举类型可以是任何整数类型，但如果它是 `int`，则可以省略：`enum Color {`。

枚举匹配必须是穷尽的或有一个 `else` 分支。
这确保了如果添加了新的枚举字段，它会在代码的 everywhere 得到处理。

枚举字段可以重用保留关键字：

```v
enum Color {
	none
	red
	green
	blue
}

color := Color.none
println(color)
```

可以为枚举字段分配整数值。

```v
enum Grocery {
	apple
	orange = 5
	pear
}

g1 := int(Grocery.apple)
g2 := int(Grocery.orange)
g3 := int(Grocery.pear)
println('Grocery IDs: ${g1}, ${g2}, ${g3}')
```

输出：`Grocery IDs: 0, 5, 6`。

不允许对枚举变量进行运算；必须将它们显式转换为 `int`。

枚举可以有方法，就像结构体一样。

```v
enum Cycle {
	one
	two
	three
}

fn (c Cycle) next() Cycle {
	match c {
		.one {
			return .two
		}
		.two {
			return .three
		}
		.three {
			return .one
		}
	}
}

mut c := Cycle.one
for _ in 0 .. 10 {
	println(c)
	c = c.next()
}
```

Output:

```
one
two
three
one
two
three
one
two
three
one
```

枚举可以从字符串或整数值创建，并可以转换为字符串

```v
enum Cycle {
	one
	two = 2
	three
}

// Create enum from value
println(Cycle.from(10) or { Cycle.three })
println(Cycle.from('two')!)

// Convert an enum value to a string
println(Cycle.one.str())
```

Output:

```
three
two
one
```

### 函数类型

您可以使用类型别名为特定的函数签名命名 - 例如：

```v
type Filter = fn (string) string
```

这就像任何其他类型一样工作 - 例如，函数可以接受
函数类型的参数：

```v
type Filter = fn (string) string

fn filter(s string, f Filter) string {
	return f(s)
}
```

V 具有鸭子类型，因此函数不需要声明与
函数类型的兼容性 - 它们只需要兼容即可：

```v
fn uppercase(s string) string {
	return s.to_upper()
}

// 现在 `uppercase` 可以在任何需要 Filter 的地方使用
```

兼容的函数也可以显式转换为函数类型：

```v oksyntax
my_filter := Filter(uppercase)
```

这里的转换纯粹是信息性的 - 再次，鸭子类型意味着
即使没有显式转换，结果类型也是相同的：

```v oksyntax
my_filter := uppercase
```

您可以将分配的函数作为参数传递：

```v oksyntax
println(filter('Hello world', my_filter)) // prints `HELLO WORLD`
```

当然，您也可以直接传递它，而不使用
局部变量：

```v oksyntax
println(filter('Hello world', uppercase))
```

这也适用于匿名函数：

```v oksyntax
println(filter('Hello world', fn (s string) string {
	return s.to_upper()
}))
```

您可以查看完整的
[示例](https://github.com/vlang/v/tree/master/examples/function_types.v)。

### 接口（Interfaces）

```v
// interface-example.1
struct Dog {
	breed string
}

fn (d Dog) speak() string {
	return 'woof'
}

struct Cat {
	breed string
}

fn (c Cat) speak() string {
	return 'meow'
}

// 与 Go 不同，但类似于 TypeScript，V 的接口可以定义字段和方法。
interface Speaker {
	breed string
	speak() string
}

fn main() {
	dog := Dog{'Leonberger'}
	cat := Cat{'Siamese'}

	mut arr := []Speaker{}
	arr << dog
	arr << cat
	for item in arr {
		println('a ${item.breed} says: ${item.speak()}')
	}
}
```

#### 实现接口

类型通过实现其方法和字段来实现接口。

接口可以有一个 `mut:` 部分。实现类型需要
有一个 `mut` 接收器，用于在接口的 `mut:` 部分
声明的方法。

```v
// interface-example.2
module main

interface Foo {
	write(string) string
}

// => the method signature of a type, implementing interface Foo should be:
// `fn (s Type) write(a string) string`

interface Bar {
mut:
	write(string) string
}

// => the method signature of a type, implementing interface Bar should be:
// `fn (mut s Type) write(a string) string`

struct MyStruct {}

// MyStruct implements the interface Foo, but *not* interface Bar
fn (s MyStruct) write(a string) string {
	return a
}

fn main() {
	s1 := MyStruct{}
	fn1(s1)
	// fn2(s1) -> compile error, since MyStruct does not implement Bar
}

fn fn1(s Foo) {
	println(s.write('Foo'))
}

// fn fn2(s Bar) { // does not match
//      println(s.write('Foo'))
// }
```

有一个**可选的** `implements` 关键字用于显式声明
意图，它适用于 `struct` 声明。

```v
struct PathError implements IError {
	Error
	path string
}

fn (err PathError) msg() string {
	return 'Failed to open path: ${err.path}'
}

fn try_open(path string) ! {
	return PathError{
		path: path
	}
}

fn main() {
	try_open('/tmp') or { panic(err) }
}
```

#### 转换接口

我们可以使用动态转换操作符测试接口的底层类型。
> [!NOTE]
> 在此示例中，动态转换将变量 `s` 转换为 `if` 语句内的指针：

```v oksyntax
// interface-example.3 (continued from interface-example.1)
interface Something {}

fn announce(s Something) {
	if s is Dog {
		println('a ${s.breed} dog') // `s` is automatically cast to `Dog` (smart cast)
	} else if s is Cat {
		println('a cat speaks ${s.speak()}')
	} else {
		println('something else')
	}
}

fn main() {
	dog := Dog{'Leonberger'}
	cat := Cat{'Siamese'}
	announce(dog)
	announce(cat)
}
```

```v
// interface-example.4
interface IFoo {
	foo()
}

interface IBar {
	bar()
}

// implements only IFoo
struct SFoo {}

fn (sf SFoo) foo() {}

// implements both IFoo and IBar
struct SFooBar {}

fn (sfb SFooBar) foo() {}

fn (sfb SFooBar) bar() {
	dump('This implements IBar')
}

fn main() {
	mut arr := []IFoo{}
	arr << SFoo{}
	arr << SFooBar{}

	for a in arr {
		dump(a)
		// 为了执行实现 IBar 的实例。
		if a is IBar {
			a.bar()
		}
	}
}
```

更多信息，请参阅[动态转换](#dynamic-casts)。

#### 接口方法定义

与 Go 不同，接口可以有自己的方法，类似于
结构体可以有它们的方法。这些"接口方法"不需要
由实现该接口的结构体来实现。
它们只是一种方便的方式来编写 `i.some_function()` 而不是
`some_function(i)`，类似于结构体方法可以被视为
编写 `s.xyz()` 而不是 `xyz(s)` 的便利方式。

> [!NOTE]
> 此功能不是像 C# 中的"默认实现"。

例如，如果结构体 `cat` 被包装在接口 `a` 中，该接口
实现了一个与结构体实现的方法同名的方法 `speak`，
当您执行 `a.speak()` 时，*只有*接口方法被调用：

```v
interface Adoptable {}

fn (a Adoptable) speak() string {
	return 'adopt me!'
}

struct Cat {}

fn (c Cat) speak() string {
	return 'meow!'
}

struct Dog {}

fn main() {
	cat := Cat{}
	assert dump(cat.speak()) == 'meow!'

	a := Adoptable(cat)
	assert dump(a.speak()) == 'adopt me!' // 调用 Adoptable 的 `speak`
	if a is Cat {
		// 然而，在这个 `if` 内部，V 知道 `a` 不仅仅是任何
		// 类型的 Adoptable，实际上是一个 Cat，所以它将使用
		// Cat 的 `speak`，而不是 Adoptable 的 `speak`：
		dump(a.speak()) // meow!
	}

	b := Adoptable(Dog{})
	assert dump(b.speak()) == 'adopt me!' // 调用 Adoptable 的 `speak`
	// if b is Dog {
	// 	dump(b.speak()) // 错误：未知方法或字段：Dog.speak
	// }
}
```

#### 嵌入接口

接口支持嵌入，就像结构体一样：

```v
pub interface Reader {
mut:
	read(mut buf []u8) ?int
}

pub interface Writer {
mut:
	write(buf []u8) ?int
}

// ReaderWriter 嵌入 Reader 和 Writer。
// 效果与复制/粘贴所有
// Reader 和所有 Writer 方法/字段到
// ReaderWriter 中相同。
pub interface ReaderWriter {
	Reader
	Writer
}
```

### 和类型（Sum types）

和类型实例可以保存几种不同类型的值。使用 `type`
关键字声明和类型：

```v
struct Moon {}

struct Mars {}

struct Venus {}

type World = Mars | Moon | Venus

sum := World(Moon{})
assert sum.type_name() == 'Moon'
println(sum)
```

内置方法 `type_name` 返回当前持有的
类型的名称。

使用和类型，您可以构建递归结构并在其上编写简洁而强大的代码。

```v
// V's binary tree
struct Empty {}

struct Node {
	value f64
	left  Tree
	right Tree
}

type Tree = Empty | Node

// sum up all node values

fn sum(tree Tree) f64 {
	return match tree {
		Empty { 0 }
		Node { tree.value + sum(tree.left) + sum(tree.right) }
	}
}

fn main() {
	left := Node{0.2, Empty{}, Empty{}}
	right := Node{0.3, Empty{}, Node{0.4, Empty{}, Empty{}}}
	tree := Node{0.5, left, right}
	println(sum(tree)) // 0.2 + 0.3 + 0.4 + 0.5 = 1.4
}
```

#### 动态转换

要检查和类型实例是否持有某种类型，请使用 `sum is Type`。
要将和类型转换为其变体之一，可以使用 `sum as Type`：

```v
struct Moon {}

struct Mars {}

struct Venus {}

type World = Mars | Moon | Venus

fn (m Mars) dust_storm() bool {
	return true
}

fn main() {
	mut w := World(Moon{})
	assert w is Moon
	w = Mars{}
	// use `as` to access the Mars instance
	mars := w as Mars
	if mars.dust_storm() {
		println('bad weather!')
	}
}
```

如果 `w` 不持有 `Mars` 实例，`as` 会 panic。
更安全的方法是使用智能转换。

#### 智能转换

```v oksyntax
if w is Mars {
	assert typeof(w).name == 'Mars'
	if w.dust_storm() {
		println('bad weather!')
	}
}
```

`w` 在 `if` 语句体内具有 `Mars` 类型。这被称为
*流敏感类型*。
如果 `w` 是一个可变标识符，编译器在没有警告的情况下智能转换它是不安全的。
这就是为什么您必须在 `is` 表达式之前声明 `mut`：

```v ignore
if mut w is Mars {
	assert typeof(w).name == 'Mars'
	if w.dust_storm() {
		println('bad weather!')
	}
}
```

否则 `w` 将保持其原始类型。
> 这适用于简单变量和复杂表达式，如 `user.name`

#### 匹配和类型

您也可以使用 `match` 来确定变体：

```v
struct Moon {}

struct Mars {}

struct Venus {}

type World = Mars | Moon | Venus

fn open_parachutes(n int) {
	println(n)
}

fn land(w World) {
	match w {
		Moon {} // no atmosphere
		Mars {
			// light atmosphere
			open_parachutes(3)
		}
		Venus {
			// heavy atmosphere
			open_parachutes(1)
		}
	}
}
```

`match` must have a pattern for each variant or have an `else` branch.

```v ignore
struct Moon {}
struct Mars {}
struct Venus {}

type World = Moon | Mars | Venus

fn (m Moon) moon_walk() {}
fn (m Mars) shiver() {}
fn (v Venus) sweat() {}

fn pass_time(w World) {
    match w {
        // using the shadowed match variable, in this case `w` (smart cast)
        Moon { w.moon_walk() }
        Mars { w.shiver() }
        else {}
    }
}
```

### Option/Result 类型和错误处理

Option 类型可以表示一个值或 `none`。Result 类型可以
表示一个值，或从函数返回的错误。

`Option` 类型通过在类型名称前添加 `?` 来声明：`?Type`。
`Result` 类型使用 `!`：`!Type`。

```v
struct User {
	id   int
	name string
}

struct Repo {
	users []User
}

fn (r Repo) find_user_by_id(id int) !User {
	for user in r.users {
		if user.id == id {
			// V 自动将其包装为 result 或 option 类型
			return user
		}
	}
	return error('User ${id} not found')
}

// 使用 option 的函数版本
fn (r Repo) find_user_by_id2(id int) ?User {
	for user in r.users {
		if user.id == id {
			return user
		}
	}
	return none
}

fn main() {
	repo := Repo{
		users: [User{1, 'Andrew'}, User{2, 'Bob'}, User{10, 'Charles'}]
	}
	user := repo.find_user_by_id(10) or { // Option/Result 类型必须由 `or` 块处理
		println(err)
		return
	}
	println(user.id) // "10"
	println(user.name) // "Charles"

	user2 := repo.find_user_by_id2(10) or { return }

	// 直接创建 Option 变量：
	my_optional_int := ?int(none)
	my_optional_string := ?string(none)
	my_optional_user := ?User(none)
}
```

V 过去将 `Option` 和 `Result` 合并为一个类型，现在它们是分开的。

将函数"升级"为 option/result 函数所需的工作量很小；
您必须在返回类型中添加 `?` 或 `!`，并在出现问题时返回 `none` 或错误（分别）。

这是 V 中错误处理的主要机制。它们仍然是值，就像在 Go 中一样，
但优点是错误不能被未处理，处理它们也不那么冗长。
与其他语言不同，V 不使用 `throw/try/catch` 块处理异常。

`err` 在 `or` 块内定义，并设置为传递给
`error()` 函数的字符串消息。

```v oksyntax
user := repo.find_user_by_id(7) or {
	println(err) // "User 7 not found"
	return
}
```

使用 `err is ...` 来比较错误：
```v oksyntax
import io

x := read() or {
	if err is io.Eof {
		println('end of file')
	}
	return
}
```

#### 返回多个值时的 Options/results

函数只允许返回一个 `Option` 或 `Result`。它是
possible to return multiple values and still signal an error.

```v
fn multi_return(v int) !(int, int) {
	if v < 0 {
		return error('must be positive')
	}
	return v, v * v
}
```

#### 处理 options/results

有四种处理 option/result 的方法。第一种方法是
传播错误：

```v
import net.http

fn f(url string) !string {
	resp := http.get(url)!
	return resp.body
}
```

`http.get` 返回 `!http.Response`。因为 `!` 跟在调用之后，错误
将被传播到 `f` 的调用者。在产生 option 的函数调用后使用 `?` 时，
封闭函数也必须返回一个 option。如果在 `main()`
函数中使用错误传播，它将 `panic`，因为错误无法进一步传播。

`f` 的主体本质上是以下内容的压缩版本：

```v ignore
    resp := http.get(url) or { return err }
    return resp.body
```

---
第二种方法是提前中断执行：

```v oksyntax
user := repo.find_user_by_id(7) or { return }
```

在这里，您可以调用 `panic()` 或 `exit()`，这将停止
整个程序的执行，或使用控制流语句（`return`、`break`、`continue` 等）
来中断当前块。

> [!NOTE]
> `break` 和 `continue` 只能在 `for` 循环内使用。

V 没有强制"解包" option 的方法（如其他语言所做的那样，
例如 Rust 的 `unwrap()` 或 Swift 的 `!`）。为此，请使用 `or { panic(err) }`。

---
第三种方法是在 `or` 块的末尾提供默认值。
如果出现错误，将分配该值，
因此它必须与正在处理的 `Option` 的内容具有相同的类型。

```v
fn do_something(s string) !string {
	if s == 'foo' {
		return 'foo'
	}
	return error('invalid string')
}

a := do_something('foo') or { 'default' } // a will be 'foo'
b := do_something('bar') or { 'default' } // b will be 'default'
println(a)
println(b)
```

---
第四种方法是使用 `if` 解包：

```v
import net.http

if resp := http.get('https://google.com') {
	println(resp.body) // resp is a http.Response, not an option
} else {
	println(err)
}
```

上面，`http.get` 返回 `!http.Response`。`resp` 仅在第一个
`if` 分支的作用域内。`err` 仅在 `else` 分支的作用域内。

### 自定义错误类型

V 允许您通过 `IError` 接口定义自定义错误类型。
该接口需要两个方法：`msg() string` 和 `code() int`。每个实现
这些方法的类型都可以用作错误。

定义自定义错误类型时，建议嵌入内置的 `Error` 默认
实现。这为两个必需方法提供了空的默认实现，
因此您只需要实现真正需要的内容，并且可以在将来提供额外的实用
函数。

```v
struct PathError {
	Error
	path string
}

fn (err PathError) msg() string {
	return 'Failed to open path: ${err.path}'
}

fn try_open(path string) ! {
	// V 自动将其转换为 IError
	return PathError{
		path: path
	}
}

fn main() {
	try_open('/tmp') or { panic(err) }
}
```

### 泛型（Generics）

```v wip

struct Repo[T] {
    db DB
}

struct User {
	id   int
	name string
}

struct Post {
	id   int
	user_id int
	title string
	body string
}

fn new_repo[T](db DB) Repo[T] {
    return Repo[T]{db: db}
}

// 这是一个泛型函数。V 会为使用它的每种类型生成它。
fn (r Repo[T]) find_by_id(id int) ?T {
    table_name := T.name // 在此示例中，获取类型的名称为我们提供表名
    return r.db.query_one[T]('select * from ${table_name} where id = ?', id)
}

db := new_db()
users_repo := new_repo[User](db) // returns Repo[User]
posts_repo := new_repo[Post](db) // returns Repo[Post]
user := users_repo.find_by_id(1)? // find_by_id[User]
post := posts_repo.find_by_id(1)? // find_by_id[Post]
```

目前泛型函数定义必须声明其类型参数，但在
未来版本中，V 将从运行时参数类型中的单字母类型名称推断泛型类型参数。
这就是为什么上面的 `find_by_id(1)` 调用可以省略 `[T]`，
因为方法声明中的接收器参数 `r` 使用泛型类型 `T`。

另一个示例：

```v
fn compare[T](a T, b T) int {
	if a < b {
		return -1
	}
	if a > b {
		return 1
	}
	return 0
}

// compare[int]
println(compare(1, 0)) // Outputs: 1
println(compare(1, 1)) //          0
println(compare(1, 2)) //         -1
// compare[string]
println(compare('1', '0')) // Outputs: 1
println(compare('1', '1')) //          0
println(compare('1', '2')) //         -1
// compare[f64]
println(compare(1.1, 1.0)) // Outputs: 1
println(compare(1.1, 1.1)) //          0
println(compare(1.1, 1.2)) //         -1
```

## 并发

### 生成并发任务

V 的并发模型类似于 Go。

`go foo()` 在 V 运行时管理的轻量级线程中并发运行 `foo()`。

`spawn foo()` 在不同的线程中并发运行 `foo()`：

```v
import math

fn p(a f64, b f64) { // 没有返回值的普通函数
	c := math.sqrt(a * a + b * b)
	println(c)
}

fn main() {
	spawn p(3, 4)
	// p 将在并行线程中运行
	// 也可以写成如下形式
	// spawn fn (a f64, b f64) {
	// 	c := math.sqrt(a * a + b * b)
	// 	println(c)
	// }(3, 4)
}
```

> [!NOTE]
> 线程依赖于机器的 CPU（核心数/线程数）。
> 请注意，使用 `spawn` 生成的 OS 线程
> 在并发方面有限制，
> 包括资源开销和可扩展性问题，
> 在高线程数的情况下可能会影响性能。

有时需要等待并行线程完成。这可以通过
为启动的线程分配一个*句柄*，然后调用该句柄的 `wait()` 方法
来实现：

```v
import math

fn p(a f64, b f64) { // 没有返回值的普通函数
	c := math.sqrt(a * a + b * b)
	println(c) // 打印 `5`
}

fn main() {
	h := spawn p(3, 4)
	// p() 在并行线程中运行
	h.wait()
	// p() 肯定已完成
}
```

这种方法也可以用于从在并行线程中运行的函数获取返回值。
无需修改函数本身即可并发调用它。

```v
import math { sqrt }

fn get_hypot(a f64, b f64) f64 { //       返回值的普通函数
	c := sqrt(a * a + b * b)
	return c
}

fn main() {
	g := spawn get_hypot(54.06, 2.08) // 生成线程并获取其句柄
	h1 := get_hypot(2.32, 16.74) //   在这里进行其他计算
	h2 := g.wait() //                 从生成的线程获取结果
	println('Results: ${h1}, ${h2}') //   打印 `Results: 16.9, 54.1`
}
```

如果有大量任务，使用线程数组管理它们
可能会更容易。

```v
import time

fn task(id int, duration int) {
	println('task ${id} begin')
	time.sleep(duration * time.millisecond)
	println('task ${id} end')
}

fn main() {
	mut threads := []thread{}
	threads << spawn task(1, 500)
	threads << spawn task(2, 900)
	threads << spawn task(3, 100)
	threads.wait()
	println('done')
}

// Output:
// task 1 begin
// task 2 begin
// task 3 begin
// task 3 end
// task 1 end
// task 2 end
// done
```

此外，对于返回相同类型的线程，在
线程数组上调用 `wait()` 将返回所有计算值。

```v
fn expensive_computing(i int) int {
	return i * i
}

fn main() {
	mut threads := []thread int{}
	for i in 1 .. 10 {
		threads << spawn expensive_computing(i)
	}
	// Join all tasks
	r := threads.wait()
	println('All jobs finished: ${r}')
}

// Output: All jobs finished: [1, 4, 9, 16, 25, 36, 49, 64, 81]
```

### 通道（Channels）

通道是线程之间通信的首选方式。它们允许线程安全地交换数据
而无需显式锁定。V 的通道类似于 Go 中的通道，使您能够
在一端将对象推入通道，在另一端弹出对象。
通道可以是缓冲的或非缓冲的，您可以使用 `select` 语句同时监控多个
通道。

#### 语法和用法

通道使用类型 `chan objtype` 声明。
您可以选择使用 `cap` 字段指定缓冲区长度：

```v
ch := chan int{} // unbuffered - "synchronous"
ch2 := chan f64{cap: 100} // buffered with a capacity of 100
```

通道不必声明为 `mut`。缓冲区长度不是类型的一部分，而是
单个通道对象的字段。通道可以像普通变量一样传递给线程：

```v
import time

fn worker(ch chan int) {
	for i in 0 .. 5 {
		ch <- i // 将值推入通道
	}
}

fn clock(ch chan int) {
	for i in 0 .. 5 {
		time.sleep(1 * time.second)
		println('Clock tick')
		ch <- (i + 1000) // 将值推入通道
	}
	ch.close() // 完成后关闭通道
}

fn main() {
	ch := chan int{cap: 5}
	spawn worker(ch)
	spawn clock(ch)
	for {
		value := <-ch or { // 从通道接收/弹出值
			println('Channel closed')
			break
		}
		println('Received: ${value}')
	}
}
```

#### 缓冲通道

缓冲通道允许您在不阻塞的情况下推送多个项目，
只要缓冲区未满：

```v
ch := chan string{cap: 2}
ch <- 'hello'
ch <- 'world'
// ch <- '!' // 这会阻塞，因为缓冲区已满

println(<-ch) // "hello"
println(<-ch) // "world"
```

#### 关闭通道

可以关闭通道以指示不能再推送对象。任何尝试
这样做都会导致运行时 panic（除了 `select` 和
`try_push()` - 见下文）。如果
关联的通道已关闭且缓冲区为空，尝试弹出将立即返回。这种情况可以
使用 `or {}` 块处理（参见[处理 options/results](#handling-optionsresults)）。

```v wip
ch := chan int{}
ch2 := chan f64{}
// ...
ch.close()
// ...
m := <-ch or {
    println('channel has been closed')
}

// 传播错误
y := <-ch2 ?
```

#### 通道选择（Channel Select）

`select` 命令允许同时监控多个通道
而不会产生明显的 CPU 负载。它由可能的传输列表和关联的语句分支组成
- 类似于 [match](#match) 命令：

```v
import time

fn main() {
	ch := chan f64{}
	ch2 := chan f64{}
	ch3 := chan f64{}
	mut b := 0.0
	c := 1.0
	// ... setup spawn threads that will send on ch/ch2
	spawn fn (the_channel chan f64) {
		time.sleep(5 * time.millisecond)
		the_channel <- 1.0
	}(ch)
	spawn fn (the_channel chan f64) {
		time.sleep(1 * time.millisecond)
		the_channel <- 1.0
	}(ch2)
	spawn fn (the_channel chan f64) {
		_ := <-the_channel
	}(ch3)

	select {
		a := <-ch {
			// 对 `a` 做一些事情
			eprintln('> a: ${a}')
		}
		b = <-ch2 {
			// 对预声明的变量 `b` 做一些事情
			eprintln('> b: ${b}')
		}
		ch3 <- c {
			// 如果 `c` 已发送，做一些事情
			time.sleep(5 * time.millisecond)
			eprintln('> c: ${c} was send on channel ch3')
		}
		500 * time.millisecond {
			// 如果在 0.5 秒内没有通道就绪，做一些事情
			eprintln('> more than 0.5s passed without a channel being ready')
		}
	}
	eprintln('> done')
}
```

超时分支是可选的。如果不存在，`select` 将无限期等待。
如果在调用 `select` 时没有通道就绪，也可以通过添加 `else { ... }` 分支
立即继续。`else` 和 `<timeout>` 是互斥的。

`select` 命令可以用作 `bool` 类型的*表达式*
如果所有通道都关闭，则变为 `false`：

```v wip
if select {
    ch <- a {
        // ...
    }
} {
    // channel was open
} else {
    // channel is closed
}
```

#### 特殊通道功能

为了特殊目的，有一些内置字段和方法：

```v
ch := chan int{cap: 2}
println(ch.try_push(42)) // 如果推送成功则为 `.success`，如果已满则为 `.not_ready`，如果已关闭则为 `.closed`
println(ch.len) // 缓冲区中的项目数
println(ch.cap) // 缓冲区容量
println(ch.closed) // 通道是否已关闭
```

```v
struct Abc {
	x int
}

a := 2.13
ch := chan f64{}
res := ch.try_push(a) // 尝试执行 `ch <- a`
println(res)
l := ch.len // 队列中的元素数
c := ch.cap // 最大队列长度
is_closed := ch.closed // 布尔标志 - `ch` 是否已关闭
println(l)
println(c)
mut b := Abc{}
ch2 := chan Abc{}
res2 := ch2.try_pop(mut b) // 尝试执行 `b = <-ch2`
```

`try_push/pop()` 方法将立即返回结果之一
`.success`、`.not_ready` 或 `.closed` - 取决于对象是否已传输或
未传输的原因。
不建议在生产中使用这些方法和字段 -
基于它们的算法经常受到竞争条件的影响。特别是 `.len` 和
`.closed` 不应用于做决策。
请改用 `or` 分支、错误传播或 `select`（参见上面的[语法和用法](#syntax-and-usage)
和[通道选择](#channel-select)）。

### 共享对象（Shared Objects）

数据可以通过共享变量在线程和调用线程之间交换。
此类变量应创建为 `shared`，并作为共享变量传递给线程。
底层 `struct` 包含一个隐藏的*互斥锁*，允许使用
`rlock` 进行只读锁定并发访问，使用 `lock` 进行读/写访问。

注意：共享变量必须是结构体、数组或映射。

#### 共享对象示例

```v
struct Counter {
mut:
	value int
}

fn (shared counter Counter) increment() {
	lock counter {
		counter.value += 1
		println('Incremented to: ${counter.value}')
	}
}

fn main() {
	shared counter := Counter{}

	spawn counter.increment()
	spawn counter.increment()

	rlock counter {
		println('Final value: ${counter.value}')
	}
}
```

### 通道和共享对象的区别

**目的**：
- 通道：用于线程之间的消息传递，确保安全通信。
- 共享对象：用于线程之间的直接数据共享和修改。

**同步**：
- 通道：隐式（通过通道操作）
- 共享对象：显式（通过 `rlock`/`lock` 块）

## JSON

由于 JSON 的普遍性，V 直接内置了对它的支持。

V 为 JSON 编码和解码生成代码。
不使用运行时反射。这带来了更好的性能。

### 解码 JSON

```v
import json

struct Foo {
	x int
}

struct User {
	// 添加 [required] 属性将使解码失败，如果该
	// 字段在输入中不存在。
	// 如果字段不是 [required]，但缺失，它将被假定
	// 为其默认值，如数字为 0，字符串为 ''，
	// 解码不会失败。
	name string @[required]
	age  int
	// 使用 `@[skip]` 属性跳过某些字段。
	// 您也可以使用 `@[json: '-']` 和 `@[sql: '-']`，这将导致只有
	// `json` 模块跳过该字段，或只有 SQL orm 跳过它。
	foo Foo @[skip]
	// 如果 JSON 中的字段名不同，可以指定
	last_name string @[json: lastName]
}

data := '{ "name": "Frodo", "lastName": "Baggins", "age": 25, "nullable": null }'
user := json.decode(User, data) or {
	eprintln('Failed to decode json, error: ${err}')
	return
}
println(user.name)
println(user.last_name)
println(user.age)
// 您也可以解码 JSON 数组：
sfoos := '[{"x":123},{"x":456}]'
foos := json.decode([]Foo, sfoos)!
println(foos[0].x)
println(foos[1].x)
```

`json.decode` 函数接受两个参数：
第一个是 JSON 值应解码到的类型，
第二个是包含 JSON 数据的字符串。

### 编码 JSON

```v
import json

struct User {
	name  string
	score i64
}

mut data := map[string]int{}
user := &User{
	name:  'Pierre'
	score: 1024
}

data['x'] = 42
data['y'] = 360

println(json.encode(data)) // {"x":42,"y":360}
println(json.encode(user)) // {"name":"Pierre","score":1024}
```

json 模块还支持匿名结构体字段，这有助于处理具有多个
层级的复杂 JSON API。

## 测试

### 断言（Asserts）

```v
fn foo(mut v []int) {
	v[0] = 1
}

mut v := [20]
foo(mut v)
assert v[0] < 4
```

`assert` 语句检查其表达式是否求值为 `true`。如果断言失败，
程序通常会中止。断言应仅用于检测编程错误。当断言失败时，
它会被报告到 *stderr*，并且比较操作符（如 `<`、`==`）两侧的值
会在可能时打印。这对于轻松找到意外值很有用。断言语句可以在任何函数中使用，
不仅仅是测试函数，这在开发新功能时很方便，可以保持您的不变量检查。

> [!NOTE]
> 当您使用 `-prod` 标志编译程序时，所有 `assert` 语句都会被*移除*。

### 带额外消息的断言

这种形式的 `assert` 语句，在失败时将打印额外消息。请注意，
您可以在那里使用任何字符串表达式 - 字符串字面量、返回字符串的函数、
插值变量的字符串等。

```v
fn test_assertion_with_extra_message_failure() {
	for i in 0 .. 100 {
		assert i * 2 - 45 < 75 + 10, 'assertion failed for i: ${i}'
	}
}
```

### 不会中止程序的断言

在最初原型化功能和测试时，有时希望
断言不会停止程序，而只是打印它们的失败。这可以通过
用 `[assert_continues]` 标签标记包含断言的函数来实现，
例如运行此程序：

```v
@[assert_continues]
fn abc(ii int) {
	assert ii == 2
}

for i in 0 .. 4 {
	abc(i)
}
```

... will produce this output:

```
assert_continues_example.v:3: FAIL: fn main.abc: assert ii == 2
   left value: ii = 0
   right value: 2
assert_continues_example.v:3: FAIL: fn main.abc: assert ii == 2
   left value: ii = 1
  right value: 2
assert_continues_example.v:3: FAIL: fn main.abc: assert ii == 2
   left value: ii = 3
  right value: 2
```

> [!NOTE]
> V 还支持命令行标志 `-assert continues`，它将全局更改
> 所有断言的行为，就像您用 `[assert_continues]` 标记了每个函数一样。

### 测试文件

```v
// hello.v
module main

fn hello() string {
	return 'Hello world'
}

fn main() {
	println(hello())
}
```

```v failcompile
// hello_test.v
module main

fn test_hello() {
	assert hello() == 'Hello world'
}
```

要运行上面的测试文件，请使用 `v hello_test.v`。这将检查函数 `hello` 是否
产生正确的输出。V 执行文件中的所有测试函数。

> [!NOTE]
> 所有 `_test.v` 文件（外部和内部），都编译为*单独的程序*。
> 换句话说，您可以拥有任意数量的 `_test.v` 文件，以及其中的测试，它们通常
> 根本不会影响您在 `.v` 文件中的其他代码的编译，只有在您
> 明确执行 `v file_test.v` 或 `v test .` 时才会影响。

* 所有测试函数必须在名称以 `_test.v` 结尾的测试文件中。
* 测试函数名称必须以 `test_` 开头以标记它们执行。
* 普通函数也可以在测试文件中定义，并应手动调用。其他
  符号也可以在测试文件中定义，例如类型。
* 有两种测试：外部测试和内部测试。
* 内部测试必须*声明*其模块，就像同一模块中的所有其他 .v
  文件一样。内部测试甚至可以调用同一模块中的私有函数。
* 外部测试必须*导入*它们测试的模块。它们无法
  访问模块的私有函数/类型。它们只能测试
  模块提供的外部/公共 API。

在上面的示例中，`test_hello` 是一个内部测试，可以调用
私有函数 `hello()`，因为 `hello_test.v` 有 `module main`，
就像 `hello.v` 一样，即两者都是同一模块的一部分。还要注意，
由于 `module main` 是与其他模块一样的常规模块，内部测试也可以
用于测试主程序 .v 文件中的私有函数。

您还可以在测试文件中定义这些特殊的测试函数：

* `testsuite_begin` 将在所有其他测试函数*之前*运行。
* `testsuite_end` 将在所有其他测试函数*之后*运行。

如果测试函数有错误返回类型，任何传播的错误都会使测试失败：

```v
import strconv

fn test_atoi() ! {
	assert strconv.atoi('1')! == 1
	assert strconv.atoi('one')! == 1 // test will fail
}
```

### 运行测试

要在单个测试文件中运行测试函数，请使用 `v foo_test.v`。

要测试整个模块，请使用 `v test mymodule`。您也可以使用 `v test .` 来测试
当前文件夹（和子文件夹）内的所有内容。您可以传递 `-stats`
选项以查看有关运行的各个测试的更多详细信息。

您可以将其他测试数据（包括 .v 源文件）放在名为
`testdata` 的文件夹中，就在您的 _test.v 文件旁边。V 的测试框架将*忽略*
此类文件夹，同时扫描要运行的测试。如果您想
放置包含无效 V 源代码的 .v 文件，或其他测试（包括已知
失败的测试），这些测试应该由父 _test.v 文件以特定方式/选项运行，
这很有用。

> [!NOTE]
> V 编译器的路径可通过 @VEXE 获得，因此 _test.v
> 文件可以轻松运行*其他*测试文件，如下所示：

```v oksyntax
import os

fn test_subtest() {
	res := os.execute('${os.quoted_path(@VEXE)} other_test.v')
	assert res.exit_code == 1
	assert res.output.contains('other_test.v does not exist')
}
```

## 内存管理

V 首先通过使用值类型、
字符串缓冲区来避免不必要的分配，促进简单无抽象的代码风格。

V 中有 4 种管理内存的方法。

默认是最小且性能良好的跟踪 GC。

第二种方法是 autofree，可以使用 `-autofree` 启用。它处理大多数对象
（~90-100%）：编译器在编译期间自动插入必要的 free 调用。
剩余的小部分对象通过 GC 释放。开发人员不需要更改
代码中的任何内容。"它只是工作"，就像在 Python、Go 或 Java 中一样，除了没有
重 GC 跟踪一切或为每个对象进行昂贵的 RC。

对于希望有更多低级控制的开发人员，可以使用
`-gc none` 手动管理内存。

通过 `-prealloc` 标志可以使用 Arena 分配。注意：目前此模式仅
适用于加速短生命周期、单线程、批处理类程序（如编译器）。

### 控制

您可以利用 V 的 autofree 引擎并在自定义
数据类型上定义 `free()` 方法：

```v
struct MyType {}

@[unsafe]
fn (data &MyType) free() {
	// ...
}
```

就像编译器使用 C 的 `free()` 释放 C 数据类型一样，它将在每个变量生命周期的
结束时为您的数据类型静态插入 `free()` 调用。

可以使用 `-autofree` 标志启用 Autofree。

对于希望有更多低级控制的开发人员，可以使用
`-manualfree` 禁用 autofree，或者在想要手动管理其
内存的每个函数上添加 `[manualfree]`。（参见[属性](#attributes)）。

> [!NOTE]
> Autofree 仍在开发中。在它稳定并成为默认值之前，请
> 避免使用它。目前分配由最小且性能良好的 GC 处理
> 直到 V 的 autofree 引擎准备好用于生产。

**Examples**

```v
import strings

fn draw_text(s string, x int, y int) {
	// ...
}

fn draw_scene() {
	// ...
	name1 := 'abc'
	name2 := 'def ghi'
	draw_text('hello ${name1}', 10, 10)
	draw_text('hello ${name2}', 100, 10)
	draw_text(strings.repeat(`X`, 10000), 10, 50)
	// ...
}
```

字符串不会逃逸 `draw_text`，因此它们会在
函数退出时被清理。

实际上，使用 `-prealloc` 标志，前两次调用根本不会导致任何分配。
这两个字符串很小，因此 V 将为它们使用预分配的缓冲区。

```v
struct User {
	name string
}

fn test() []int {
	number := 7 // 栈变量
	user := User{} // 在栈上分配的结构体
	numbers := [1, 2, 3] // 在堆上分配的数组，将在函数退出时释放
	println(number)
	println(user)
	println(numbers)
	numbers2 := [4, 5, 6] // 正在返回的数组，不会在这里释放
	return numbers2
}
```

### 栈和堆

#### 栈和堆基础

与大多数其他编程语言一样，有两个位置可以
存储数据：

* *栈*允许快速分配，几乎零管理开销。栈
  随函数调用深度增长和收缩 &ndash; 因此每个被调用的
  函数都有其栈段，该段在函数返回之前保持有效。
  不需要释放，但是，这也意味着对栈
  对象的引用在函数返回时变得无效。此外，栈空间是
  有限的（通常每个线程几兆字节）。
* *堆*是一个大的内存区域（通常几吉字节），由
  操作系统管理。堆对象通过特殊函数调用分配和释放
  这些调用将管理任务委托给 OS。这意味着它们可以
  在多个函数调用中保持有效，但是，管理是
  昂贵的。

#### V 的默认方法

由于性能考虑，V 尽可能尝试将对象放在栈上，
但在明显必要时将它们分配在堆上。示例：

```v
struct MyStruct {
	n int
}

struct RefStruct {
	r &MyStruct
}

fn main() {
	q, w := f()
	println('q: ${q.r.n}, w: ${w.n}')
}

fn f() (RefStruct, &MyStruct) {
	a := MyStruct{
		n: 1
	}
	b := MyStruct{
		n: 2
	}
	c := MyStruct{
		n: 3
	}
	e := RefStruct{
		r: &b
	}
	x := a.n + c.n
	println('x: ${x}')
	return e, &c
}
```

这里 `a` 存储在栈上，因为它的地址永远不会离开函数 `f()`。
但是对 `b` 的引用是返回的 `e` 的一部分。另外，对
`c` 的引用也被返回。因此 `b` 和 `c` 将在堆上分配。

当对象的引用作为函数参数传递时，情况变得不那么明显：

```v
struct MyStruct {
mut:
	n int
}

fn main() {
	mut q := MyStruct{
		n: 7
	}
	w := MyStruct{
		n: 13
	}
	x := q.f(&w) // references of `q` and `w` are passed
	println('q: ${q}\nx: ${x}')
}

fn (mut a MyStruct) f(b &MyStruct) int {
	a.n += b.n
	x := a.n * b.n
	return x
}
```

这里调用 `q.f(&w)` 传递对 `q` 和 `w` 的引用，因为 `a` 是
`mut` 且 `b` 在 `f()` 的声明中是 `&MyStruct` 类型，所以从技术上讲
这些引用正在离开 `main()`。但是这些引用的*生命周期*
位于 `main()` 的作用域内，因此 `q` 和 `w` 在栈上分配。

#### 栈和堆的手动控制

在上一个示例中，V 编译器可以将 `q` 和 `w` 放在栈上
因为它假设在调用 `q.f(&w)` 中，这些引用仅
用于读取和修改引用的值 &ndash; 而不是将
引用本身传递到其他地方。这可以看作是
对 `q` 和 `w` 的引用仅*借用*给 `f()`。

如果 `f()` 对引用本身做一些事情，情况就不同了：

```v
struct RefStruct {
mut:
	r &MyStruct
}

// see discussion below
@[heap]
struct MyStruct {
	n int
}

fn main() {
	mut m := MyStruct{}
	mut r := RefStruct{
		r: &m
	}
	r.g()
	println('r: ${r}')
}

fn (mut r RefStruct) g() {
	s := MyStruct{
		n: 7
	}
	r.f(&s) // reference to `s` inside `r` is passed back to `main() `
}

fn (mut r RefStruct) f(s &MyStruct) {
	r.r = s // would trigger error without `[heap]`
}
```

这里 `f()` 看起来很无辜，但正在做糟糕的事情 &ndash; 它将
对 `s` 的引用插入到 `r` 中。问题是 `s` 只在
`g()` 运行时存在，但 `r` 在那之后在 `main()` 中使用。因此
编译器会抱怨 `f()` 中的赋值，因为 `s` *"可能
引用存储在栈上的对象"*。在 `g()` 中假设调用
`r.f(&s)` 只会借用对 `s` 的引用是错误的。

解决这个困境的方法是在声明
`struct MyStruct` 时使用 `[heap]` [属性](#attributes)。它指示编译器*始终*在堆上分配 `MyStruct` 对象。
这样，即使在 `g()` 返回后，对 `s` 的引用仍然有效。
编译器在检查 `f()` 时考虑到 `MyStruct` 对象总是在堆上
分配，并允许将对 `s` 的引用分配给
`r.r` 字段。

There is a pattern often seen in other programming languages:

```v failcompile
fn (mut a MyStruct) f() &MyStruct {
	// do something with a
	return &a // would return address of borrowed object
}
```

这里 `f()` 接收一个引用 `a` 作为接收器，该引用被传递回调用者并同时
作为结果返回。这种声明背后的意图是方法链，如
`y = x.f().g()`。但是，这种方法的问题是创建了第二个
对 `a` 的引用 &ndash; 所以它不仅仅是借用的，`MyStruct` 必须
声明为 `[heap]`。

在 V 中，更好的方法是：

```v
struct MyStruct {
mut:
	n int
}

fn (mut a MyStruct) f() {
	// do something with `a`
}

fn (mut a MyStruct) g() {
	// do something else with `a`
}

fn main() {
	x := MyStruct{} // stack allocated
	mut y := x
	y.f()
	y.g()
	// instead of `mut y := x.f().g()
}
```

这样可以避免 `[heap]` 属性 &ndash; 从而获得更好的性能。

但是，如上所述，栈空间非常有限。因此，即使不需要上面提到的用例，
`[heap]` 属性也可能适用于非常大的结构。

有一种替代方法可以逐个手动控制分配。这种
方法不推荐，但为了完整性在此显示：

```v
struct MyStruct {
	n int
}

struct RefStruct {
mut:
	r &MyStruct
}

// 简单函数 - 只是为了覆盖 `g()` 之前使用的栈段

fn use_stack() {
	x := 7.5
	y := 3.25
	z := x + y
	println('${x} ${y} ${z}')
}

fn main() {
	mut m := MyStruct{}
	mut r := RefStruct{
		r: &m
	}
	r.g()
	use_stack() // to erase invalid stack contents
	println('r: ${r}')
}

fn (mut r RefStruct) g() {
	s := &MyStruct{ // `s` 显式引用堆对象
		n: 7
	}
	// 将上面的 `&MyStruct` -> `MyStruct` 和下面的 `r.f(s)` -> `r.f(&s)` 更改
	// 以查看栈段中的数据被覆盖
	r.f(s)
}

fn (mut r RefStruct) f(s &MyStruct) {
	r.r = unsafe { s } // 覆盖编译器检查
}
```

这里编译器检查被 `unsafe` 块抑制。即使没有 `[heap]` 属性，也要使 `s` 在堆上
分配，结构体字面量以
& 符号为前缀：`&MyStruct{...}`。

最后一步不是编译器要求的，但没有它，`r` 内部的引用
会变得无效（指向的内存区域将被
`use_stack()` 覆盖），程序可能会崩溃（或至少产生不可预测的
最终输出）。这就是为什么这种方法是*不安全的*，应该避免！

## ORM

（这仍处于 alpha 状态）

V 有一个内置的 ORM（对象关系映射），支持 SQLite、MySQL 和 Postgres，
但很快它将支持 MS SQL 和 Oracle。

V 的 ORM 提供了许多好处：

- 所有 SQL 方言的一种语法。（在数据库之间迁移变得更容易。）
- 使用 V 的语法构建查询。（无需学习另一种语法。）
- 安全性。（所有查询都会自动清理以防止 SQL 注入。）
- 编译时检查。（这可以防止只能在运行时捕获的拼写错误。）
- 可读性和简单性。（您不需要手动解析查询结果，然后
  从解析的结果手动构造对象。）

```v
import db.sqlite

// 设置自定义表名。默认是结构体名称（区分大小写）
@[table: 'customers']
struct Customer {
	id        int @[primary; serial] // 名为 `id` 的整数字段必须是第一个字段
	name      string
	nr_orders int
	country   ?string
}

db := sqlite.connect('customers.db')!

// 您可以从结构体声明创建表。例如，下一个查询将发出类似于此的 SQL：
// CREATE TABLE IF NOT EXISTS `Customer` (
//      `id` INTEGER PRIMARY KEY,
//      `name` TEXT NOT NULL,
//      `nr_orders` INTEGER NOT NULL,
//      `country` TEXT
// )
sql db {
	create table Customer
}!

// 插入新客户：
new_customer := Customer{
	name:      'Bob'
	country:   'uk'
	nr_orders: 10
}
sql db {
	insert new_customer into Customer
}!

us_customer := Customer{
	name:      'Martin'
	country:   'us'
	nr_orders: 5
}
sql db {
	insert us_customer into Customer
}!

none_country_customer := Customer{
	name:      'Dennis'
	country:   none
	nr_orders: 2
}
sql db {
	insert none_country_customer into Customer
}!

// 更新客户：
sql db {
	update Customer set nr_orders = nr_orders + 1 where name == 'Bob'
}!

// select count(*) from customers
nr_customers := sql db {
	select count from Customer
}!
println('number of all customers: ${nr_customers}')

// 可以使用 V 的语法构建查询：
uk_customers := sql db {
	select from Customer where country == 'uk' && nr_orders > 0 order by id desc limit 10
}!
println('We found a total of ${uk_customers.len} customers matching the query.')
for c in uk_customers {
	println('customer: ${c.id}, ${c.name}, ${c.country}, ${c.nr_orders}')
}

none_country_customers := sql db {
	select from Customer where country is none
}!
println('We found a total of ${none_country_customers.len} customers, with no country set.')
for c in none_country_customers {
	println('customer: ${c.id}, ${c.name}, ${c.country}, ${c.nr_orders}')
}

// 删除客户
sql db {
	delete from Customer where name == 'Bob'
}!
```

更多示例和文档，请参阅 [vlib/orm](https://github.com/vlang/v/tree/master/vlib/orm)。

### 在 Windows 上解决 SQLite 编译问题
在 Windows 上，如果您遇到关于缺少 sqlite3.h 文件的编译错误，您必须运行：
`v vlib/db/sqlite/install_thirdparty_sqlite.vsh` 一次，然后重试编译。

### 使用自包含的 SQLite 模块
V 还维护一个单独的 `sqlite` 模块，它包装了 SQLite 合并，但除此之外
与 `db.sqlite` 模块具有相同的 API。它的好处是，使用它，您不需要
在系统上安装单独的系统级 sqlite 包/库（这在某些系统上可能很困难，
例如 Windows 或使用 musl 的系统）。
它的缺点是它可能使您的编译稍微慢一些（因为它从 C 编译 SQLite，
除了您自己的代码之外）。

要使用它，请执行：
```sh
v install sqlite
```
and later, in your code, use this:
```v ignore
import sqlite
```
instead of:
```v ignore
import db.sqlite
```

## 编写文档

它的工作方式与 Go 非常相似。它非常简单：不需要
为代码单独编写文档，
vdoc 将从源代码中的文档字符串生成它。

每个函数/类型/常量的文档必须放在声明之前：

```v
// clearall 清除数组中的所有位
fn clearall() {
}
```

注释必须以定义的名称开头。

有时一行不足以解释函数的作用，在这种情况下，注释应该
使用单行注释跨越到文档化的函数：

```v
// copy_all 递归地按值复制数组的所有元素，
// 如果 `dupes` 为 false，则在此过程中消除所有重复值。
fn copy_all(dupes bool) {
	// ...
}
```

按照约定，注释最好用*现在时*编写。

模块的概述必须放在模块名称之后的第一个注释中。

要生成文档，请使用 vdoc，例如 `v doc net.http`。

### 文档注释中的换行

跨越多行的注释使用空格合并在一起，除非

- 该行为空
- 该行纯粹由至少 3 个 `-`、`=`、`_`、`*`、`~` 组成（水平规则）
- 该行以至少一个 `#` 后跟空格开头（标题）
- 该行以 `|` 开头和结尾（表格）
- 该行以 `- ` 开头（列表）

## 工具

### v fmt

您不需要担心格式化代码或设置样式指南。
`v fmt` 会处理这些：

```shell
v fmt file.v
```

建议设置您的编辑器，以便在每次保存时运行 `v fmt -w`。
vfmt 运行通常很便宜（需要 <30ms）。

在推送代码之前，始终运行 `v fmt -w file.v`。

#### 在本地禁用格式化

要禁用代码块的格式化，请用 `// vfmt off` 和
`// vfmt on` 注释包装它。

```bash
// Not affected by fmt
// vfmt off

... your code here ...

// vfmt on

// Affected by fmt
... your code here ...
```

### v shader

您可以在 V 图形应用程序中使用 GPU 着色器。您可以在
[带注释的 GLSL 方言](https://github.com/vlang/v/blob/master/examples/sokol/02_cubes_glsl/cube_glsl.glsl)
中编写着色器，并使用 `v shader` 为所有支持的目标平台编译它们。

```shell
v shader /path/to/project/dir/or/file.v
```

目前您需要
[包含头文件并声明粘合函数](https://github.com/vlang/v/blob/master/examples/sokol/02_cubes_glsl/cube_glsl.v#L25-L28)
才能在代码中使用着色器。

### 性能分析

V 对性能分析有很好的支持：`v -profile profile.txt run file.v`
这将生成一个 profile.txt 文件，然后您可以分析它。

生成的 profile.txt 文件将包含 4 列的行：

1. 函数被调用的次数。
2. 函数总共花费的时间（以毫秒为单位）。
3. 函数自己花费的时间（以毫秒为单位），不包括其中的调用。
   当不使用 tcc 时，这对于多线程程序是可靠的。
4. 平均每次函数调用花费的时间（以纳秒为单位）。
5. v 函数的名称。

您可以使用以下命令按第 3 列（每个函数的平均时间）排序：
`sort -n -k3 profile.txt|tail`

您还可以使用秒表明确测量代码的特定部分：

```v
import time

fn main() {
	sw := time.new_stopwatch()
	println('Hello world')
	println('Greeting the world took: ${sw.elapsed().nanoseconds()}ns')
}
```

## 包管理

V *模块*是包含 .v 文件的单个文件夹。V *包*可以
包含一个或多个 V 模块。V *包*应该在其顶层文件夹中有一个 `v.mod` 文件，
描述包的内容。

V 包通常安装在您的 `~/.vmodules` 文件夹中。该
位置可以通过设置环境变量 `VMODULES` 来覆盖。

### 包命令

您可以使用 V 前端进行包操作，就像您可以
使用它来编译代码、格式化代码、审查代码等。

```powershell
v [package_command] [param]
```

其中包命令可以是以下之一：

```
   install           从 VPM 安装包。
   remove            删除从 VPM 安装的包。
   search            从 VPM 搜索包。
   update            从 VPM 更新已安装的包。
   upgrade           升级所有过时的包。
   list              列出所有已安装的包。
   outdated          显示需要更新的已安装包。
```

您可以使用 [VPM](https://vpm.vlang.io/) 安装其他人已经创建的包：

```powershell
v install [package]
```

**示例：**

```powershell
v install ui
```

包可以直接从 git 或 mercurial 存储库安装。

```powershell
v install [--once] [--git|--hg] [url]
```

**Example:**

```powershell
v install --git https://github.com/vlang/markdown
```

有时您可能只想在未安装依赖项时安装它们：

```
v install --once [package]
```

使用 v 删除包：

```powershell
v remove [package]
```

**示例：**

```powershell
v remove ui
```

从 [VPM](https://vpm.vlang.io/) 更新已安装的包：

```powershell
v update [package]
```

**示例：**

```powershell
v update ui
```

或者您可以更新所有包：

```powershell
v update
```

要查看您已安装的所有包，可以使用：

```powershell
v list
```

**示例：**

```powershell
> v list
Installed packages:
  markdown
  ui
```

要查看所有需要更新的包：

```powershell
v outdated
```

**示例：**

```powershell
> v outdated
包是最新的。
```

### 发布包

1. 在包的顶层文件夹中放置一个 `v.mod` 文件（如果您
   使用命令 `v new mypackage` 或 `v init` 创建包，
   您已经有一个 `v.mod` 文件）。

   ```sh
   v new mypackage
   Input your project description: My nice package.
   Input your project version: (0.0.0) 0.0.1
   Input your project license: (MIT)
   Initialising ...
   Complete!
   ```

   示例 `v.mod`：
   ```v ignore
   Module {
       name: 'mypackage'
       description: 'My nice package.'
       version: '0.0.1'
       license: 'MIT'
       dependencies: []
   }
   ```

   最小文件结构：
   ```
   v.mod
   mypackage.v
   ```

   您的包名称应该与包中所有文件顶部的 `module` 指令
   一起使用。对于 `mypackage.v`：
   ```v
   module mypackage

   pub fn hello_world() {
       println('Hello World!')
   }
   ```

2. 在包含 `v.mod` 文件的文件夹中创建 git 存储库
   （如果您使用 `v new` 或 `v init`，则不需要）：
   ```sh
   git init
   git add .
   git commit -m "INIT"
   ````

3. 在 github.com 上创建一个公共存储库。
4. 将本地存储库连接到远程存储库并推送更改。
5. 将您的包添加到公共 V 包注册表 VPM：
   https://vpm.vlang.io/new

   您必须使用 Github 帐户登录才能注册包。
   **警告：** _目前提交后无法编辑您的条目。
   请仔细检查您的包名称和 github url，因为您以后无法更改它。_
6. 最终的包名称是您的 github 帐户和
   您提供的包名称的组合，例如 `mygithubname.mypackage`。

**可选：** 在 github.com 上用 `vlang` 和 `vlang-package` 标记您的 V 包
以允许更好的搜索体验。

# 高级主题

## 属性

V 有几个修改函数和结构体行为的属性。

属性是在函数/结构体/枚举声明之前
在 `[]` 内指定的编译器指令，仅适用于以下声明。

```v
// @[flag] 使枚举类型可以用作位字段

@[flag]
enum BitField {
	read
	write
	other
}

fn main() {
	assert 1 == int(BitField.read)
	assert 2 == int(BitField.write)
	mut bf := BitField.read
	assert bf.has(.read | .other) // 测试是否设置了*至少一个*标志
	assert !bf.all(.read | .other) // 测试是否设置了*所有*标志
	bf.set(.write | .other)
	assert bf.has(.read | .write | .other)
	assert bf.all(.read | .write | .other)
	bf.toggle(.other)
	assert bf == BitField.read | .write
	assert bf.all(.read | .write)
	assert !bf.has(.other)
	empty := BitField.zero()
	assert empty.is_empty()
	assert !empty.has(.read)
	assert !empty.has(.write)
	assert !empty.has(.other)
	mut full := empty
	full.set_all()
	assert int(full) == 7 // 0x01 + 0x02 + 0x04
	assert full == .read | .write | .other
	mut v := full
	v.clear(.read | .other)
	assert v == .write
	v.clear_all()
	assert v == empty
	assert BitField.read == BitField.from('read')!
	assert BitField.other == BitField.from('other')!
	assert BitField.write == BitField.from(2)!
	assert BitField.zero() == BitField.from('')!
}
```

```v
// @[_allow_multiple_values] 允许枚举具有多个重复值。
// 请谨慎使用，仅在真正需要时使用。

@[_allow_multiple_values]
enum ButtonStyle {
	primary   = 1
	secondary = 2
	success   = 3

	blurple = 1
	grey    = 2
	gray    = 2
	green   = 3
}

fn main() {
	assert int(ButtonStyle.primary) == 1
	assert int(ButtonStyle.blurple) == 1

	assert int(ButtonStyle.secondary) == 2
	assert int(ButtonStyle.gray) == 2
	assert int(ButtonStyle.grey) == 2

	assert int(ButtonStyle.success) == 3
	assert int(ButtonStyle.green) == 3

	assert ButtonStyle.primary == ButtonStyle.blurple
	assert ButtonStyle.secondary == ButtonStyle.grey
	assert ButtonStyle.secondary == ButtonStyle.gray
	assert ButtonStyle.success == ButtonStyle.green
}
```

结构体字段弃用：

```v oksyntax
module abc

// 注意：只有*其他模块*中对 Xyz.d 的*直接*访问才会产生弃用通知/警告：
pub struct Xyz {
pub mut:
	a int
	d int @[deprecated: 'use Xyz.a instead'; deprecated_after: '2999-03-01']
	// 上面的标签将产生通知，因为弃用日期在遥远的未来
}
```

函数/方法弃用：

函数在最终删除之前会被弃用，以给用户时间迁移他们的代码。
在大多数情况下，添加日期是可取的。没有弃用日期的立即更改可能
用于在概念上被发现有缺陷并被更好的功能淘汰的
函数。除此之外，建议设置日期以给予用户宽限期。

弃用的函数会导致警告，如果使用 `-prod` 构建，这些警告会导致错误。为了避免立即
CI 中断，建议设置一个未来的日期，在代码合并日期之前。这
给积极开发 V 项目的人至少一次看到弃用通知并修复使用的机会。
设置未来 30 天的日期，假设他们会在该时间内至少手动编译一次他们的
项目。对于小的更改，这应该是足够的
时间。对于复杂的更改，这个时间可能需要更长。

不同的 V 项目和维护者可能会合理地选择不同的弃用策略。
根据更改的类型和影响，您可能希望在
弃用函数之前先与他们协商。


```v
// 调用此函数将导致弃用警告

@[deprecated]
fn old_function() {
}

// 它也可以显示自定义弃用消息

@[deprecated: 'use new_function() instead']
fn legacy_function() {}

// 您还可以指定一个日期，在此之后函数将被
// 视为已弃用。在该日期之前，对函数的调用
// 将是编译器通知 - 您会看到它们，但编译
// 不受影响。在该日期之后，调用将变为警告，
// 因此普通编译仍然有效，但使用 -prod 编译
// 将不会（所有警告都被视为 -prod 的错误）。
// 弃用日期后 6 个月，调用将是硬
// 编译器错误。

@[deprecated: 'use new_function2() instead']
@[deprecated_after: '2021-05-27']
fn legacy_function2() {}
```

```v globals
// 此函数的调用将被内联。
@[inline]
fn inlined_function() {
}

// 此函数的调用将不会被内联。
@[noinline]
fn function() {
}

// 此函数不会返回到其调用者。
// 此类函数可以在 or 块的末尾使用，
// 就像 exit/1 或 panic/1 一样。此类函数不能
// 有返回类型，应该以 for{} 结束，或
// 通过调用其他 `[noreturn]` 函数。
@[noreturn]
fn forever() {
	for {}
}

// 以下结构体必须在堆上分配。因此，它只能用作
// 引用（`&Window`）或在另一个引用内（`&OuterStruct{ Window{...} }`）。
// 参见"栈和堆"部分
@[heap]
struct Window {
}

// 对以下函数的调用必须在 unsafe{} 块中。
// 请注意，`risky_business()` 主体中的代码仍将被
// 检查，除非您也将其包装在 `unsafe {}` 块中。
// 这很有用，当您想要一个 `[unsafe]` 函数，该函数
// 在某个不安全操作之前/之后进行检查，仍将
// 受益于 V 的安全功能。
@[unsafe]
fn risky_business() {
	// 将被检查的代码，可能检查前置条件
	unsafe {
		// *不会被*检查的代码，如指针算术、
		// 访问联合体字段、调用其他 `[unsafe]` 函数等...
		// 通常，尝试最小化包装在
		// unsafe{} 中的代码是一个好主意。
		// 另请参阅[内存不安全代码](#memory-unsafe-code)
	}
	// 将被检查的代码，可能检查后置条件和/或
	// 保持不变量
}

// V 的 autofree 引擎不会在此函数中处理内存管理。
// 您将负责在其中手动释放内存。
// 注意：它与垃圾收集器无关。它只会使
// -autofree 机制忽略该函数的主体。
@[manualfree]
fn custom_allocations() {
}

// 此函数的指针参数指向的内存不会在函数返回之前
// 被垃圾收集器（如果正在使用）释放
// 仅用于 C 互操作。
@[keep_args_alive]
fn C.my_external_function(voidptr, int, voidptr) int

// @[weak] 标签告诉 C 编译器，下一个声明将是弱声明，即在链接时，
// 如果有另一个同名符号的声明（一个"强"声明），应该
// 使用它，*不会出现关于重复符号的链接器错误*。
// 仅用于 C 互操作。

@[weak]
__global abc = u64(1)

// 告诉 V，以下全局变量是在 C 端定义的，
// 因此 V 不会初始化它，而只会让您访问它。
// 仅用于 C 互操作。

@[c_extern]
__global my_instance C.my_struct
struct C.my_struct {
	a int
	b f64
}

// 告诉 V 以下结构体在 C 中使用 `typedef struct` 定义。
// 仅用于 C 互操作。
@[typedef]
pub struct C.Foo {}

// 用于向函数添加自定义调用约定，可用的调用约定：stdcall、fastcall 和 cdecl。
// 此列表也适用于类型别名（见下文）。
// 仅用于 C 互操作。
@[callconv: 'stdcall']
fn C.DefWindowProc(hwnd int, msg int, lparam int, wparam int)

// 用于向函数类型别名添加自定义调用约定。
// 仅用于 C 互操作。

@[callconv: 'fastcall']
type FastFn = fn (int) bool

// 对以下函数的调用必须以某种方式使用其返回值。
// 忽略它，将发出警告。
@[must_use]
fn f() int {
	return 42
}

fn g() {
	// 仅在这里调用 `f()`，将产生警告
	println(f()) // 这很好，因为返回值被用作参数
}

// 仅 Windows（已过时；相反，在编译时使用 `-subsystem windows`）
// 没有此属性，所有图形应用程序在 Windows 上都将具有以下行为：
// 如果从控制台或终端运行；保持终端打开，以便可以查看所有 (e)println 语句。
// 如果从例如资源管理器运行，通过双击；应用程序打开，但不打开终端，并且看不到
// (e)println 输出。
// 使用它强制打开终端以查看输出，即使应用程序是从资源管理器启动的。
// 仅在 main() 之前有效。
@[console]
fn main() {
}
```

## 条件编译

此功能的目标是告诉 V 在最终
可执行文件中*不编译*一个函数及其所有调用，如果未传递提供的自定义标志。

V 仍将类型检查函数及其所有调用，*即使*由于传递的 -d 标志，
它们不会出现在最终可执行文件中。

要查看它的作用，请使用 `v run example.v` 运行以下示例一次，
然后使用 `v -d trace_logs example.v` 运行第二次：
```v
@[if trace_logs ?]
fn elog(s string) {
	eprintln(s)
}

fn main() {
	elog('some expression: ${2 + 2}') // 如果未传递 `-d trace_logs`，此类调用将*根本*不会执行
	println('hi')
	elog('finish')
}
```

基于自定义标志的条件编译也可以用于生成略有不同的
可执行文件，它们共享大部分相同的代码，但其中一些逻辑只在
某些时候需要，例如网络服务器/客户端程序可以这样编写：
```v ignore
fn act_as_client() { ... }
fn act_as_server() { ... }
fn main() {
	$if as_client ? {
		act_as_client()
	}
	$if as_server ? {
		act_as_server()
	}
}
```
要生成 `client.exe` 可执行文件，请执行：`v -d as_client -o client.exe .`
要生成 `server.exe` 可执行文件，请执行：`v -d as_server -o server.exe .`

### 编译时伪变量

V 还允许您的代码访问一组伪字符串变量，
它们在编译时被替换：

- `@FN` => 替换为当前 V 函数的名称。
- `@METHOD` => 替换为 ReceiverType.MethodName。
- `@MOD` => 替换为当前 V 模块的名称。
- `@STRUCT` => 替换为当前 V 结构体的名称。
- `@FILE` => 替换为 V 源文件的绝对路径。
- `@DIR` => 替换为 V 源文件所在*文件夹*的绝对路径。
- `@LINE` => 替换为它出现的 V 行号（作为字符串）。
- `@FILE_LINE` => 类似于 `@FILE:@LINE`，但文件部分是相对路径。
- `@LOCATION` => 当前类型 + 方法的文件、行和名称；适用于日志记录。
- `@COLUMN` => 替换为它出现的列（作为字符串）。
- `@VEXE` => 替换为 V 编译器的路径。
- `@VEXEROOT`  => 将被替换为 V 可执行文件所在的*文件夹*
  （作为字符串）。
- `@VHASH`  => 替换为 V 编译器的缩短提交哈希（作为字符串）。
- `@VCURRENTHASH` => 类似于 `@VHASH`，但在编译器
  在不同提交上重新编译时更改（在本地修改后，或
  使用 git bisect 等之后）。
- `@VMOD_FILE` => 替换为最近的 v.mod 文件的内容（作为字符串）。
- `@VMODHASH` => 被替换为缩短的提交哈希，从最近的 v.mod 文件旁边的 .git 目录
  派生（作为字符串）。
- `@VMODROOT` => 将被替换为最近的 v.mod 文件所在的*文件夹*
  （作为字符串）。
- `@BUILD_DATE` => 替换为构建日期，例如 '2024-09-13'。
- `@BUILD_TIME` => 替换为构建时间，例如 '12:32:07'。
- `@BUILD_TIMESTAMP` => 替换为构建时间戳，例如 '1726219885'。
- `@OS` => 替换为 OS 类型，例如 'linux'。
- `@CCOMPILER` => 替换为 C 编译器类型，例如 'gcc'。
- `@BACKEND` => 替换为当前语言后端，例如 'c' 或 'golang'。
- `@PLATFORM` => 替换为平台类型，例如 'amd64'。
注意：`@BUILD_DATE`、`@BUILD_TIME`、`@BUILD_TIMESTAMP` 表示 UTC 时区的时间。
默认情况下，它们基于编译/构建的当前时间。可以通过
设置环境变量 `SOURCE_DATE_EPOCH` 来覆盖它们。这在制作
发布版本时也很有用，因为您可以在构建系统/脚本中使用等效的：
`export SOURCE_DATE_EPOCH=$(git log -1 --pretty=%ct) ;`，然后在程序内部使用 `@BUILD_DATE` 等，
例如当您向用户打印版本信息时。
另请参阅 https://reproducible-builds.org/docs/source-date-epoch/。

编译时伪变量允许您执行以下
示例，这在调试/日志记录/跟踪代码时很有用：

```v
eprintln(@LOCATION)
```

另一个示例是，如果您想将 v.mod 中的版本/名称嵌入到可执行文件*内部*：

```v ignore
import v.vmod

vm := vmod.decode( @VMOD_FILE )!
eprintln('${vm.name} ${vm.version}\n${vm.description}')
```

打印自己源代码的程序（一个 quine）：
```v
print($embed_file(@FILE).to_string())
```

> [!NOTE]
> 您可以在文件中拥有任意源代码，没有问题，因为完整文件
> 将被嵌入到通过编译它生成的可执行文件中。还要注意，打印
> 使用 `print` 而不是 `println`，以不添加源代码中缺少的另一个换行符。

打印其构建时间的程序：
```v
import time

println('This program, was compiled at ${time.unix(@BUILD_TIMESTAMP.i64()).format_ss_milli()} .')
```

### 编译时反射

`$` 用作编译时（也称为 'comptime'）操作的前缀。

内置 JSON 支持很好，但 V 还允许您为任何数据格式创建高效的
序列化器。V 有编译时 `if` 和 `for` 构造：

#### <h4 id="comptime-fields">.fields</h4>

您可以使用 `.fields` 迭代结构体字段，它也适用于泛型类型
（例如 `T.fields`）和泛型参数（例如 `param.fields`，其中 `fn gen[T](param T) {`）。

```v
struct User {
	name string
	age  int
}

fn main() {
	$for field in User.fields {
		$if field.typ is string {
			println('${field.name} is of type string')
		}
	}
}

// Output:
// name is of type string
```

#### <h4 id="comptime-values">.values</h4>

您可以读取[枚举](#enums)值及其属性。

```v
enum Color {
	red   @[RED]  // first attribute
	blue  @[BLUE] // second attribute
}

fn main() {
	$for e in Color.values {
		println(e.name)
		println(e.attrs)
	}
}

// Output:
// red
// ['RED']
// blue
// ['BLUE']
```

#### <h4 id="comptime-attrs">.attributes</h4>

您可以读取[结构体](#structs)属性。

```v
@[COLOR]
struct Foo {
	a int
}

fn main() {
	$for e in Foo.attributes {
		println(e)
	}
}

// Output:
// StructAttribute{
//    name: 'COLOR'
//    has_arg: false
//    arg: ''
//    kind: plain
// }
```

#### <h4 id="comptime-variants">.variants</h4>

You can read variant types from [Sum type](#sum-types).

```v
type MySum = int | string

fn main() {
	$for v in MySum.variants {
		$if v.typ is int {
			println('has int type')
		} $else $if v.typ is string {
			println('has string type')
		}
	}
}

// Output:
// has int type
// has string type
```

#### <h4 id="comptime-methods">.methods</h4>

您可以检索有关结构体方法的信息。

```v
struct Foo {
}

fn (f Foo) test() int {
	return 123
}

fn (f Foo) test2() string {
	return 'foo'
}

fn main() {
	foo := Foo{}
	$for m in Foo.methods {
		$if m.return_type is int {
			print('${m.name} returns int: ')
			println(foo.$method())
		} $else $if m.return_type is string {
			print('${m.name} returns string: ')
			println(foo.$method())
		}
	}
}

// Output:
// test returns int: 123
// test2 returns string: foo
```

#### <h4 id="comptime-method-params">.params</h4>

您可以检索有关结构体方法参数的信息。

```v
struct Test {
}

fn (t Test) foo(arg1 int, arg2 string) {
}

fn main() {
	$for m in Test.methods {
		$for param in m.params {
			println('${typeof(param.typ).name}: ${param.name}')
		}
	}
}

// Output:
// int: arg1
// string: arg2
```

See [`examples/compiletime/reflection.v`](/examples/compiletime/reflection.v)
for a more complete example.

### 编译时代码

#### `$if` condition

```v
fn main() {
	// Support for multiple conditions in one branch
	$if ios || android {
		println('Running on a mobile device!')
	}
	$if linux && x64 {
		println('64-bit Linux.')
	}
	// Usage as expression
	os := $if windows { 'Windows' } $else { 'UNIX' }
	println('Using ${os}')
	// $else-$if branches
	$if tinyc {
		println('tinyc')
	} $else $if clang {
		println('clang')
	} $else $if gcc {
		println('gcc')
	} $else {
		println('different compiler')
	}
	$if test {
		println('testing')
	}
	// v -cg ...
	$if debug {
		println('debugging')
	}
	// v -prod ...
	$if prod {
		println('production build')
	}
	// v -d option ...
	$if option ? {
		println('custom option')
	}
}
```

如果您希望 `if` 在编译时求值，它必须以 `$` 符号为前缀。
现在它可以用于检测操作系统、编译器、平台或编译选项。
`$if debug` is a special option like `$if windows` or `$if x32`, it's enabled if the program
is compiled with `v -g` or `v -cg`.
If you're using a custom ifdef, then you do need `$if option ? {}` and compile with`v -d option`.
Full list of builtin options:

| OS                             | Compilers        | Platforms                     | Other                                         |
|--------------------------------|------------------|-------------------------------|-----------------------------------------------|
| `windows`, `linux`, `macos`    | `gcc`, `tinyc`   | `amd64`, `arm64`, `aarch64`   | `debug`, `prod`, `test`                       |
| `darwin`, `ios`, `bsd`         | `clang`, `mingw` | `i386`, `arm32`               | `js`, `glibc`, `prealloc`                     |
| `freebsd`, `openbsd`, `netbsd` | `msvc`           | `rv64`, `rv32`, `s390x`       | `no_bounds_checking`, `freestanding`          |
| `android`, `mach`, `dragonfly` | `cplusplus`      | `ppc64le`                     | `no_segfault_handler`, `no_backtrace`         |
| `gnu`, `hpux`, `haiku`, `qnx`  |                  | `x64`, `x32`                  | `no_main`, `fast_math`, `apk`, `threads`      |
| `solaris`, `termux`            |                  | `little_endian`, `big_endian` | `js_node`, `js_browser`, `js_freestanding`    |
| `serenity`, `vinix`, `plan9`   |                  |                               | `interpreter`, `es5`, `profile`, `wasm32`     |
|                                |                  |                               | `wasm32_emscripten`, `wasm32_wasi`            |
|                                |                  |                               | `native`, `autofree`                          |

#### `$embed_file`

```v ignore
import os
fn main() {
	embedded_file := $embed_file('v.png')
	os.write_file('exported.png', embedded_file.to_string())!
}
```

V can embed arbitrary files into the executable with the `$embed_file(<path>)`
compile time call. Paths can be absolute or relative to the source file.

Note that by default, using `$embed_file(file)`, will always embed the whole content
of the file, but you can modify that behaviour by passing: `-d embed_only_metadata`
when compiling your program. In that case, the file will not be embedded. Instead,
it will be loaded *the first time* your program calls `embedded_file.data()` at runtime,
making it easier to change in external editor programs, without needing to recompile
your program.

Embedding a file inside your executable, will increase its size, but
it will make it more self contained and thus easier to distribute.
When that happens (the default), `embedded_file.data()` will cause *no IO*,
and it will always return the same data.

`$embed_file` supports compression of the embedded file when compiling with `-prod`.
Currently only one compression type is supported: `zlib`.

```v ignore
import os
fn main() {
	embedded_file := $embed_file('x.css', .zlib) // compressed using zlib
	os.write_file('exported.css', embedded_file.to_string())!
}
```

注意：压缩像 png 或 zip 文件这样的二进制资源通常不会带来太多好处，
在某些情况下甚至可能在最终可执行文件中占用更多空间，因为它们
已经被压缩了。

`$embed_file` returns
[EmbedFileData](https://modules.vlang.io/v.embed_file.html#EmbedFileData)
which could be used to obtain the file contents as `string` or `[]u8`.

#### `$tmpl` for embedding and parsing V template files

V has a simple template language for text and html templates, and they can easily
be embedded via `$tmpl('path/to/template.txt')`:

```v ignore
fn build() string {
	name := 'Peter'
	age := 25
	numbers := [1, 2, 3]
	return $tmpl('1.txt')
}

fn main() {
	println(build())
}
```

1.txt:

```
name: @name

age: @age

numbers: @numbers

@for number in numbers
  @number
@end
```

output:

```
name: Peter

age: 25

numbers: [1, 2, 3]

1
2
3
```

See more [details](https://github.com/vlang/v/blob/master/vlib/v/TEMPLATES.md)

#### `$env`

```v
module main

fn main() {
	compile_time_env := $env('ENV_VAR')
	println(compile_time_env)
}
```

V can bring in values at compile time from environment variables.
`$env('ENV_VAR')` can also be used in top-level `#flag` and `#include` statements:
`#flag linux -I $env('JAVA_HOME')/include`.

#### `$d`

V can bring in values at compile time from `-d ident=value` flag defines, passed on
the command line to the compiler. You can also pass `-d ident`, which will have the
same meaning as passing `-d ident=true`.

To get the value in your code, use: `$d('ident', default)`, where `default`
can be `false` for booleans, `0` or `123` for i64 numbers, `0.0` or `113.0`
for f64 numbers, `'a string'` for strings.

When a flag is not provided via the command line, `$d()` will return the `default`
value provided as the *second* argument.

```v
module main

const my_i64 = $d('my_i64', 1024)

fn main() {
	compile_time_value := $d('my_string', 'V')
	println(compile_time_value)
	println(my_i64)
}
```

Running the above with `v run .` will output:
```
V
1024
```

Running the above with `v -d my_i64=4096 -d my_string="V rocks" run .` will output:
```
V rocks
4096
```

Here is an example of how to use the default values, which have to be *pure* literals:
```v
fn main() {
	val_str := $d('id_str', 'value') // can be changed by providing `-d id_str="my id"`
	val_f64 := $d('id_f64', 42.0) // can be changed by providing `-d id_f64=84.0`
	val_i64 := $d('id_i64', 56) // can be changed by providing `-d id_i64=123`
	val_bool := $d('id_bool', false) // can be changed by providing `-d id_bool=true`
	val_char := $d('id_char', `f`) // can be changed by providing `-d id_char=v`
	println(val_str)
	println(val_f64)
	println(val_i64)
	println(val_bool)
	println(rune(val_char))
}
```

`$d('ident','value')` can also be used in top-level statements like `#flag` and `#include`:
`#flag linux -I $d('my_include','/usr')/include`. The default value for `$d` when used in these
statements should be literal `string`s.

`$d('ident', false)` can also be used inside `$if $d('ident', false) {` statements,
granting you the ability to selectively turn on/off certain sections of code, at compile
time, without modifying your source code, or keeping different versions of it.

#### `$compile_error` and `$compile_warn`

These two comptime functions are very useful for displaying custom errors/warnings during
compile time.

Both receive as their only argument a string literal that contains the message to display:

```v failcompile nofmt
// x.v
module main

$if linux {
    $compile_error('Linux is not supported')
}

fn main() {
}

$ v run x.v
x.v:4:5: error: Linux is not supported
    2 |
    3 | $if linux {
    4 |     $compile_error('Linux is not supported')
      |     ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    5 | }
    6 |
```

### 编译时类型

Compile time types group multiple types into a general higher-level type. This is useful in
functions with generic parameters, where the input type must have a specific property, for example
the `.len` attribute in arrays.

V supports the following compile time types:

- `$alias` => matches [Type aliases](#type-aliases).
- `$array` => matches [Arrays](#arrays) and [Fixed Size Arrays](#fixed-size-arrays).
- `$array_dynamic` => matches [Arrays](#arrays), but not [Fixed Size Arrays](#fixed-size-arrays).
- `$array_fixed` => matches [Fixed Size Arrays](#fixed-size-arrays), but not [Arrays](#arrays)
- `$enum` => matches [Enums](#enums).
- `$float` => matches `f32`, `f64` and float literals.
- `$function` => matches [Function Types](#function-types).
- `$int` => matches `int`, `i8`, `i16`, `i32`, `i64`, `u8`, `u16`, `u32`, `u64`, `isize`, `usize`
  and integer literals.
- `$interface` => matches [Interfaces](#interfaces).
- `$map` => matches [Maps](#maps).
- `$option` => matches [Option Types](#optionresult-types-and-error-handling).
- `$shared` => matches [Shared Types](#shared-objects).
- `$struct` => matches [Structs](#structs).
- `$sumtype` => matches [Sum Types](#sum-types).
- `$string` => matches [Strings](#strings).
- `$pointer` => matches [Reference Types](#references).
- `$voidptr` => matches C's `void*`.

### 环境特定文件

如果文件具有特定于环境的后缀，它将仅针对该环境编译。

- `.js.v` => will be used only by the JS backend. These files can contain JS. code.
- `.c.v` => will be used only by the C backend. These files can contain C. code.
- `.native.v` => will be used only by V's native backend.
- `_nix.c.v` => will be used only on Unix systems (non Windows).
- `_${os}.c.v` => will be used only on the specific `os` system.
  For example, `_windows.c.v` will be used only when compiling on Windows, or with `-os windows`.
- `_default.c.v` => will be used only if there is NOT a more specific platform file.
  For example, if you have both `file_linux.c.v` and `file_default.c.v`,
  and you are compiling for linux, then only `file_linux.c.v` will be used,
  and `file_default.c.v` will be ignored.

Here is a more complete example:

`main.v`:

```v ignore
module main
fn main() { println(message) }
```

`main_default.c.v`:

```v ignore
module main
const message = 'Hello world'
```

`main_linux.c.v`:

```v ignore
module main
const message = 'Hello linux'
```

`main_windows.c.v`:

```v ignore
module main
const message = 'Hello windows'
```

With the example above:

- when you compile for Windows, you will get `Hello windows`
- when you compile for Linux, you will get `Hello linux`
- when you compile for any other platform, you will get the
  non specific `Hello world` message.

- `_d_customflag.v` => will be used *only* if you pass `-d customflag` to V.
  That corresponds to `$if customflag ? {}`, but for a whole file, not just a
  single block. `customflag` should be a snake_case identifier, it can not
  contain arbitrary characters (only lower case latin letters + numbers + `_`).
  > **Note**
  >
  > A combinatorial `_d_customflag_linux.c.v` postfix will not work.
  > If you do need a custom flag file, that has platform dependent code, use the
  > postfix `_d_customflag.v`, and then use platform dependent compile time
  > conditional blocks inside it, i.e. `$if linux {}` etc.

- `_notd_customflag.v` => similar to _d_customflag.v, but will be used
  *only* if you do NOT pass `-d customflag` to V.

See also [Cross Compilation](#cross-compilation).

## 调试器

To use the native *V debugger*, add the `$dbg` statement to your source, where you
want the debugger to be invoked.

```v
fn main() {
	a := 1
	$dbg;
}
```

Running this V code, you will get the debugger REPL break when the execution
reaches the `$dbg` statement.

```
$ v run example.v
Break on [main] main in example.v:3
example.v:3 vdbg>
```

此时，执行已停止，调试器现在可用。

要查看可用命令，请键入
?、h 或 help。（命令补全有效 - 仅限非 Windows）

```
example.v:3 vdbg> ?
vdbg commands:
  anon?                 check if the current context is anon
  bt                    prints a backtrace
  c, continue           continue debugging
  generic?              check if the current context is generic
  heap                  show heap memory usage
  h, help, ?            show this help
  l, list [lines]       show some lines from current break (default: 3)
  mem, memory           show memory usage
  method?               check if the current context is a method
  m, mod                show current module name
  p, print <var>        prints an variable
  q, quit               exits debugging session in the code
  scope                 show the vars in the current scope
  u, unwatch <var>      unwatches a variable
  w, watch <var>        watches a variable
```

让我们尝试 `scope` 命令，以检查当前作用域上下文。

```
example.v:3 vdbg> scope
a = 1 (int)
```

太好了！我们有变量名、它的值和它的类型名。

What about printing only a variable, not the whole scope?

Just type `p a`.

To watch a variable by its name, use:

`w a` (where `a` is the variable name)

To stop watching the variable (`unwatch` it), use `u a`.

Lets see more one example:

```
fn main() {
	for i := 0; i < 4; i++ {
		$dbg
	}
}
```

Running again, we'll get:
`Break on [main] main in example.v:3`

如果我们想读取源代码上下文，可以使用 `l` 或 `list` 命令。

```
example.v:3 vdbg> l
0001  fn main() {
0002    for i := 0; i < 4; i++ {
0003>           $dbg
0004    }
0005  }
```

The default is read 3 lines before and 3 lines after, but you can
pass a parameter to the command to read more lines, like `l 5`.

现在，让我们观察这个循环中变量的变化。

```
example.v:3 vdbg> w i
i = 0 (int)
```

要继续到下一个断点，请键入 `c` 或 `continue` 命令。

```
example.v:3 vdbg> c
Break on [main] main in example.v:3
i = 1 (int)
```

`i` and it's value is automatically printed, because it is in the watch list.

To repeat the last command issued, in this case the `c` command,
just hit the *enter* key.

```
example.v:3 vdbg>
Break on [main] main in example.v:3
i = 2 (int)
example.v:3 vdbg>
Break on [main] main in example.v:3
i = 3 (int)
example.v:3 vdbg>
```

You can also see memory usage with `mem` or `memory` command, and
check if the current context is an anon function (`anon?`), a method (`method?`)
or a generic method (`generic?`) and clear the terminal window (`clear`).

## 调用栈

You can also show the current call stack with `v.debug`.

To enable this feature, add the `-d callstack` switch when building or running
your code:

```v
import v.debug

fn test(i int) {
	if i > 9 {
		debug.dump_callstack()
	}
}

fn do_something() {
	for i := 0; i <= 10; i++ {
		test(i)
	}
}

fn main() {
	do_something()
}
```

```
$ v -d callstack run example.v
Backtrace:
--------------------------------------------------
example.v:16   | > main.main
example.v:11   |  > main.do_something
example.v:5    |   > main.test
--------------------------------------------------
```

## 跟踪

Another feature of `v.debug` is the possibility to add hook functions
before and after each function call.

To enable this feature, add the `-d trace` switch when building or running
your code:

```v
import v.debug

fn main() {
	hook1 := debug.add_before_call(fn (fn_name string) {
		println('> before ${fn_name}')
	})
	hook2 := debug.add_after_call(fn (fn_name string) {
		println('> after ${fn_name}')
	})
	anon := fn () {
		println('call')
	}
	anon()

	// optionally you can remove the hooks:
	debug.remove_before_call(hook1)
	debug.remove_after_call(hook2)
	anon()
}
```

```
$ v -d trace run example.v
> before anon
call
> after anon
call
```

## 内存不安全代码

Sometimes for efficiency you may want to write low-level code that can potentially
corrupt memory or be vulnerable to security exploits. V supports writing such code,
but not by default.

V requires that any potentially memory-unsafe operations are marked intentionally.
Marking them also indicates to anyone reading the code that there could be
memory-safety violations if there was a mistake.

Examples of potentially memory-unsafe operations are:

* Pointer arithmetic
* Pointer indexing
* Conversion to pointer from an incompatible type
* Calling certain C functions, e.g. `free`, `strlen` and `strncmp`.

To mark potentially memory-unsafe operations, enclose them in an `unsafe` block:

```v wip
// allocate 2 uninitialized bytes & return a reference to them
mut p := unsafe { malloc(2) }
p[0] = `h` // Error: pointer indexing is only allowed in `unsafe` blocks
unsafe {
    p[0] = `h` // OK
    p[1] = `i`
}
p++ // Error: pointer arithmetic is only allowed in `unsafe` blocks
unsafe {
    p++ // OK
}
assert *p == `i`
```

Best practice is to avoid putting memory-safe expressions inside an `unsafe` block,
so that the reason for using `unsafe` is as clear as possible. Generally any code
you think is memory-safe should not be inside an `unsafe` block, so the compiler
can verify it.

If you suspect your program does violate memory-safety, you have a head start on
finding the cause: look at the `unsafe` blocks (and how they interact with
surrounding code).

> [!NOTE]
> This is work in progress.

## 带引用字段的结构体

Structs with references require explicitly setting the initial value to a
reference value unless the struct already defines its own initial value.

Zero-value references, or nil pointers, will **NOT** be supported in the future,
for now data structures such as Linked Lists or Binary Trees that rely on reference
fields that can use the value `0`, understanding that it is unsafe, and that it can
cause a panic.

```v
struct Node {
	a &Node
	b &Node = unsafe { nil } // Auto-initialized to nil, use with caution!
}

// Reference fields must be initialized unless an initial value is declared.
// Nil is OK but use with caution, it's a nil pointer.
foo := Node{
	a: unsafe { nil }
}
bar := Node{
	a: &foo
}
baz := Node{
	a: unsafe { nil }
	b: unsafe { nil }
}
qux := Node{
	a: &foo
	b: &bar
}
println(baz)
println(qux)
```

## sizeof 和 __offsetof

* `sizeof(Type)` 给出类型的字节大小。
* `__offsetof(Struct, field_name)` 给出结构体字段的字节偏移量。

```v
struct Foo {
	a int
	b int
}

assert sizeof(Foo) == 8
assert __offsetof(Foo, a) == 0
assert __offsetof(Foo, b) == 4
```

## 受限运算符重载

运算符重载定义了某些类型对某些二元运算符的行为。

```v
struct Vec {
	x int
	y int
}

fn (a Vec) str() string {
	return '{${a.x}, ${a.y}}'
}

fn (a Vec) + (b Vec) Vec {
	return Vec{a.x + b.x, a.y + b.y}
}

fn (a Vec) - (b Vec) Vec {
	return Vec{a.x - b.x, a.y - b.y}
}

fn main() {
	a := Vec{2, 3}
	b := Vec{4, 5}
	mut c := Vec{1, 2}

	println(a + b) // "{6, 8}"
	println(a - b) // "{-2, -2}"
	c += a
	//^^ autogenerated from + overload
	println(c) // "{3, 5}"
}
```

> Operator overloading goes against V's philosophy of simplicity and predictability.
> But since scientific and graphical applications are among V's domains,
> operator overloading is an important feature to have in order to improve readability:
>
> `a.add(b).add(c.mul(d))` is a lot less readable than `a + b + c * d`.

Operator overloading is possible for the following binary operators: `+, -, *, /, %, <, ==`.

### 隐式生成的重载

- `==` is automatically generated by the compiler, but can be overridden.

- `!=`, `>`, `<=` and `>=` are automatically generated when `==` and `<` are defined.
  They cannot be explicitly overridden.
- Assignment operators (`*=`, `+=`, `/=`, etc) are automatically generated when the corresponding
  operators are defined and the operands are of the same type.
  They cannot be explicitly overridden.

### 限制

为了提高安全性和可维护性，运算符重载是有限的。

#### 类型限制

- When overriding `<` and `==`, the return type must be strictly `bool`.
- Both arguments must have the same type (just like with all operators in V).
- Overloaded operators have to return the same type as the argument
  (the exceptions are `<` and `==`).

#### 其他限制

- Arguments cannot be changed inside overloads.
- Calling other functions inside operator functions is not allowed (**planned**).

## 性能调优

When compiled with `-prod`, V's generated C code usually performs well. However, in specialized
scenarios, additional compiler flags and attributes can further optimize the executable for
performance, memory usage, or size.

> [!NOTE]
> These are *rarely* needed, and should not be used unless you
> *profile your code*, and then see that there are significant benefits for them.
> To cite GCC's documentation: "Programmers are notoriously bad at predicting
> how their programs actually perform".

| Tuning Operation         | Benefits                        | Drawbacks                                         |
|--------------------------|---------------------------------|---------------------------------------------------|
| `@[inline]`              | Performance                     | Increased executable size                         |
| `@[direct_array_access]` | Performance                     | Safety risks                                      |
| `@[packed]`              | Memory usage                    | Potential performance loss                        |
| `@[minify]`              | Performance, Memory usage       | May break binary serialization/reflection         |
| `_likely_/_unlikely_`    | Performance                     | Risk of negative performance impact               |
| `-fast-math`             | Performance                     | Risk of incorrect mathematical operations results |
| `-d no_segfault_handler` | Compile time, Size              | Loss of segfault trace                            |
| `-cflags -march=native`  | Performance                     | Risk of reduced CPU compatibility                 |
| `-compress`              | Size                            | Harder to debug, extra dependency `upx`           |
| `PGO`                    | Performance, Size               | Usage complexity                                  |

### 调优操作详情

#### `@[inline]`

You can tag functions with `@[inline]`, so the C compiler will try to inline them, which in some
cases, may be beneficial for performance, but may impact the size of your executable.

**When to Use**

- Functions that are called frequently in performance-critical loops.

**When to Avoid**

- Large functions, as it might cause code bloat and actually decrease performance.
- Large functions in `if` expressions - may have negative impact on instructions cache.

#### `@[direct_array_access]`

In functions tagged with `@[direct_array_access]` the compiler will translate array operations
directly into C array operations - omitting bounds checking. This may save a lot of time in a
function that iterates over an array but at the cost of making the function unsafe - unless the
boundaries will be checked by the user.

**When to Use**

- In tight loops that access array elements, where bounds have been manually verified or you are
sure that the access index will be valid.

**When to Avoid**

- Everywhere else.

#### `@[packed]`

The `@[packed]` attribute can be applied to a structure to create an unaligned memory layout,
which decreases the overall memory footprint of the structure. Using the `@[packed]` attribute
may negatively impact performance or even be prohibited on certain CPU architectures.

**When to Use**

- When memory usage is more critical than performance, e.g., in embedded systems.

**When to Avoid**

- On CPU architectures that do not support unaligned memory access or when high-speed memory access
is needed.

#### `@[aligned]`

The `@[aligned]` attribute can be applied to a structure or union to specify a minimum alignment
(in bytes) for variables of that type. Using the `@[aligned]` attribute you can only *increase*
the default alignment. Use `@[packed]` if you want to *decrease* it. The alignment of any struct
or union, should be at least a perfect multiple of the lowest common multiple of the alignments of
all of the members of the struct or union.

Example:
```v
// Each u16 in the `data` field below, takes 2 bytes, and we have 3 of them = 6 bytes.
// The smallest power of 2, bigger than 6 is 8, i.e. with `@[aligned]`, the alignment
// for the entire struct U16s, will be 8:
@[aligned]
struct U16s {
	data [3]u16
}
```
**When to Use**

- Only if the instances of your types, will be used in performance critical sections, or with
specialised machine instructions, that do require a specific alignment to work.

**When to Avoid**

- On CPU architectures, that do not support unaligned memory access. If you are not working on
performance critical algorithms, you do not really need it, since the proper minimum alignment
is CPU specific, and the compiler already usually will choose a good default for you.

> [!NOTE]
> You can leave out the alignment factor, i.e. use just `@[aligned]`, in which case the compiler
> will align a type to the maximum useful alignment for the target machine you are compiling for,
> i.e. the alignment will be the largest alignment which is ever used for any data type on the
> target machine. Doing this can often make copy operations more efficient, because the compiler
> can choose whatever instructions copy the biggest chunks of memory, when performing copies to or
> from the variables which have types that you have aligned this way.

See also ["What Every Programmer Should Know About Memory", by Ulrich Drepper](https://people.freebsd.org/~lstewart/articles/cpumemory.pdf) .

#### `@[minify]`

The `@[minify]` attribute can be added to a struct, allowing the compiler to reorder the fields in
a way that minimizes internal gaps while maintaining alignment. Using the `@[minify]` attribute may
cause issues with binary serialization or reflection. Be mindful of these potential side effects
when using this attribute.

**When to Use**

- When you want to minimize memory usage and you're not using binary serialization or reflection.

**When to Avoid**

- When using binary serialization or reflection, as it may cause unexpected behavior.

#### `_likely_/_unlikely_`

`if _likely_(bool expression) {` - 向 C 编译器提示，传递的布尔表达式
很可能为真，因此它可以生成汇编代码，减少分支预测错误的机会。
在 JS 后端中，这不起作用。

`if _unlikely_(bool expression) {` 类似于 `_likely_(x)`，但它提示布尔
表达式极不可能。在 JS 后端中，这不起作用。

**When to Use**

- In conditional statements where one branch is clearly more frequently executed than the other.

**When to Avoid**

- When the prediction can be wrong, as it might cause a performance penalty due to branch
misprediction.

**When to Use**

- For production builds where you want to reduce the executable size and improve runtime
performance.

**When to Avoid**

- Where it doesn't work for you.

#### `-fast-math`

This flag enables optimizations that disregard strict compliance with the IEEE standard for
floating-point arithmetic. While this could lead to faster code, it may produce incorrect or
less accurate mathematical results.

The full specter of math operations that `-fast-math` affects can be found
[here](https://clang.llvm.org/docs/UsersManual.html#cmdoption-ffast-math).

**When to Use**

- In applications where performance is more critical than precision, like certain graphics
rendering tasks.

**When to Avoid**

- In applications requiring strict mathematical accuracy, such as scientific simulations or
financial calculations.

#### `-d no_segfault_handler`

Using this flag omits the segfault handler, reducing the executable size and potentially improving
compile time. However, in the case of a segmentation fault, the output will not contain stack trace
information, making debugging more challenging.

**When to Use**

- In small, well-tested utilities where a stack trace is not essential for debugging.

**When to Avoid**

- In large-scale, complex applications where robust debugging is required.

#### `-cflags -march=native`

This flag directs the C compiler to generate instructions optimized for the host CPU. This can
improve performance but will produce an executable incompatible with other/older CPUs.

**When to Use**

- When the software is intended to run only on the build machine or in a controlled environment
with identical hardware.

**When to Avoid**

- When distributing the software to users with potentially older CPUs.

#### `-compress`

This flag executes `upx` to compress the resultant executable, reducing its size by around 50%-70%.
可执行文件将在运行时解压缩，因此启动需要更多时间。
它最初也会占用额外的 RAM，因为应用程序的压缩版本将被加载到
内存中，然后扩展到另一个内存块。
如果您不考虑这一点，调试这样的应用程序可能会更困难。
一些防病毒程序也使用启发式方法，对压缩应用程序的触发更频繁。

**When to Use**

- For really tiny environments, where the size of the executable on the file system,
or when deploying is important (docker containers, rescue disks etc).

**When to Avoid**

- When you need to debug the application
- When the app's startup time is extremely important (where 1-2ms can be meaningful for you)
- When you can not afford to allocate more memory during application startup
- When you are deploying an app to users with antivirus software that could misidentify your
app as malicious, just because it decompresses its code at runtime.

#### PGO（配置文件引导优化）

PGO allows the compiler to optimize code based on its behavior during sample runs. This can improve
performance and reduce the size of the output executable, but it adds complexity to the build
process.

**When to Use**

- For performance-critical applications where the added build complexity is justifiable.

**When to Avoid**

- For small, short-lived, or rapidly-changing projects where the added build complexity isn't
justified.

**PGO with Clang**

这是一个示例 bash 脚本，您可以使用它来优化您的 CLI V 程序，而无需用户交互。
在大多数情况下，您需要修改此脚本以使其适合您的特定程序。

```bash
#!/usr/bin/env bash

# Get the full path to the current directory
CUR_DIR=$(pwd)

# Remove existing PGO data
rm -f *.profraw
rm -f default.profdata

# Initial build with PGO instrumentation
v -cc clang -prod -cflags -fprofile-generate -o pgo_gen .

# Run the instrumented executable 10 times
for i in {1..10}; do
    ./pgo_gen
done

# Merge the collected data
llvm-profdata merge -o default.profdata *.profraw

# Compile the optimized version using the PGO data
v -cc clang -prod -cflags "-fprofile-use=${CUR_DIR}/default.profdata" -o optimized_program .

# Remove PGO data and instrumented executable
rm *.profraw
rm pgo_gen
```

## 原子操作

V 目前还没有对原子操作提供专门的支持，不过可以通过从 V 调用 C 函数的方式 (#v-and-c) 将变量当作原子变量来处理。标准的 C11 原子函数，比如 `atomic_store()`，通常是借助宏和 C 编译器的魔法来定义的，从而提供了一种类似*重载 C 函数*的功能。
由于 V 有意不支持函数重载，所以在名为 `atomic.h` 的 C 头文件中定义了包装函数，这些头文件是 V 编译器基础设施的一部分。

为所有无符号整数类型和指针提供了专用包装器。
(`u8` is not fully supported on Windows) &ndash; the function names include the type name
as suffix. e.g. `C.atomic_load_ptr()` or `C.atomic_fetch_add_u64()`.

要使用这些函数，必须包含所用操作系统的 C 头文件，并且必须声明打算使用的函数。例如：

```v globals
$if windows {
	#include "@VEXEROOT/thirdparty/stdatomic/win/atomic.h"
} $else {
	#include "@VEXEROOT/thirdparty/stdatomic/nix/atomic.h"
}

// declare functions we want to use - V does not parse the C header
fn C.atomic_store_u32(&u32, u32)
fn C.atomic_load_u32(&u32) u32
fn C.atomic_compare_exchange_weak_u32(&u32, &u32, u32) bool
fn C.atomic_compare_exchange_strong_u32(&u32, &u32, u32) bool

const num_iterations = 10000000

// see section "Global Variables" below
__global (
	atom u32 // ordinary variable but used as atomic
)

fn change() int {
	mut races_won_by_change := 0
	for {
		mut cmp := u32(17) // addressable value to compare with and to store the found value
		// atomic version of `if atom == 17 { atom = 23 races_won_by_change++ } else { cmp = atom }`
		if C.atomic_compare_exchange_strong_u32(&atom, &cmp, 23) {
			races_won_by_change++
		} else {
			if cmp == 31 {
				break
			}
			cmp = 17 // re-assign because overwritten with value of atom
		}
	}
	return races_won_by_change
}

fn main() {
	C.atomic_store_u32(&atom, 17)
	t := spawn change()
	mut races_won_by_main := 0
	mut cmp17 := u32(17)
	mut cmp23 := u32(23)
	for i in 0 .. num_iterations {
		// atomic version of `if atom == 17 { atom = 23 races_won_by_main++ }`
		if C.atomic_compare_exchange_strong_u32(&atom, &cmp17, 23) {
			races_won_by_main++
		} else {
			cmp17 = 17
		}
		desir := if i == num_iterations - 1 { u32(31) } else { u32(17) }
		// atomic version of `for atom != 23 {} atom = desir`
		for !C.atomic_compare_exchange_weak_u32(&atom, &cmp23, desir) {
			cmp23 = 23
		}
	}
	races_won_by_change := t.wait()
	atom_new := C.atomic_load_u32(&atom)
	println('atom: ${atom_new}, #exchanges: ${races_won_by_main + races_won_by_change}')
	// prints `atom: 31, #exchanges: 10000000`)
	println('races won by\n- `main()`: ${races_won_by_main}\n- `change()`: ${races_won_by_change}')
}
```

In this example both `main()` and the spawned thread `change()` try to replace a value of `17`
in the global `atom` with a value of `23`. The replacement in the opposite direction is
done exactly 10000000 times. The last replacement will be with `31` which makes the spawned
thread finish.

It is not predictable how many replacements occur in which thread, but the sum will always
be 10000000. (With the non-atomic commands from the comments the value will be higher or the program
will hang &ndash; dependent on the compiler optimization used.)

## 全局变量

By default V does not allow global variables. However, in low-level applications they have their
place so their usage can be enabled with the compiler flag `-enable-globals`.
Declarations of global variables must be surrounded with a `__global ( ... )`
specification &ndash; as in the example [above](#atomics).

An initializer for global variables must be explicitly converted to the
desired target type. If no initializer is given a default initialization is done.
某些对象（如信号量和互斥锁）需要*就地*显式初始化，即
不是使用函数调用返回的值，而是通过引用进行方法调用。
A separate `init()` function can be used for this purpose &ndash; it will be called before `main()`:

```v globals
import sync

__global (
	sem   sync.Semaphore // needs initialization in `init()`
	mtx   sync.RwMutex // needs initialization in `init()`
	f1    = f64(34.0625) // explicitly initialized
	shmap shared map[string]f64 // initialized as empty `shared` map
	f2    f64 // initialized to `0.0`
)

fn init() {
	sem.init(0)
	mtx.init()
}
```

Be aware that in multi threaded applications the access to global variables is subject
to race conditions. There are several approaches to deal with these:

- use `shared` types for the variable declarations and use `lock` blocks for access.
  This is most appropriate for larger objects like structs, arrays or maps.
- handle primitive data types as "atomics" using special C-functions (see [above](#atomics)).
- use explicit synchronization primitives like mutexes to control access. The compiler
  cannot really help in this case, so you have to know what you are doing.
- don't care &ndash; this approach is possible but makes only sense if the exact values
  of global variables do not really matter. An example can be found in the `rand` module
  where global variables are used to generate (non cryptographic) pseudo random numbers.
  In this case data races lead to random numbers in different threads becoming somewhat
  correlated, which is acceptable considering the performance penalty that using
  synchronization primitives would represent.

## 静态变量

V also supports *static variables*, which are like *global variables*, but
available only *inside* a single unsafe function (you can look at them as
namespaced globals).

注意：也不鼓励使用它们，原因与不鼓励使用全局变量
类似。支持此功能是为了能够使用 `v translate` 将现有的
低级 C 代码转换为 V 代码。

注意：使用静态变量的函数必须标记
为 @[unsafe]。此外，与使用全局变量不同，使用静态变量不需要
传递 `-enable-globals` 标志，因为它们只能在
单个函数内读取/更改，该函数对存储在其中的状态有完全控制。

以下是一个如何使用静态变量的小示例：
```v
@[unsafe]
fn counter() int {
	mut static x := 42
	// Note: x is initialised to 42, just _once_.
	x++
	return x
}

fn f() int {
	return unsafe { counter() }
}

println(f()) // prints 43
println(f()) // prints 44
println(f()) // prints 45
```

## 交叉编译

Cross compilation is supported for Windows, Linux and FreeBSD.

To cross compile your project simply run:

```shell
v -os windows .
```

or

```shell
v -os linux .
```

or

```shell
v -os freebsd .
```

> [!NOTE]
> Cross-compiling a Windows binary on a Linux machine requires the GNU C compiler for
> MinGW-w64 (targeting Win64) to first be installed.

For Ubuntu/Debian-based distributions:

```shell
sudo apt install gcc-mingw-w64-x86-64
```

For Arch based distributions:

```shell
sudo pacman -S mingw-w64-gcc
```

(Cross compiling for macOS is temporarily not possible.)

If you don't have any C dependencies, that's all you need to do. This works even
when compiling GUI apps using the `ui` module or graphical apps using `gg`.

You will need to install Clang, LLD linker, and download a zip file with
libraries and include files for Windows and Linux. V will provide you with a link.

## 调试

### C 后端二进制文件（默认）

To debug issues in the generated binary (flag: `-b c`), you can pass these flags:

- `-g` - produces a less optimized executable with more debug information in it.
  V will enforce line numbers from the .v files in the stacktraces, that the
  executable will produce on panic. It is usually better to pass -g, unless
  you are writing low-level code, in which case use the next option `-cg`.
- `-cg` - produces a less optimized executable with more debug information in it.
  The executable will use C source line numbers in this case. It is frequently
  used in combination with `-keepc`, so that you can inspect the generated
  C program in case of panic, or so that your debugger (`gdb`, `lldb` etc.)
  can show you the generated C source code.
- `-showcc` - prints the C command that is used to build the program.
- `-show-c-output` - prints the output, that your C compiler produced
  while compiling your program.
- `-keepc` - do not delete the generated C source code file after a successful
  compilation. Also keep using the same file path, so it is more stable,
  and easier to keep opened in an editor/IDE.

For best debugging experience if you are writing a low-level wrapper for an existing
C library, you can pass several of these flags at the same time:
`v -keepc -cg -showcc yourprogram.v`, then just run your debugger (gdb/lldb) or IDE
on the produced executable `yourprogram`.

If you just want to inspect the generated C code,
without further compilation, you can also use the `-o` flag (e.g. `-o file.c`).
这将使 V 生成 `file.c` 然后停止。

If you want to see the generated C source code for *just* a single C function,
for example `main`, you can use: `-printfn main -o file.c`.

To debug the V executable itself you need to compile from src with `./v -g -o v cmd/v`.

You can debug tests with for example `v -g -keepc prog_test.v`. The `-keepc` flag is needed,
so that the executable is not deleted, after it was created and ran.

To see a detailed list of all flags that V supports,
use `v help`, `v help build` and `v help build-c`.

**Commandline Debugging**

1. compile your binary with debugging info `v -g hello.v`
2. debug with [lldb](https://lldb.llvm.org) or [GDB](https://www.gnu.org/software/gdb/)
   e.g. `lldb hello`

[Troubleshooting (debugging) executables created with V in GDB](https://github.com/vlang/v/wiki/Troubleshooting-(debugging)-executables-created-with-V-in-GDB)

**Visual debugging Setup:**

* [Visual Studio Code](vscode.md)

### 原生后端二进制文件

Currently there is no debugging support for binaries, created by the
native backend (flag: `-b native`).

### Javascript 后端

To debug the generated Javascript output you can activate source maps:
`v -b js -sourcemap hello.v -o hello.js`

For all supported options check the latest help:
`v help build-js`

## V 和 C

The basic mapping between C and V types is described in
[C and V Type Interoperability](https://github.com/vlang/v/blob/master/doc/c_and_v_type_interoperability.md).

### 从 V 调用 C

V currently does not have a parser for C code. That means that even
though it allows you to `#include` existing C header and source files,
it will not know anything about the declarations in them. The `#include`
statement will only appear in the generated C code, to be used by the
C compiler backend itself.

**Example of #include**
```v oksyntax
#include <stdio.h>
```
After this statement, V will *not* know anything about the functions and
structs declared in `stdio.h`, but if you try to compile the .v file,
it will add the include in the generated C code, so that if that header file
is missing, you will get a C error (you will not in this specific case, if you
have a proper C compiler setup, since `<stdio.h>` is part of the
standard C library).

To overcome that limitation (that V does not have a C parser), V needs you to
redeclare the C functions and structs, on the V side, in your `.c.v` files.
Note that such redeclarations only need to have enough details about the
functions/structs that you want to use.
还要注意，它们*不必*完整，与 .h 文件中的不同。


**C. struct redeclarations**
For example, if a struct has 3 fields on the C side, but you want to only
refer to 1 of them, you can declare it like this:

**Example of C struct redeclaration**
```v oksyntax
struct C.NameOfTheStruct {
	a_field int
}
```
Another feature, that is very frequently needed for C interoperability,
is the `@[typedef]` attribute. It is used for marking `C.` structs,
that are defined with `typedef struct SomeName { ..... } TypeName;` in the C headers.

For that case, you will have to write something like this in your .c.v file:
```v oksyntax
@[typedef]
pub struct C.TypeName {
}
```
请注意，V 中 `C.` 结构体的名称是 `struct SomeName {...}` *之后*的那个。

**C. function redeclarations**
The situation is similar for `C.` functions. If you are going to call just 1 function in a
library, but its .h header declares dozens of them, you will only need to declare that single
function, for example:

**Example of C function redeclaration**
```v oksyntax
fn C.name_of_the_C_function(param1 int, const_param2 &char, param3 f32) f64
```
... and then later, you will be able to call the same way you would V function:
```v oksyntax
f := C.name_of_the_C_function(123, c'here is some C style string', 1.23)
dump(f)
```

**Example of using a C function from stdio, by redeclaring it on the V side**
```v
#include <stdio.h>

// int dprintf(int fd, const char *format, ...)
fn C.dprintf(fd int, const_format &char, ...voidptr) int

value := 12345
x := C.dprintf(0, c'Hello world, value: %d\n', value)
dump(x)
```

If your C backend compiler is properly setup, you should see something like this, when you try
to run it:
```console
#0 10:42:32 /v/examples> v run a.v
Hello world, value: 12345
[a.v:8] x: 26
#0 10:42:33 /v/examples>
```

Note, that the C function redeclarations look very similar to the V ones, with some differences:
1) They lack a body (they are defined on the C side) .
2) Their names start with `C.` .
3) Their names can have capital letters (unlike V ones, that are required to use snake_case) .

Note also the second parameter `const char *format`, which was redeclared as `const_format &char` .
The `const_` prefix in that redeclaration may seem arbitrary, but it is important, if you want
to compile your code with `-cstrict` or thirdparty C static analysis tools. V currently does not
have another way to express that this parameter is a const (this will probably change in V 1.0).

For some C functions, that use variadics (`...`) as parameters, V supports a special syntax for
the parameters - `...voidptr`, that is not available for ordinary V functions (V's variadics are
*required* to have the same exact type). Usually those are functions of the printf/scanf family
i.e for `printf`, `fprintf`, `scanf`, `sscanf`, etc, and other formatting/parsing/logging
functions.

**Example**

```v
#flag freebsd -I/usr/local/include -L/usr/local/lib
#flag -lsqlite3
#include "sqlite3.h"
// See also the example from https://www.sqlite.org/quickstart.html
pub struct C.sqlite3 {
}

pub struct C.sqlite3_stmt {
}

type FnSqlite3Callback = fn (voidptr, int, &&char, &&char) int

fn C.sqlite3_open(&char, &&C.sqlite3) int

fn C.sqlite3_close(&C.sqlite3) int

fn C.sqlite3_column_int(stmt &C.sqlite3_stmt, n int) int

// ... you can also just define the type of parameter and leave out the C. prefix

fn C.sqlite3_prepare_v2(&C.sqlite3, &char, int, &&C.sqlite3_stmt, &&char) int

fn C.sqlite3_step(&C.sqlite3_stmt)

fn C.sqlite3_finalize(&C.sqlite3_stmt)

fn C.sqlite3_exec(db &C.sqlite3, sql &char, cb FnSqlite3Callback, cb_arg voidptr, emsg &&char) int

fn C.sqlite3_free(voidptr)

fn my_callback(arg voidptr, howmany int, cvalues &&char, cnames &&char) int {
	unsafe {
		for i in 0 .. howmany {
			print('| ${cstring_to_vstring(cnames[i])}: ${cstring_to_vstring(cvalues[i]):20} ')
		}
	}
	println('|')
	return 0
}

fn main() {
	db := &C.sqlite3(unsafe { nil }) // this means `sqlite3* db = 0`
	// passing a string literal to a C function call results in a C string, not a V string
	C.sqlite3_open(c'users.db', &db)
	// C.sqlite3_open(db_path.str, &db)
	query := 'select count(*) from users'
	stmt := &C.sqlite3_stmt(unsafe { nil })
	// Note: You can also use the `.str` field of a V string,
	// to get its C style zero terminated representation
	C.sqlite3_prepare_v2(db, &char(query.str), -1, &stmt, 0)
	C.sqlite3_step(stmt)
	nr_users := C.sqlite3_column_int(stmt, 0)
	C.sqlite3_finalize(stmt)
	println('There are ${nr_users} users in the database.')

	error_msg := &char(unsafe { nil })
	query_all_users := 'select * from users'
	rc := C.sqlite3_exec(db, &char(query_all_users.str), my_callback, voidptr(7), &error_msg)
	if rc != C.SQLITE_OK {
		eprintln(unsafe { cstring_to_vstring(error_msg) })
		C.sqlite3_free(error_msg)
	}
	C.sqlite3_close(db)
}
```

### 从 C 调用 V

由于 V 可以编译为 C，一旦您知道如何操作，从 C 调用 V 代码就非常容易。

使用 `v -o file.c your_file.v` 生成与 V 代码对应的 C 文件。

More details in [call_v_from_c example](../examples/call_v_from_c).

### 传递 C 编译标志

在 V 文件顶部添加 `#flag` 指令以提供 C 编译标志，例如：

- `-I` 用于添加 C 头文件搜索路径
- `-l` 用于添加您想要链接的 C 库名称
- `-L` 用于添加 C 库文件搜索路径
- `-D` 用于设置编译时变量

您（可选地）可以为不同的目标使用不同的标志。
目前支持 `linux`、`darwin`、`freebsd` 和 `windows` 标志。

> [!NOTE]
> 每个标志必须单独一行（目前）

```v oksyntax
#flag linux -lsdl2
#flag linux -Ivig
#flag linux -DCIMGUI_DEFINE_ENUMS_AND_STRUCTS=1
#flag linux -DIMGUI_DISABLE_OBSOLETE_FUNCTIONS=1
#flag linux -DIMGUI_IMPL_API=
```

在控制台构建命令中，您可以使用：

* `-cc` 来更改默认的 C 后端编译器。
* `-cflags` 将自定义标志传递给后端 C 编译器（在其他 C 选项之前传递）。
* `-ldflags` 将自定义标志传递给后端 C 链接器（在所有其他 C 选项之后传递）。
* 例如：`-cc gcc-9 -cflags -fsanitize=thread`。

您可以在终端中定义 `VFLAGS` 环境变量来存储您的 `-cc`
和 `-cflags` 设置，而不是每次都将其包含在构建命令中。

### #pkgconfig

添加 `#pkgconfig` 指令以告诉编译器应使用哪些模块进行编译
和链接，使用相应依赖项提供的 pkg-config 文件。

由于不能在 `#flag` 中使用反引号，并且出于安全
和可移植性原因不希望生成进程，V 使用自己的 pkgconfig 库，该库与标准
freedesktop 兼容。

如果未传递标志，它将向 pkgconfig（而不是 V）添加 `--cflags` 和 `--libs`。
换句话说，下面的两行做同样的事情：

```v oksyntax
#pkgconfig r_core
#pkgconfig --cflags --libs r_core
```

`.pc` 文件在硬编码的默认 pkg-config 路径列表中查找，用户可以通过
使用 `PKG_CONFIG_PATH` 环境变量添加额外路径。可以传递多个模块。

要检查 pkg-config 是否存在，请使用 `$pkgconfig('pkg')` 作为编译时 "if" 条件来
检查 pkg-config 是否存在。如果存在，将创建该分支。使用 `$else` 或 `$else $if`
来处理其他情况。

```v ignore
$if $pkgconfig('mysqlclient') {
	#pkgconfig mysqlclient
} $else $if $pkgconfig('mariadb') {
	#pkgconfig mariadb
}
```

### 包含 C 代码

您也可以直接在 V 模块中包含 C 代码。
例如，假设您的 C 代码位于模块文件夹内名为 'c' 的文件夹中。
然后：

* 在模块的顶层文件夹中放置一个 v.mod 文件（如果您
  使用 `v new` 创建了模块，您已经有了 v.mod 文件）。例如：

```v ignore
Module {
	name: 'mymodule',
	description: 'My nice module wraps a simple C library.',
	version: '0.0.1'
	dependencies: []
}
```

* 将这些行添加到模块的顶部：

```v oksyntax
#flag -I @VMODROOT/c
#flag @VMODROOT/c/implementation.o
#include "header.h"
```

> [!NOTE]
> @VMODROOT 将被 V 替换为 *最近的父文件夹，
> 其中包含 v.mod 文件*。
> v.mod 文件所在文件夹旁边或下方的任何 .v 文件，
> 都可以使用 `#flag @VMODROOT/abc` 来引用此文件夹。
> @VMODROOT 文件夹也会*前置*到模块查找路径，
> 因此您可以通过命名来*导入* @VMODROOT 下的其他模块。

上面的说明将使 V 在
您的模块 `folder/c/implementation.o` 中查找已编译的 .o 文件。
如果 V 找到它，.o 文件将被链接到使用该模块的主可执行文件。
如果找不到，V 假设存在 `@VMODROOT/c/implementation.c` 文件，
并尝试将其编译为 .o 文件，然后使用该文件。

这允许您拥有包含在 V 模块中的 C 代码，从而使其分发更容易。
您可以在此处查看在 V 包装模块中使用 C 代码的完整最小示例：
[project_with_c_code](https://github.com/vlang/v/tree/master/vlib/v/tests/project_with_c_code)。
另一个示例，演示从 C 传递结构体到 V 再返回：
[在 C 到 V 到 C 之间互操作](https://github.com/vlang/v/tree/master/vlib/v/tests/project_with_c_code_2)。

### C 类型

普通的以零结尾的 C 字符串可以使用
`unsafe { &char(cstring).vstring() }` 转换为 V 字符串，或者如果您已经知道它们的长度，可以使用
`unsafe { &char(cstring).vstring_with_len(len) }`。

> [!NOTE]
> `.vstring()` 和 `.vstring_with_len()` 方法不会创建 `cstring` 的副本，
> 因此您不应在调用 `.vstring()` 方法后释放它。
> 如果您需要复制 C 字符串（某些 libc API 如 `getenv` 几乎需要这样做，
> 因为它们返回指向内部 libc 内存的指针），您可以使用 `cstring_to_vstring(cstring)`。

在 Windows 上，C API 经常返回所谓的 `wide` 字符串（UTF-16 编码）。
可以使用 `string_from_wide(&u16(cwidestring))` 将它们转换为 V 字符串。

V 具有以下类型以便与 C 更容易互操作：

- `voidptr` 对应 C 的 `void*`，
- `&u8` 对应 C 的 `byte*` 和
- `&char` 对应 C 的 `char*`。
- `&&char` 对应 C 的 `char**`

要将 `voidptr` 转换为 V 引用，请使用 `user := &User(user_void_ptr)`。

`voidptr` 也可以通过类型转换解引用为 V 结构体：`user := User(user_void_ptr)`。

[一个从 V 调用 C 代码的模块示例](https://github.com/vlang/v/blob/master/vlib/v/tests/project_with_c_code/mod1/wrapper.c.v)

### C 声明

C 标识符使用 `C` 前缀访问，类似于访问模块特定
标识符的方式。函数必须在 V 中重新声明才能使用。
任何 C 类型都可以在 `C` 前缀后使用，但类型必须在 V 中重新声明
才能访问类型成员。

要重新声明复杂类型，例如以下 C 代码：

```c
struct SomeCStruct {
	uint8_t implTraits;
	uint16_t memPoolData;
	union {
		struct {
			void* data;
			size_t size;
		};

		DataView view;
	};
};
```

子数据结构的成员可以直接在包含的结构体中声明，如下所示：

```v
pub struct C.SomeCStruct {
	implTraits  u8
	memPoolData u16
	// 这些成员是子数据结构的一部分，目前无法在 V 中表示。
	// 像这样直接声明它们足以进行访问。
	// union {
	// struct {
	data voidptr
	size usize
	// }
	view C.DataView
	// }
}
```

数据成员的存在已告知 V，可以在不完全
重新创建原始结构的情况下使用它们。

或者，您可以[嵌入](#embedded-structs)子数据结构以保持
并行代码结构。

### 导出到共享库

默认情况下，所有 V 函数在 C 中具有以下命名方案：`[模块名]__[函数名]`。

例如，模块 `bar` 中的 `fn foo() {}` 将产生 `bar__foo()`。

要使用自定义导出名称，请使用 `@[export]` 属性：

```
@[export: 'my_custom_c_name']
fn foo() {
}
```

### 将 C 转换为 V

V 可以将您的 C 代码转换为人类可读的 V 代码，并在
C 库之上生成 V 包装器。

C2V 目前使用 Clang 的 AST 来生成 V，因此要将 C 文件转换为 V，
您需要在机器上安装 Clang。

让我们先创建一个简单的程序 `test.c`：

```c
#include "stdio.h"

int main() {
	for (int i = 0; i < 10; i++) {
		printf("hello world\n");
	}
        return 0;
}
```

运行 `v translate test.c`，V 将生成 `test.v`：

```v
fn main() {
	for i := 0; i < 10; i++ {
		println('hello world')
	}
}
```

要在 C 库之上生成包装器，请使用此命令：

```bash
v translate wrapper c_code/libsodium/src/libsodium
```

这将生成一个包含 V 模块的 `libsodium` 目录。

C2V 生成的 libsodium 包装器示例：

https://github.com/vlang/libsodium

<br>

何时应该翻译 C 代码，何时应该简单地从 V 调用 C 代码？

如果您有编写良好、经过充分测试的 C 代码，
那么您当然可以简单地从 V 调用此 C 代码。

将其翻译为 V 有几个优势：

- 如果您计划开发该代码库，现在您在一个语言中拥有所有内容，
  这比 C 更安全且更容易开发。
- 交叉编译变得容易得多。您根本不必担心它。
- 也不再需要构建标志和包含文件。

### 解决 C 问题

在某些情况下，C 互操作可能非常困难。
其中一种情况是头文件相互冲突。
例如，V 需要包含 Windows 头文件库，以便您的 V 二进制文件
在所有平台上无缝工作。

但是，由于 Windows 头文件库使用极其通用的名称（如 `Rectangle`），
如果您希望使用也定义了 `Rectangle` 名称的 C 代码，这将导致冲突。

对于这样的非常特殊的情况，V 有 `#preinclude` 和 `#postinclude` 指令。

这些指令允许在 V 添加其内置库*之前*配置内容，
以及在所有 V 代码生成完成*之后*（因此所有原型、
声明和定义已经存在）。

使用示例：
```v ignore
// 这将在使用内置库之前包含。
#preinclude "pre_include.h"

// 这将在使用内置库之后包含。
#include "include.h"

// 这将在所有 V 代码生成完成后包含，
// 包括项目的 main 函数
#postinclude "post_include.h"
```

`pre_include.h` 中可能包含的内容示例
可以在[此处找到](https://github.com/irishgreencitrus/raylib.v/blob/main/include/pre.h)

另一方面，`#postinclude` 指令对于允许集成
像 SDL3 或 Sokol 这样的框架很有用，这些框架坚持在代码中使用回调，而不是
像普通库一样行为，并允许您决定何时调用它们。

注意：这些是高级功能，在非常特定的 C 互操作
情况之外不需要。除此之外，使用它们可能会引起比解决的问题更多的问题。

请考虑将它们作为最后的手段！

## 其他 V 特性

### 内联汇编

<!-- ignore because it doesn't pass fmt test (why?) -->

```v ignore
a := 100
b := 20
mut c := 0
asm amd64 {
    mov eax, a
    add eax, b
    mov c, eax
    ; =r (c) as c // output
    ; r (a) as a // input
      r (b) as b
}
println('a: ${a}') // 100
println('b: ${b}') // 20
println('c: ${c}') // 120
```

更多示例，请参阅
[vlib/v/slow_tests/assembly/asm_test.amd64.v](https://github.com/vlang/v/tree/master/vlib/v/slow_tests/assembly/asm_test.amd64.v)

### 热代码重载

```v live
module main

import time

@[live]
fn print_message() {
	println('Hello! Modify this message while the program is running.')
}

fn main() {
	for {
		print_message()
		time.sleep(500 * time.millisecond)
	}
}
```

使用 `v -live message.v` 构建此示例。

您也可以使用 `v -live run message.v` 运行此示例。
请确保在命令中使用 V 文件的路径，
**而不是**文件夹的路径（如 `v -live run .`）-
在这种情况下，您需要修改文件夹的内容（例如，添加新文件），
因为 *message.v* 中的更改将不会生效。

您想要重新加载的函数必须在定义之前具有 `@[live]` 属性。

目前无法在程序运行时修改类型。

更多示例，包括图形应用程序：
[github.com/vlang/v/tree/master/examples/hot_reload](https://github.com/vlang/v/tree/master/examples/hot_reload)。

#### 关于在使用 v -live run 的热重载函数中保持状态
V 的热代码重载依赖于用 `@[live]` 标记您想要重新加载的函数，
然后编译这些 `@[live]` 函数的共享库，然后
您的 V 程序在运行时加载该共享库。

V（使用 -live 选项）启动一个新线程，监控源文件的更改，
当它检测到修改时，它会重新编译共享库，并在运行时重新加载它，
以便对这些 @[live] 函数的新调用将使用新加载的库。

它保留所有累积的状态（来自 @[live] 函数外部的局部变量、
来自堆变量和全局变量），允许快速调整合并函数中的代码。

当有更重大的更改（对数据结构或未标记的函数）时，
您将需要手动重启正在运行的应用程序。

### V 中的跨平台 shell 脚本

V 可以用作 Bash 的替代品来编写部署脚本、构建脚本等。

使用 V 的优势在于语言的简单性和可预测性，以及
跨平台支持。"V 脚本"可以在类 Unix 系统以及 Windows 上运行。

要使用 V 的脚本模式，请将源文件保存为 `.vsh` 文件扩展名。
它将使 `os` 模块中的所有函数变为全局函数（这样您可以使用 `mkdir()` 而不是
`os.mkdir()`，例如）。

V 也知道立即编译和运行 `.vsh` 文件，因此您不需要单独的
编译步骤。V 还会重新编译由 `.vsh` 文件生成的可执行文件，
*仅当它比 .vsh 源文件旧时*，即第一次运行后的运行将
更快，因为不需要重新编译未更改的脚本。

一个 `deploy.vsh` 示例：

```v oksyntax
#!/usr/bin/env -S v

// 注意：上面的 shebang 行将 .vsh 文件与类 Unix 系统上的 V 关联，
// 因此一旦使用 `chmod +x deploy.vsh` 使其可执行，就可以通过指定 .vsh 文件的路径来运行它，
// 即在该 chmod 命令之后，您可以通过输入其名称/路径来运行 .vsh 脚本，例如：`./deploy.vsh`

// print command then execute it
fn sh(cmd string) {
	println('❯ ${cmd}')
	print(execute_or_exit(cmd).output)
}

// Remove if build/ exits, ignore any errors if it doesn't
rmdir_all('build') or {}

// Create build/, never fails as build/ does not exist
mkdir('build')!

// Move *.v files to build/
result := execute('mv *.v build/')
if result.exit_code != 0 {
	println(result.output)
}

sh('ls')

// Similar to:
// files := ls('.')!
// mut count := 0
// if files.len > 0 {
//     for file in files {
//         if file.ends_with('.v') {
//              mv(file, 'build/') or {
//                  println('err: ${err}')
//                  return
//              }
//         }
//         count++
//     }
// }
// if count == 0 {
//     println('No files')
// }
```

现在您可以像普通 V 程序一样编译它，并获得一个可以在任何地方部署和运行的可执行文件：
`v -skip-running deploy.vsh && ./deploy`

或者像传统的 Bash 脚本一样运行它：
`v run deploy.vsh`（或简单地使用 `v deploy.vsh`）

在类 Unix 平台上，使用 `chmod +x` 使其可执行后，可以直接运行该文件：
`./deploy.vsh`

### 无扩展名的 Vsh 脚本

虽然 V 通常不允许没有指定文件扩展名的 vsh 脚本，但有一种方法
可以绕过此规则，并拥有一个完全自定义名称和 shebang 的文件。虽然此功能
存在，但仅建议用于特定用例，例如将放在路径中的脚本，并且
**不应**用于构建或部署脚本等用途。要使用此功能，请以
`#!/usr/bin/env -S v -raw-vsh-tmp-prefix tmp` 开头文件，其中 `tmp` 是
构建的可执行文件的前缀。这将在 crun 模式下运行，因此只有在脚本
发生更改时才会重新构建，并将二进制文件保存为 `tmp.<scriptfilename>`。**注意**：如果此文件名已
存在，文件将被覆盖。如果您想每次都重新构建而不保留此二进制文件，
请改用 `#!/usr/bin/env -S v -raw-vsh-tmp-prefix tmp run`。

注意：有一个小的 shell 脚本 `cmd/tools/vrun`，对于有
env 程序（`/usr/bin/env`）但仍不支持 `-S` 选项的系统（如 BusyBox）可能很有用。
有关更多详细信息，请参阅 https://github.com/vlang/v/blob/master/cmd/tools/vrun。

# 附录

## 附录 I：关键字

V 有 45 个保留关键字（3 个是字面量）：

```v ignore
as
asm
assert
atomic
break
const
continue
defer
else
enum
false
fn
for
go
goto
if
implements
import
in
interface
is
isreftype
lock
match
module
mut
none
or
pub
return
rlock
select
shared
sizeof
spawn
static
struct
true
type
typeof
union
unsafe
volatile
__global
__offsetof
```

See also [V Types](#v-types).

## 附录 II：运算符

这仅列出 [primitive types](#primitive-types) 的运算符。

```v ignore
+    sum                    integers, floats, strings
-    difference             integers, floats
*    product                integers, floats
/    quotient               integers, floats
%    remainder              integers

~    bitwise NOT            integers
&    bitwise AND            integers
|    bitwise OR             integers
^    bitwise XOR            integers

!    logical NOT            bools
&&   logical AND            bools
||   logical OR             bools
!=   logical XOR            bools

<<   left shift             integer << unsigned integer
>>   right shift            integer >> unsigned integer
>>>  unsigned right shift   integer >> unsigned integer


优先级    运算符
    5            *  /  %  <<  >> >>> &
    4            +  -  |  ^
    3            ==  !=  <  <=  >  >=
    2            &&
    1            ||


赋值运算符
+=   -=   *=   /=   %=
&=   |=   ^=
>>=  <<=  >>>=
&&= ||=
```

注意：在 V 中，`assert -10 % 7 == -3` 通过。在编程中，余数的符号
取决于除数和被除数的符号。

## 其他在线资源

### [V 贡献指南](https://github.com/vlang/v/blob/master/CONTRIBUTING.md)

如果没有所有贡献者的帮助，V 将远不如今天。
如果您喜欢并想帮助 V 项目成功，
请阅读该文档，选择一个任务，然后开始吧！

### [V 语言文档](https://docs.vlang.io/introduction.html)
该网站包含与此文档相同的信息，但分为多个页面，
以便在移动设备上更轻松地阅读。在每次提交到主存储库时
自动更新。

### [V 标准模块文档](https://modules.vlang.io/)
该网站包含 V 标准库（vlib）中所有模块的
文档。在每次提交到主存储库时自动更新。

### [V 在线游乐场](https://play.vlang.io/)
该网站允许您输入和编辑小型 V 程序，然后编译
并运行它们。在每次提交到主存储库时自动更新。
当您无法访问已安装 V 的计算机或 Android 手机时，
可以使用它来测试您的想法。

### [Awesome V](https://github.com/vlang/awesome-v)
当您创建一个很酷的新项目或库时，您也可以将其提交到该
列表。您也可以使用该列表，获取有关使用 V 进行新项目的想法。

### [V 语言 Discord](https://discord.gg/vlang)
这是讨论 V 语言、了解最新
发展、快速获得问题帮助、见证/参与
~~史诗般的口水战~~建设性批评交流和设计决策的地方。
加入它，了解更多关于语言、游戏、编辑器、人、克林贡人、
康威定律和宇宙的信息。
