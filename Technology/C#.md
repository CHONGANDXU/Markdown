带 `*` 号的是推荐使用。
- Console
- WPF(Windows Presentation Foundation)`*`
- Windows Forms(Old)
- ASP.NET Web Forms(Old)
- ASP.NET MVC(Model-View-Controller)`*`
  - 分离不同种类的代码
  - 结构清晰，易于维护
- WCF(Windows Communication Foundation)`*`
- Windows Store Application`*`
- Windows Phone Application`*`
- Cloud(Windows Azure)`*`
- WF(Workflow Foundation)

- 关键字
- 操作符
- 标识符
  - 什么是合法的标识符
  - 大小写规范
  - 命名规范
- 标点符号
- 文本（字面值）
  - 整数
    - 多种后缀
  - 实数
    - 多种后缀
  - 字符
  - 字符串
  - 布尔
  - 空（NULL）
- 注释与空白
  - 单行
  - 多行（块注释）

### 标识符
合法标识符
- 标识符不允许是关键字，如果非要用关键字就在前面加`@`符号
- 标识符必须以字符或下划线开头
  - 字符包括英文字符，也包括汉语、俄语等字符
- 开始字符的后面可以跟字符、数字、下划线

### 基本命名规范
- 变量名都用驼峰法 Camel
  - 首字母小写，后续单词首字母大写
  - 例子：`apple` `smallApple`
- 方法、类、命名空间都用帕斯卡 `Pascal`
  - 每个单词的首字母都大写
  - 例子：`Apple` `SmallApple`
- 方法名应该是动词或动词短语，例如 ~~Date~~ `GetDate`

### 初识类型、变量和方法
- 类型
  - 亦称为数据类型
- 变量是存放数据的地方。简称数据
  - 变量的声明
  - 变量的使用
- 方法是处理数据的逻辑，又称为“算法”
  - 方法的声明
  - 方法的调用
- 程序 = 数据 + 算法
  - 有了变量和方法就可以写有意义的程序

- Type 又名数据类型（Data Type）
  - A data type is a homogeneous collection of values, effectively presented, equipped with a set of operations which manipulate these values.
  - 是数据在内存中存储时的“型号”
  - 小内存容纳大尺寸数据会丢失精度、发生错误
  - 大内存容纳小尺寸数据会导致浪费
  - 编程语言的数据类型与数据的数据类型不完全相同
- 强类型语言与弱类型语言的比较
  - 强类型：编写程序时，程序中的数据受到数据类型的约束，即强类型编程语言
  - 弱类型：数据受类型约束不严格，或基本不受约束，即弱类型编程语言（如 JavaScript 动态类型）
  - C# 从 4.0 开始引入了 Dynamic，让它可以利用动态语言的一些特性，但 C# 依然是强类型编程语言

一个 C# 类型中所包含的信息有：
● 存储此类型变量所需的内存空间大小
● 此类型的值可表示的最大、最小值范围
● 此类型所包含的成员（如方法、属性、事件等）
● 此类型由何基类派生而来
● 程序运行的时候，此类型的变量被分配在内存的什么位置
  ○ Stack 简介
  ○ Stack overflow
  ○ Heap 简介
  ○ 使用 Performance Monitor 查看进程的堆内存使用量
  ○ 关于内存泄漏
● 此类型所允许的操作（运算）

对一个程序来说，静态指编辑期、编译期，动态指运行期。 
静态时装在硬盘里，动态时装在内存里。

```c#
static void Main(string[] args)
{
    Type myType = typeof(Form);
    Console.WriteLine(myType.BaseType.FullName + Environment.NewLine + myType.FullName);
    var pInfos = myType.GetProperties();
    foreach (var p in pInfos)
    {
        Console.WriteLine(p.Name);
    }
    var mInfos = myType.GetMethods();
    foreach (var m in mInfos)
    {
        Console.WriteLine(m.Name);
    }
}
```