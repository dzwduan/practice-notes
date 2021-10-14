### 主要参考

https://blog.csdn.net/qq_34291505/article/details/86744581

### <>

https://stackoverflow.com/questions/52123791/what-is-the-operator-in-chisel

```scala
class MyQueue extends Module {
    // Example circuit using a Queue
    val io = IO(new Bundle {
        val in = Flipped(Decoupled(UInt(8.W)))
        val out = Decoupled(UInt(8.W))
    })
    val queue = Queue(io.in, 2)  // 2-element queue
    io.out <> queue
}

//上半部分等价
val io = IO(new Bundle {
  val in = new Bundle {
    val valid = Input(Bool())
    val ready = Output(Bool())
    val bits  = Input(UInt(8.W))
  }
  val out = new Bundle {
    val valid = Output(Bool())
    val ready = Input(Bool())
    val bits  = Output(UInt(8.W))
  }
}

//下半部分带queue的等价于
io.out.valid := queue.valid
io.out.bits := queue.bits
queue.ready := io.out.ready
```



### decoupled

https://zhouyuqian.com/2021/05/24/chisel-start/

`Decoupled`可以将基本的chisel数据类型包装起来，并为其提供`ready`和`valid`信号。 Testers2提供了一些很好的工具，可以自动并可靠地测试这些接口。



### flipped

https://stackoverflow.com/questions/48343073/what-does-flipped-do-in-chisel3

翻转方向



### implicit

https://zhuanlan.zhihu.com/p/128664699



### case object

https://blog.csdn.net/qq_34291505/article/details/87023284

可以通过类名(参数)构造对象，不需要new ;参数列表的每个参数都隐式地获得了一个val前缀

在Scala里，除了用new可以构造一个对象，也可以用“object”开头定义一个对象。它类似于类的定义，只不过不能像类那样有参数，也没有构造方法。

如果某个单例对象和某个类同名，那么单例对象称为这个类的“伴生对象”，同样，类称为这个单例对象的“伴生类”

伴生类和伴生对象必须在同一个文件里，而且两者可以互访对方所有成员。类似c的静态变量







### field view

https://blog.csdn.net/qq_34291505/article/details/87923459

为什么要引入Field[Int]而不是“case object isInt extends Int”呢？因为Scala的基本类型Int、Float、Double、Boolean等都是final修饰的抽象类，不能被继承。

在Scala里，类是用关键字“class”开头的代码定义。它是对象的蓝图，一旦定义完成，就可以通过“new 类名”的方式来构造一个对象

辅助构造方法都是以“def this(......)”来开头的，而且第一步行为必须是调用该类的另一个构造方法，即第一条语句必须是“this(......)”——要么是主构造方法，要么是之前的另一个辅助构造方法。这种规则的结果就是任何构造方法最终都会调用该类的主构造方法，使得主构造方法成为类的单一入口。





### 工厂方法

定义一个方法专门用来构造某一个类的对象，那么这种方法就称为“工厂方法”



### apply方法

如果定义了这个方法，那么既可以显式调用——“对象.apply(参数)” ，也可以隐式调用——“对象(参数)



### abstract

https://blog.csdn.net/qq_34291505/article/details/86766873

如果类里包含了没有具体定义的成员——没有初始化的字段或没有函数体的方法，那么这个类就是抽象类，必须用关键字“abstract”修饰



### trait

https://blog.csdn.net/qq_34291505/article/details/86773401

特质是用关键字“trait”为开头来定义的，它与单例对象很像，两者都不能有入参。但是，单例对象天生就是具体的，特质天生就是抽象的，不过不需要用“abstract”来说明。所以，特质可以包含抽象成员，而单例对象却不行



### RawModule

前面可以没有io.前缀，且可以定义多个Io

https://www.chisel-lang.org/chisel3/docs/explanations/modules.html



### Mem

https://www.chisel-lang.org/chisel3/docs/explanations/memories.html



```scala
class DecoupledIO[T <: Data](data: T) extends Bundle {
  val ready = Input(Bool())
  val valid = Output(Bool())
  val bits  = Output(data)
}
```

