---
layout: post
title: "概述"
date: 2024-6-6
tags: [design-pattern]
comments: true
author: zxy
---

### 为什么有设计模式

- 设计模式是开发的产物，它是由开发人员在相当长的时间内的开发经验和错误总结出来的
- 设计模式通常是针对面向对象语言的，有两个原则：对接口编程而不是对实现编程 + 优先使用对象组合而不是继承
- 设计模式的优点是可重用代码，代码可读性高，代码可靠性高，适合团队编程
- 设计模式与现实关联紧密，原理是围绕现实生活问题进行设计的

### 设计模式的分类

| 模式       | 描述                                                                                                            | 具体包括                                                                                                                                                                           |
| ---------- | --------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 创建型模式 | 提供一种创建对象且隐藏创建逻辑的方式，不使用 new 直接实例化对象。使得程序在判断某个实例需要创建哪些对象时更灵活 | 工厂模式<br />抽象工厂模式<br />单例模式<br />建造者模式<br />原型模式                                                                                                             |
| 结构型模式 | 关注对象间的组合和关系，旨在解决如何构建灵活且可复用的类和对象结构                                              | 适配器模式<br />桥接模式<br />过滤器模式<br />组合模式<br />装饰器模式<br />外观模式<br />享元模式<br />代理模式                                                                   |
| 行为型模式 | 关注对象间通信和交互，旨在解决对象之间的责任分配和算法的封装                                                    | 责任链模式<br />命令模式<br />解释器模式<br />迭代器模式<br />中介者模式<br />备忘录模式<br />观察者模式<br />状态模式<br />空对象模式<br />策略模式<br />模板模式<br />访问者模式 |
| J2EE 模式  | 关注表示层                                                                                                      | MVC 模式<br />业务代表模式<br />组合实体模式<br />数据访问对象模式<br />前端控制器模式<br />拦截过滤器模式<br />服务定位器模式<br />传输对象模式                                   |

![设计模式之间的关系](https://www.runoob.com/wp-content/uploads/2014/08/mmexport1707099938077.png)

### 设计模式的原则

**开闭原则：**

- 对扩展开放，对修改关闭。程序需要拓展新功能时，不允许修改原有代码。
- 程序拓展性好，易于维护和升级。
- 使用接口和抽象类实现

**里氏代换原则：**

- 当子类可以替换掉父类且软件功能不受影响，父类才真正被复用。子类可以在父类基础上添加新的行为
- 里氏代换原则是抽象化的具体步骤和规范，是开闭原则的补充

**依赖倒转原则：**

- 针对接口编程，依赖于抽象而不是具体

**接口隔离原则：**

- 使用多个接口比一个接口好
- 目标是降低接口依赖，降低耦合

**最少知道原则：**

- 一个实体应该尽可能少地和其他实体之间发生相互作用，使得系统功能模块相对独立

**合成复用原则：**

- 尽量使用合成方式，而不是使用继承
