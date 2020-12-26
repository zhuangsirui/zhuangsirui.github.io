---
layout: post
title: "学习 Swift （一）"
date: 2017-02-14 22:42 +0800
---

## 使用可交互 shell（REPL）

> 我在使用 swift 交互 shell 时，遇到以下报错
>
>     Traceback (most recent call last):
>       File "<string>", line 1, in <module>
>     NameError: name 'run_one_line' is not defined
>
> 解决方法是：
>
>     pip install --upgrade pip
>     pip install six

在 shell 中键入 `swift --version` 查看当前 swift 版本，键入 `swift` 即可进入 swift 的交互模式，类似 ruby 的 irb。

## 初识 swift（使用 REPL）

```swift
(swift) 1 + 1 // 整形 - Int
// r0 : Int = 2
(swift) "Hi!" // 字符串 - String
// r1 : String = "Hi!"
(swift) let hello = "Hello" // 常量赋值
// hello : String = "Hello"
(swift) var world = "World!" // 变量赋值
// world : String = "World!"
(swift) let integers = [1,2,3] // 声明数组（编译时可推导的类型不用显示声明）
// integers : [Int] = [1, 2, 3]
(swift) let floats = [Float]() // 声明数组（编译时不可推导则需要显示声明类型）
// floats : [Float] = []
(swift) for n in integers.reversed() {
	print(n)
}
1
2
3
```

## 使用 swift 包管理器（Package Manager）

查看包管理器帮助：

```bash
swift package --help
```

创建一个空目录，进入后输入 `swift package init` 即可初始化一个以该目录为名称的 swift package。

```bash
$ mkdir hello && cd hello && swift package init
Creating library package: hello
Creating Package.swift
Creating .gitignore
Creating Sources/
Creating Sources/hello.swift
Creating Tests/
Creating Tests/LinuxMain.swift
Creating Tests/helloTests/
Creating Tests/helloTests/helloTests.swift
```

接着输入 `swift build` 即可编译包。在期间 swift build 会自动下载、解析和编译在 `Package.swift` 中声明的其他依赖包。

> `swift package init --type executable` 可以创建一个可运行的包。其中 `main.swift` 是其入口文件。
