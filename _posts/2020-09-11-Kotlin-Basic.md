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

##### for

```ko
for (i in 1..10) ...
for (i : Int in 1..10) ...
for (i in 1..10 step 2) // 每次+2
for (i in 10 downTo 1 step 2) //倒序
for (i in 1 until 10) //不包含10 ->  123456789
```

##### 范围表达式 1..10

##### in

类似contains,

```kotlin
"a" in listOf("b","c") // false
"a" !in listOf("b","c") // true
for((index,value) in array.withIndex()) ...
```

##### 中缀表达式

`1 to "one"` 返回一个<1,"one">

##### 可变参数函数

用varargs定义参数则参数数量可变,类似java中的`...`不过没有任何位置限制

```kotlin
fun printLetters(vararg letters: String, count : Int){
     print("${count} letter is")
     for(letter in letters) print(letter)
 }
 printLetters("a","b","c",count= 3)
 val letters= arrayOf("a","b","c")
 printLetters(*letters,count = 3)
```

不过调用时注意调用方法

* 直接输入多个参数,对应的非可变参数要写明`count=3`
* 用*引用数组

#### 字符串

`"aaa" +１`　正确

`1 + "aaa"` 错误

其他的和java一致

##### 原生字符串

```kotlin
val rawString = """
 \n Kotlin is awesome.
 \n Kotlin is a better Java."""

print(rawString)

\n Kotlin is awesome.
\n Kotlin is a better Java.
```

也就是不会进行转义,也不用手动加\n等



```kotlin
fun message(name: String, lang: String) = "Hi ${name}, welcome to ${lang}!"
"Kotlin has ${if ("Kotlin".length > 0) "Kotlin".length else "no"} letters."
```

不止可以直接引用变量,还能插入代码



##### 判断

== 用来判断内容是否一致, === 用来判断引用是否一致, !== 与===相反



## 对象

### 类和接口

#### 类

```kotlin
class Bird {
    val widget : Double = 500.0
    val color : String = "blue"
    val age : Int = 1
    fun fly(){} //public
}
```

* 除非显示声明延迟初始化,则必须赋初始值
* 默认public,而Java中默认包可见

#### 接口

```kotlin
interface Flyer {
    val speed : Int
    fun kind()
    fun fly(){
        print("Flying")
    }
}
```

---> Java

```kotlin
public interface Flyer {
int getSpeed();
void kind();
void fly();
public static final class DefaultImpls {
public static void fly(Flyer $this) {
String var1 = "I can fly";
System.out.println(var1);
}
}
```

kotlin可以增加默认实现的方法,而Java则在jdk8之后才通过default实现了这一特性

接口中的常量是通过java的get方法实现,所以定义初始值时不能直接=,而是:

```kotlin
interface Flyer {
val height
get() = 1000
}

```

#### 对象

```kotlin
val bird = Bird()
```

##### 构造方法重载

```kotlin
class Bird(val weight : Double = 0.0,val age : Int = 0, val color: String="white")
// 可以省略{}
val bird1 = Bird(color = "black")
val bird2 = Bird(weight = 1000.00, color = "black")
```

不必在一个个重载构造,新建对象时如果不指定属性名称,则必须按照顺序依次赋值.

增加val则会自动在类内部定义一个同名成员变量,如果没有val则只相当于在新建时传入了一个参数.

##### init

类似与static代码快,不过可以访问非static成员

