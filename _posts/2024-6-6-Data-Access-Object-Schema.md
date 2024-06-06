---
layout: post
title: "数据访问对象模式"
date: 2024-6-6
tags: [design-pattern]
comments: true
author: zxy
---

## 概要

> 意图

将数据访问逻辑从业务逻辑中分离出来，并将数据访问操作封装在一个专用的类中。

> 主要解决的问题

- 解决业务逻辑与数据访问逻辑紧密耦合的问题，提高代码的可维护性和可重用性。

> 使用场景

- 当需要将应用程序的数据访问逻辑集中管理，以便简化业务逻辑并易于进行数据操作时。

> 实现方式

- **定义 DAO 接口**：声明数据访问操作的方法。
- **实现 DAO 类**：实现 DAO 接口，封装对数据源（如数据库）的所有访问逻辑。
- **数据传输对象（DTO）**：可选，用于封装从数据源检索的数据。

> 关键代码

- **DAO 接口**：声明数据访问和操作的方法。
- **DAO 实现**：提供 DAO 接口的具体实现，包含对数据库的访问代码。

> 应用实例

- **用户管理应用**：用户 DAO 类负责与数据库交互，管理用户数据的增删改查。

> 优点

1. **分离关注点**：将数据访问逻辑与业务逻辑分离，降低系统的耦合度。
2. **易于维护**：数据访问逻辑集中管理，便于维护和更新。
3. **可扩展性**：更换数据源或修改数据访问逻辑时，不影响业务逻辑层。

> 缺点

- **可能增加复杂性**：对于简单的应用程序，引入 DAO 模式可能增加额外的抽象层次。

> 使用建议

- 当应用程序需要与多种数据源交互，或者数据访问逻辑较为复杂时，使用 DAO 模式。

> 注意事项

- DAO 类应该尽量简单，只包含数据访问逻辑，不包含业务逻辑。

> 包含的几个主要角色

1. **DAO 接口（DAO Interface）**：
   - 声明数据访问和操作的方法。
2. **DAO 实现（DAO Implementation）**：
   - 实现 DAO 接口，封装对数据源的所有访问逻辑。
3. **数据传输对象（Data Transfer Object, DTO）（可选）**：
   - 封装从数据源检索的数据，用于业务逻辑层与数据访问层之间的数据传输。
4. **业务逻辑层（Business Logic Layer）**：
   - 使用 DAO 接口与数据源交互，不直接依赖于数据访问的具体实现。
5. **数据源（Data Source）**：
   - 数据库或其他持久化存储，DAO 实现与之交互以存取数据。

数据访问对象模式通过提供一个清晰的数据访问层，有助于保持应用程序的整洁和可管理性。

## 实现

我们将创建一个作为模型对象或数值对象的 _Student_ 对象。_StudentDao_ 是数据访问对象接口。_StudentDaoImpl_ 是实现了数据访问对象接口的实体类。_DaoPatternDemo_，我们的演示类使用 _StudentDao_ 来演示数据访问对象模式的用法。

![数据访问对象模式的 UML 图](https://www.runoob.com/wp-content/uploads/2014/08/dao_pattern_uml_diagram.jpg)

**步骤 1**

创建数值对象。

```java
Student.java
public class Student {
   private String name;
   private int rollNo;

   Student(String name, int rollNo){
      this.name = name;
      this.rollNo = rollNo;
   }

   public String getName() {
      return name;
   }

   public void setName(String name) {
      this.name = name;
   }

   public int getRollNo() {
      return rollNo;
   }

   public void setRollNo(int rollNo) {
      this.rollNo = rollNo;
   }
}
```

**步骤 2**

创建数据访问对象接口。

```java
StudentDao.java
import java.util.List;

public interface StudentDao {
   public List<Student> getAllStudents();
   public Student getStudent(int rollNo);
   public void updateStudent(Student student);
   public void deleteStudent(Student student);
}
```

**步骤 3**

创建实现了上述接口的实体类。

```java
StudentDaoImpl.java
import java.util.ArrayList;
import java.util.List;

public class StudentDaoImpl implements StudentDao {

   //列表是当作一个数据库
   List<Student> students;

   public StudentDaoImpl(){
      students = new ArrayList<Student>();
      Student student1 = new Student("Robert",0);
      Student student2 = new Student("John",1);
      students.add(student1);
      students.add(student2);
   }
   @Override
   public void deleteStudent(Student student) {
      students.remove(student.getRollNo());
      System.out.println("Student: Roll No " + student.getRollNo()
         +", deleted from database");
   }

   //从数据库中检索学生名单
   @Override
   public List<Student> getAllStudents() {
      return students;
   }

   @Override
   public Student getStudent(int rollNo) {
      return students.get(rollNo);
   }

   @Override
   public void updateStudent(Student student) {
      students.get(student.getRollNo()).setName(student.getName());
      System.out.println("Student: Roll No " + student.getRollNo()
         +", updated in the database");
   }
}
```

**步骤 4**

使用 _StudentDao_ 来演示数据访问对象模式的用法。

```java
DaoPatternDemo.java
public class DaoPatternDemo {
   public static void main(String[] args) {
      StudentDao studentDao = new StudentDaoImpl();

      //输出所有的学生
      for (Student student : studentDao.getAllStudents()) {
         System.out.println("Student: [RollNo : "
            +student.getRollNo()+", Name : "+student.getName()+" ]");
      }


      //更新学生
      Student student =studentDao.getAllStudents().get(0);
      student.setName("Michael");
      studentDao.updateStudent(student);

      //获取学生
      studentDao.getStudent(0);
      System.out.println("Student: [RollNo : "
         +student.getRollNo()+", Name : "+student.getName()+" ]");
   }
}
```
