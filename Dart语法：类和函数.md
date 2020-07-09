## 目录

1. 前言
2. 类
3. 函数
4. 枚举
5. 泛型
6. 导入其它库



## 前言

1. 本篇是 Dart语法系列第二篇，第一篇[Dart语法：基础](https://juejin.im/post/5f00c254e51d45347a0d19a9)。



## 类

1. #### mixin 继承机制

2. #### 实例对象

   **创建实例对象**

   从Dart 2 开始 `new` 关键字变成了可选项。Dart创建一个实例对象和Kotlin、Python一样，不需要 `new `关键字，直接创建即可。

   **调用实例对象的变量和方法**

   我们创建完一个实例对象后，直接通过点操作符`.`即可调用该对象的成员变量和方法。

   **空安全**

   在Java和Python中，当一个实例对象为空的时候，我们调用该对象的某些成员变量或者方法，会报空指针异常NPE。所以在调用前要手动判空。Dart和Kotlin就没这么麻烦了，在调用成员变量或者方法时，使用`?.`代替`.`就可以实现只在非空时再调用，当为空的时候不进行调用，从而避免了NPE。

   **示例代码**

   ```dart
   class Point {
     num x;
     num y;
   }
   var p = Point();  // 创建一个Point实例对象
   p.y = 3;  // 对属性y进行赋值
   print(p.y);  // 输出p对象的y属性值
   
   p?.y = 4;  // 如果p对象非空时，设置它变量y的值为 4。
   ```

3. #### 定义一个类

   格式为：`class 类名{...}`。和kotlin相同，和Java相比不需要public修饰符。

   ```dart
   class Point {
     num x, y;
     Point(this.x, this.y);
   }
   ```

4. #### 构造函数

   **默认构造函数**

   在没有声明构造函数的情况下， Dart 会提供一个默认的构造函数。 默认构造函数没有参数并会调用父类的无参构造函数。

   **定义构造函数**

   创建一个与其类同名的函数来声明构造函数。

   ```dart
   class Point {
     num x, y;
   
     // 标准的构造函数
     Point(num x, num y) {
       this.x = x;  // this关键字表示引用当前实例
       this.y = y;
     }
     
     // 简化版的构造函数，会自动将传入的值赋值给x和y，等同于标准的构造函数
     Point(this.x, this.y);
   }
   ```

   **命名构造函数**

   一个类可以有多个构造函数，这些构造函数名相同但传参不同，不同的构造函数是为了表明不同的构造函数意图。为了更清晰的表明构造函数意图，可以使用命名构造函数来定义构造函数。如下面代码，让不同构造函数更有可读性。

   ```dart
   class Point {
     num x, y;
   
     Point(this.x, this.y);
   
     // 命名构造函数，可以表明从Json来构造
     Point.fromJson(String json) {
       // to do sth.
     }
   }
   ```

   **初始化列表**

   如果我们希望在执行构造函数之前，先按照我们期望的顺序去执行一些初始化代码。那么可以以初始化列表的形式来实现。

   格式：在构造函数之后，添加一个冒号，然后在冒号后面添加一系列表达式，多个表达式之间用逗号分隔。

   这样在初始化的时候，会根据冒号后表达式的顺序，依次进行初始化。当这些初始化执行结束后，才开始执行剩下的构造函数初始化。

   ```dart
   Point.fromJson(Map jsonMap)
         : x = jsonMap['x'],
           y = jsonMap['y'] {
       print('In Point.fromJson(): ($x, $y)');
     }
   ```

   **重定向构造函数**

   在一个构造函数会调动类中的其他构造函数

   ```dart
   class Point {
     num x, y;
   
     // 类的主构造函数。
     Point(this.x, this.y);
   
     // 指向主构造函数
     Point.alongXAxis(num x) : this(x, 0);
   }
   ```

   **继承时的构造函数**

   * 子类的构造函数在执行之前会自动调用父类的默认构造函数（也就是上面说到的无参构造函数）；
   * 如果子类有初始化列表，那么初始化参数列表在父类构造函数执行之前执行；
   * 如果父类中没有匿名无参的构造函数， 则需要手工调用父类的其他构造函数。 在当前构造函数冒号 (`:`) 之后，函数体之前，声明调用父类构造函数。

   示例代码

   ```dart
   class Person {
     String firstName;
   
     Person.fromJson(Map data) {
       print('in Person');
     }
   }
   
   class Employee extends Person {
     // 父类person没有默认的无参构造函数，所以必须手动指定调用父类哪一个构造函数
     Employee.fromJson(Map data) : super.fromJson(data) { 
       print('in Employee');
     }
   }
   ```

5. #### 抽象类

   使用 `abstract` 修饰符来定义 抽象类。抽象类可以实现部分代码。

   ```dart
   abstract class AbstractContainer {
     void updateChildren(); // 抽象方法。
   }
   ```

6. #### 接口

   Dart中这个一点和java、kotlin都不同，没有`interface`修饰符来定义接口。在Dart中，每个类都都隐式的定义了一个接口，接口包含了该类所有的实例成员及其实现的接口。 也就是说Dart中每个类都是一个接口。所以在Dart中使用接口的话，就和普通类一样使用就可以了。

   我们使用`implements`来实现目标类的接口，然后就可以来重新目标类中的方法和成员变量了。

   ```dart
   // person 类。 隐式接口里面包含了 greet() 方法声明。
   class Person {
     // 包含在接口里，但只在当前库中可见。
     final _name;
   
     // 不包含在接口里，因为这是一个构造函数。
     Person(this._name);
   
     // 包含在接口里。
     String greet(String who) => 'Hello, $who. I am $_name.';
   }
   
   // person 接口的实现。
   class Impostor implements Person {
     get _name => '';
   
     String greet(String who) => 'Hi $who. Do you know who I am?';
   }
   ```

7. #### 继承

   使用 `extends` 关键字来继承父类， 使用 `super` 关键字来引用父类