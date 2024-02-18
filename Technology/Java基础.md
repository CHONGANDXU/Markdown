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