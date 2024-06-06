---
layout: post
title: "中介者模式"
date: 2024-6-6
tags: [design-pattern]
comments: true
author: zxy
---

## 概要

> 意图

通过引入一个中介者对象来封装和协调多个对象之间的交互，从而降低对象间的耦合度。

> 主要解决的问题

- 解决对象间复杂的一对多关联问题，避免对象之间的高度耦合，简化系统结构。

> 使用场景

- 当系统中多个类相互耦合，形成网状结构时。

> 实现方式

- **定义中介者接口**：规定中介者必须实现的接口。
- **创建具体中介者**：实现中介者接口，包含协调各同事对象交互的逻辑。
- **定义同事类**：各个对象不需要显式地相互引用，而是通过中介者来进行交互。

> 关键代码

- **中介者**：封装了对象间的交互逻辑。
- **同事类**：通过中介者进行通信。

> 优点

1. **降低复杂度**：将多个对象间的一对多关系转换为一对一关系。
2. **解耦**：对象之间不再直接引用，通过中介者进行交互。
3. **符合迪米特原则**：对象只需知道中介者，不需要知道其他对象。

> 缺点

- **中介者复杂性**：中介者可能会变得庞大和复杂，难以维护。

> 使用建议

- 当系统中对象间存在复杂的引用关系时，考虑使用中介者模式。
- 通过中介者封装多个类的行为，避免生成过多的子类。

> 注意事项

- 避免在职责不明确或混乱的情况下使用中介者模式，这可能导致中介者承担过多职责。

> 结构

中介者模式包含以下几个主要角色：

- **中介者（Mediator）**：定义了一个接口用于与各个同事对象通信，并管理各个同事对象之间的关系。通常包括一个或多个事件处理方法，用于处理各种交互事件。
- **具体中介者（Concrete Mediator）**：实现了中介者接口，负责实现各个同事对象之间的通信逻辑。它会维护一个对各个同事对象的引用，并协调它们的交互。
- **同事对象（Colleague）**：定义了一个接口，用于与中介者进行通信。通常包括一个发送消息的方法，以及一个接收消息的方法。
- **具体同事对象（Concrete Colleague）**：实现了同事对象接口，是真正参与到交互中的对象。它会将自己的消息发送给中介者，由中介者转发给其他同事对象。

## 实现

我们通过聊天室实例来演示中介者模式。实例中，多个用户可以向聊天室发送消息，聊天室向所有的用户显示消息。我们将创建两个类 _ChatRoom_ 和 _User_。_User_ 对象使用 _ChatRoom_ 方法来分享他们的消息。

_MediatorPatternDemo_，我们的演示类使用 _User_ 对象来显示他们之间的通信。

![中介者模式的 UML 图](https://www.runoob.com/wp-content/uploads/2014/08/mediator_pattern_uml_diagram.jpg)

### 步骤 1

创建中介类。

```java
ChatRoom.java
import java.util.Date;

public class ChatRoom {
   public static void showMessage(User user, String message){
      System.out.println(new Date().toString()
         + " [" + user.getName() +"] : " + message);
   }
}
```

### 步骤 2

创建 user 类。

```java
User.java
public class User {
   private String name;

   public String getName() {
      return name;
   }

   public void setName(String name) {
      this.name = name;
   }

   public User(String name){
      this.name  = name;
   }

   public void sendMessage(String message){
      ChatRoom.showMessage(this,message);
   }
}
```

### 步骤 3

使用 _User_ 对象来显示他们之间的通信。

```java
MediatorPatternDemo.java
public class MediatorPatternDemo {
   public static void main(String[] args) {
      User robert = new User("Robert");
      User john = new User("John");

      robert.sendMessage("Hi! John!");
      john.sendMessage("Hello! Robert!");
   }
}
```
