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

适配器模式（*Adapter Pattern*）是一种结构型设计模式，它可以将一个类的接口转换成客户希望的另一个接口，主要目的是充当两个不同接口之间的桥梁，将一个接口转换成客户希望的另一个接口，使接口不兼容的那些对象可以一起工作。

比如，笔记本电脑的工作电压是 `DC 20V`，我们的生活用电是 `220V`。正常使用笔记本电脑，必须有一个电源适配器。

> 定义：将一个接口转换成客户希望的另一个接口，适配器模式使接口不兼容的那些类可以一起工作，其别名为包装器（`Wrapper`）。

## 类图和组件

在适配器模式中，有以下几个角色：

1. **目标（`Target`）**：这是客户端期望的接口。比如目标是DC 20V的电压。

2. **适配者（`Adaptee`）**：这是需要被适配的类或接口。比如适配者是220V的交流电。

3. **适配器（`Adapter`）**：这是适配器模式的核心。适配器包含了一个适配者的实例，并实现了目标接口。适配器的任务就是将目标接口的调用转换为对适配者的调用。在上述例子中，适配器就是电源转换器。

在开发中，适配器模式可以帮助我们实现以下目标：

- **重用现有的类**：如果一个类的功能基本满足需求，但是它的接口与新的系统不兼容，那么我们可以通过适配器模式来重用这个类，而不需要修改这个类的代码。

- **扩展现有类的功能**：如果我们希望在不修改现有类的情况下，为这个类添加新的功能，那么我们可以通过适配器模式来实现。我们可以创建一个新的适配器类，这个类包含了现有类的实例，并实现了新的接口。在这个新的接口中，我们可以添加新的功能。

## 模式实现

### 类适配器模式

![image-20240320142124150](images/2_01_Adapter/image-20240320142124150.png)

我们先来看类适配器模式：

```java
//角色1：适配者类 (已有的、需要被适配的类)
class Adaptee {
    public void specificRequest() {
        System.out.println("Specific request...");
    }
}

//角色2：目标接口 (我们期望的接口)
interface Target {
    void request();
}

//角色3：适配器类 (继承了Adaptee并实现Target接口，解决接口不兼容的问题)
class Adapter extends Adaptee implements Target {
	@Override
	public void request() {
		super.specificRequest(); //调用基类方法
	}
}
```

适配器类`Adapter`继承了 `Adaptee` 并实现了 `Target` 接口。在 `request` 方法中，它调用了 `Adaptee` 的 `specificRequest` 方法。这样，当客户端通过 `Target` 接口调用 `request` 方法时，实际上会执行 `Adaptee` 的 `specificRequest` 方法，从而实现了接口的适配。

客户端代码如下：

```java
public class Client {
	public static void main(String[] args) {
		Target adapter = new Adapter();  // 可选择不同类型的适配器
		adapter.request(); // 调用接口方法
	}
}
```

### 对象适配器模式

![image-20240320145217836](images/2_01_Adapter/image-20240320145217836.png)

我们再来看对象适配器模式：

```java
//角色1：适配者类 (已有的、需要被适配的类)
class Adaptee {
	public void specificRequest() {
		System.out.println("Specific request...");
	}
}

//角色2：目标接口 (我们期望的接口)
interface Target {
	void request();
}

//角色3：适配器类 (聚合Adaptee类，实现Target接口，解决接口不兼容的问题)
class Adapter implements Target {
    
	private Adaptee adaptee; // 持有一个 Adaptee 的实例

	public Adapter(Adaptee adaptee) {
		this.adaptee = adaptee;
	}

	@Override
	public void request() {
		adaptee.specificRequest(); // 调用成员对象的方法，实现接口兼容
	}
}
```

客户端代码如下：

```java
public class Client {
	public static void main(String[] args) {
		Target adapter = new Adapter(new Adaptee());
		adapter.request();
	}
}
```



## 对比其他设计模式

1. **适配器模式 vs 装饰器模式**：装饰器模式用于在运行时动态地添加新功能到对象中，而适配器模式则是用于使两个不兼容的接口能够一起工作。

2. **适配器模式 vs 桥接模式**：桥接模式主要用于将抽象部分与实现部分分离，使它们可以独立地变化。而适配器模式则是用于使两个已有接口能够一起工作。

3. **适配器模式 vs 代理模式**：代理模式为其他对象提供一种代理以控制对这个对象的访问。而适配器模式则是用于使两个不兼容的接口能够一起工作。

4. **适配器模式 vs 外观模式**：外观模式提供了一个统一的接口，用来访问子系统中的一群接口。外观模式定义了一个高层接口，使得子系统更易于使用。而适配器模式则是使两个不兼容的接口能够一起工作。

## 总结

