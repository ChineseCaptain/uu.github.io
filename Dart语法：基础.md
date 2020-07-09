# Dart语法：基础

## 目录

1. 前言
2. 代码注释
3. 变量和常量
4. 数据类型
5. 操作符
6. 控制流
7. 异常

## 前言

1. 本篇开始讲解Dart语法，期间会通过对比Java、kotlin、Python、Groovy来区分语言直接的不同。

2. #### 行尾分号

   Dart中每行代码以分号为标示，代表本行结束。和Java一样，kotlin、groovy、Python中不需要行尾分号。

3. #### 生产环境和开发环境

   说下个人理解生产环境就是正式上线后的环境，开发环境就是开发过程中的环境。有点类似Android中的Release模式和Debug模式。



## 代码注释

1. #### 行注释

   和java一样，双引号//来注释单行代码

2. #### 多行注释

1. Dart中使用`/* … */`用来注释一段代码（可以是一行，也可以是多行）。

3. #### 文档注释

   * 用来对一个类或者方法进行说明的注释内容。位于类之前或者方法之前。

   * 可以使用 /// 开始， 也可以使用 /** 开始 并以 */ 结束

   * 在文档注释中可以使用中括号[]来引用      classes、 methods、 fields、 top-level variables、 functions、 和      parameters等内容。



## 变量和常量

1. #### 变量的定义

   * var 自动类型推断

     如这种样式：`var name = 'Bob';` 使用var来声明一个变量，然后通过初始化赋值来自动进行类型推断。

   * 显示声明

     如这种：`String name = 'Bob';`和Java一样直接声明变量的类型。

2. #### 变量的默认值

   所有变量的默认值都是null。Dart中一切都是对象，即使是数字类型，也是一个对象。

   示例代码：

   ```Dart
   int lineCount;
   assert(lineCount == null);  // 生产环境中asset会忽略，开发环境如果条件非true会抛异常
   ```

3. #### 常量

   在使用过程中不会被修改的变量，我们称之为常量。Dart中的常量分为两种：

   * final：只能被赋值一次的变量。
   * const：在编译时就固定的常量。如果希望常量是类级别的，需要使用需要标记为 `static const`来修饰，类似java中的static。

   定义和初始化方法：

   ```Dart
   final name = 'Bob';  // 自动类型推断，此时不需要var来修饰；
   final String nickname = 'Bobby';  // 显示声明，直接执行数据类型；
   const bar = 1000000;
   const double atm = 1.01325 * bar;
   ```



## 数据类型

1. Dart中支持的数据类型，也叫内建类型。包含：Number、String、Boolean、List、Map、Set、Rune、Symbol。

2. #### Number

   Number是数字类型，又细分为int和double：

   * int：整数类型。取值范围取决于平台，DartVM上是-2^63 到 2^63 - 1之间。如果被编译为JS，那么取值范围就是-2^53 到 2^53 - 1。
   * double：双精度浮点类型，也就是带小数点的数据类型。

   **Number类型的初始化**：

   ```dart
   var x = 1;  // 默认十进制
   var hex = 0xDEADBEEF;  // 十六进制
   var y = 1.1;  // 自动推断为double
   var exponents = 1.42e5;  // 科学计数法
   double z = 1; // 相当于 double z = 1.0  从Dart2.1开始直接，之前的版本会报错
   ```

   **Number和字符串转换**

   字符串转Number，直接使用int/double中的parse函数，直接传入字符串即可

   ```dart
   var one = int.parse('1');  // String -> int , 输出为1
   var onePointOne = double.parse('1.1');  // String -> double
   ```

   Number转字符串，直接使用toString()函数即可

   ```dart
   String oneAsString = 1.toString();  // int转字符串
   String piAsString = 3.14159.toStringAsFixed(2);  // 转为字符串并只保留后两位，输出为3.14
   ```

3. #### String字符串

   **创建字符串**

   可以使用单引号、双引号、三引号来定义一个字符串

   * 单引号：使用单引号来定义一个字符串。单引号中如果需要嵌套其它引号，需要在引号前加`\`来进行转义。

   * 双引号：和单引号类似，但双引号中可以直接嵌套单引号，不需要转义。

   * 三引号：使用三个单引号或者三个双引号来定义一段字符串。这种字符串可以包含多行。

     ```dart
     var s1 = '''
     You can create
     multi-line strings like this one.
     ''';
     
     var s2 = """This is also a
     multi-line string.""";
     ```

   **字符串中引用变量**

   字符串可以通过 `${`*`expression`*`}` 的方式内嵌表达式。  如果表达式是一个标识符，则 {} 可以省略。

   ```dart
   var s = 'string interpolation';
   var result = '${s.toUpperCase()} is very handy!';
   ```

   **原始字符串**

   当我们希望整个字符串中任何转移字符都不生效，而是要保留纯文本含义，那么可以在字符前加一个r标志来表明这是一个原始字符串。类似于Python。

   示例代码：

   ```dart
   var s = r"在Dart中使用 \n 来标示换行";  // 这里的\n并没有直接被转义为换行符
   ```

   **字符串相等比较**

   使用`==`来比较两个字符串是否相等。

   **字符串拼接**

   将多个字符串拼接为一个字符串，直接使用`+`来连接两个字符串即可。

4. #### Boolean布尔

   有两个值`true`和`false`。这两个值也是一个对象。

   定义一个布尔变量使用`bool`

5. #### List数组

   **定义方式**

   使用中括号

   ```dart
   var list = [1, 2, 3];
   ```

   注意：只能插入同类型的元素，如果类型不一致会报错。

   **读取元素**

   使用下标来获取某个元素。下标从0开始。

6. #### Set

   一个无序不重复的序列。在Set中每个元素都是唯一的，不会出现重复的元素，有的话会自动删除。类似Python中的元祖。

   **定义Set**

   使用大括号来定义

   ```dart
   var halogens = {'fluorine', 'chlorine', 'bromine', 'iodine', 'astatine'};
   var names = <String>{};;  // 定义一个空字符串Set
   var names = {};  // 定义一个空Set
   ```

   **添加元素**

   ```dart
   var elements = <String>{};
   elements.add('fluorine');
   elements.addAll(halogens);
   ```

7. #### Map字典

   Dart中用来存放**键值对**的一种数据类型，我们称之为Map，也就是字典。在map中一个key只能出现一次。当我们从map中取元素时，如果key不存在，那么不会抛异常，但是会返回null。key可以是字符串，也可以是纯数字。

   **Map对象的定义和初始化**

   Map和Set一样，都是使用大括号来进行初始化赋值。区别就是Set存的是单个元素，而Map存的是键值对。

   ```dart
   var gifts = {
     // Key:    Value
     'first': 'partridge',
     'second': 'turtledoves',
     'fifth': 'golden rings'  // 最后一个元素后，可以带逗号，也可以不带
   };
   
   var gifts = Map();
   gifts['first'] = 'partridge';
   gifts['second'] = 'turtledoves';
   gifts['fifth'] = 'golden rings';
   
   // 使用数字作为key
   var nobleGases = Map();
   nobleGases[2] = 'helium';
   nobleGases[10] = 'neon';
   nobleGases[18] = 'argon';
   ```

   **Map对象插入值和取值**

   除了初始化时对map进行赋值，后续也可以直接通过key进行赋值。

   ```dart
   var gifts = {'first': 'partridge'};
   gifts['fourth'] = 'calling birds';   // 赋值
   
   assert(gifts['first'] == 'partridge');  // 取值
   ```

8. Runne和Symbol，平时没用到，用到的时候再来补充。

## 操作符

1. 操作符就是两个对象之间相互操作的一些运算操作，比如两个整数相加。

2. 操作符的优先级：从左到右，从上到下。

3. #### 算数运算符

   | **操作符** | **解释**                                               |
   | ---------- | ------------------------------------------------------ |
   | +          | 相加运算，`var a = 1 + 1;`                             |
   | –          | 减号  ,`var a = 2 - 1;`                                |
   | -*expr*    | 负号                                                   |
   | *          | 乘号                                                   |
   | /          | 相除，5 / 2 的结果是 2.5                               |
   | ~/         | 除号，但是返回值为整数。5 / 2 的结果是 2 。            |
   | %          | 取模，相除之后取余数。                                 |
   | ++*var*    | 变量自动加一。如果前面有赋值语句，那么会先加一后赋值。 |
   | *var*++    | 变量自动加一。如果前面有赋值语句，那么会先赋值后加一。 |
   | --*var*    | 变量自动减一。如果前面有赋值语句，那么会先减一后赋值。 |
   | *var*--    | 变量自动减一。如果前面有赋值语句，那么会先赋值后减一。 |

4. #### 关系运算符

   | **操作符** | **解释**                                                     |
   | ---------- | ------------------------------------------------------------ |
   | ==         | 相等。判断两个对象x和y是否表示相同的事物， 使用 `==` 运算符。 |
   | !=         | 不等                                                         |
   | >          | 大于                                                         |
   | <          | 小于                                                         |
   | >=         | 大于等于                                                     |
   | <=         | 小于等于                                                     |

5. #### 类型运算符

   | **操作符** | **解释**                                                     |
   | ---------- | ------------------------------------------------------------ |
   | as         | 类型转换。`a as b`将a转换为b；                               |
   | is         | 如果对象是指定的类型返回 True  。例如， `obj is Object` 总是 true |
   | is!        | 如果对象是指定的类型返回 False                               |

6. #### 逻辑操作符

   | **操作符** | **解释**                                              |
   | ---------- | ----------------------------------------------------- |
   | !*表达式*  | 对表达式结果取反（true 变为 false ，false 变为 true） |
   | \|\|       | 逻辑 OR                                               |
   | &&         | 逻辑 AND                                              |

7. #### 条件表达式

   `if-else`表达式的两种种简写方式。

8. **三元表达式**

9. 格式为：`condition ? 表达式1 : 表达式2  `

10. 如果 *condition* 是 true，执行 表达式1  (并返回执行的结果)； 否则执行 表达式2  `并返回其结果。

11. **二元表达式**

12. 格式如下：`expr1 ?? expr2`

13. 如果 expr1 是 non-null，返回其值； 否则执行 expr2 并返回其结果。

8. #### 级联操作符

   级联操作符 (..) 可以在同一个对象上 连续调用多个函数以及访问成员变量。使用级联操作符可以避免创建 临时变量， 并且写出来的代码看起来 更加流畅。

   **示例代码**

   ```dart
   querySelector('#confirm') // 获取对象。
     ..text = 'Confirm' // 调用成员变量。
     ..classes.add('important')
     ..onClick.listen((e) => window.alert('Confirmed!'));
   
   // 等同于下面的代码
   var button = querySelector('#confirm');
   button.text = 'Confirm';
   button.classes.add('important');
   button.onClick.listen((e) => window.alert('Confirmed!'));
   
   // 嵌套使用级联代码
   final addressBook = (AddressBookBuilder()
         ..name = 'jenny'
         ..email = 'jenny@example.com'
         ..phone = (PhoneNumberBuilder()
               ..number = '415-555-0100'
               ..label = 'home')
             .build())
       .build();
   ```



## 控制流

1. Dart中的控制流分为如下几种：if-else、for循环、while循环、switch、assert

2. #### if-else

   和Java完全一样，直接看demo即可：

   ```dart
   if (isRaining()) {
     you.bringRainCoat();
   } else if (isSnowing()) {
     you.wearJacket();
   } else {
     car.putTopDown();
   }
   ```

3. #### for循环

   **标准写法**

   和Java完全一样

   ```dart
   var message = StringBuffer('Dart is fun');
   for (var i = 0; i < 5; i++) {
     message.write('!');
   }
   ```

   **for-in迭代写法**

   ```dart
   var collection = [0, 1, 2];
   for (var x in collection) {
     print(x); // 0 1 2
   }
   ```

   **迭代器对象forEach写法**

   实现了 Iterable 接口的对象， 可以使用 forEach()方法

   ```dart
   candidates.forEach((candidate) => candidate.interview());
   ```

   **跳过循环**

   * break：结束循环；
   * continue：跳过本次循环，开始下次循环；

4. #### while循环

   用法和Java中完全一样

   ```dart
   while (!isDone()) {
     doSomething();
   }
   
   do {
     printLine();
   } while (!atEndOfPage());
   ```

5. #### switch

   用法和java中的用法完全一直。不过Dart中全部是使用`==`来进行条件判断的。

   示例代码

   ```dart
   var command = 'OPEN';
   switch (command) {
     case 'CLOSED':
     case 'NOW_CLOSED':  // 多个条件可以执行同一个条件语句
       executeClosed();
       break;
     case 'OPEN':  // 满足open条件，执行对应代码
       executeOpen();
       break;  // case执行结束要手动调用break代码，否则会继续向下执行。
     default:  // 默认条件
       executeUnknown();
   }
   ```

   注意：如果没有满足的条件，且没有defalut case，那么会抛异常。

6. #### 断言assert

   通常在自动化测试和单元测试的时候会使用到断言。在Dart中， assert 语句只在开发环境中有效， 在生产环境是无效的。 Flutter 中的 assert 只在debug 模式中有效。

   **使用方法**

   assert的使用方法很简单，传入一个条件语句：

   * 如果为true就通过，程序正常运行；
   * 如果不为true，正常的程序执行流程会被中断。

   **示例代码**

   ```dart
   // 确认变量值小于100。
   assert(number < 100);
   ```



## 异常

1. #### 抛出异常

1. **使用方法**

2. ```dart
   throw new FormatException('Expected at least 1 section');  // 抛出一个格式异常
   throw 'Out of llamas!';  // 抛出任意对象
   ```

4. **胖箭头简写**

   因为抛出异常只有一行，所以可以使用胖箭头来简写

   ```void distanceTo(Point other) => throw UnimplementedError();```

2. #### 捕获异常 catch

   **示例代码**

   ```dart
   try {
     breedMoreLlamas();
   } on OutOfLlamasException {
     // 一个特殊的异常
     buyMoreLlamas();
   } on Exception catch (e) {
     // 其他任何异常
     print('Unknown exception: $e');
   } catch (e) {
     // 没有指定的类型，处理所有异常
     print('Something really unknown: $e');
   }
   ```
   * 使用try-catch来尝试捕获异常；

   * 使用on来判断异常的类型；

   * 多个异常就用多个on来判断异常类型；

   * 不加on则捕获所有异常

   * 函数 catch()     可以带有一个或者两个参数， 第一个参数为抛出的异常对象， 第二个为堆栈信息 (一个 StackTrace 对象)。

     ```dart
     try {
       // ···
     } on Exception catch (e) {
       print('Exception details:\n $e');
     } catch (e, s) {
       print('Exception details:\n $e');
       print('Stack trace:\n $s');
     }
     ```

   **重新抛出异常**

   我们在catch完一个异常后，可能需要把这个异常再次抛出。可以使用rethrow，将之前的异常重新抛出。

   ```dart
   try {
       dynamic foo = true;
       print(foo++); // Runtime error
     } catch (e) {
       print('misbehave() partially handled ${e.runtimeType}.');
       rethrow; // Allow callers to see the exception.
     }
   ```

3. #### Finally

   和java一样，不管是否抛出异常， finally中的代码都会被执行。 

   * 如果 `catch` 没有匹配到异常， 异常会在finally执行完成后，再次被抛出;
   * 如果有匹配到catch，那么会等匹配的 `catch` 执行完成后，再执行finally；

   ```dart
   try {
     breedMoreLlamas();
   } catch (e) {
     print('Error: $e'); // Handle the exception first.
   } finally {
     cleanLlamaStalls(); // Then clean up.
   }
   ```



## 参考

[Dart 编程语言概览](https://www.dartcn.com/guides/language/language-tour)

