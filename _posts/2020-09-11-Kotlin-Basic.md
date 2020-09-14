---
title: Kotlin 笔记
layout: post
typora-root-url: ../
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



## 面向对象

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

```kotlin
class Bird(weight: Double, age: Int, color: String) {
    val weight: Double
    val age: Int
    val color: String
    init {
        this.weight = weight
        println("The bird's weight is ${this.weight}.")
        this.age = age
        println("The bird's age is ${this.age}.")
    }
}
```

类似与static代码快,不过可以访问非static成员

一个类可以有多个init块,类初始化时按照顺序执行.

##### 延迟初始化

有的属性不会在构造时赋值,可以用`by lazy` `latinit`让它延迟赋值

> 即它可以不用在类对象初始化的时候就必须有值

```kotlin
class Bird(val weight: Double,val age: Int,val color: String){
    val sex : String by lazy {
        if(color == "yellow") "male" else "female"
    }
}
```

使用`by lazy` 

* 必须是val类型,不能var

* 首次被调用时赋值,并且后续不能改变.

* 具备同步锁,只能有一个线程访问,线程安全.可以传入如下参数取消单线程限制.

  ```kotlin
  val sex: String by lazy(LazyThreadSafetyMode.PUBLICATION){
      if(color == "yellow") "male" else "female"
  }
  
  val sex: String by lazy(LazyThreadSafetyMode.NONE){
      if(color == "yellow") "male" else "female"
  }
  ```

使用`lateinit`

* 不能用于基本数据类型
* 只能用于var类型

```kotlin
class Bird(val weight: Double,val age: Int,val color: String){
    lateinit var sex : String
    fun printSex() {
        this.sex = if(this.color == "yellow") "male" else "female"
        println("sex - " + sex);
    }
}
```

**Kotlin int 不会自动转换成double,所以需要double的地方必须手动加上.0**

`Delegates.notNull<T>`

对基本数据类型延迟赋值

```kotlin
var test by Delegates.notNull<Int>()
fun xXXXX(){
    test = 1
    test = 2
}
```

#### 构造

Kotlin也可以实现多个构造方法,不过分为主从构造,类外面那个`class Bird(....)`为主构造,其他的为从

从构造必须直接或间接调用主构造

```kotlin
class MyView : View {
    constructor(context : Context) : this(context,null)
    constructor(context : Context,attrs : AttributeSet?) : this(context,attrs,0)
    constructor(context : Context,attrs : AttributeSet?,defStyleAttr: Int) : super(context,attrs,defStyleAttr) {
        ....
    }
}
```

#### 继承

```kotlin
open class Bird {
    open fun fly() {
        println("Flying..")
    }
}

class Penguin : Bird {
    override fun fly() {
        println("Can't fly,QAQ")
    }
}
```

* 直接使用 : 代替 extends implements
* 默认均为final,所以允许被继承的类和成员需要使用open修饰

#### 访问控制

*子类应该尽量避免重写父类的非抽象方法*

> 里氏替换原则
>
> 对里氏替换原则通俗的理解是:子类可以扩展父类的功能,但不能改变父类原有的功
> 能。它包含以下4个设计原则:
> ·子类可以实现父类的抽象方法,但不能覆盖父类的非抽象方法;
> ·子类可以增加自己特有的方法;
> ·当子类的方法实现父类的方法时,方法的前置条件(即方法的形参)要比父类方法
> 的输入参数更宽松;
> ·当子类的方法实现父类的抽象方法时,方法的后置条件(即方法的返回值)要比父
> 类更严格。

##### 默认修饰符 final

##### sealed

`sealed class Bird` 则可以被继承,但是子类必须和父类在同一个文件中.但是sealed声明的类为抽象类,无法被初始化.

##### abstrat 同Java,使用后可以被继承且没有sealed限制

##### 可见性控制

> 1)Kotlin与Java的默认修饰符不同,Kotlin中是public,而Java中是
> default。
> 2)Kotlin中有一个独特的修饰符internal。
> 3)Kotlin可以在一个文件内单独声明方法及常量,同样支持可见性修饰符。
> 4)Java中除了内部类可以用private修饰以外,其他类都不允许private修饰,
> 而Kotlin可以。
> 5)Kotlin和Java中的protected的访问范围不同,Java中是包、类及子类可访
> 问,而Kotlin只允许类及子类。

###### internal :　模块内访问，

> ·一个Eclipse项目
> ·一个Intellij IDEA项目
> ·一个Maven项目
> ·一个Grandle项目
> ·一组由一次Ant任务执行编译的代码

部分java类库的类会使用default.只能在包内访问,这时第三方引用的时候只要创建同名包就能访问到这个类,而internal的不会.

###### private

仅当前文件内可以访问,java是内部类可以访问.

###### protected

java 中是包,类,子类. kotlin中没有包作用域,所以只是类,子类

![Visibility](/assets/KotlinCore/3_visibility.png)



