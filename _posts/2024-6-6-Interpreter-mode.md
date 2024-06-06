---
layout: post
title: "解释器模式"
date: 2024-6-6
tags: [design-pattern]
comments: true
author: zxy
---

## 概要

> 意图

定义一种语言的文法表示，并创建一个解释器，该解释器能够解释该语言中的句子。

> 主要解决的问题

- 解释器模式用于构建一个能够解释特定语言或文法的句子的解释器。

> 使用场景

- 当某一特定类型的问题频繁出现，并且可以通过一种简单的语言来表达这些问题的实例时。

> 实现方式

- **定义文法**：明确语言的终结符和非终结符。
- **构建语法树**：根据语言的句子构建对应的语法树结构。
- **创建环境类**：包含解释过程中所需的全局信息，通常是一个 HashMap。

> 关键代码

- **终结符与非终结符**：定义语言的文法结构。
- **环境类**：存储解释过程中需要的外部环境信息。

> 应用实例

- **编译器**：解释器模式可以用于编译器设计，将源代码解释为目标代码。
- **正则表达式**：解释器模式可以用于解析和执行正则表达式。
- **SQL 解析**：解释器模式可以用于解析和执行 SQL 语句。

> 优点

- **可扩展性好**：容易添加新的解释表达式的方式。
- **灵活性**：可以根据需要轻松扩展或修改文法。
- **易于实现简单文法**：对于简单的语言，实现起来相对容易。

> 缺点

1. **使用场景有限**：只适用于适合使用解释的简单文法。
2. **维护困难**：对于复杂的文法，维护和扩展变得困难。
3. **类膨胀**：可能会产生很多类，每个文法规则对应一个类。
4. **递归调用**：解释器模式通常使用递归调用，这可能难以理解和跟踪。

> 使用建议

- 在需要解释执行语言中的句子时，考虑使用解释器模式。
- 确保文法简单，以避免系统变得过于复杂。

> 注意事项

- 解释器模式在 Java 中可能不是首选，如果遇到适用场景，可以考虑使用如 expression4J 之类的库来代替。

> 结构

解释器模式包含以下几个主要角色：

- **抽象表达式（Abstract Expression）**：定义了解释器的抽象接口，声明了解释操作的方法，通常是一个抽象类或接口。
- **终结符表达式（Terminal Expression）**：实现了抽象表达式接口的终结符表达式类，用于表示语言中的终结符（如变量、常量等），并实现了对应的解释操作。
- **非终结符表达式（Non-terminal Expression）**：实现了抽象表达式接口的非终结符表达式类，用于表示语言中的非终结符（如句子、表达式等），并实现了对应的解释操作。
- **上下文（Context）**：包含解释器之外的一些全局信息，在解释过程中提供给解释器使用，通常用于存储变量的值、保存解释器的状态等。
- **客户端（Client）**：创建并配置具体的解释器对象，并将需要解释的表达式传递给解释器进行解释。

## 实现

我们将创建一个接口 _Expression_ 和实现了 _Expression_ 接口的实体类。定义作为上下文中主要解释器的 _TerminalExpression_ 类。其他的类 _OrExpression_、_AndExpression_ 用于创建组合式表达式。

_InterpreterPatternDemo_，我们的演示类使用 _Expression_ 类创建规则和演示表达式的解析。

![解释器模式的 UML 图](https://www.runoob.com/wp-content/uploads/2014/08/interpreter_pattern_uml_diagram.jpg)

### 步骤 1

创建一个表达式接口。

```java
Expression.java
public interface Expression {
   public boolean interpret(String context);
}
```

### 步骤 2

创建实现了上述接口的实体类。

```java
TerminalExpression.java
public class TerminalExpression implements Expression {

   private String data;

   public TerminalExpression(String data){
      this.data = data;
   }

   @Override
   public boolean interpret(String context) {
      if(context.contains(data)){
         return true;
      }
      return false;
   }
}

OrExpression.java
public class OrExpression implements Expression {

   private Expression expr1 = null;
   private Expression expr2 = null;

   public OrExpression(Expression expr1, Expression expr2) {
      this.expr1 = expr1;
      this.expr2 = expr2;
   }

   @Override
   public boolean interpret(String context) {
      return expr1.interpret(context) || expr2.interpret(context);
   }
}

AndExpression.java
public class AndExpression implements Expression {

   private Expression expr1 = null;
   private Expression expr2 = null;

   public AndExpression(Expression expr1, Expression expr2) {
      this.expr1 = expr1;
      this.expr2 = expr2;
   }

   @Override
   public boolean interpret(String context) {
      return expr1.interpret(context) && expr2.interpret(context);
   }
}
```

### 步骤 3

_InterpreterPatternDemo_ 使用 _Expression_ 类来创建规则，并解析它们。

```java
InterpreterPatternDemo.java
public class InterpreterPatternDemo {

   //规则：Robert 和 John 是男性
   public static Expression getMaleExpression(){
      Expression robert = new TerminalExpression("Robert");
      Expression john = new TerminalExpression("John");
      return new OrExpression(robert, john);
   }

   //规则：Julie 是一个已婚的女性
   public static Expression getMarriedWomanExpression(){
      Expression julie = new TerminalExpression("Julie");
      Expression married = new TerminalExpression("Married");
      return new AndExpression(julie, married);
   }

   public static void main(String[] args) {
      Expression isMale = getMaleExpression();
      Expression isMarriedWoman = getMarriedWomanExpression();

      System.out.println("John is male? " + isMale.interpret("John"));
      System.out.println("Julie is a married women? "
      + isMarriedWoman.interpret("Married Julie"));
   }
}
```
