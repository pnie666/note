- 基础类型的Optional对象，以及为什么应该避免使用它们不知道你注意到了没有，与 Stream对象一样，Optional也提供了类似的基础类型——OptionalInt、OptionalLong以及OptionalDouble——所以代码清单10-6中的方法可以不返回Optional<Integer>，而是直接返回一个OptionalInt类型的对象。
- 第5章中，我们讨论过使用基础类型Stream的场景，尤其是如果Stream对象包含了大量元素，出于性能的考量，使用基础类型是不错的选择，但对Optional对象而言，这个理由就不成立了，因为Optional对象最多只包含一个值。
  
- 我们不推荐大家使用基础类型的Optional，因为基础类型的Optional不支持map、flatMap以及filter方法，而这些却是Optional类最有用的方法

<hr/>

# 你学到了以下的内容。
 null引用在历史上被引入到程序设计语言中，目的是为了表示变量值的缺失。
 Java 8中引入了一个新的类java.util.Optional<T>，对存在或缺失的变量值进行
建模。
 你可以使用静态工厂方法Optional.empty、Optional.of以及Optional.ofNullable创建Optional对象。
 Optional类支持多种方法，比如map、flatMap、filter，它们在概念上与Stream类
中对应的方法十分相似。
 使用Optional会迫使你更积极地解引用Optional对象，以应对变量值缺失的问题，最
终，你能更有效地防止代码中出现不期而至的空指针异常。
 使用Optional能帮助你设计更好的API，用户只需要阅读方法签名，就能了解该方法是
否接受一个Optional类型的值。
