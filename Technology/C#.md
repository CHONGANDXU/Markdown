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