title: 《Effective Java》— Java进阶必备
author: ergz
tags: [Java]
categories:
  - Java进阶
---

《Effective Java》是 Java 领域的经典之作，其影响力不亚于《Think in Java》。它是每个 Java 开发者的必读书籍，值得多次阅读品味，并不断实践其中的经验技巧。

**《Effective Java》第三版英文原版下载地址：**[Effective-Java-3rd.pdf](http://cdn.ergzcode.com/%E6%96%87%E6%A1%A3/Effective-Java-3rd%20%E8%8B%B1%E6%96%87%E5%8E%9F%E7%89%88.pdf)    
**《Effective Java》第三版中文翻译下载地址：**[Effective-Java-3rd-LaTex-Pattern.pdf](http://cdn.ergzcode.com/%E6%96%87%E6%A1%A3/Effective-Java-3rd-LaTex-Pattern.pdf)  

两年前读过此书，当时自身技术水平并不算高，对于其中的原则不求甚解。现在重温以前的笔记，加上这两年的编程经验，竟然有种豁然开朗的感觉。
「书读百遍，其义自见。」古人说得有道理啊。

这里简单介绍几条开发中经常用到的技巧，比如 Builder、对象方法、接口定义等。

<!--more-->

# 1. 存在多个构造参数时，考虑使用构建器（Builder）
这里讲的就是构建者模式，使用 Builder 给对象设置构造参数，从而去除重叠的构造方法。

在第三方库中，比如 OkHttp、Retrofit，经常见到这样的设计。我们用 Builder 设置参数，然后调用 build 方法构造对象，代码看起来非常清爽，没有多余的构造参数，使用起来也方便。

如果类的构造方法中有多个参数，而且参数可选，Builder 模式是不错的选择。


# 2. 使用私有构造器或者枚举类型强化 Singleton 属性
单例模式推荐使用静态内部类，保证线程安全的同时，延迟对象的初始化。枚举的话，不是很推荐。

# 3. 避免创建不必要的对象
创建对象是有开销的，对于不可变的（可以理解为无状态的）对象，可以始终被重用。善于使用缓存（cache）是好的习惯，我们要做一个环保的程序员。比如 Android Message 里面的对象池，就是一种优化策略。

优先使用基本数据类型而不是装箱数据类型，因为基本类型占用更少的内存空间，对于移动端来说，内存优化是一个永恒的话题。

# 4. 覆盖 equals 时总要覆盖 hashCode，始终覆盖 toString
对于一般的 Java Bean 对象，我都会重写其 toString 方法， 这样在打印日志的时候，就能看见它的内部数据，这点非常有用。

对于需要作为 HashMap 的键的对象，一定要重写 hashCode 和 equals 方法，这样有利于散列表均匀分布，提升查找的性能。
另外，两个对象 equals 相等，hashCode 必须相等；hashCode 相等，equals不一定相等。这个很好理解，equals 保证 HashMap 键的唯一性；hashCode 相等表示两个对象放在 HashMap 数组的同一个位置上。

提示：现在的 IDE 实在是太傻瓜式了，可以一键生成 toString, equals 和 hashCode 方法，这时候有什么理由不重写呢。

# 5. 接口优于抽象类
接口本质上是一种规范，用来定义通用的规则，然后各个功能提供者进行不同的实现。这里，我想到了一条软件设计原则--接口隔离，是说一个接口只用来定义一种规范，每个接口都是独立的，不能让一个接口有多个定义。

骨架实现，AbstractXXX，为抽象类提供了实现上的帮助，又不强加抽象类定义类型的限制。比如 Java 里面的 AbstractList，是列表类的抽象实现，其子类有 ArrayList 和 LinkedList 等。

# 6. 优先考虑静态成员类
静态成员类不持有外部类的引用，所以不会造成内存泄漏问题，主要是用来辅助外部类。还有匿名内部类，比如直接创建的 Runnable，建议内部保持简短，大约 10 行或者更少些，否则影响程序的可读性。

# 7. 列表优先于数组
列表和数组的不同点：

数组是可变的，泛型是不可变的。
数组是具体化的，泛型通过擦除来实现的。
数组提供了运行时的类型安全，但是没有编译时的类型安全。列表里面有许多实用的方法，而数组就没有提供哦。建议优先使用集合类型 List<E>，而不是数组类型 E[]。

# 8. 使用枚举代替 int 常量
枚举类型是实例受控的，它们是单例的泛化，本质上是单元素的枚举。枚举的优点是易读性好，更加安全，功能强大。但是与 int 类型相比，枚举有些性能缺点：装载和初始化枚举时会有时间和空间的成本。在 Android 端，通常不建议使用。

# 9. for-each 循环优先于传统的 for 循环
for-each 循环在简洁性和预防 bug 方面优于传统的 for 循环，而且没有性能损失，应该尽可能地使用 for-each。

这一点深有感触，IDE 通常会提示使用 for-each，毕竟简洁就是美啊。

# 10. 如果需要精确的答案，请避免使用 float 和 double
float 和 double 并没有提供完全精确的结果，所以不应该用于需要精确计算的场合，尤其是货币计算。如果想要系统记录十进制小数点，可以使用 BigDecimal。

float 类型确实是个坑，经常会出现计算不准确的问题，一般推荐使用双精度浮点型 double。

以上就是从《Effective Java》里面摘录的十条编程建议，另外还有好多干货呢，赶快去阅读吧。


**链接：<https://www.jianshu.com/p/54e122c8e765>  
来源：简书  
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处**