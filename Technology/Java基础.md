## Java基本语法

### 移位运算符

在Java中，移位运算符用于对整数进行二进制位的移动。Java中的移位运算符包括三项，并且支持的类型只有`int`和`long`，编译器在对`short`、`byte`、`char`类型进行移位前，都会将其转换为`int`类型再操作。

而Java中的int类型是32位的，long类型是64位的，下面举例以32位的int类型

1. **左移运算符 (`<<`)：**
   - 格式：`a << b`
   - 功能：将`a`的二进制表示向左移动`b`位，右侧用0填充。
   - 示例：`5 << 2` 的结果是 20，因为 5 的二进制表示是 `101`，向左移动两位后变成 `10100`，对应的十进制是 20。
2. **右移运算符 (`>>`)：**
   - 格式：`a >> b`
   - 功能：将`a`的二进制表示向右移动`b`位，左侧用原来的符号位填充（即正数补0，负数补1）。
   - 示例：`-10 >> 2` 的结果是 `-3`
     - 因为 `10` 的二进制表示是 `00001010`，`-10`的二进制表示为其正数的二进制取反的`11110101`加`1`得`11110110`
     - 向右移动两位后其二进制表示变成 `11111101`，最左侧为`1`表示为负数，减`1`后得`11111100`，取反得`00000011`，其转换成负数表示为`-3`。
3. **无符号右移运算符 (`>>>`)：**
   - 格式：`a >>> b`
   - 功能：将`a`的二进制表示向右移动`b`位，左侧用0填充。
   - 示例：`-10 >>> 2` 的结果是 `1073741821`
     - 因为 -10 的二进制表示是 `00000000` `00000000` `00000000` `00001010`，取反后加`1`得 `11111111` `11111111` `11111111` `11110110`
     - 向右移动两位后变成 `00111111` `11111111` `11111111` `11111101`，最左侧为`0`表示为正数，直接转换成十进制为 `1073741821`

> **当 int 类型左移/右移位数大于等于 32 位操作时，会先求余（%）后再进行左移/右移操作。**



## 基本数据类型

| 类型    | 位数 | 字节 | 默认值   | 十进制范围                                                  | 二进制范围                                                   |
| ------- | ---- | ---- | -------- | ----------------------------------------------------------- | ------------------------------------------------------------ |
| boolean | -    | -    | false    | true 或 false                                               | true 或 false                                                |
| byte    | 8    | 1    | 0        | -128(-2^7) 到 127(2^7-1)                                    | 10000000 到 01111111                                         |
| short   | 16   | 2    | 0        | -32768(-2^7) 到 32767(2^7-1)                                | 1000000000000000 到 0111111111111111                         |
| int     | 32   | 4    | 0        | -2147483648(-2^31)  到 2147483647(2^31-1)                   | 10000000000000000000000000000000 到 01111111111111111111111111111111 |
| long    | 64   | 8    | 0L       | -9223372036854775808(-2^63)  到 9223372036854775807(2^63-1) | 1000000000000000000000000000000000000000000000000000000000000000 到 0111111111111111111111111111111111111111111111111111111111111111 |
| float   | 32   | 4    | 0.0f     | IEEE 754标准表示范围                                        | IEEE 754标准表示范围                                         |
| double  | 64   | 8    | 0.0      | IEEE 754标准表示范围                                        | IEEE 754标准表示范围                                         |
| char    | 16   | 2    | '\u0000' | 0 到 65535                                                  | 00000000 00000000 到 11111111 11111111                       |

### 基本类型和包装类型

区别：

- **用途**：除了定义一些常量和局部变量之外，我们在其他地方比如方法参数、对象属性中很少会使用基本类型来定义变量。并且，包装类型可用于泛型，而基本类型不可以。

- **存储方式**：基本数据类型的局部变量存放在 Java 虚拟机栈中的局部变量表中，基本数据类型的成员变量（未被 `static` 修饰 ）存放在 Java 虚拟机的堆中。包装类型属于对象类型，我们知道几乎所有对象实例都存在于堆中。

- **占用空间**：相比于包装类型（对象类型）， 基本数据类型占用的空间往往非常小。

- **默认值**：成员变量包装类型不赋值就是 `null` ，而基本类型有默认值且不是 `null`。

- **比较方式**：对于基本数据类型来说，`==` 比较的是值。对于包装数据类型来说，`==` 比较的是对象的内存地址。所有整型包装类对象之间值的比较，全部使用 `equals()` 方法。



Java 基本数据类型的包装类型的大部分都用到了缓存机制来提升性能。

- `Byte`,`Short`,`Integer`,`Long` 这 4 种包装类默认创建了数值 **[-128，127]** 的相应类型的缓存数据，`Character` 创建了数值在 **[0,127]** 范围的缓存数据，`Boolean` 直接返回 `True` or `False`。

- 两种浮点数类型的包装类 `Float`,`Double` 并没有实现缓存机制。



## 变量

**语法形式**：从语法形式上看，成员变量是属于类的，而局部变量是在代码块或方法中定义的变量或是方法的参数；成员变量可以被 `public`,`private`,`static` 等修饰符所修饰，而局部变量不能被访问控制修饰符及 `static` 所修饰；但是，成员变量和局部变量都能被 `final` 所修饰。

**存储方式**：从变量在内存中的存储方式来看，如果成员变量是使用 `static` 修饰的，那么这个成员变量是属于类的，如果没有使用 `static` 修饰，这个成员变量是属于实例的。而对象存在于堆内存，局部变量则存在于栈内存。

**生存时间**：从变量在内存中的生存时间上看，成员变量是对象的一部分，它随着对象的创建而存在，而局部变量随着方法的调用而自动生成，随着方法的调用结束而消亡。

**默认值**：从变量是否有默认值来看，成员变量如果没有被赋初始值，则会自动以类型的默认值而赋值（一种情况例外:被 `final` 修饰的成员变量也必须显式地赋值），而局部变量则不会自动赋值。



**静态变量有什么作用**

静态变量也就是被 `static` 关键字修饰的变量。它可以被类的所有实例共享，无论一个类创建了多少个对象，它们都共享同一份静态变量。也就是说，静态变量只会被分配一次内存，即使创建多个对象，这样可以节省内存。

> 通常情况下，静态变量会被 `final` 关键字修饰成为常量。



在Java中，继承下的父子类的初始化和消亡（构造函数和析构函数）的顺序遵循以下规则：

**初始化（构造函数）顺序：**

1. 父类的静态变量和静态初始化块（按它们在类中出现的顺序）。
2. 子类的静态变量和静态初始化块（按它们在类中出现的顺序）。
3. 父类的实例变量和实例初始化块（按它们在类中出现的顺序）。
4. 父类的构造函数。
5. 子类的实例变量和实例初始化块（按它们在类中出现的顺序）。
6. 子类的构造函数。



**实现接口的要求**【两同两小一大原则】

1. 方法名相同，参数类型相同
2. 子类返回类型小于等于父类方法返回类型
3. 子类抛出异常小于等于父类方法抛出异常
4. 子类访问权限大于等于父类方法访问权限



## IO

IO 即 `Input/Output`，输入和输出。数据输入到计算机内存的过程即输入，反之输出到外部存储（比如数据库，文件，远程主机）的过程即输出。数据传输过程类似于水流，因此称为 IO 流。IO 流在 Java 中分为输入流和输出流，而根据数据的处理方式又分为字节流和字符流。

Java IO 流的 40 多个类都是从如下 4 个抽象类基类中派生出来的。

- `InputStream`/`Reader`: 所有的输入流的基类，前者是字节输入流，后者是字符输入流。
- `OutputStream`/`Writer`: 所有输出流的基类，前者是字节输出流，后者是字符输出流。

![IO流](./pictures/IO流.png)



## JavaDoc

Structure of a Javadoc comment

A Javadoc comment is set off from code by standard multi-line comment tags `/*` and `*/`. The opening tag (called begin-comment delimiter), has an extra asterisk, as in `/**`.

1. The first paragraph is a description of the method documented.
2. Following the description are a varying number of descriptive tags, signifying:
   1. The parameters of the method (`@param`)
   2. What the method returns (`@return`)
   3. Any exceptions the method may throw (`@throws`)
   4. Other less-common tags such as `@see` (a "see also" tag)

| Tag  & Parameter                                                                | Usage                                                                                                                                                                                     | Applies to                              | Since |
| ------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------- | ----- |
| **@author** *John Smith*                                                        | Describes  an author.<br />描述作者                                                                                                                                                       | Class, Interface, Enum                  |       |
| {**@docRoot**}                                                                  | Represents the relative path to the generated document's root directory from any generated page.<br />表示从任何生成的页面到生成的文档根目录的相对路径                                    | Class, Interface, Enum, Field, Method   |       |
| **@version** *version*                                                          | Provides  version entry. Max one per Class or Interface.<br />版本条目,每个类或接口最多有一个                                                                                             | Module, Package, Class, Interface, Enum |       |
| **@since** *since-text*                                                         | Describes when this functionality has first existed.<br />描述这个功能块从何时有的                                                                                                        | Class, Interface, Enum, Field, Method   |       |
| **@see** *reference*                                                            | Provides  a link to other element of documentation.<br />提供链接到其他文档元素的链接                                                                                                     | Class, Interface, Enum, Field, Method   |       |
| **@param** *name description*                                                   | Describes  a method parameter.<br />描述一个参数                                                                                                                                          | Method                                  |       |
| **@return** *description*                                                       | Describes  the return value.<br />描述返回值                                                                                                                                              | Method                                  |       |
| **@exception** *classname description*<br />**@throws** *classname description* | Describes  an exception that may be thrown from this method.<br />描述该方法可能抛出的异常                                                                                                | Method                                  |       |
| **@deprecated** *description*                                                   | Describes  an outdated method.<br />描述一个过期的方法                                                                                                                                    | Class, Interface, Enum, Field, Method   |       |
| {**@inheritDoc**}                                                               | Copies  the description from the overridden method.<br />从被复写的原方法复制描述                                                                                                         | Overriding  Method                      | 1.4.0 |
| {**@link** *reference*}                                                         | Link  to other symbol.<br />链接到其他的引用                                                                                                                                              | Class, Interface, Enum, Field, Method   |       |
| {**@linkplain** *reference*}                                                    | Identical to {`@link`}, except the link's label is displayed in plain text than code font.<br />与 {`@link`} 相同，只是链接的标签以纯文本而不是代码字体显示                               | Class, Interface, Enum, Field, Method   |       |
| {**@value** *#STATIC_FIELD*}                                                    | Return  the value of a static field.<br />返回一个静态作用域的值                                                                                                                          | Static  Field                           | 1.4.0 |
| {**@code** *literal*}                                                           | Formats literal text in the code font. It is equivalent to <code>{@literal}</code>.<br />以代码字体格式化文字文本                                                                         | Class, Interface, Enum, Field, Method   | 1.5.0 |
| {**@literal** *literal*}                                                        | Denotes literal text. The enclosed text is interpreted as not containing HTML markup or nested javadoc tags.<br />表示文字文本, 所包含的文本被解释为不包含 HTML 标记或嵌套的 javadoc 标记 | Class, Interface, Enum, Field, Method   | 1.5.0 |
| {**@serial** *literal*}                                                         | Used in the doc comment for a default serializable field.<br />在文档注释中用于默认可序列化字段                                                                                           | Field                                   |       |
| {**@serialData** *literal*}                                                     | Documents the data written by the writeObject( ) or writeExternal( ) methods.<br />记录由 writeObject( ) 或 writeExternal( ) 方法写入的数据                                               | Field, Method                           |       |
| {**@serialField** *literal*}                                                    | Documents an ObjectStreamField component.<br />记录 ObjectStreamField 组件                                                                                                                | Field                                   |       |