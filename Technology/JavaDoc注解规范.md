### Structure of a Javadoc comment

A Javadoc comment is set off from code by standard multi-line comment tags `/*` and `*/`. The opening tag (called begin-comment delimiter), has an extra asterisk, as in `/**`.

1. The first paragraph is a description of the method documented.
2. Following the description are a varying number of descriptive tags, signifying:
   1. The parameters of the method (`@param`)
   2. What the method returns (`@return`)
   3. Any exceptions the method may throw (`@throws`)
   4. Other less-common tags such as `@see` (a "see also" tag)

| Tag  & Parameter                                             | Usage                                                        | Applies to                              | Since |
| ------------------------------------------------------------ | ------------------------------------------------------------ | --------------------------------------- | ----- |
| **@author** *John Smith*                                     | Describes  an author.<br />描述作者                          | Class, Interface, Enum                  |       |
| {**@docRoot**}                                               | Represents the relative path to the generated document's root directory from any generated page.<br />表示从任何生成的页面到生成的文档根目录的相对路径 | Class, Interface, Enum, Field, Method   |       |
| **@version** *version*                                       | Provides  version entry. Max one per Class or Interface.<br />版本条目,每个类或接口最多有一个 | Module, Package, Class, Interface, Enum |       |
| **@since** *since-text*                                      | Describes when this functionality has first existed.<br />描述这个功能块从何时有的 | Class, Interface, Enum, Field, Method   |       |
| **@see** *reference*                                         | Provides  a link to other element of documentation.<br />提供链接到其他文档元素的链接 | Class, Interface, Enum, Field, Method   |       |
| **@param** *name description*                                | Describes  a method parameter.<br />描述一个参数             | Method                                  |       |
| **@return** *description*                                    | Describes  the return value.<br />描述返回值                 | Method                                  |       |
| **@exception** *classname description*<br />**@throws** *classname description* | Describes  an exception that may be thrown from this method.<br />描述该方法可能抛出的异常 | Method                                  |       |
| **@deprecated** *description*                                | Describes  an outdated method.<br />描述一个过期的方法       | Class, Interface, Enum, Field, Method   |       |
| {**@inheritDoc**}                                            | Copies  the description from the overridden method.<br />从被复写的原方法复制描述 | Overriding  Method                      | 1.4.0 |
| {**@link** *reference*}                                      | Link  to other symbol.<br />链接到其他的引用                 | Class, Interface, Enum, Field, Method   |       |
| {**@linkplain** *reference*}                                 | Identical to {`@link`}, except the link's label is displayed in plain text than code font.<br />与 {`@link`} 相同，只是链接的标签以纯文本而不是代码字体显示 | Class, Interface, Enum, Field, Method   |       |
| {**@value** *#STATIC_FIELD*}                                 | Return  the value of a static field.<br />返回一个静态作用域的值 | Static  Field                           | 1.4.0 |
| {**@code** *literal*}                                        | Formats literal text in the code font. It is equivalent to <code>{@literal}</code>.<br />以代码字体格式化文字文本 | Class, Interface, Enum, Field, Method   | 1.5.0 |
| {**@literal** *literal*}                                     | Denotes literal text. The enclosed text is interpreted as not containing HTML markup or nested javadoc tags.<br />表示文字文本, 所包含的文本被解释为不包含 HTML 标记或嵌套的 javadoc 标记 | Class, Interface, Enum, Field, Method   | 1.5.0 |
| {**@serial** *literal*}                                      | Used in the doc comment for a default serializable field.<br />在文档注释中用于默认可序列化字段 | Field                                   |       |
| {**@serialData** *literal*}                                  | Documents the data written by the writeObject( ) or writeExternal( ) methods.<br />记录由 writeObject( ) 或 writeExternal( ) 方法写入的数据 | Field, Method                           |       |
| {**@serialField** *literal*}                                 | Documents an ObjectStreamField component.<br />记录 ObjectStreamField 组件 | Field                                   |       |