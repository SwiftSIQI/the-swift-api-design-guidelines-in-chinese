# API Design Guidelines 中文版

## 原文地址

[英文原版在线版](https://swift.org/documentation/api-design-guidelines/)

## 更新记录

* 2019.06.13 完成初稿

## 贡献力量

如果想做出贡献的话，你可以：

* 参与翻译
* 帮忙校对，挑错别字、病句等等
* 提出修改建议
* 提出术语翻译建议

## 翻译建议

如果你有兴趣参与项目，请仔细阅读说明：

排版格式和流程说明：

* 翻译排版格式要求参考 SwiftGG [排版指南](https://github.com/SwiftGGTeam/translation/blob/master/SwiftGG%20排版指南.md)
* Pull Request 发起方式参考 SwiftGG [Pull Request 说明](https://github.com/SwiftGGTeam/translation/blob/master/%E7%BF%BB%E8%AF%91%E6%B5%81%E7%A8%8B%E6%A6%82%E8%BF%B0%E5%8F%8APR%E8%AF%B4%E6%98%8E.md#%E5%A6%82%E4%BD%95%E5%8F%91%E8%B5%B7-pull-request)

其他说明：

* 相关术语请严格按照术语表来翻译，如果有问题可以发 Issue 大家一起讨论
* 使用 Markdown 进行翻译，文件名必须使用英文
* 翻译后的文档请放到 source 文件夹下的对应章节中，然后 Pull Request 即可，我们会用 GitBook 编译成网页
* 有其他任何问题都欢迎发 Issue

## 目录（Table of Contents）

* [基本准则/Fundamentals](##基本准则/Fundamentals)
* [命名/Naming](##命名/Naming)
  * [明确的使用含义/Promote Clear Usage](###明确的使用含义/Promote-Clear-Usage)
  * [流畅的使用体验/Strive for Fluent Usage](###流畅的使用体验/Strive-for-Fluent-Usage)
  * [合理的使用术语/Use Terminology Well](###合理的使用术语/Use-Terminology-Well)
* [约定/Conventions](##约定/Conventions)
  * [基本约定/General Conventions](###基本约定/General-Conventions)
  * [形参/Parameters](###形参/Parameters)
  * [实参标签/Argument Labels](###实参标签/Argument-Labels)
* [特殊说明/Special Instructions](##特殊说明/Special-Instructions)

## 基本准则/Fundamentals

* **Clarity at the point of use** is your most important goal. Entities such as methods and properties are declared only once but used repeatedly. Design APIs to make those uses clear and concise. When evaluating a design, reading a declaration is seldom sufficient; always examine a use case to make sure it looks clear in context.

* **清晰的传达使用意图**是最主要的目标。方法和属性这样的实体虽然只被声明了一次，但却会被重复调用。因此在设计 API 时，应尽可能保证这些实体的使用简单明了。当评估某个设计优劣时，只阅读其声明是不够的；还需要将其放在使用场景下，并结合上下文来检查其含义是否做到清晰明了。

* **Clarity is more important than brevity.** Although Swift code can be compact, it is a non-goal to enable the smallest possible code with the fewest characters. Brevity in Swift code, where it occurs, is a side-effect of the strong type system and features that naturally reduce boilerplate.

* **传达清晰的意图比文字简洁更重要**。尽管 Swift 代码可以写的十分小巧紧凑，但用最少的字符来书写代码并非 Swift 的设计本意。如果 Swift 代码看起来非常简洁，那只是强类型系统和各种特性带来的副作用，无形中减少了模板代码（boilerplate）的使用。
  
  >译者注：boilerplate 是指 boilerplate code, [详情请点击这里](https://en.wikipedia.org/wiki/Boilerplate_code)。

* **Write a documentation comment** for every declaration. Insights gained by writing documentation can have a profound impact on your design, so don’t put it off.

* **将编写文档注释的工作**落实到每一个声明中。编写文档注释可以加深对代码理解，从而对你的设计产生深远的影响，所以不要偷懒。

  > If you are having trouble describing your API’s functionality in simple terms, **you may have designed the wrong API.**    
  >
  > 如果无法用简单的术语来描述你设计的 API ，那么**你很有可能在设计 API 上犯了错。**

  * **Use Swift’s dialect of Markdown.**

  * **使用 Swift 提供的 [特别版 Markdown 语法](https://developer.apple.com/library/archive/documentation/Xcode/Reference/xcode_markup_formatting_ref/)**。

  * **Begin with a summary** that describes the entity being declared. Often, an API can be completely understood from its declaration and its summary.

  * **开头的摘要**用来描述实体。通常情况下，阅读 API 的声明和摘要就可以完全理解其用途。

    ```swift
    /// Returns a "view" of `self` containing the same elements in
    /// reverse order.
    func reversed() -> ReverseCollection
    ```

    * **Focus on the summary**; it’s the most important part. Many excellent documentation comments consist of nothing more than a great summary.

    * **专注于摘要**；这是最重要的部分。高质量的摘要足以让文档注释成为优秀的典范。

    * **Use a single sentence fragment if possible**, ending with a period. Do not use a complete sentence.

    * **用一个语句片段进行描述**，并用句号结尾。不要写一个复杂的句子。

      > 译者注：sentence fragment 是指能表达明确含义但从语法上并不完整的句子，[详情请点击这里](https://en.wiktionary.org/wiki/sentence_fragment)。

    * **Describe what a function or method does and what it returns**, omitting null effects and Void returns:

    * **描述一个函数或方法做什么，以及会返回什么**，对于那些无返回值或者什么都不做的情况直接省略：

      ```swift
      /// Inserts `newHead` at the beginning of `self`.
      mutating func prepend(_ newHead: Int)

      /// Returns a `List` containing `head` followed by the elements
      /// of `self`.
      func prepending(_ head: Element) -> List

      /// Removes and returns the first element of `self` if non-empty;
      /// returns `nil` otherwise.
      mutating func popFirst() -> Element?
      ```

      Note: in rare cases like `popFirst` above, the summary is formed of multiple sentence fragments separated by semicolons.

      注意：类似上面 `popFirst` 这样极少数的情况，摘要应该用分号分割成多条语句。

    * **Describe what a subscript accesses:**

    * **描述下标访问的内容：**

      ```swift
      /// Accesses the `index`th element.
      subscript(index: Int) -> Element { get set }
      ```

    * **Describe what an initializer creates:**

    * **描述构造器创建的内容：**

      ```swift
      /// Creates an instance containing `n` repetitions of `x`.
      init(count n: Int, repeatedElement x: Element)
      ```

    * For all other declarations, **describe what the declared entity is.**

    * 对于其他 API 的声明，**需要描述清楚声明的实体到底是什么。**

      ```swift
      /// A collection that supports equally efficient insertion/removal
      /// at any position.
      struct List {

      /// The element at the beginning of `self`, or `nil` if self is
      /// empty.
      var first: Element?
      ...
      ```

  * **Optionally, continue** with one or more paragraphs and bullet items. Paragraphs are separated by blank lines and use complete sentences.

  * **如果需要，可以继续添加**一个或多个段落并有序排列这些条目。但是段落应该使用空行隔开，并使用完整的句子进行描述。

    ```swift
    /// Writes the textual representation of each    ← Summary
    /// element of `items` to the standard output.
    ///                                              ← Blank line
    /// The textual representation for each item `x` ← Additional discussion
    /// is generated by the expression `String(x)`.
    ///
    /// - Parameter separator: text to be printed    ⎫
    ///   between items.                             ⎟
    /// - Parameter terminator: text to be printed   ⎬ Parameters section
    ///   at the end.                                ⎟
    ///                                              ⎭
    /// - Note: To print without a trailing          ⎫
    ///   newline, pass `terminator: ""`             ⎟
    ///                                              ⎬ Symbol commands
    /// - SeeAlso: `CustomDebugStringConvertible`,   ⎟
    ///   `CustomStringConvertible`, `debugPrint`.   ⎭
    public func print(
      _ items: Any..., separator: String = " ", terminator: String = "\n")
    ```

    * **Use recognized symbol documentation markup elements to** add information beyond the summary, whenever appropriate.

    * **使用可识别的符号文档标记元素**，为注释添加摘要以外的任意信息。

    * **Know and use recognized bullet items with symbol command syntax**. Popular development tools such as Xcode give special treatment to bullet items that start with the following keywords:

    * **了解并使用符号命令语法做条目**。像 Xcode 等主流开发工具为这些条目提供了特殊的处理，其关键词如下：

      |[Attention](https://developer.apple.com/library/archive/documentation/Xcode/Reference/xcode_markup_formatting_ref/Attention.html)|[Author](https://developer.apple.com/library/archive/documentation/Xcode/Reference/xcode_markup_formatting_ref/Author.html)|[Authors](https://developer.apple.com/library/archive/documentation/Xcode/Reference/xcode_markup_formatting_ref/Authors.html)|[Bug](https://developer.apple.com/library/archive/documentation/Xcode/Reference/xcode_markup_formatting_ref/Bug.html)|
      |:-|:-|:-|:-|
      |[Complexity](https://developer.apple.com/library/archive/documentation/Xcode/Reference/xcode_markup_formatting_ref/Complexity.html)|[Copyright](https://developer.apple.com/library/archive/documentation/Xcode/Reference/xcode_markup_formatting_ref/Copyright.html)|[Date](https://developer.apple.com/library/archive/documentation/Xcode/Reference/xcode_markup_formatting_ref/Date.html)|[Experiment](https://developer.apple.com/library/archive/documentation/Xcode/Reference/xcode_markup_formatting_ref/Experiment.html)|
      |[Important](https://developer.apple.com/library/archive/documentation/Xcode/Reference/xcode_markup_formatting_ref/Important.html)|[Invariant](https://developer.apple.com/library/archive/documentation/Xcode/Reference/xcode_markup_formatting_ref/Invariant.html)|[Note](https://developer.apple.com/library/archive/documentation/Xcode/Reference/xcode_markup_formatting_ref/Note.html)|[Parameter](https://developer.apple.com/library/archive/documentation/Xcode/Reference/xcode_markup_formatting_ref/Parameter.html)|
      |[Parameters](https://developer.apple.com/library/archive/documentation/Xcode/Reference/xcode_markup_formatting_ref/Parameters.html)|[Postcondition](https://developer.apple.com/library/archive/documentation/Xcode/Reference/xcode_markup_formatting_ref/Postcondition.html)|[Precondition](https://developer.apple.com/library/archive/documentation/Xcode/Reference/xcode_markup_formatting_ref/Precondition.html)|[Remark](https://developer.apple.com/library/archive/documentation/Xcode/Reference/xcode_markup_formatting_ref/Remark.html)|
      |[Requires](https://developer.apple.com/library/archive/documentation/Xcode/Reference/xcode_markup_formatting_ref/Requires.html)|[Returns](https://developer.apple.com/library/archive/documentation/Xcode/Reference/xcode_markup_formatting_ref/Returns.html)|[SeeAlso](https://developer.apple.com/library/archive/documentation/Xcode/Reference/xcode_markup_formatting_ref/SeeAlso.html)|[Since](https://developer.apple.com/library/archive/documentation/Xcode/Reference/xcode_markup_formatting_ref/Since.html)|
      |[Throws](https://developer.apple.com/library/archive/documentation/Xcode/Reference/xcode_markup_formatting_ref/Throws.html)|[ToDo](https://developer.apple.com/library/archive/documentation/Xcode/Reference/xcode_markup_formatting_ref/Todo.html)|[Version](https://developer.apple.com/library/archive/documentation/Xcode/Reference/xcode_markup_formatting_ref/Version.html)|[Warning](https://developer.apple.com/library/archive/documentation/Xcode/Reference/xcode_markup_formatting_ref/Warning.html)|

## 命名/Naming

### 明确的使用含义/Promote Clear Usage

* **Include all the words needed to avoid ambiguity** for a person reading code where the name is used.

* **最大程度的避免歧义**，以免造成使用者的困惑。

  For example, consider a method that removes the element at a given position within a collection.

  例如：假设下面这个方法可以移除集合中指定位置的元素。

    ```swift
    👍👍👍
    extension List {
      public mutating func remove(at position: Index) -> Element
    }
    employees.remove(at: x)
    ```
  
  If we were to omit the word `at` from the method signature, it could imply to the reader that the method searches for and removes an element equal to `x`, rather than using `x` to indicate the position of the element to remove.
  
  如果我们省略方法名中的 `at`，它可能暗示读者该方法会搜索并删除集合中等于 `x` 的元素，而不是删除位置是 `x` 的元素。

    ```swift
    👎👎👎
    employees.remove(x) // unclear: are we removing x?
    ```

* **Omit needless words**. Every word in a name should convey salient information at the use site.

* **省略无用的单词**。每个单词都需要传达出相应的关键信息。

  More words may be needed to clarify intent or disambiguate meaning, but those that are redundant with information the reader already possesses should be omitted. In particular, omit words that merely repeat type information.

  更多的单词或许可以明确意图并消除歧义，但是那些众所周知的冗余信息是可以省略的。尤其是那些重复类型信息的词语。

    ```swift
    👎👎👎
    public mutating func removeElement(_ member: Element) -> Element?
    allViews.removeElement(cancelButton)
    ```

  In this case, the word Element adds nothing salient at the call site. This API would be better:

  在上面的代码中，`Element` 在调用时没有提供任何有效的信息，API 修改成下面这样会更好。

    ```swift
    👍👍👍
    public mutating func remove(_ member: Element) -> Element?
    allViews.remove(cancelButton) // clearer
    ```

  Occasionally, repeating type information is necessary to avoid ambiguity, but in general it is better to use a word that describes a parameter’s role rather than its type. See the next item for details.

  在个别情况下，重复类型信息对于消除歧义是有帮助的。但总的来说，最好能描述参数作用，而不是参数类型。详见下一规则。

* **Name variables, parameters, and associated types according to their roles**, rather than their type constraints.

* **根据变量、参数、关联类型的作用来命名**，而不是基于它们的类型。

  ```swift
  👎👎👎
  var string = "Hello"
  protocol ViewController {
    associatedtype ViewType : View
  }
  class ProductionLine {
    func restock(from widgetFactory: WidgetFactory)
  }
  ```

  Repurposing a type name in this way fails to optimize clarity and expressivity. Instead, strive to choose a name that expresses the entity’s role.

  这里需要再提醒一遍，单纯的重复类型名称对传递清晰的意图和提升表达性来说，帮助并不大。相反，你应该尽量选用那些表明实体作用的名字。

  ```swift
  👍👍👍
  var greeting = "Hello"
  protocol ViewController {
    associatedtype ContentView : View
  }
  class ProductionLine {
    func restock(from supplier: WidgetFactory)
  }
  ```

  If an associated type is so tightly bound to its protocol constraint that the protocol name is the role, avoid collision by appending Protocol to the protocol name:

  如果关联类型（associated type）和遵循的协议命名冲突，可以在协议名后面加上 `protocol`：

  ```swift
  protocol Sequence {
    associatedtype Iterator : IteratorProtocol
  }
  protocol IteratorProtocol { ... }
  ```

* **Compensate for weak type information** to clarify a parameter’s role.

* **为弱类型参数补充信息**以明确参数的作用。

  Especially when a parameter type is `NSObject`, `Any`, `AnyObject`, or a fundamental type such `Int` or `String`, type information and context at the point of use may not fully convey intent. In this example, the declaration may be clear, but the use site is vague.

  当参数类型是 `NSObject`、`Any`、`AnyObject` 或者是基础类型比如 `Int`、`String` 时，调用处的类型信息和上下文环境可能无法完全表明函数意图。在下面的例子中，它的声明看起来语义明确，但从调用者的角度来看，就显得不够清晰。

  ```swift
  👎👎👎
  func add(_ observer: NSObject, for keyPath: String)
  grid.add(self, for: graphics) // vague
  ```

  To restore clarity, **precede each weakly typed parameter with a noun describing its role:**

  为了清晰的传达 API 本身的意图，**可以在每个弱类型参数前加一个名词来描述它的作用。**

  ```swift
  👍👍👍
  func addObserver(_ observer: NSObject, forKeyPath path: String)
  grid.addObserver(self, forKeyPath: graphics) // clear
  ```

### 流畅的使用体验/Strive for Fluent Usage

* **Prefer method and function names that make use sites form grammatical English phrases.**
* **方法或者函数名最好能在调用处形成符合语法规范的英语短语。**

  ```swift
  👍👍👍
  x.insert(y, at: z)          “x, insert y at z”
  x.subViews(havingColor: y)  “x's subviews having color y”
  x.capitalizingNouns()       “x, capitalizing nouns”
  ```

  ```swift
  👎👎👎
  x.insert(y, position: z)
  x.subViews(color: y)
  x.nounCapitalize()
  ```

  It is acceptable for fluency to degrade after the first argument or two when those arguments are not central to the call’s meaning:

  为了使用起来更流畅，可以从第二个或者第三个参数开始降低命名要求，前提是这些参数不影响整个 API 的语义。

  ```swift
  AudioUnit.instantiate(
    with: description, 
    options: [.inProcess], completionHandler: stopProgressBar)
  ```

* **Begin names of factory methods with “make”**, e.g. `x.makeIterator()`.

* **工厂方法的命名要以 make 开头**，如：`x.makeIterator()`。

* The first argument to **initializer and factory methods calls** should not form a phrase starting with the base name, e.g. `x.makeWidget(cogCount: 47)`

* **构造器和 [工厂方法](https://en.wikipedia.org/wiki/Factory_method_pattern)** 的第一个参数命名不应该考虑方法名，应该独立命名，如：`x.makeWidget(cogCount: 47)`

  For example, the first arguments to these calls do not read as part of the same phrase as the base name:

  下面这些方法调用中，第一个参数并没有和方法名组成连续的短语：

  ```swift
  👍👍👍
  let foreground = Color(red: 32, green: 64, blue: 128)
  let newPart = factory.makeWidget(gears: 42, spindles: 14)
  let ref = Link(target: destination)
  ```

  In the following, the API author has tried to create grammatical continuity with the first argument.
  
  下面的例子中，API 作者试图将第一个参数名和方法名拼成连续的短语。

  ```swift
  👎👎👎
  let foreground = Color(havingRGBValuesRed: 32, green: 64, andBlue: 128)
  let newPart = factory.makeWidget(havingGearCount: 42, andSpindleCount: 14)
  let ref = Link(to: destination)
  ```

  In practice, this guideline along with those for [argument labels](https://swift.org/documentation/api-design-guidelines/#argument-labels) means the first argument will have a label unless the call is performing a [value preserving type conversion](https://swift.org/documentation/api-design-guidelines/#type-conversion).
  
  在实际使用中，本规则和 [实参标签](###实参标签/Argument-Labels) 的相关规则组合起来，意味着第一个参数一般都会有标签，除非执行的是 [值保留类型转换](#type-conversion) 操作。

  ```swift
  let rgbForeground = RGBColor(cmykForeground)
  ```

* **Name functions and methods according to their side-effects**

* **根据函数和方法的副作用进行命名**

  * Those without side-effects should read as noun phrases, e.g. `x.distance(to: y)`, `i.successor()`.

  * 没有副作用的方法和函数读起来应该像名词短语，例如，`x.distance(to: y)`，`i.successor()`。

  * Those with side-effects should read as imperative verb phrases, e.g., print(x), x.sort(), x.append(y).

  * 有副作用的方法和函数读起来应该像祈使动词，例如，`print(x)`，`x.sort()`，`x.append(y)`。

  * **Name Mutating/nonmutating method pairs** consistently. A mutating method will often have a nonmutating variant with similar semantics, but that returns a new value rather than updating an instance in-place.

  * **可变/不可变方法的命名要成对出现**。一个可变方法通常都有一个不可变方法与之对应，二者的语义相近，区别在于前者更新实例，而后者返回一个新值。

    * When the operation is **naturally described by a verb**, use the verb’s imperative for the mutating method and apply the “ed” or “ing” suffix to name its nonmutating counterpart.

    * 当一项操作**恰好能够被一个动词描述时**，使用动词原形为可变方法命名；使用动词的过去分词 (ed) 或现在分词 (ing) 为不可变方法命名。

      |Mutating|Nonmutating|
      |:-:|:-:|
      |x.sort()|z = x.sorted()|
      |x.append(y)|z = x.appending(y)|

      * Prefer to name the nonmutating variant using the verb’s past participle (usually appending “ed”):

      * 命名不可变方法，最好使用过去分词（通常是增加后缀 “ed”）：

        ```swift
        /// Reverses `self` in-place.
        mutating func reverse()

        /// Returns a reversed copy of `self`.
        func reversed() -> Self
        ...
        x.reverse()
        let y = x.reversed()
        ```

      * When adding “ed” is not grammatical because the verb has a direct object, name the nonmutating variant using the verb’s present participle, by appending “ing.”

      * 如果由于动词后面直接跟随一个对象，无法添加 “ed” 时，使用现在分词命名不可变方法，即后缀 “ing”。

        ```swift
        /// Strips all the newlines from `self`
        mutating func stripNewlines()

        /// Returns a copy of `self` with all the newlines stripped.
        func strippingNewlines() -> String
        ...
        s.stripNewlines()
        let oneLine = t.strippingNewlines()
        ```

    * When the operation is **naturally described by a noun**, use the noun for the nonmutating method and apply the “form” prefix to name its mutating counterpart.

    * 当一项操作**恰好能够被一个名词描述时**，使用名词本身为不可变方法命名；使用名词前加 “form” 的方式为可变方法命名。

      |Nonmutating|Mutating|
      |:-:|:-:|
      |x = y.union(z)|y.formUnion(z)|
      |j = c.successor(i)|c.formSuccessor(&i)|

* **Uses of Boolean methods and properties should read as assertions about the receiver** when the use is nonmutating, e.g. `x.isEmpty`, `line1.intersects(line2)`.

* **对于返回值是布尔类型的方法和属性，读起来应该像是对被调用对象的断言**，其使用场景是不可变方法。例如，`x.isEmpty`，`line1.intersects(line2)`。

* **Protocols that describe what something is should read as nouns** (e.g. `Collection`).

* **描述事物的协议，读起来应该像名词**（例如，`Collection`）。

* **Protocols that describe a capability should be named using the suffixes `able`, `ible`, or `ing`**(e.g. Equatable, ProgressReporting).

* **描述能力的协议，应该使用后缀 `able`，`ible` 或 `ing`**（例如，Equatable，ProgressReporting）。

* The names of other **types, properties, variables, and constants should read as nouns**.

* 其他**类型，属性，变量以及常量的名称，读起来应该像名词**。

### 合理的使用术语/Use Terminology Well

> Term of Art  
> noun - a word or phrase that has a precise, specialized meaning within a particular field or profession.  
>
> 术语的艺术  
> 名词——在某个领域或行业内，有着明确特殊含义的词或短语。

* **Avoid obscure terms** if a more common word conveys meaning just as well. Don’t say “epidermis” if “skin” will serve your purpose. Terms of art are an essential communication tool, but should only be used to capture crucial meaning that would otherwise be lost.

* **避免使用晦涩的术语**，特别是在有一个常见词汇能够表达同样含义时。例如，如果 ”skin“ 能够满足表述需求，就不要使用 ”epidermis“。术语是重要的交流工具，但应该仅在其他表述方式会丢失关键意义时，才使用这些比较生僻的术语。

* **Stick to the established meaning** if you do use a term of art.
* 在坚持使用术语的情况下，**应该紧扣其明确的含义**。

  The only reason to use a technical term rather than a more common word is that it precisely expresses something that would otherwise be ambiguous or unclear. Therefore, an API should use the term strictly in accordance with its accepted meaning.

  使用术语而非常见词汇的唯一原因就是其能够准确表述事物，否则含义便会模糊，甚至造成歧义。因此，API 应该严格按照既定含义使用术语。

  * **Don’t surprise an expert**: anyone already familiar with the term will be surprised and probably angered if we appear to have invented a new meaning for it.

  * **别吓着专家**：如果对某个术语十分熟悉的人发现 API 设计者为该术语发明了新的含义。他们可能会感到惊讶甚至愤怒。

  * **Don’t confuse a beginner**: anyone trying to learn the term is likely to do a web search and find its traditional meaning.

  * **别迷惑新手**：尝试学习术语的人一般都会通过网络搜索的方式查询术语的原始含义。

* **Avoid abbreviations**. Abbreviations, especially non-standard ones, are effectively terms-of-art, because understanding depends on correctly translating them into their non-abbreviated forms.

* 避免缩写。缩写，特别是非标准缩写，本质上是一个术语，因为对缩写的理解建立在正确将其翻译为全称的基础上。

  > The intended meaning for any abbreviation you use should be easily found by a web search.

  > API 中使用到的缩写，其含义必须能够在互联网上轻松找到。

* **Embrace precedent**. Don’t optimize terms for the total beginner at the expense of conformance to existing culture.

* **遵循先例**。如果现有术语已经能够完美表述一个含义，那么就不要为了迁就新手，打破这种先例。

  It is better to name a contiguous data structure `Array` than to use a simplified term such as `List`, even though a beginner might grasp of the meaning of `List` more easily. Arrays are fundamental in modern computing, so every programmer knows—or will soon learn—what an array is. Use a term that most programmers are familiar with, and their web searches and questions will be rewarded.

  例如，最好将一个连续的数据结构命名为 `Array`，而非更简单的 `List`，虽然对于新手来说，后者的含义更容易掌握。数组是现代计算机科学的基础数据结构，所以每个程序员都知道——或者很快就能理解——什么是数组。使用大多数程序员所熟悉的术语，这样，即便有问题，互联网和其他人也能够提供帮助。

  Within a particular programming domain, such as mathematics, a widely precedented term such as `sin(x)` is preferable to an explanatory phrase such as `verticalPositionOnUnitCircleAtOriginOfEndOfRadiusWithAngle(x)`. Note that in this case, precedent outweighs the guideline to avoid abbreviations: although the complete word is sine, “sin(x)” has been in common use among programmers for decades, and among mathematicians for centuries.
  
  在某些特定的编程领域，例如数学， 诸如 `sin(x)` 这样已经广为人们所接受的术语，要比诸如 `verticalPositionOnUnitCircleAtOriginOfEndOfRadiusWithAngle(x)` 这样解释性的命名好的多。注意，这里先例打破了避免缩写的规则：尽管单词的完整拼写是 `sine`，但 ”`sin(x)`“ 已经被程序员使用了数十年，在数学中更是数百年。

## 约定/Conventions

### 基本约定/General Conventions

* **Document the complexity of any computed property that is not O(1)**. People often assume that property access involves no significant computation, because they have stored properties as a mental model. Be sure to alert them when that assumption may be violated.

* **对于复杂度不是 O(1) 的计算型属性，要通过注释特别说明** 。人们总是认为属性访问不牵扯大量计算，因为在人类的心智模型里会潜意识的认为当前访问的只是一个存储型属性。当这个假设被打破时，有必要提醒他们。

  > mental model 是指心智模型，可以通俗的理解为人们的思考方式或者思维过程，[详情请点击这里](https://en.wikipedia.org/wiki/Mental_model)

* **Prefer methods and properties to free functions**. Free functions are used only in special cases:

* **优先选择方法或属性，而非全局函数**。后者只在下述情况中使用：

  > 译者注：free function 在这里翻译为全局函数，[详情请点击这里](https://en.wikipedia.org/wiki/Free_function)

  1. When there’s no obvious self:
  * 没有明显的self:

      ```swift
      min(x, y, z)
      ```

  2. When the function is an unconstrained generic:
  * 函数是不受限的范型函数：

      ```swift
      print(x)
      ```

  3. When function syntax is part of the established domain notation:
  * 在特定的领域中已经有约定俗成函数语法在：

      ```swift
      sin(x)
      ```

* **Follow case conventions.** Names of types and protocols are `UpperCamelCase`. Everything else is `lowerCamelCase`.

* **遵守大小写的惯例**。类型和协议名称使用以`大写字母开头的驼峰命名法`。其他名称使用以`小写字母开头的驼峰命名法`。

  [Acronyms and initialisms](https://en.wikipedia.org/wiki/Acronym) that commonly appear as all upper case in American English should be uniformly up- or down-cased according to case conventions:
  
  对于那些在美语中全部以大写的形式出现的 [首字母缩写](https://en.wikipedia.org/wiki/Acronym)，要根据大小写惯例统一大写或小写：

  ```swift
  var utf8Bytes: [UTF8.CodeUnit]
  var isRepresentableAsASCII = true
  var userSMTPServer: SecureSMTPServer
  ```

  Other acronyms should be treated as ordinary words:

  其他缩写作为普通单词对待：

  ```swift
  var radarDetector: RadarScanner
  var enjoysScubaDiving = true
  ```

* **Methods can share a base name** when they share the same basic meaning or when they operate in distinct domains.

* 当某些方法的含义基本一致，或者只是在不同范围内使用的同类型方法，那么它们可以**共享一个基础方法名**。

  For example, the following is encouraged, since the methods do essentially the same things:
  
  例如，下面的命名方式值得认可，因为这些方法本质上是在做同一件事：

  ```swift
  👍👍👍
  extension Shape {
    /// Returns `true` iff `other` is within the area of `self`.
    func contains(_ other: Point) -> Bool { ... }

    /// Returns `true` iff `other` is entirely within the area of `self`.
    func contains(_ other: Shape) -> Bool { ... }

    /// Returns `true` iff `other` is within the area of `self`.
    func contains(_ other: LineSegment) -> Bool { ... }
  }
  ```

  And since geometric types and collections are separate domains, this is also fine in the same program:
  
  由于几何类型和集合类型所处的范围不同，下面的命名方式也不错：

  ```swift
  👍👍👍
  extension Collection where Element : Equatable {
    /// Returns `true` iff `self` contains an element equal to
    /// `sought`.
    func contains(_ sought: Element) -> Bool { ... }
  }
  ```

  However, these `index` methods have different semantics, and should have been named differently:
  
  然而，下面的 `index` 方法含义各不相同，应区别命名：

  ```swift
  👎👎👎
  extension Database {
    /// Rebuilds the database's search index
    func index() { ... }

    /// Returns the `n`th row in the given table.
    func index(_ n: Int, inTable: TableID) -> TableRow { ... }
  }
  ```

  Lastly, avoid “overloading on return type” because it causes ambiguities in the presence of type inference.
  
  最后，避免“重载返回类型”，这样会导致类型推断系统产生歧义。

  > 译者注：overloading on return type 在这里翻译为重载返回类型，[详情请点击这里](https://stackoverflow.com/questions/442026/function-overloading-by-return-type)。


  ```swift
  👎👎👎
  extension Box {
    /// Returns the `Int` stored in `self`, if any, and
    /// `nil` otherwise.
    func value() -> Int? { ... }

    /// Returns the `String` stored in `self`, if any, and
    /// `nil` otherwise.
    func value() -> String? { ... }
  }
  ```

### 形参/Parameters

```swift
func move(from start: Point, to end: Point)
```

* **Choose parameter names to serve documentation**. Even though parameter names do not appear at a function or method’s point of use, they play an important explanatory role.

* **选择具有说明作用的形参名**。虽然形参名在函数或方法调用时并不出现，但它们扮演着重要的解释作用。

  Choose these names to make documentation easy to read. For example, these names make documentation read naturally:

  选择能够提升文档可读性的名称。下面的例子中，形参名使得文档读起来自然流畅：

    ```swift
    👍👍👍
    /// Return an `Array` containing the elements of `self`
    /// that satisfy `predicate`.
    func filter(_ predicate: (Element) -> Bool) -> [Generator.Element]

    /// Replace the given `subRange` of elements with `newElements`.
    mutating func replaceRange(_ subRange: Range, with newElements: [E])
    ```

  These, however, make the documentation awkward and ungrammatical:
  
  而下面的文档读起来很别扭，不符合文法：

    ```swift
    👎👎👎
    /// Return an `Array` containing the elements of `self`
    /// that satisfy `includedInResult`.
    func filter(_ includedInResult: (Element) -> Bool) -> [Generator.Element]


    /// Replace the range of elements indicated by `r` with
    /// the contents of `with`.
    mutating func replaceRange(_ r: Range, with: [E])
    ```

* Take advantage of defaulted parameters when it simplifies common uses. Any parameter with a single commonly-used value is a candidate for a default.

* 利用默认参数简化用例。如果参数有一个常用值，就可以为其提供一个默认参数。

  Default arguments improve readability by hiding irrelevant information. For example:

  通过隐藏无关信息并提供默认参数的方式，可以提升 API 可读性。例如：

  ```swift
  👎👎👎
  let order = lastName.compare(
    royalFamilyName, options: [], range: nil, locale: nil)
  ```

  can become the much simpler:

  加入默认参数后，代码变得更加简洁：

  ```swift
  👍👍👍
  let order = lastName.compare(royalFamilyName)
  ```

  Default arguments are generally preferable to the use of method families, because they impose a lower cognitive burden on anyone trying to understand the API.
  
  默认参数通常适用于一组类似的方法，这样可以降低理解 API 的认知负担。

  ```swift
  👍👍👍
  extension String {
    /// ...description...
    public func compare(
      _ other: String, options: CompareOptions = [],
      range: Range? = nil, locale: Locale? = nil
    ) -> Ordering
  }
  ```

  The above may not be simple, but it is much simpler than:
  
  上述方法可能看起来不怎么简单，但比下面这些强多了：

  ```swift
  👎👎👎
  extension String {
    /// ...description 1...
    public func compare(_ other: String) -> Ordering
    /// ...description 2...
    public func compare(_ other: String, options: CompareOptions) -> Ordering
    /// ...description 3...
    public func compare(
      _ other: String, options: CompareOptions, range: Range) -> Ordering
    /// ...description 4...
    public func compare(
      _ other: String, options: StringCompareOptions,
      range: Range, locale: Locale) -> Ordering
  }
  ```

  Every member of a method family needs to be separately documented and understood by users. To decide among them, a user needs to understand all of them, and occasional surprising relationships—for example, `foo(bar: nil)` and `foo()` aren’t always synonyms—make this a tedious process of ferreting out minor differences in mostly identical documentation. Using a single method with defaults provides a vastly superior programmer experience.

  每个方法都要分开注释；为了选择使用哪一个，用户必须全部理解，并搞清它们之间的关系。有时，这些关系让人感到诧异，例如 `foo(bar: nil)` 和 `foo()` 的作用并不总是相同——这种在文档中寻找细微区别的工作是让人厌恶的。利用默认参数，将这些类似的方法简化为一个方法，会极大提升用户体验。

* **Prefer to locate parameters with defaults toward the end** of the parameter list. Parameters without defaults are usually more essential to the semantics of a method, and provide a stable initial pattern of use where methods are invoked.

* **将具有默认参数的参数项放到方法最后**。从语义上来说，没有默认参数的参数项对于方法来说更为重要，并且这样做可以在调用时提供稳定的格式。

### 实参标签/Argument Labels

```swift
func move(from start: Point, to end: Point)
x.move(from: x, to: y)
```

* **Omit all labels when arguments can’t be usefully distinguished**, e.g. `min(number1, number2), zip(sequence1, sequence2).`

* **如果区分参数的意义不大，可以省略所有实参标签**。例如：`min(number1, number2)`, `zip(sequence1, sequence2)`。

* **In initializers that perform value preserving type conversions, omit the first argument label**, e.g. `Int64(someUInt32)`

<a name="type-conversion"></a>
* **如果构造函数进行的是值保留类型转换操作，则省略第一个实参标签**。例如：`Int64(someUint32)`。

  The first argument should always be the source of the conversion.
  
  第一个参数是要转换的内容。

  ```swift
  extension String {
    // Convert `x` into its textual representation in the given radix
    init(_ x: BigInt, radix: Int = 10)   ← Note the initial underscore
  }

  text = "The value is: "
  text += String(veryLargeNumber)
  text += " and in hexadecimal, it's"
  text += String(veryLargeNumber, radix: 16)
  ```

  In “narrowing” type conversions, though, a label that describes the narrowing is recommended.

  而对于“值省略类型转换”来说，最好使用第一个标签描述所省略的内容。

  > 译者注：此处将 narrowing type conversions 翻译为值省略类型转换，[详情请点击这里](https://docs.microsoft.com/en-us/dotnet/visual-basic/programming-guide/language-features/data-types/widening-and-narrowing-conversions)

  ```swift
  extension UInt32 {
    /// Creates an instance having the specified `value`.
    init(_ value: Int16)            ← Widening, so no label
    /// Creates an instance having the lowest 32 bits of `source`.
    init(truncating source: UInt64)
    /// Creates an instance having the nearest representable
    /// approximation of `valueToApproximate`.
    init(saturating valueToApproximate: UInt64)
  }
  ```

  > A value preserving type conversion is a [monomorphism]((https://en.wikipedia.org/wiki/Monomorphism)), i.e. every difference in the value of the source results in a difference in the value of the result. For example, conversion from Int8 to Int64 is value preserving because every distinct Int8 value is converted to a distinct Int64 value. Conversion in the other direction, however, cannot be value preserving: Int64 has more possible values than can be represented in an Int8. 
  >
  >值保留类型转换是 [单态](https://en.wikipedia.org/wiki/Monomorphism)，即一个值对应一个结果。例如，将一个 `Int8` 值转换为一个 `Int64` 值属于这种操作，因为不同的 `Int8` 值都对应不同的 `Int64` 值。反过来就不是：`Int64` 能表示的值要比 `Int8` 多得多。
  >
  > Note: the ability to retrieve the original value has no bearing on whether a conversion is value preserving.
  >  
  > 注意：能否追溯原始值，和是不是值保留类型转换没有联系。

* **When the first argument forms part of a `prepositional phrase`, give it an argument label**. The argument label should normally begin at the preposition, e.g. `x.removeBoxes(havingLength: 12)`.

* **如果第一个参数参与组成 [介词短语](https://en.wikipedia.org/wiki/Adpositional_phrase#Prepositional_phrases)，则需要使用实参标签**。实参标签一般起 [介词](https://en.wikipedia.org/wiki/Preposition_and_postposition) 的作用。例如，`x.removeBoxed(havingLength: 12)`。

  An exception arises when the first two arguments represent parts of a single abstraction.

  如果前两个或多个参数共同组成一个抽象概念，就不需要遵守上面的约定。

  ```swift
  👎👎👎
  a.move(toX: b, y: c)
  a.fade(fromRed: b, green: c, blue: d)
  ```

  In such cases, begin the argument label after the preposition, to keep the abstraction clear.

  这种情况下，将介词从参数标签中抽取并提前，会使该方法的语义更清晰。

  ```swift
  👍👍👍
  a.moveTo(x: b, y: c)
  a.fadeFrom(red: b, green: c, blue: d)
  ```

* **Otherwise, if the first argument forms part of a grammatical phrase, omit its label**, appending any preceding words to the base name, e.g. `x.addSubview(y)`

* **否则，如果第一个参数组成的是一个常规短语，则省略标签**，在基础名中补全短语。例如 `x.addSubView(y)`。

  This guideline implies that if the first argument doesn’t form part of a grammatical phrase, it should have a label.

  这条规则隐含的意思是，如果第一个参数不组成任何短语，应该给其加上标签。

  ```swift
  👍👍👍
  view.dismiss(animated: false)
  let text = words.split(maxSplits: 12)
  let studentsByName = students.sorted(isOrderedBefore: Student.namePrecedes)
  ```

  Note that it’s important that the phrase convey the correct meaning. The following would be grammatical but would express the wrong thing.
  
  注意，短语传达的含义要正确。下述短语的语法正确，但会产生歧义。

  ```swift
  👎👎👎
  view.dismiss(false)   Don't dismiss? Dismiss a Bool?
  words.split(12)       Split the number 12?
  ```

  Note also that arguments with default values can be omitted, and in that case do not form part of a grammatical phrase, so they should always have labels.

  另外，有默认值的参数可以省略。在这种情况下，这些参数不应该参与短语的组成，所以它们总是有标签。

* **Label all other arguments.**

* **其他参数都需要加上标签。**

## 特殊说明/Special Instructions

* **Label tuple members and name closure parameters where they appear in your API**.

* **如果 API 使用使用了闭包和元组，则为闭包参数和元组成员添加标签**。

  These names have explanatory power, can be referenced from documentation comments, and provide expressive access to tuple members.

  这些参数的标签不仅具有解释作用，还可以用来直接访问元组成员。相关的说明可以放在文档注释中。

  ```swift
  /// Ensure that we hold uniquely-referenced storage for at least
  /// `requestedCapacity` elements.
  ///
  /// If more storage is needed, `allocate` is called with
  /// `byteCount` equal to the number of maximally-aligned
  /// bytes to allocate.
  ///
  /// - Returns:
  ///   - reallocated: `true` iff a new block of memory
  ///     was allocated.
  ///   - capacityChanged: `true` iff `capacity` was updated.
  mutating func ensureUniqueStorage(
    minimumCapacity requestedCapacity: Int, 
    allocate: (_ byteCount: Int) -> UnsafePointer<Void>
  ) -> (reallocated: Bool, capacityChanged: Bool)
  ```

  Names used for closure parameters should be chosen like parameter names for top-level functions. Labels for closure arguments that appear at the call site are not supported.

  在命名闭包参数时，应当与顶层函数的标准一致。在调用的时候，不支持闭包参数里的参数标签。

* Take extra care with unconstrained polymorphism (e.g. `Any`, `AnyObject`, and unconstrained generic parameters) to avoid ambiguities in overload sets.

* 需要格外注意不受约束、具有多态的类型（例如，`Any`，`AnyObject` 及不受限的范型参数）在重载时产生的歧义。

  For example, consider this overload set:

  考虑如下一组重载方法：

  ```swift
  👎👎👎
  struct Array {
    /// Inserts `newElement` at `self.endIndex`.
    public mutating func append(_ newElement: Element)

    /// Inserts the contents of `newElements`, in order, at
    /// `self.endIndex`.
    public mutating func append(_ newElements: S)
      where S.Generator.Element == Element
  }
  ```

  These methods form a semantic family, and the argument types appear at first to be sharply distinct. However, when `Element` is `Any`, a single element can have the same type as a sequence of elements.

  这些方法从语义上构成一个方法族，参数的类型乍一看也有很大区别。但是，如果 `Element` 的类型是 `Any` 的话，那么这两个方法的参数类型就可能重复了。

  ```swift
  👎👎👎
  var values: [Any] = [1, "a"]
  values.append([2, 3, 4]) // [1, "a", [2, 3, 4]] or [1, "a", 2, 3, 4]?
  ```

  To eliminate the ambiguity, name the second overload more explicitly.

  为了消除歧义，重新命名第二个方法，赋予其更多含义。

  ```swift
  👍👍👍
  struct Array {
    /// Inserts `newElement` at `self.endIndex`.
    public mutating func append(_ newElement: Element)

    /// Inserts the contents of `newElements`, in order, at
    /// `self.endIndex`.
    public mutating func append(contentsOf newElements: S)
      where S.Generator.Element == Element
  }
  ```
  
  Notice how the new name better matches the documentation comment. In this case, the act of writing the documentation comment actually brought the issue to the API author’s attention.

  可以看到，新名字能更好的对应文档注释。这种情况下，写注释其实能让 API 设计者注意到潜在的问题。
