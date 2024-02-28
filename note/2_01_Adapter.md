# 适配器模式

> 在Java中，适配器模式的经典应用例子包括：
>
> 1. **Java集合框架**：在`Java`的集合框架中，`Arrays`类的`asList`方法返回一个将指定数组视为列表的固定大小的列表，这是适配器模式的一个例子。
>
> 2. **Java输入/输出流**：在`Java`的输入/输出流中，`InputStreamReader`和`OutputStreamWriter`是适配器模式的例子，它们分别将字节流适配为字符流。
>
> 3. **Java数据库连接**：在`Java`的数据库连接技术`JDBC`中，有许多适配器模式的例子，如`DriverManager`是一个适配器，它在各种数据库驱动程序之间进行适配。
>
> 4. **Swing和AWT组件**：在`Java`的`Swing`和`AWT`组件中，事件监听器通常使用适配器模式。例如，`WindowAdapter`类是一个适配器，它实现了`WindowListener`接口，我们可以创建这个类的匿名子类，并只覆盖我们感兴趣的方法。
>
> 这些都是适配器模式在Java中的经典应用例子，它们都利用了适配器模式的特性，通过定义一个包装类，用于包装不兼容接口的对象，使得原本由于接口不兼容而不能一起工作的类可以一起工作。

## 定义和目的

理解设计模式的定义。

## 类图和组件

理解设计模式的结构和它的主要组件。

## 代码示例

通过具体的代码示例来理解和学习设计模式，帮助理解工作原理

## 对比其他设计模式

理解这个设计模式与其他设计模式的区别和联系。

## 总结

该模式试图解决的问题是？

理解在什么情况下使用这个设计模式(使用场景)。
