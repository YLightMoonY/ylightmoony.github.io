---
title: Kotlin 笔记
layout: post
typora-root-url: ../
---

# Kotlin 笔记

[TOC]



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

#### var val

var 声明变量

val final声明的变量,与java中加了final的特性一致,也就是常量了

优先使用val以免引用莫名其妙变动导致问题,java中其实也更倾向与使用final,但是每次都多写一个关键字太麻烦了.

val也可以仅声明,然后后续使用前再赋值.不过这样写声明时一定要声明类型

```kotlin
val i : Int
i = 1
```

#### 循环

##### for

```kotlin
for (i in 1..10) ...
for (i : Int in 1..10) ...
for (i in 1..10 step 2) // 每次+2
for (i in 10 downTo 1 step 2) //倒序
for (i in 1 until 10) //不包含10 ->  123456789
```

*包含10*

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

#### 函数返回类型

```kotlin
fun sum(x : Int, y : Int) : Int { return x + y}
```

向这种简单的返回,可以使用 **表达式函数体**,与之对应,上面那种{}形式则称为 **代码块函数体**

```kotlin
fun sum(x: Int, y: Int) = x + y
```

*不过递归并不能推导出返回值类型,所以无法使用*

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

会自动生成相应的get方法

* 除非显式声明延迟初始化,则必须赋初始值
* 默认public,而Java中默认包可见
* 建议使用val,也就是不可变成员属性 *使用var有什么后果?*

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

###### getter setter

当定义一个成员变量时,编译器会自动帮忙生成getter setter,不用再生成模板代码.

1. val因为不可更改,所以只有getter方法(因为实际java实现的val就是一个getter方法),var会同时又getter setter
2. private修饰的变量将不会有getset,因为外部不可访问了

##### init

```kotlin
class Bird(weight: Double, age: Int, color: String) {
    val weight: Double
    val age: Int
    val color: String
    init {
        // 构造方法参数可以在init块里调用
        //　也可以直接在声明成员变量时赋值
        // 但是不能在其他方法内部直接调用
        this.weight = weight
        println("The bird's weight is ${this.weight}.")
        this.age = age
        println("The bird's age is ${this.age}.")
    }
}
```

类似与static代码快,不过可以访问非static成员

一个类可以有多个init块,类初始化时按照顺序执行.

*与下面的by lazy有啥区别*

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

* 接受的是一个返回Lazy<T>返回值的lambda表达式

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

* 不能用于基本数据类型,只能用于包装类,**但是Kotlin的包装类怎么表达?**

  全限定名可以引用到`lateinit var age2 : java.lang.Double`或者使用下面的`Delegates.notNull<T>`

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

```kotlin
class Passer(age: Int){
    val age : Int
    init{
        this.age = age
    }
    constructor(birthDay:Date) : this(getAgeByBirth(birthDay))// 这里调用的方法不能调用本类成员方法
}

fun getAgeByBirth(birthDay: Date): Int {
    return 0
}

```

*因为不了解工厂模式对这个感知不深*

[必须在完全构造的对象上使用成员方法](https://www.codenong.com/38481135/)

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

* 直接使用 `: `代替 `extends implements`
* 默认均为final,所以允许被继承的类和成员需要使用open修饰

### 访问控制

*子类应该尽量避免重写父类的非抽象方法*

> 里氏替换原则
>
> 对里氏替换原则通俗的理解是:子类可以扩展父类的功能,但不能改变父类原有的功
> 能。它包含以下4个设计原则:
>
> * 子类可以实现父类的抽象方法,但不能覆盖父类的非抽象方法;
> * 子类可以增加自己特有的方法;
> * 当子类的方法实现父类的方法时,方法的前置条件(即方法的形参)要比父类方法
>   的输入参数更宽松;
> * 当子类的方法实现父类的抽象方法时,方法的后置条件(即方法的返回值)要比父
>   类更严格。

##### sealed

```kotlin
sealed class Bird2 {
    open fun fly() = "I can fly"
}
class Passer2 : Bird2() {
}
```

则可以被继承,但是子类必须和父类在同一个文件中.但是sealed声明的类为抽象类,无法被初始化.

*为何一定要调用父类构造方法?是java的限制吗?*

*-> 子类实例化都会调用父类构造,java中可以省略空的调用,kotlin必须显式声明*

##### abstrat 

同Java,使用后可以被继承且没有sealed限制

##### 可见性控制

> 1. Kotlin与Java的默认修饰符不同,Kotlin中是public,而Java中是
>    default。
> 2. Kotlin中有一个独特的修饰符internal。
> 3. Kotlin可以在一个文件内单独声明方法及常量,同样支持可见性修饰符。
> 4. Java中除了内部类可以用private修饰以外,其他类都不允许private修饰,
>    而Kotlin可以。
> 5. **Kotlin和Java中的protected的访问范围不同,Java中是包、类及子类可访
>    问,而Kotlin只允许类及子类。**

*Java可见性是啥玩意来着*

* private 本类 **外部类可以访问内部类私有成员**
* 默认 包内
* protected 包内+子类
* public 所有

###### internal :　模块内访问，

> * 一个Eclipse项目
> * 一个Intellij IDEA项目
> * 一个Maven项目
> * 一个Grandle项目
> * 一组由一次Ant任务执行编译的代码

部分java类库的类会使用default.只能在包内访问,这时第三方引用的时候只要创建同名包就能访问到这个类,而internal的不会.

###### private

仅当前文件内可以访问,java是内部类可以访问.

###### protected

java 中是包,类,子类. kotlin中没有包作用域,所以只是类,子类

![Visibility](/assets/KotlinCore/3_visibility.png)

### 多继承

#### 接口多继承

```kotlin
interface Flyer {
    fun fly()
    fun kind() = "Flying animals"
}
interface Animal {
    val name : String
    fun eat()
    fun kind() = "Flying animals"
}

class Bird(override val name : String) : Flyer,Animal{
    override fun eat() {
        println("I can eat")
    }

    override fun fly() {
        println("I can fly")
    }

    override fun kind(): String {
        return super<Flyer>.kind()
    }
}

fun main(args : Array<String>){
    val bird = Bird("sparrow")
    println(bird.kind())
}
```

多继承实现接口:

1. 需要实现所有抽象成员方法成员变量.当有同名默认实现方法时,需要重写.
2. 使用`super<T>.xxx()`这种范型的方式调用相应接口同名方法
3. 必须使用`override`关键字

#### 内部类 & 其多继承

```kotlin
package chap03

class OuterKotlin{
    val name = "Truely kotlin's inner class"
    inner class InnerKotlin {
        fun printNmae() {
            println("The name is $name")
        }
    }
}

fun main() {
    OuterKotlin().InnerKotlin().printNmae()
}
```

kotlin 中需要用inner class声明一个内部类,此时才可以直接引用外部类属性.

如果不声明inner,则默认是一个static内部类(嵌套类),不能引用外部类非static属性.



使用内部类多继承就是用组合代替封装.

使用private修饰类声明,确保封闭性

#### **委托代替多继承** `by`

kotlin新语法.

```kotlin
interface  CanFly{
    fun fly()
}
interface CanEat {
    fun eat()
}
open class Flyer1 : CanFly {
    override fun fly() {
        println("I can fly")
    }
}
open class Animal1 : CanEat {
    override fun eat() {
        println("I can eat")
    }
}
class Bird1(flyer : Flyer1,animal : Animal1) : CanFly by flyer,CanEat by animal{}

fun main() {
    val flyer = Flyer1()
    val animal = Animal1()
    val b = Bird1(flyer,animal)
    b.fly()
    b.eat()
}
```

这样调用的时候可以直接通过Bird1本身的fly eat方法,会自动委托flyer animal对应方法去执行.

名为继承,实则组装

*kotlin作用域到底怎么算,同一个包不同文件不算不同作用域不能定义同名类* -> 默认public

### 数据类

*JavaBean 的kotlin版本*

```kotlin
data class Bird(var weight : Double,var age : Int,var color : String)
```

通过data关键字定义

这样的类会编译会自动生成getter,setter,构造,equals,toString,copy,componentN等一系列方法

componentN的意义

可以将类属性一次性绑定到变量上

``` kotlin
varl b1 = Bird(10.0,1."blue")
val (weight,age,color) = b1
```

就是通过componentN实现

类似操作

```kotlin
val (weight,age,color) = "10.0,1,blue".split(",")
```

不过这种数组型的一次性赋值最多支持5个,如果太多就会完全搞不明白怎么对应.

自己实现componentN:

```kotlin
data class Bird(var weight: Double, var age: Int, var color: String) {
    var sex = 1
    operator fun component4(): Int { //operator关键字
    	return this.sex
    }
    constructor(weight: Double, age: Int, color: String, sex: Int) : this(weight, age, color) {
    	this.sex = sex
    }
}
```

Pair, Triple 一次性赋值:

```kotlin
val (weightP, ageP) = Pair(20.0, 1)
val (weightT, ageT, colorT) = Triple(20.0, 1, "blue")
```

#### 约定和使用

> * 数据类必须拥有一个构造方法,该方法至少包含一个参数,一个没有数据的数据类是没有任何用处的;
>
> * 与普通的类不同,数据类构造方法的参数强制使用var或者val进行声明;
>
> * data class之前不能用abstract、open、sealed或者inner进行修饰; -> 不可抽象,被继承,不能在类内部定义非嵌套数据类,
>
>   *那使用static是否可以?* -> kotlin没有static关键字.不过直接可以直接在类内部定义数据类本身就是嵌套的,static的.放在`companion object`里也可以

### object

#### 伴生 `companion object`

```kotlin
class Prize(val name: String, val count: Int, val type: Int){
    companion object{
        val TYPE_REDPACK = 0
        val TYPE_COUPON = 1
        fun isRedpack(prize : Prize) : Boolean {
            return prize.type == TYPE_REDPACK
        }
    }
}

fun main() {
    val prize = Prize("红包啊",10,Prize.TYPE_REDPACK)
    println(Prize.isRedpack(prize))
}
```

`companion object{`块内直接就被定义为static属性和方法.

#### 工厂模式

```kotlin
class Prize private constructor(val name: String, val count: Int, val type: Int) {
	companion object {
        val TYPE_COMMON = 1
        val TYPE_REDPACK = 2
        val TYPE_COUPON = 3
        val defaultCommonPrize = Prize("普通奖品", 10, Prize.TYPE_COMMON)
        fun newRedpackPrize(name: String, count: Int) = Prize(name, count, Prize.TYPE_REDPACK)
        fun newCouponPrize(name: String, count: Int) = Prize(name, count, Prize.TYPE_COUPON)
        fun defaultCommonPrize() = defaultCommonPrize //无须构造新对象
    }
}
fun main(args: Array<String>) {
    val redpackPrize = Prize.newRedpackPrize("红包", 10)
    val couponPrize = Prize.newCouponPrize("十元代金券", 10)
    val commonPrize = Prize.defaultCommonPrize()
}

```

#### 单例

使用object代替class声明

```kotlin
object DatabaseConfig{
    var host : String = "127.0.0.1"
    var .........
}
```

之后直接用DatabaseConfig调用即可,直接调用的就是一个单例对象,不是类.

得到的是最简单的饿汉式单例模式

#### object表达式

object 实现匿名内部类:

```kotlin
Collections.sort(list, object : Comparator<String>{
        override fun compare(o1: String?, o2: String?): Int {
            if(o1 == null)
                return -1
            if(o2 == null)
                return 1
            return o1.compareTo(o2) 
        }
    })
```

object 匿名内部类可以继承或者实现多个类或接口,而java版只能一个.

当然这种简单的直接用lambda表达式更方便:

```kotlin
val comparator = Comparator<String> { s1, s2 ->
    if (s1 == null)
    	return@Comparator -1 //我们已经在第2章中接触过这种语法了
    else if (s2 == null)
    	return@Comparator 1
    s1.compareTo(s2)
}
Collections.sort(list, comparator)

```

*前面说的那么点根本没高明白lambda到底怎么用的,太乱了*



## 代数数据类型和模式匹配

### 代数数据类型 (Algebraic Data Type,ADT)

> This is a type where we specify the shape of each of the elements

从后面看就是可以计数的数据类型?

#### 计数

Boolean有true和false,计数2

Unit 只有本身一个值,计数1.

#### 积类型

```kotlin
class BooleanProduceUnit(a : Boolean, b : Unit){}
```

计数 Boolean(2) * Unit(1) = BooleanProduceUnit(2)

#### 和类型

```kotlin
enum class Day{SUN,MON,TUE,WED,THU,FRI,SAT}
```

计数 Day.SUN + Day.MON + ... + Day.SAT = 7

##### 密封类

子类只能定义在父类或者同一个文件中.

```kotlin
sealed class Day {
    class SUN : Day()
    class MON : Day()
    ...
    class SAT : Day()
}
```

这样使用when进行分支处理时可以不用处理else情况,保证能将每个值都列举出来.

```kotlin
fun schedule(day : Day) = when(day){
    is Day.SUN -> xxxx()
    is Day.MON -> xxx()
    ...
    is Day.SAT -> x()
}
```

编译器会检查是否所有情况都被列举出来.

#### 构造代数数据类型

```kotlin
sealed class Shap {
    class Circle(val radius : Double) : Shap()
    class Rectangle(val width : Double,val height: Double) : Shap()
    class Triangle(val base : Double,val height : Double) : Shap()
}
```

### 模式匹配

模式 也就是表达式,比如 5,a+b, a> b ......

#### 常见模式

##### 常量模式

比较常量是否相等

```kotlin
fun constantPattern(a: Int) : String = when (a) {
    1 -> "It's 1"
    2 -> "It's 2"
    else -> "Not my number"
}
```

##### 类型模式

如上面的Shap

##### 逻辑表达式模式

```kotlin
fun logicPattern(a : Int) = when {
    a in 1..11 -> println("${a} is smaller than 12 and bigger than 0")
    else -> println("ELSE")
}
```

更类似于if..else if..else了

#### 嵌套表达式

```kotlin
fun simplifyExpr(expr: Expr) = when (expr) {
    is Expr.Num -> expr
    is Expr.Operate -> when (expr) {
        Expr.Operate("+",expr.left,Expr.Num(0)) -> expr.left
        Expr.Operate("+",Expr.Num(0), expr.right) -> expr.right
        else -> expr
    }
}
```

通过解构进行类似常量匹配直接对比每一个值是否相等.

不能省略外层嵌套,因为那个时候还不能判断到底是跟`Expr.Num`还是`Expr.Operate`对比.通过外层类型匹配之后自动Smart Cast转换expr变量类型才能再匹配

*还举了半天scala例子,不懂scala真的看的很困难*

### 增强kotlin模式匹配

#### 类型测试/类型转换

Java:

```Java
if(expr instanceof Expr.Operate) {
(Expr.Operate) expr ...
}
```

kotlin因为支持smart-cast,所以只需要类型测试即可.

```kotlin
expr.left is Expr.Num && expr.left.value == 0
```

#### 面向对象分解

> 通过在父类中定义一系列的测试方法(比如上面的测试是否为数值),然后在子类中实现这些方法,就可以在不同的子类中来做相应操作了。

```kotlin
sealed class Expr {
    abstract fun isZero() : Boolean
    abstract fun isAddZero() : Boolean
    abstract fun left() : Expr
    abstract fun  right() : Expr
    data class Num(val value: Int) : Expr() {
        override fun isAddZero(): Boolean {
            return false
        }

        override fun isZero(): Boolean {
            return this.value == 0
        }

        override fun left(): Expr {
            throw Throwable("no element")
        }

        override fun right(): Expr {
            throw Throwable("no element")
        }
    }

    data class Operate(val opName:String,val left:Expr,val right: Expr) : Expr() {
        override fun left(): Expr {
            return left
        }

        override fun right(): Expr {
            return right
        }

        override fun isZero(): Boolean {
            return false
        }

        override fun isAddZero(): Boolean {
            return this.opName == "+" &&
                    (this.left.isZero() || this.right.isZero())
        }
    }
}

val expr = Expr.Operate("+", Expr.Num(0), Expr.Operate("+", Expr.Num(0), Expr.Num(1)))
fun simplifyExpr(expr: Expr): Expr = when {
    expr.isAddZero() && expr.right().isAddZero() && expr.right().left().isZero() -> expr.right().right()
    else -> expr
}

```

* 优势:　简单结构的数据极大精简语法
* 劣势: 复杂结构会给每个子类都增加大量无用方法,比如对Num来说必须要实现一次left right

*其本身操作也不符合面向对象逻辑,感觉不怎么实用*

#### 访问者模式

> 表示一个作用于某对象结构中的各元素的操作,它使你可以在不改变各元素类的前提下定义作用于这些元素的新操作。

核心就是将对成员变量的判断封装到另一个类里面.就像上面要判断'0+x'.属于对象分解的衍生.将需要每个子类都要实现一遍的方法全部整合到一个专门的判断类里.

```kotlin
sealed class Expr{
    abstract fun isZero(v:Visitor) : Boolean
    abstract fun isAddZero(v: Visitor) : Boolean
    //abstract fun simplifyExpr(v:Visitor) : Expr

    class Num(val value:Int) :Expr(){
        override fun isZero(v: Visitor): Boolean {
            return v.matchZero(this)
        }

        override fun isAddZero(v: Visitor): Boolean {
            return v.matchAddZero(this)
        }

//        override fun simplifyExpr(v: Visitor): Expr {
//            return v.matchSimplifyExpr(this)
//        }

    }

    class Operate(val name : String,val left:Expr,val right:Expr) : Expr(){
        override fun isZero(v: Visitor): Boolean = v.matchZero(this)

        override fun isAddZero(v: Visitor): Boolean = v.matchAddZero(this)

//        override fun simplifyExpr(v: Visitor): Expr = v.matchSimplifyExpr(this)

    }
}

class Visitor {
    fun matchZero(num: Expr.Num) : Boolean = num.value == 0
    fun matchZero(operate: Expr.Operate): Boolean = false

    fun matchAddZero(num: Expr.Num): Boolean = false
    fun matchAddZero(operate: Expr.Operate): Boolean = when(operate){
        Expr.Operate("+",operate.left,Expr.Num(0)) -> true
        Expr.Operate("+",Expr.Num(0),operate.right) -> true
        else -> false
    }
}

```

**缺点:** 每个子类都要去定义一遍判断逻辑,后期维护困难.

#### 暂时在Kotlin中还不能实现

> * Typecase
> * 样本类(Case Classes)
> * 抽取器(Extractor)

## 类型系统

#### Null的问题

NullPointerException非常常见.经常需要加入大量的非空判断来解决.Java8通过引入Optional<T>来规避,可是因为使用了范型,导致很高的性能开销.看看kotlin如何解决

*不是之前以为的空不报错,而是通过种种语法检查规避空的问题*

### Kotlin 可空类型

kotlin默认的类型声明为不可空,`val a:Long = null` 会在编译器就报错

需要可空时 : `val a:Long? = null` 加个问号

如果是成员变量可空,则引用时也需要加上?

```kotlin
data class Glass(val degreeOfMyopia: Double)
data class Student(val glass: Glass?)
data class Seat(val student: Student?)

fun getDegree(seat: Seat) {
    println("眼睛度数: ${seat.student?.glass?.degreeOfMyopia}")
}
fun main() {
    val seat = Seat(null)
    getDegree(seat)
}

// --> 眼睛度数: null
```

#### Elvis操作符 `?:`  (又名合并运算符)

指定为空时默认值.

```kotlin
val degree = student.glass?.degreeOfMyopia?:-1
```

#### 非空断言 `!!.`

为空时抛出NPE异常

```kotlin
val degree = student.glass!!.degreeOfMyopia
```

反编译为java可以看出,kotlin空判断本身是使用`if..else`实现

相较于Java Optional<T>:

* 兼容性更好
* 性能更好开销更低
* 语法简洁

#### Either & let

```kotlin
// 假定是从服务器获得的数据,可能为null
String food : String? = "meat"
// let: 当food不为空执行{}里面
food?.let{
    // food不为空执行
    doSomething()
    
    // 用it代替food访问其成员
    ToastUtils.showShort(it)
}
```



#### 类型检查

`is` `!is` 

```kotlin
if(obj is String){
    println("obj is String value")
}
if(obj !is String){
    println("Obj isn't a String")
}
```

#### 类型转换 Smart Cast

kotlin会自动转换类型

```kotlin
when(obj){
    is String -> println(obj.length)
    !is String -> pringln("not a string")
}
```

> 当且仅当Kotlin的编译器确定在类型检查后该变量不会再改变,才会产生Smart Casts

所以使用var声明的变量无法自动转换

```kotlin
class Kot {
    var stu:Student? = getStu()
    fun dealStu() {
        if (stu != null) {
            println(stu.glass)
        }
    }
}
// 编译错误: Kotlin: Smart cast to 'Student' is impossible, because 'stu' is a mutable property that could have been changed by this time
```

可以将var 改为 val

* 使用let

  ```kotlin
  class Kot {
      var stu:Student? = getStu()
      fun dealStu() {
          stu?.let{
              println(it.glass)
          }
      }
  }
  ```

* 使用`as` 进行强制转换 **下述as部分无法编译**

  ```kotlin
  class Kot {
      var stu: Student? = getStu() as Student?
      fun dealStu() {
          if(stu != null){
              println(stu.glass)
          }
      }
  }
  ```

* 这样倒是可以

  ```kotlin
  var stu1:Student? = getStu()
  fun dealStu1() {
      if (stu1 != null) {
          println((stu1 as Student).glass)
      }
  }
  ```

### kotlin类型结构

![Visibility](/assets/KotlinCore/4_class.png)

kotlin中`Any`对应Java中的`Object` 

Kotlin视Java中.Object作为参数和返回值时为`平台类型`

#### 平台类型

> 所有Java引用类型在Kotlin中都表现为平台类型。当在Kotlin中处理平台类型的值的时候,它既可以被当作可空类型来处理,也可以被当作非空类型来操作。

#### `Any?` 所有类型的根类型

> 如果你只有Java这门编程语言的开发经验,很容易陷入一个误区:继承关系决定父子类型关系。因为在Java中,类与类型大部分情况下都是“等价”的(在Java泛型出现前)。
>
> 事实上,“继承”和“子类型化”是两个完全不同的概念。子类型化的核心是一种类型的替代关系
>
> 作为比较,继承强调的是一种“实现上的复用”,而子类型化是一种类型语义的关系,与实现没关系。部分语言如Java,由于在声明父子类型关系的同时也声明了继承的关系,所以造成了某种程度上的混淆。
>
> 因此,我们可以很大胆地说,Any?是Any的父类型,而且是所有类型的根类型,虽然当前的Kotlin官网文档没有介绍过这一点。



#### Noting与Noting?

![Visibility](/assets/KotlinCore/5_nothin.png)

kotlin引入了 Nothing类型作为所有类型的底层.

返回Nothing的表达式之后的语句均无法执行.

实际效果类似与break return

而Nothing?作为Nothing父类,与null类似,可以直接替换为null

#### 自动拆箱装箱

根据反编译代码来看.kotlin的`Int`等同于Java的`int`,没有进行装箱操作,而`Int?`才会等同于`Integer`

#### 数组

kotlin中不再使用java的方式,而是如下:

```kotlin
val array = arrayOf() // 声明长度0数组
val array = arrayOf(a1,a2,a3...an) // 声明并实例化数组
```

并且,kotlin对基本数据类型做了单独的数组(避免自动装箱带来的开销).

```kotlin
val array = intArrayOf(1,2,3)
```

* 数组大小固定,且存放相同类型数据
* 数组内存连续,性能较高

### 范型

* 编译阶段就能检查集合中的类型
* 取出集合元素能自动转换类型
* 能写出更通用的代码
* 代码可读性提升

#### 定义

同样使用`<T>`之类的就行

```kotlin
class MyArrayList<T> : ArrayList<T>(){
    fun find(item: T) : T? {
        val index = this.indexOf(item)
        return if (index >= 0) item else null
    }
}
```

*超前: 使用扩展函数+范型*

```kotlin
fun main() {
    val array = ArrayList<String>()
    array.add("123")
    println(array.find("123"))
}
fun <T> ArrayList<T>.find(item : T) : T?{
    val index = this.indexOf(item)
    return if(index >= 0) item else null
}
```

**Java中在使用带有范型的方法/类时,可以省略范型,而kotlin中不能省**

不过因为Smart Cast, 如果能推倒出范型所代表的实际类型,则可以省略

#### 上界约束

就是范型可以指定为某个类及其子类

```kotlin
open class Fruit(val weight: Double)
class Apple(weight:Double) :Fruit(weight)
class FruitPlate<T : Fruit>(val t : T)

val plate = FruitPlate<Fruit>(Apple(12.2))
val plate2 = FruitPlate(Apple(12.2)) // 可以推导出是Apple
```

使用`where`对同一个范型做多个上界约束

```kotlin
interface Packed{}
class PackedFruit(weight: Double) : Fruit(weight),Packed

fun<T> cut(t:T) where T:Fruit,T:Packed{
    println("Can cut")
}

val fruit = PackedFruit(12.2)
cut<PackedFruit>(fruit)
```

### 类型擦除

#### Java中数组不能使用范型

Java中的数组是*协变*的,其在运行时能获取元素类型.`Object[]`是所有数组的父类

```Java
Fruit[] fruits = new Apple[10]; // 正确
List<Fruit> fruits2 = new ArrayList<Apple>(); // 错误
System.out.println(Apple[].getClass());
// --> class [Ljavat.Apple;
System.out.println((new ArrayList<Apple>()).getClass());
// --> class java.util.ArrayList;
```

而集合不同,`List<Object>`并不是`List<T>`父类

Kotlin可以定义范型数组,不过相对应的,是去了*协变*

#### 为何范型是类型擦除的

