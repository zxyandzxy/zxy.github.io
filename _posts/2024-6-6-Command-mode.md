---
layout: post
title: "命令模式"
date: 2024-6-6
tags: [design-pattern]
comments: true
author: zxy
---

## 概要

> 意图

将请求封装为一个对象，允许用户使用不同的请求对客户端进行参数化。

> 主要解决的问题

- 解决在软件系统中请求者和执行者之间的紧耦合问题，特别是在需要对行为进行记录、撤销/重做或事务处理等场景。

> 使用场景

- 当需要对行为进行记录、撤销/重做或事务处理时，使用命令模式来解耦请求者和执行者。

> 实现方式

- **定义命令接口**：所有命令必须实现的接口。
- **创建具体命令**：实现命令接口的具体类，包含执行请求的方法。
- **调用者**：持有命令对象并触发命令的执行。
- **接收者**：实际执行命令的对象。

> 关键代码

- **接收者（Receiver）**：执行命令的实际对象。
- **命令（Command）**：定义执行命令的接口。
- **调用者（Invoker）**：使用命令对象的入口点。

> 优点

1. **降低耦合度**：请求者和执行者之间的耦合度降低。
2. **易于扩展**：新命令可以很容易地添加到系统中。

> 缺点

- **过多命令类**：系统可能会有过多的具体命令类，增加系统的复杂度。

> 使用建议

- 在 GUI 中，每个按钮或菜单项可以视为一条命令。
- 在需要模拟命令行操作的场景中使用命令模式。

> 注意事项

- 如果系统需要支持命令的撤销（Undo）和恢复（Redo）操作，命令模式是一个合适的选择。

> 结构

**主要涉及到以下几个核心角色：**

- **命令（Command）:**
  - 定义了执行操作的接口，通常包含一个 `execute` 方法，用于调用具体的操作。
- **具体命令（ConcreteCommand）:**
  - 实现了命令接口，负责执行具体的操作。它通常包含了对接收者的引用，通过调用接收者的方法来完成请求的处理。
- **接收者（Receiver）:**
  - 知道如何执行与请求相关的操作，实际执行命令的对象。
- **调用者/请求者（Invoker）:**
  - 发送命令的对象，它包含了一个命令对象并能触发命令的执行。调用者并不直接处理请求，而是通过将请求传递给命令对象来实现。
- **客户端（Client）:**
  - 创建具体命令对象并设置其接收者，将命令对象交给调用者执行。

## 实现

我们首先创建作为命令的接口 _Order_，然后创建作为请求的 _Stock_ 类。实体命令类 _BuyStock_ 和 _SellStock_，实现了 _Order_ 接口，将执行实际的命令处理。创建作为调用对象的类 _Broker_，它接受订单并能下订单。

_Broker_ 对象使用命令模式，基于命令的类型确定哪个对象执行哪个命令。_CommandPatternDemo_ 类使用 _Broker_ 类来演示命令模式。

![img](https://www.runoob.com/wp-content/uploads/2014/08/20220427-command-1-command-1.svg)

### 步骤 1

创建一个命令接口。

```java
Order.java
public interface Order {
   void execute();
}
```

### 步骤 2

创建一个请求类。

```java
Stock.java
public class Stock {

   private String name = "ABC";
   private int quantity = 10;

   public void buy(){
      System.out.println("Stock [ Name: "+name+",
         Quantity: " + quantity +" ] bought");
   }
   public void sell(){
      System.out.println("Stock [ Name: "+name+",
         Quantity: " + quantity +" ] sold");
   }
}
```

### 步骤 3

创建实现了 _Order_ 接口的实体类。

```java
BuyStock.java
public class BuyStock implements Order {
   private Stock abcStock;

   public BuyStock(Stock abcStock){
      this.abcStock = abcStock;
   }

   public void execute() {
      abcStock.buy();
   }
}

SellStock.java
public class SellStock implements Order {
   private Stock abcStock;

   public SellStock(Stock abcStock){
      this.abcStock = abcStock;
   }

   public void execute() {
      abcStock.sell();
   }
}
```

### 步骤 4

创建命令调用类。

```java
Broker.java
import java.util.ArrayList;
import java.util.List;

public class Broker {
   private List<Order> orderList = new ArrayList<Order>();

   public void takeOrder(Order order){
      orderList.add(order);
   }

   public void placeOrders(){
      for (Order order : orderList) {
         order.execute();
      }
      orderList.clear();
   }
}
```

### 步骤 5

使用 Broker 类来接受并执行命令。

```java
CommandPatternDemo.java
public class CommandPatternDemo {
   public static void main(String[] args) {
      Stock abcStock = new Stock();

      BuyStock buyStockOrder = new BuyStock(abcStock);
      SellStock sellStockOrder = new SellStock(abcStock);

      Broker broker = new Broker();
      broker.takeOrder(buyStockOrder);
      broker.takeOrder(sellStockOrder);

      broker.placeOrders();
   }
}
```
