---
layout: post
title: "空对象模式"
date: 2024-6-6
tags: [design-pattern]
comments: true
author: zxy
---

## 概要

> 意图

使用一个空对象代替 null 值，这个空对象实现了相同的接口，但对请求不做任何操作或提供默认操作。

> 主要解决的问题

- 空对象模式解决的是在系统中使用 null 值可能导致的问题，如 NullPointerException 异常。
- 它允许系统在没有合适对象时，使用一个"安全"的空对象继续运行，而不是失败。

> 使用场景

- 当系统中需要处理 null 对象，但又希望避免 null 检查或处理 null 值时。

> 实现方式

- **定义协议**：定义一个协议或接口，规定需要实现的行为。
- **创建具体对象**：实现协议的具体对象，提供实际的行为。
- **创建空对象**：也实现相同的协议，但提供"空"的实现，即不执行任何有意义的操作。

> 关键代码

- **协议或接口**：规定对象需要实现的方法。
- **具体对象**：实现了协议，包含实际的业务逻辑。
- **空对象**：实现了协议，但方法实现为空或默认行为。

> 应用实例

- **日志系统**：在日志系统中，空对象可能代表一个不执行任何操作的日志器。
- **默认用户**：在一个系统中，如果当前没有用户，可以使用一个空用户对象代替 null。

> 优点

1. **避免空值检查**：消除了代码中的 null 值检查。
2. **简化客户端代码**：客户端可以无视对象是否为空，直接调用方法。
3. **扩展性**：添加新的具体对象对客户端透明，无需修改现有代码。

> 缺点

- **可能隐藏错误**：使用空对象可能隐藏了错误或异常情况，导致难以调试。
- **增加设计复杂性**：需要为每个可能返回 null 的接口实现一个空对象。

> 使用建议

- 在系统中需要处理 null 值，但又希望简化错误处理和避免 null 检查时，考虑使用空对象模式。

> 注意事项

- 空对象模式应该谨慎使用，确保它不会掩盖错误或隐藏系统的真实状态。
- 空对象应该提供和具体对象相同的接口，使得客户端代码无需改变即可使用。

> 结构

空对象模式包含以下几个主要角色：

- **抽象对象（Abstract Object）**：定义了客户端所期望的接口。这个接口可以是一个抽象类或接口。
- **具体对象（Concrete Object）**：实现了抽象对象接口的具体类。这些类提供了真实的行为。
- **空对象（Null Object）**：实现了抽象对象接口的空对象类。这个类提供了默认的无效行为，以便在对象不可用或不可用时使用。它可以作为具体对象的替代者，在客户端代码中代替空值检查。

## 实现

我们将创建一个定义操作（在这里，是客户的名称）的 _AbstractCustomer_ 抽象类，和扩展了 _AbstractCustomer_ 类的实体类。工厂类 _CustomerFactory_ 基于客户传递的名字来返回 _RealCustomer_ 或 _NullCustomer_ 对象。

_NullPatternDemo_，我们的演示类使用 _CustomerFactory_ 来演示空对象模式的用法。

![空对象模式的 UML 图](https://www.runoob.com/wp-content/uploads/2014/08/null_pattern_uml_diagram.jpg)

### 步骤 1

创建一个抽象类。

```java
AbstractCustomer.java
public abstract class AbstractCustomer {
   protected String name;
   public abstract boolean isNil();
   public abstract String getName();
}
```

### 步骤 2

创建扩展了上述类的实体类。

```java
RealCustomer.java
public class RealCustomer extends AbstractCustomer {

   public RealCustomer(String name) {
      this.name = name;
   }

   @Override
   public String getName() {
      return name;
   }

   @Override
   public boolean isNil() {
      return false;
   }
}

NullCustomer.java
public class NullCustomer extends AbstractCustomer {

   @Override
   public String getName() {
      return "Not Available in Customer Database";
   }

   @Override
   public boolean isNil() {
      return true;
   }
}
```

### 步骤 3

创建 _CustomerFactory_ 类。

```java
CustomerFactory.java
public class CustomerFactory {

   public static final String[] names = {"Rob", "Joe", "Julie"};

   public static AbstractCustomer getCustomer(String name){
      for (int i = 0; i < names.length; i++) {
         if (names[i].equalsIgnoreCase(name)){
            return new RealCustomer(name);
         }
      }
      return new NullCustomer();
   }
}
```

### 步骤 4

使用 _CustomerFactory_，基于客户传递的名字，来获取 _RealCustomer_ 或 _NullCustomer_ 对象。

```java
NullPatternDemo.java
public class NullPatternDemo {
   public static void main(String[] args) {

      AbstractCustomer customer1 = CustomerFactory.getCustomer("Rob");
      AbstractCustomer customer2 = CustomerFactory.getCustomer("Bob");
      AbstractCustomer customer3 = CustomerFactory.getCustomer("Julie");
      AbstractCustomer customer4 = CustomerFactory.getCustomer("Laura");

      System.out.println("Customers");
      System.out.println(customer1.getName());
      System.out.println(customer2.getName());
      System.out.println(customer3.getName());
      System.out.println(customer4.getName());
   }
}
```
