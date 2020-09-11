---
title: Kotlin 笔记
layout: post
---

# Kotlin 笔记

## 认识Kotlin

#### Kotlin Scala Java

三种主要的运行于JVM的编程语言

Scala 主要是面向对象的优化以及函数式编程的支持,导致语法复杂化.

Kotlin 主要目的在于Java的优化.虽然新版本Java也实现了很多新的语法特性,但是Android不支持,所以Kotlin成了一个更好的选择



## 基本语法

很多有个大概印象即可,讲也没有详细讲,挺凌乱的.

#### 定义变量

```kotlin
val a : String = "I am Kotlin"
```

相比较于Java,Kotlin类型放在后面

#### 类型推导

Kotlin编译器可以自动判断类型,所以生命变量可以直接省略类型声明

```kotlin
val a = "I am Kotlin"
```



#### 函数返回类型

```kotlin
fun sum(x : Int, y : Int) : Int { return x + y}
```

向这种简单的返回,可以使用 **表达式函数体**,与之对应,上面那种{}形式则称为 **代码块函数体**

```kotlin
fun sum(x: Int, y: Int) = x + y
```

*不过递归并不能推导出返回值类型,所以无法使用*

#### var val

var 声明变量

val final声明的变量,与java中加了final的特性一致,也就是常量了

优先使用val以免引用莫名其妙变动导致问题,java中其实也更倾向与使用final,但是每次都多写一个关键字太麻烦了.

val也可以仅声明,然后后续使用前再赋值.不过这样写声明时一定要声明类型

```kotlin
val i : Int
i = 1
```

#### 高阶函数,Lambda

```kotlin
fun filterCountries(
countries: List<Country>,
test: (Country) -> Boolean): List<Country> // 增加了一个函数类型的参数test
{
    val res = mutableListOf<Country>()
    for (c in countries) {
        if (test(c)) { // 直接调用test来进行筛选
        	res.add(c)
        }
    }
    return res
}
```

test就是一个函数类型变量

声明方式:

```kotlin
() -> Unit
(Int, String) -> Unit
(errCode: Int, errMsg: String) -> Unit
```

那么实际调用时可以通过`::`引用函数

```kotlin
filterCountries(countries,A::isBigEuropeanCountry)
```

#### 匿名函数

类似于匿名内部类,这次连函数也不声明了,直接定义

```kotlin
countryApp.filterCountries(countries, fun(country: Country): Boolean {
	return country.continient == "EU" && country.population > 10000
})
```

#### Lambda表达式

*一种语法糖*

简化后的匿名函数

这里介绍的太混乱,大量语法糖夹杂在一起

#### 面向表达式编程

带有返回值的语句

Kotlin将很多Java语句改成了表达式,可以带有返回值

```kotlin
fun ifExperssion(flag : Boolean){
    val a = if(flag) "Dive into Kotlin" else ""
    println(a)
}
```

#### 枚举 和 when

##### 枚举

```kotlin
enum class DayOfWeek(val day : Int){
    MON(1),
    TUE(2),
    WEN(3),
    THU(4),
    FRI(5),
    SAT(6),
    SUN(7)
    ; // 如果以下有额外的方法或属性定义,则必须强制加上分号
    
    fun getDayNumber(): Int {
   		return day
    }
}
```

##### when

无when版本:

```kotlin
fun schedule(day: Day, sunny: Boolean) = {
    if (day == Day.SAT) {
    	basketball()
    } else if (day == Day.SUN) {
    	fishing()
    } else if (day == Day.FRI) {
    	appointment()
    } else {
        if (sunny) {
        	library()
        } else {
        	study()
        }
    }
}
```

转换成有when版本:

```kotlin
fun schedule(sunny: Boolean, day: Day) = when (day) {
    Day.SAT -> basketball()
    Day.SUN -> fishing()
    Day.FRI -> appointment()
    else -> when {
        sunny -> library()
        else -> study()
    }
}
```

#### 循环



