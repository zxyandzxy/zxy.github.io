---
layout: post
title: "桥接模式"
date: 2024-6-6
tags: [design-pattern]
comments: true
author: zxy
---

## 概要

### 意图

用于将抽象部分与实现部分分离（就是将接口与接口的实现类相分离，实现类不直接实现接口），使得它们可以独立地变化

### 主要解决的问题

避免使用继承导致的类爆炸问题，提供更灵活的扩展方式。

### 使用场景

当系统可能从多个角度进行分类，且每个角度都可能独立变化时，桥接模式是合适的。

> 实现方式

- **分离多角度分类**：将不同角度的分类逻辑分离，允许它们独立变化。
- **减少耦合**：降低抽象与实现之间的耦合度。

> 关键代码

- **抽象类**：定义一个抽象类，作为系统的一部分。
- **实现类**：定义一个或多个实现类，与抽象类通过聚合（而非继承）关联。

> 优点

- **抽象与实现分离**：提高了系统的灵活性和可维护性。
- **扩展能力强**：可以独立地扩展抽象和实现。
- **实现细节透明**：用户不需要了解实现细节。

> 缺点

- **理解与设计难度**：桥接模式增加了系统的理解与设计难度。
- **聚合关联**：要求开发者在抽象层进行设计与编程。

> 使用建议

- 当系统需要在抽象化角色和具体化角色之间增加灵活性时，考虑使用桥接模式。
- 对于不希望使用继承或因多层次继承导致类数量急剧增加的系统，桥接模式特别适用。
- 当一个类存在两个独立变化的维度，且这两个维度都需要扩展时，使用桥接模式。

> 注意事项

- 桥接模式适用于两个独立变化的维度，确保它们可以独立地扩展和变化。

> 结构

以下是桥接模式的几个关键角色：

- 抽象（Abstraction）：定义抽象接口，通常包含对实现接口的引用。
- 扩展抽象（Refined Abstraction）：对抽象的扩展，可以是抽象类的子类或具体实现类。
- 实现（Implementor）：定义实现接口，提供基本操作的接口。
- 具体实现（Concrete Implementor）：实现实现接口的具体类。

## 实现

我们有一个作为桥接实现的 _DrawAPI_ 接口和实现了 _DrawAPI_ 接口的实体类 _RedCircle_、_GreenCircle_。_Shape_ 是一个抽象类，将使用 _DrawAPI_ 的对象。_BridgePatternDemo_ 类使用 _Shape_ 类来画出不同颜色的圆。

![桥接模式的 UML 图](https://www.runoob.com/wp-content/uploads/2014/08/20201015-bridge.svg)

抽象：Shape 扩展抽象： DrawAPI 实现：Circle 扩展实现：RedCircle / GreenCircle

### 步骤 1

创建桥接实现接口。

```java
DrawAPI.java
public interface DrawAPI {
   public void drawCircle(int radius, int x, int y);
}
```

### 步骤 2

创建实现了 _DrawAPI_ 接口的实体桥接实现类。

```java
RedCircle.java
public class RedCircle implements DrawAPI {
   @Override
   public void drawCircle(int radius, int x, int y) {
      System.out.println("Drawing Circle[ color: red, radius: "
         + radius +", x: " +x+", "+ y +"]");
   }
}

GreenCircle.java
public class GreenCircle implements DrawAPI {
   @Override
   public void drawCircle(int radius, int x, int y) {
      System.out.println("Drawing Circle[ color: green, radius: "
         + radius +", x: " +x+", "+ y +"]");
   }
}
```

### 步骤 3

使用 _DrawAPI_ 接口创建抽象类 _Shape_。

```java
Shape.java
public abstract class Shape {
   protected DrawAPI drawAPI;
   protected Shape(DrawAPI drawAPI){
      this.drawAPI = drawAPI;
   }
   public abstract void draw();
}
```

### 步骤 4

创建实现了 _Shape_ 抽象类的实体类。

```java
Circle.java
public class Circle extends Shape {
   private int x, y, radius;

   public Circle(int x, int y, int radius, DrawAPI drawAPI) {
      super(drawAPI);  //调用父类Shape的构造函数，将Circle的成员变量drawAPI赋值
      this.x = x;
      this.y = y;
      this.radius = radius;
   }

   public void draw() {
      drawAPI.drawCircle(radius,x,y);
   }
}
```

### 步骤 5

使用 _Shape_ 和 _DrawAPI_ 类画出不同颜色的圆。

```java
BridgePatternDemo.java
public class BridgePatternDemo {
   public static void main(String[] args) {
      Shape redCircle = new Circle(100,100, 10, new RedCircle());
      Shape greenCircle = new Circle(100,100, 10, new GreenCircle());

      redCircle.draw();
      greenCircle.draw();
   }
}
```
