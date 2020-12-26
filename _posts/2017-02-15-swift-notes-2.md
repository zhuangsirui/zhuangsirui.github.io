---
layout: post
title: "学习 Swift （二）"
date: 2017-02-15 10:40 +0800
---

## 数据类型

基础类型

* `Int`
* `Float`
* `Double`
* `Bool`
* `String`

集合类型

* `Array`
* `Set`
* `Dictionary`
* `Tuple`

## 常量、变量

使用 `let` 声明常量，用 `var` 声明变量。

```swift
let maximumNumberOfLoginAttempts = 10
var currentLoginAttempt = 0
```

如果初始值没有提供足够的信息（或者没有初始值），那你需要在变量后面声明类型，用冒号分割。

```swift
let implicitInteger = 70
let implicitDouble = 70.0
let explicitDouble: Double = 70
```

值永远不会被隐式转换为其他类型。如果你需要把一个值转换成其他类型，请显式转换。

```swift
let label = "The width is"
let width = 94
let widthLabel = label + String(width)
```

转换为字符串：

```swift
let apples = 3
print("I have \(apples) apples.")
```

## 数组和字典

数组和字典都使用 `[]` 声明。

```swift
// 数组
var shoppingList = ["catfish", "water", "tulips", "blue paint"]
shoppingList[1] = "bottle of water"

// 字典
var occupations = [
	"Malcolm": "Captain",
	"Kaylee": "Mechanic",
]
occupations["Jayne"] = "Public Relations"
```

要创建一个空数组或者字典，使用初始化语法。

```swift
let emptyArray = [String]()
let emptyDictionary = [String: Float]()
```

如果类型信息可以被推断出来，你可以用 `[]` 和 `[:]` 来创建空数组和空字典——就像你声明变量或者给函数传参数的时候一样（这句话不明白）。

```swift
shoppingList = []
occupations = [:]
```

## 控制流

* `if`
* `switch`
* `for-in`
* `for`
* `while`
* `repeat-while`

### if

`if` 语句后面必须是一个布尔值或者布尔值表达式，swift 并不会自动将 0 或者 nil 判定为 false。但是可以用 `if let` 判定一个 `optional` 的值是否为 nil：

```swift
var optionalName: String? = "Jack"
var greeting: String
if let name = optionalName {
	greeting = "Hello, \(name)!"
} else {
	greeting = "Hello!"
}
print(greeting)
```

另一个判定 `optional` 值的方式是使用 `??` 提供一个默认的值。如果 `optional` 值为空，则使用默认值替代。

```swift
let nickName: String? = nil
let fullName: String = "John Appleseed"
let informalGreeting = "Hi \(nickName ?? fullName)"
```

### switch

`switch` 支持任意数据类型的比较操作。

```swift
let vegetable = "red pepper"
switch vegetable {
case "celery":
	print("Add some raisins and make ants on a log.")
case let x where x.hasSuffix("pepper"):
	print("Is it a spicy \(x)?")
default:
	print("Everything tastes good in soup.")
}
```

`case` 后跟 `let` 可以进行模式匹配。

>> 上述例子中，如果不添加 `default` 情况，编译会抛错 `error: switch must be exhaustive, consider adding a default clause`，强制穷举所有可能发生的情况。

### for-in

使用 `for-in` 来遍历字典或数组：

```swift
let interestingNumbers = [
	"Prime": [2, 3, 5, 7, 11, 13],
	"Fibonacci": [1, 1, 2, 3, 5, 8],
	"Square": [1, 4, 9, 16, 25],
]
var largest = 0
for (kind, numbers) in interestingNumbers {
	for number in numbers {
		if number > largest {
			largest = number
		}
	}
}
print(largest)
```

### while or do-while

```swift
var n = 2
while n < 100 {
	n = n * 2
}
print(n)

var m = 2
do {
	m = m * 2
} while m < 100
print(m)
```

## 函数和闭包

使用关键字 `func` 生命一个函数，使用 `->` 来表示返回值。

```swift
func greet(person: String, day: String) -> String {
	return "Hello \(person), today is \(day)."
}
greet(person: "Bob", day: "Tuesday")
```

函数默认用参数名称，上例中的 `person` 和 `day` 作为调用时候的参数标签。如果需要自定义参数标签，则在参数前添加想要的标签名或者 `_` 来指定不需要标签：

```swift
func greet(_ person: String, on day: String) -> String {
	return "Hello \(person), today is \(day)."
}
greet("John", on: "Wednesday")
```

使用元组（`tuple`）可以表示一个符合类型的值，比如让一个函数返回一个元组实现多返回值的返回。元组的元素可以通过名称或者数字（从 0 开始）访问。

```swift
func calculateStatistics(scores: [Int]) -> (min: Int, max: Int, sum: Int) {
	var min = scores[0]
	var max = scores[0]
	var sum = 0
	for score in scores {
		if score > max {
			max = score
		} else if score < min {
			min = score
		}
		sum += score
	}
	return (min, max, sum)
}
let statistics = calculateStatistics(scores: [5, 3, 100, 3, 9])
print(statistics.sum)
print(statistics.2)
```

函数可以传入可变个数的相同类型参数，函数内部会自动转化为一个数组。

```swift
func sumOf(numbers: Int...) -> Int {
	var sum = 0
	for number in numbers {
		sum += number
	}
	return sum
}
sumOf()
sumOf(numbers: 42, 597, 12)
```

函数可以嵌套，生命周期以及变量访问范围按照常规理解即可：

```swift
func returnFifteen() -> Int {
	var y = 10
	func add() {
		y += 5
	}
	add()
	return y
}
returnFifteen()
```

函数属于第一数据类型（一等公民），可以当做参数传入。

```swift
func makeIncrementer() -> ((Int) -> Int) {
	func addOne(number: Int) -> Int {
		return 1 + number
	}
	return addOne
}
var increment = makeIncrementer()
increment(7)
```

上面例子 `makeIncrementer` 返回了一个参数是 Int，返回值也是 Int 的函数。这里要注意的是，如果函数作为值进行传递的话，不支持参数标签的，如果指定了参数标签会在编译时报错 `Function types cannot have argument label 'xxx'; use '_' instead`。

函数也可以当做参数传入另一个函数：

```swift
func hasAnyMatches(list: [Int], condition: (Int) -> Bool) -> Bool {
	for item in list {
		if condition(item) {
			return true
		}
	}
	return false
}
func lessThanTen(number: Int) -> Bool {
	return number < 10
}
var numbers = [20, 19, 7, 12]
hasAnyMatches(list: numbers, condition: lessThanTen)
```

函数实际上就是一类特殊的闭包：可以放在以后调用而已。闭包中的代码可以访问闭包创建环境中的变量、方法等，甚至在其他环境下调用也可以。关键字 `in` 用以区分开 参数、返回值和闭包代码。

```swift
numbers.map({
	(number: Int) -> Int in
	let result = 3 * number
	return result
})
```

如果一个闭包的类型已知，比如作为一个回调函数，你可以忽略参数的类型和返回值。单个语句闭包会把它语句的值当做结果返回。

```swift
let mappedNumbers = numbers.map({ number in 3 * number })
print(mappedNumbers)
```
