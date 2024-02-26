# 抽象工厂模式

## 定义和目的

在工厂方法模式中，每个具体工厂只负责创建**单一的**具体产品。有时，我们需要一个工厂可以提供多个产品对象，而不是单一的产品对象。但如果有多类产品呢？

> **抽象工厂模式（Abstract Factory Pattern）提供一个创建一系列相关或相互依赖对象的接口，而无需指定它们具体的类**。

抽象工厂模式属于对象创建型设计模式。它提供了一种方式，可以将一组具有同一主题的单独但相关/依赖的工厂封装起来，而客户端不需要知道它从这些内部工厂中获取的对象是如何创建的。

简单来说，**抽象⼯⼚模式可以确保⼀系列相关的产品被⼀起创建，这些产品能够相互配合使⽤**。

## 类图和组件

> 抽象⼯⼚模式包含⼀个抽象⼯⼚接⼝，多个具体工厂，多个抽象产品接⼝，多个具体产品类，其中每个具体⼯⼚负责创建⼀组相关的产品。 

抽象工厂模式的主要组件：

1. **抽象产品（`AbstractProduct`）**：这是一个产品家族，声明了产品的主要特性和行为，可定义多个抽象产品接口。
2. **具体产品（`ConcreteProduct`）**：这些类实现了抽象产品的接口，以生成具体的产品。
3. **抽象工厂（`AbstractFactory`）**：这是一个接口，声明了创建抽象产品的方法，每个方法对应一个产品。
4. **具体工厂（`ConcreteFactory`）**：这些类实现了抽象工厂的创建方法，以生成具体的产品。

在抽象工厂模式中，客户端通过抽象工厂接口创建产品，而无需知道实际的产品类。这样，客户端可以从具体工厂中独立出来，只需要与抽象工厂和产品接口交互。这使得系统在不修改客户端代码的情况下，可以更换或添加新的产品或产品家族。



![image-20240226093800499](images/1_02_Abstract_Factory_Pattern/image-20240226093800499.png)

在上图的示例中： `AbstractFactory` 定义了抽象⼯⼚接⼝，接⼝⾥的⽅法⽤于创建具体的产品，⽽`ConcreteFactory1/2`就是具体⼯⼚类，可以创建⼀组相关的产品，`AbstractProductA/B` 就是抽象产品， `ConcreteProductA2/A2/B1/B2`就是抽象产品的实现。

## 代码示例

我们以电视机为例，理解抽象工厂模式：

```java
abstract class AbstractProductA {  //角色1：抽象产品（电视）
    public abstract void methodA();  //播放
}

class ConcreteProductA1 extends AbstractProductA {  //角色2：具体产品（如海尔电视）
    @Override
    public void methodA() {
        System.out.println("海尔电视机播放中......");
    }
}

class ConcreteProductA2 extends AbstractProductA {  //角色2：具体产品（如TCL电视）
    @Override
    public void methodA() {
        System.out.println("TCL电视机播放中......");
    }
}

abstract class AbstractProductB {   //角色1：抽象产品（空调）
    public abstract void methodB();  //调温
}

class ConcreteProductB1 extends AbstractProductB {  //角色2：具体产品（如海尔空调）
	@Override
    public void methodB() {
        System.out.println("海尔空调温度改变中......");
    }
}

class ConcreteProductB2 extends AbstractProductB {  //角色2：具体产品（TCL空调）
	@Override
    public void methodB() {
        System.out.println("TCL空调温度改变中......");
    }
}

abstract class AbstractFactory { //角色3：抽象工厂
    public abstract AbstractProductA createProductA(); //生产电视
    public abstract AbstractProductB createProductB(); //生产空调
}

class ConcreteFactory1 extends AbstractFactory {   //角色4：具体工厂（海尔）
    @Override
    public AbstractProductA createProductA() {
        System.out.println("海尔工厂生产海尔电视机");
        return new ConcreteProductA1();
    }

    @Override
    public AbstractProductB createProductB() {
        System.out.println("海尔工厂生产海尔空调");
        return new ConcreteProductB1();
    }
}

class ConcreteFactory2 extends AbstractFactory {  //角色4：具体工厂（TCL）
    @Override
    public AbstractProductA createProductA() {
        System.out.println("TCL工厂生产TCL电视机");
        return new ConcreteProductA2();
    }
    @Override
    public AbstractProductB createProductB() {
        System.out.println("TCL工厂生产TCL空调");
        return new ConcreteProductB2();
    }
}
```

以上是电器生产的例子，使用了抽象工厂模式：

1. **抽象产品（`AbstractProduct`）**：这是一个产品家族，声明了产品的主要特性和行为。在这个例子中，有两个抽象产品：电视（`AbstractProductA`）和空调（`AbstractProductB`）。
2. **具体产品（`ConcreteProduct`）**：这些类实现了抽象产品的接口，以生成具体的产品。在这个例子中，有四个具体产品：海尔电视（`ConcreteProductA1`），TCL电视（`ConcreteProductA2`），海尔空调（`ConcreteProductB1`）和TCL空调（`ConcreteProductB2`）。
3. **抽象工厂（`AbstractFactory`）**：这是一个接口，声明了创建抽象产品的方法。在这个例子中，抽象工厂有两个方法：创建电视（`createProductA`）和创建空调（`createProductB`）。
4. **具体工厂（`ConcreteFactory`）**：这些类实现了抽象工厂的创建方法，以生成具体的产品。在这个例子中，有两个具体工厂：海尔工厂（`ConcreteFactory1`）和TCL工厂（`ConcreteFactory2`）。

 那客户端就可以这样写：

```java
public class Client {   //客户端
    public static void main(String args[]) {
        // 创建海尔工厂
        AbstractFactory haierFactory = new ConcreteFactory1();
        // 使用海尔工厂创建产品
        AbstractProductA haierTV = haierFactory.createProductA();
        AbstractProductB haierAirConditioner = haierFactory.createProductB();
        // 使用海尔产品
        haierTV.methodA();
        haierAirConditioner.methodB();

        // 创建TCL工厂
        AbstractFactory tclFactory = new ConcreteFactory2();
        // 使用TCL工厂创建产品
        AbstractProductA tclTV = tclFactory.createProductA();
        AbstractProductB tclAirConditioner = tclFactory.createProductB();
        // 使用TCL产品
        tclTV.methodA();
        tclAirConditioner.methodB();
    }
}
```

运行结果：

```java
海尔工厂生产海尔电视机
海尔工厂生产海尔空调
海尔电视机播放中......
海尔空调温度改变中......
TCL工厂生产TCL电视机
TCL工厂生产TCL空调
TCL电视机播放中......
TCL空调温度改变中......
```

后续如何扩展？

举个栗子，比如增加索尼工厂：

```java
class ConcreteProductA3 extends AbstractProductA {  //角色2：具体产品（Sony电视）
    @Override
    public void methodA() {
        System.out.println("Sony电视机播放中......");
    }
}

class ConcreteProductB3 extends AbstractProductB {  //角色2：具体产品（Sony空调）
	@Override
    public void methodB() {
        System.out.println("Sony空调温度改变中......");
    }
}

class ConcreteFactory3 extends AbstractFactory {   //角色4：具体工厂（Sony）
    @Override
    public AbstractProductA createProductA() {
        System.out.println("Sony工厂生产Sony电视机");
        return new ConcreteProductA3();
    }
    @Override
    public AbstractProductB createProductB() {
        System.out.println("Sony工厂生产Sony空调");
        return new ConcreteProductB3();
    }
}
```



## 对比其他设计模式

| 模式名称       | 描述                                                                                                                                                      |
|----------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------|
| 简单工厂 | 有一个工厂类，它有一个方法，根据输入参数决定创建哪种具体产品。这个模式的缺点是，如果需要添加新的产品，就需要修改工厂类的代码。                     |
| 工厂方法 | 定义了一个创建对象的接口，但由子类决定实例化哪一个类。工厂方法让类的实例化推迟到子类。每个工厂只创建一种具体产品。                                       |
| 抽象工厂 | 提供一个接口，用于创建相关或依赖对象的家族，而不需要明确指定具体类。每个工厂可以创建一系列相关的产品。                                                      |




## 总结

抽象工厂模式试图解决的问题是，当一个系统必须独立于它的产品的创建、组合和表示时，或者当一个系统需要配置多个系列中的一个时，或者强调一系列相关的产品对象的设计以便进行联合使用时。

因此抽象工厂模式适用于以下场景：

1. **产品族和产品等级结构**：当一个系统需要提供多个产品系列，每个产品系列包含多个相关的产品，而系统只需要使用其中某一系列的产品时，可以使用抽象工厂模式。例如，一个家具工厂可能生产椅子、桌子和沙发，这些产品可以按照风格（现代、古典）或材料（木制、塑料）进行分类。

2. **系统需要独立于其产品的创建、组合和表示**：当一个系统需要与具体产品类解耦，以便在不修改客户端代码的情况下更换或添加新的产品或产品系列时，可以使用抽象工厂模式。

3. **强调一系列相关产品的接口**：当你想强调一系列相关产品的接口以便于共享产品实现时，可以使用抽象工厂模式。

4. **提供一个产品库**：当你想提供一个产品类库，而只想显示它们的接口而不是实现时，可以使用抽象工厂模式。
