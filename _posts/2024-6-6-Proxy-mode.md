---
layout: post
title: "代理模式"
date: 2024-6-6
tags: [design-pattern]
comments: true
author: zxy
---

## 概要

> 意图

为其他对象提供一种代理以控制对这个对象的访问。

> 主要解决的问题

- 代理模式解决的是在直接访问某些对象时可能遇到的问题，例如对象创建成本高、需要安全控制或远程访问等。

> 使用场景

- 当需要在访问一个对象时进行一些控制或额外处理时。

> 实现方式

- **增加中间层**：创建一个代理类，作为真实对象的中间层。
- **代理与真实对象组合**：代理类持有真实对象的引用，并在访问时进行控制。

> 关键代码

- **代理类**：实现与真实对象相同的接口，并添加额外的控制逻辑。
- **真实对象**：实际执行任务的对象。

> 优点

- **职责分离**：代理模式将访问控制与业务逻辑分离。
- **扩展性**：可以灵活地添加额外的功能或控制。
- **智能化**：可以智能地处理访问请求，如延迟加载、缓存等。

> 缺点

- **性能开销**：增加了代理层可能会影响请求的处理速度。
- **实现复杂性**：某些类型的代理模式实现起来可能较为复杂。

> 使用建议

- 根据具体需求选择合适的代理类型，如远程代理、虚拟代理、保护代理等。
- 确保代理类与真实对象接口一致，以便客户端透明地使用代理。

> 注意事项

- **与适配器模式的区别**：适配器模式改变接口，而代理模式不改变接口。
- **与装饰器模式的区别**：装饰器模式用于增强功能，代理模式用于控制访问。

> 结构

**主要涉及到以下几个核心角色：**

- **抽象主题（Subject）:**
  - 定义了真实主题和代理主题的共同接口，这样在任何使用真实主题的地方都可以使用代理主题。
- **真实主题（Real Subject）:**
  - 实现了抽象主题接口，是代理对象所代表的真实对象。客户端直接访问真实主题，但在某些情况下，可以通过代理主题来间接访问。
- **代理（Proxy）:**
  - 实现了抽象主题接口，并持有对真实主题的引用。代理主题通常在真实主题的基础上提供一些额外的功能，例如延迟加载、权限控制、日志记录等。
- **客户端（Client）:**
  - 使用抽象主题接口来操作真实主题或代理主题，不需要知道具体是哪一个实现类。

## 实现

我们将创建一个 _Image_ 接口和实现了 _Image_ 接口的实体类。_ProxyImage_ 是一个代理类，减少 _RealImage_ 对象加载的内存占用。

_ProxyPatternDemo_ 类使用 _ProxyImage_ 来获取要加载的 _Image_ 对象，并按照需求进行显示。

![代理模式的 UML 图](https://www.runoob.com/wp-content/uploads/2014/08/20211025-proxy.svg)

### 步骤 1

创建一个接口。

```java
Image.java
public interface Image {
   void display();
}
```

### 步骤 2

创建实现接口的实体类。

```java
RealImage.java
public class RealImage implements Image {

   private String fileName;

   public RealImage(String fileName){
      this.fileName = fileName;
      loadFromDisk(fileName);
   }

   @Override
   public void display() {
      System.out.println("Displaying " + fileName);
   }

   // 真正耗时部分代码
   private void loadFromDisk(String fileName){
      System.out.println("Loading " + fileName);
   }
}

ProxyImage.java
public class ProxyImage implements Image{

   private RealImage realImage;
   private String fileName;

   public ProxyImage(String fileName){
      this.fileName = fileName;
   }

   @Override
   public void display() {
      if(realImage == null){
         realImage = new RealImage(fileName);
      }
      realImage.display();
   }
}
```

### 步骤 3

当被请求时，使用 _ProxyImage_ 来获取 _RealImage_ 类的对象。

```java
ProxyPatternDemo.java
public class ProxyPatternDemo {

   public static void main(String[] args) {
      Image image = new ProxyImage("test_10mb.jpg");

      // 图像将从磁盘加载
      image.display();
      System.out.println("");
      // 图像不需要从磁盘加载
      image.display();
   }
}
```
