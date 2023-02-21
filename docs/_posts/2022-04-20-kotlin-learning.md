---
layout: post
title:  "Kotlin学习笔记"
date:   2022-04-20 
categories: kotlin

---

#### Kotlin构造函数
Kotlin中讲构造方法独立了出来，使用关键字constructor来表示。在Kotlin类中只有一个主构造函数(主构造器)，而辅助构造函数（次级构造器）可以是一个或者多个。
如果主构造函数没有任何注解或者可见性修饰符，可以省略这个 constructor 关键字。
`class Person(firstName: String) { /*……*/ }`

主构造函数不能包含任何的代码。初始化的代码可以放到以 init 关键字作为前缀的初始化块（initializer blocks）中。
注意 Kotlin 并没有 new 关键字
`val person = Person("Joe Smith")`

默认情况下，Kotlin 类是最终（final）的：它们不能被继承。 要使一个类可继承，请用 open 关键字标记它。
`open class Base(p: Int)// 该类可继承

class Derived(p: Int) : Base(p)
`


Kotlin中as关键字可以用于对象的类型转换


#### Kotlin V.S. Java
我们知道，Java中的一切是以class为基础的，都要在class中，但是Kotlin却不是。

在Kotlin中方法是一等公民，意思就是方法就和类一样，类可以干的事，方法都可以干。

Kotlin给Java开发者带来最大改变之一就是废弃了static修饰符。与Java不同的是在Kotlin的类中不允许你声明静态成员或方法。相反，你必须向类中添加Companion对象来包装这些静态引用: 差异看起来似乎很小，但是它有一些明显的不同。

The Unit type is what you return from a function that doesn't return anything of interest. Such a function is usually performing some kind of side effect. The unit type has only one possible value, which is the Unit object. You use Unit as a return type in Kotlin when you would use void (lowercase v) in Java. 


#### Companion Object
类内部的对象声明可以用 companion 关键字标记
`
class MyClass {
    companion object Factory {
        fun create(): MyClass = MyClass()
    }
}

val instance = MyClass.create()
`
在 JVM 平台，如果使用 @JvmStatic 注解，你可以将伴生对象的成员生成为真正的静态方法和字段。

#### When
when 表达式取代了类 C 语言的 switch 语句。其最简单的形式如下：
{% highlight kotlin %}
`when (x) {
    1 -> print("x == 1")
    2 -> print("x == 2")
    else -> { // 注意这个块
        print("x is neither 1 nor 2")
    }
}`
{% endhighlight %}


##### Data Class: classes whose main purpose is to hold data

`data class User(val name: String, val age: Int)`



##### ? VS !!
参考：[kotlin中?和!!的区别](https://www.jianshu.com/p/51b2e5aa3dd8)

"?"加在变量名后，系统在任何情况不会报它的空指针异常。
"!!"加在变量名后，如果对象为null，那么系统一定会报异常！

大多数情况下都会使用?来检测null，轮不到!!出场。!!只会在你需要对某对象进行非空判断，并且需要抛出异常时才会使用到。


!!使用场景：
{% highlight kotlin %}
        val roomList: ArrayList<Room>? = null
        if (roomList?.size > 0) {      // 建议写成：if(roomList?.size!! > 0)
            Log.d("TAG", "-->> 房间数不是0")
        }
{% endhighlight %}
当我们判断list.size的时候，编译器会告诉我们"Operator call corresponds to a dot-qualified call 'roomList?.size.compareTo(0)' which is not allowed on a nullable receiver 'roomList?.size'."。大概意思是，当roomList为null的时，它的size返回就是"null"，但是"null"不可以和int值比大小，所以编译器建议我们写成roomList?.size!! > 0。没错，经过编译器的建议加上了!!，我们程序运行到这行代码，roomList为null时它一定会报异常。

?:表示的意思是，当对象A值为null的时候，那么它就会返回后面的对象B。
{% highlight kotlin %}
        val roomList: ArrayList<Room>? = null
        val mySize= roomList?.size ?: 0  
{% endhighlight %}
此时mySize的值就为0，因为roomList?.size为空。

所以我们可以把上面的代码改成这样：
{% highlight kotlin %}
        val roomList: ArrayList<Room>? = null
        if (roomList?.size ?: 0 > 0) {    // 这一行添加了?:
            Log.d("TAG", "-->> 房间数不是0")
        }
{% endhighlight %}





./gradlew :prime:publishToMavenLocal
./gradlew clean installGoogleBeta
https://glowing.com/debug/premium_status