# C#语言开发规范

| 版本 | 时间       | 编辑人员 |
| ---- | ---------- | -------- |
| 1.1  | 2022.03.09 | tyx      |

[TOC]

## C# 编码约定

Version-220309

编码约定可实现以下目的：

-   它们为代码创建一致的外观，以确保读取器专注于内容而非布局。

    **附录有提供EditorConfig文件，在 Visual Studio 中启用C# 自动格式化来满足准则。**

-   它们使得读取器可以通过基于之前的经验进行的假设更快地理解代码。

-   它们便于复制、更改和维护代码。

-   它们展示 C# 最佳做法。

我们遵循的一般规则是“使用 Visual Studio 默认值”。

### 命名约定

编写 C# 代码时需要考虑几个命名约定。

-   <font color=red>使用英文命名标识符。</font>严禁使用拼音或者中文

-   不允许变量名、类名、属性名、方法名等与系统标识符重名。

-   不要在标识符中使用下划线。例外：可以在私有字段前加上下划线

    将 `_camelCase `用于内部和私有字段，并在可能的情况下使用`readonly`。 内部和私有实例字段以 `_ `为前缀，静态字段以 `s_` 前缀，线程静态字段以` t_ `前缀。 

-   不要在属性名称中使用包含父类名称。

-   不要将匈牙利符号用于标识符中的任何其他类型标识

-   在 Async 方法后面加上`Async`.

-   在创建包含事件信息的新类时使用后缀 EventArgs

-   使用“EventHandler”后缀命名事件处理程序（用作事件类型的委托）

-   务必在事件处理程序中使用名为 sender 和 e 的两个参数。sender 参数表示引发事件的对象。sender 参数通常是 object 类型，即使可以使用更具体的类型。

-   枚举类名带上Enum后缀，枚举成员名称需要全大写，单词间用下划线隔开。

    >   说明：枚举其实就是特殊的常量类，且构造方法被默认强制是私有。
    >   正例：枚举名字为ProcessStatusEnum的成员名称：SUCCESS / UNKNOWN_REASON

-   在创建包含异常信息的新类时使用后缀 Exception

-   对布尔标识符使用前缀 Any、Is、Have 或类似关键字

-   如果模块、接口、类、方法使用了设计模式，在命名时要体现出具体模式。

-   在不包括 `using `的示例中，使用命名空间限定。 如果你知道命名空间默认导入项目中，则不必完全限定来自该命名空间的名称。 如果限定名称太长，无法单行显示， 那么它们可在点 (.) 后中断，如下例所示。

    ```csharp
    var currentPerformanceCounterCategory = new System.Diagnostics.
        PerformanceCounterCategory();
    ```

-   <font color=red>例外:</font>是互操作代码，其中常量值应与您通过互操作调用的代码的名称和值完全匹配。

-   你不必更改使用 Visual Studio 设计器工具创建的对象的名称以使它们适合其他准则。

#### Pascal命名

-   命名 `class`、`record` 或 `struct` 时，使用 Pascal命名（“PascalCasing”）。

    ```csharp
    public class DataService
    {
    }
    ```
    
    ```csharp
    public record PhysicalAddress(
        string Street,
        string City,
        string StateOrProvince,
        string ZipCode);
    ```

    ```csharp
    public struct ValueCoordinate
    {
    }
    ```

-   命名 `interface` 时，使用 Pascal命名写并在名称前面加上前缀 `I`。 这可以清楚地向使用者表明这是 `interface`。名称应该是名词或形容词。

    ```csharp
    public interface IWorkerQueue
    {
    }
    ```

-   命名类型的 `public` 成员（例如字段、属性、事件、方法和本地函数）时，请使用 Pascal命名。

    ```csharp
    public class ExampleEvents
    {
        // A public field, these should be used sparingly
        public bool IsValid;
    
        // An init-only property
        public IWorkerQueue WorkerQueue { get; init; }
    
        // An event
        public event Action EventProcessing;
    
        // Method
        public void StartEventProcessing()
        {
            // Local function
            static int CountQueueItems() => WorkerQueue.Count;
            // ...
        }
    }
    ```

- 编写Address `record`时，对参数使用 pascal 大小写，因为它们是记录的公共属性。

    ```csharp
    public record PhysicalAddress(
        string Street,
        string City,
        string StateOrProvince,
        string ZipCode);
    ```
    
-   谨慎使用公共字段，使用时应遵循不带前缀的 PascalCasing。

#### Camel命名

-   命名 `private` 或 `internal` 字段时，使用驼峰式大小写（“camelCasing”），并且它们以 `_` 作为前缀。

    ```csharp
    public class DataService
    {
        private IWorkerQueue _workerQueue;
    }
    ```

    >    <font color="#00dd00" > 提示</font>
    >    在支持语句完成的 IDE 中编辑遵循这些命名约定的 C# 代码时，键入 `_` 将显示所有对象范围的成员。

-   使用为 `private` 或 `internal` 的`static` 字段时 请使用 `s_` 前缀，对于线程静态，请使用 `t_`。

    ```csharp
    public class DataService
    {
        private static IWorkerQueue s_workerQueue;

        [ThreadStatic]
        private static TimeSpan t_timeSpan;
    }
    ```

-   编写方法参数时，请使用驼峰式大小写。

    ```csharp
    public T SomeMethod<T>(int someNumber, bool isValid)
    {
    }
    ```

#### 常量命名

-   常量命名应该全部大写，单词间用下划线隔开，力求语义表达完整清楚，不要嫌名字长。

-   不允许任何魔法值（即未经预先定义的常量）直接出现在代码中。

-   浮点数类型的数值后缀统一为大写的`D`或`F`,long或Long赋值时类型统一使用`L`

-   不要使用一个常量类维护所有常量，要按常量功能进行归类，分开维护。

    >   大而全的常量类，杂乱无章，使用查找功能才能定位到要修改的常量，不利于理解，也不利于维护。

### 布局约定

好的布局利用格式设置来强调代码的结构并使代码更便于阅读。 Microsoft 示例和样本符合以下约定：

1.  我们使用 **Allman 风格**的大括号，每个大括号都从新行开始。 单行语句块可以不使用大括号，但该块必须在其自己的行上正确缩进，并且不能嵌套在使用大括号的其他语句块中（有关更多详细信息，请参见规则 18）。 一个例外是允许一个 `using` 语句嵌套在另一个 `using` 语句中，方法是从同一缩进级别的下一行开始，即使嵌套的 `using` 包含受控块。 

2.  应在文件顶部导入命名空间，在`namespace`声明*外部*，并应按字母顺序排序，`System.*`命名空间应放在所有其他命名空间之前。

3.  在类的顶部声明所有成员变量，静态/常量成员在最顶部

    一般来说，我们遵循以下顺序：

    -   `private const`
    -   `private static readonly`
    -   `private readonly`
    -   `private`
    -   `public properties`

    ```csharp
    public class Developer
    {
        private const string Prefix = "SWEMU";
        private static readonly string Seed = Cryptography.GenerateSeed(32);
    
        private readonly string _language;
        private int _age;
    
        public string Number { get; set; }
        public decimal Wage { get; set; }
    
        // Constructor
        public Developer()
        {
            // ...
        }
    }
    ```

4.  属性应出现在与其关联的字段、属性或方法上方的行上，并用换行符与成员分隔。多个属性应该用换行符分隔。这允许更轻松地添加和删除属性，并确保每个属性都易于搜索。

5.  尽量保持你的代码文件结构化。在方法定义与属性定义之间添加一个空白行。但是任何时候都要避免一个以上的空行。**段落式的思维**

6.  多个程序元素进行对等操作时， 操作符前后都要加空格。

7.  每行只写一条语句。每行只写一个声明。

8.  使用区域（`#region--#endregion`）把相似的内容放在一起划分代码块。使用region可以帮助以有效的方式组织和构建代码文件。

9.  单行字符数限制不超过120个，超出需要换行，换行时遵循如下原则:

    1). 第二行相对第一行缩进4个空格，从第三行开始，不再继续缩进。
    2). 运算符与下文一起换行。
    3). 方法调用的点符号与下文一起换行。
    4). 方法调用中的多个参数需要换行时，在逗号后进行。
    5). 在括号前不要换行

10.  方法参数在定义和传入时，多个参数逗号后面必须加空格。

11.  在进行类型强制转换时，右括号与强制转换值之间不需要任何空格隔开。

12.  避免虚假的空闲空格。 例如，避免 if (someVar == 0)···，其中的点标记虚假的空闲空格。 如果使用 Visual Studio 辅助检测，请考虑启用“查看空白 (Ctrl+R, Ctrl+W)”或“编辑 -> 高级 -> 查看空白”。 

13.  如果连续行未自动缩进，请将它们缩进一个制表符位（四个空格）。

14.  使用括号突出表达式中的子句，如下面的代码所示。

     ```c#
     if ((val1 > val2) && (val1 > val3))
     {
         // Take appropriate action.
     }
     ```

15.  使用默认的代码编辑器设置：智能缩进，使用4个空格缩进（禁用制表符）

16.  单个方法的总行数不超过80行。

17.  没有必要增加若干空格来使变量的赋值等号与上一行对应位置的等号对齐。
### 注释约定

-   如果类、属性、事件、方法或参数不能通过其名称清楚地进行自我解释，请对其进行注释。对方法和类使用“///”三斜线注释。

-   将注释放在单独的行上，禁止行尾注释。

-   以大写字母开始注释文本，以句点结束注释文本。

-   在注释分隔符 (//) 与注释文本之间插入一个空格，如下面的示例所示。

    ```csharp
    // The following declaration creates a query. It does not run
    // the query.
    ```

-   请勿在注释周围创建格式化的星号块。

-   所有的枚举类型字段必须要有注释，说明每个数据项的用途。

-   请确保所有公共成员都有必要的 XML 注释，从而提供有关其行为的适当说明。

-   一般情况下使用中文注释，专有名词与关键字保持英文原文即可。

-   代码修改的同时，注释也要进行相应的修改，尤其是参数、返回值、异常、核心逻辑等。

### 语言准则

以下各节介绍 C# 遵循以准备代码示例和样本的做法。

#### 不要省略访问修饰符

使用适当的访问修饰符声明所有标识符，而不是允许默认值。而且可访问性修饰符总是在最前面（例如 `public abstract` 而不是 `abstract public`）。

#### 避免使用缩写

存在例外，例如`Id`, `Ftp`, `Uri`, `Dto`...

#### 字符串数据类型

-   采用string.Empty来初始化。

-   使用*字符串内插*来连接短字符串，如下面的代码所示。

    ```csharp
    string displayName = $"{nameList[n].LastName}, {nameList[n].FirstName}";
    ```

-   若要在循环中追加字符串，尤其是在使用大量文本时，请使用 `StringBuilder`对象。

    ```csharp
    var phrase = "lalalalalalalalalalalalalalalalalalalalalalalalalalalalalala";
    var manyPhrases = new StringBuilder();
    for (var i = 0; i < 10000; i++)
    {
        manyPhrases.Append(phrase);
    }
    //Console.WriteLine("tra" + manyPhrases);
    ```

#### 隐式类型本地变量

-   当变量类型明显来自赋值的右侧时，或者当精度类型不重要时，请对本地变量进行[隐式类型化](https://docs.microsoft.com/zh-cn/dotnet/csharp/programming-guide/classes-and-structs/implicitly-typed-local-variables)。

    ```csharp
    var var1 = "This is clearly a string.";
    var var2 = 27;
    ```

-   当类型并非明显来自赋值的右侧时，请勿使用 [var](https://docs.microsoft.com/zh-cn/dotnet/csharp/language-reference/keywords/var)。 请勿假设类型明显来自方法名称。 如果变量类型为 `new` 运算符或显式强制转换，则将其视为明显来自方法名称。

    ```csharp
    int var3 = Convert.ToInt32(Console.ReadLine()); 
    int var4 = ExampleClass.ResultSoFar();
    ```

-   请勿依靠变量名称来指定变量的类型。 它可能不正确。 在以下示例中，变量名称 `inputInt` 会产生误导性。 它是字符串。

    ```csharp
    var inputInt = Console.ReadLine();
    Console.WriteLine(inputInt);
    ```

-   避免使用 `var` 来代替 `dynamic`。 如果想要进行运行时类型推理，请使用 `dynamic`。 有关详细信息，请参阅[使用类型 dynamic（C# 编程指南）](https://docs.microsoft.com/zh-cn/dotnet/csharp/programming-guide/types/using-type-dynamic)。

-   使用隐式类型化来确定 [`for`](https://docs.microsoft.com/zh-cn/dotnet/csharp/language-reference/statements/iteration-statements#the-for-statement) 循环中循环变量的类型。

    下面的示例在 `for` 语句中使用隐式类型化。
    
    ```csharp
    var phrase = "lalalalalalalalalalalalalalalalalalalalalalalalalalalalalala";
    var manyPhrases = new StringBuilder();
    for (var i = 0; i < 10000; i++)
    {
        manyPhrases.Append(phrase);
    }
    //Console.WriteLine("tra" + manyPhrases);
    ```

-   不要使用隐式类型化来确定 [`foreach`](https://docs.microsoft.com/zh-cn/dotnet/csharp/language-reference/statements/iteration-statements#the-foreach-statement) 循环中循环变量的类型。

    下面的示例在 `foreach` 语句中使用显式类型化。
    
    ```csharp
    foreach (char ch in laugh)
    {
        if (ch == 'h')
            Console.Write("H");
        else
            Console.Write(ch);
    }
    Console.WriteLine();
    ```
    
    >   <font color="#dd00dd">备注</font>
    >
    >   注意不要意外更改可迭代集合的元素类型。 例如，在 `foreach` 语句中从 [System.Linq.IQueryable](https://docs.microsoft.com/zh-cn/dotnet/api/system.linq.iqueryable) 切换到 [System.Collections.IEnumerable](https://docs.microsoft.com/zh-cn/dotnet/api/system.collections.ienumerable) 很容易，这会更改查询的执行。

#### 数据类型

-   对于类型引用和方法调用，应当使用语言预定义的类型(C#别名)而不是系统类型名称（例如`int, string, float` 而不是`Int32, String, Single` 等）。

    示例：`int.Parse` 而不是 `Int32.Parse`

-   通常，使用 `int` 而非无符号类型。 `int` 的使用在整个 C# 中都很常见，并且当你使用 `int` 时，更易于与其他库交互。

#### 数组

当在声明行上初始化数组时，请使用简洁的语法。 在以下示例中，请注意不能使用 `var` 替代 `string[]`。


```csharp
string[] vowels1 = { "a", "e", "i", "o", "u" };
```

如果使用显式实例化，则可以使用 `var`。


```csharp
var vowels2 = new string[] { "a", "e", "i", "o", "u" };
```

如果指定数组大小，只能一次初始化一个元素。


```csharp
var vowels3 = new string[5];
vowels3[0] = "a";
vowels3[1] = "e";
// And so on.
```

#### 委托

使用 `Func<>`和 `Action<>`，而不是定义委托类型。 在类中，定义委托方法。


```csharp
public static Action<string> ActionExample1 = x => Console.WriteLine($"x is: {x}");

public static Action<string, string> ActionExample2 = (x, y) => 
    Console.WriteLine($"x is: {x}, y is {y}");

public static Func<string, int> FuncExample1 = x => Convert.ToInt32(x);

public static Func<int, int, int> FuncExample2 = (x, y) => x + y;
```

使用 `Func<>` 或 `Action<>` 委托定义的签名来调用方法。


```csharp
ActionExample1("string for x");

ActionExample2("string for x", "string for y");

Console.WriteLine($"The value is {FuncExample1("1")}");

Console.WriteLine($"The sum is {FuncExample2(1, 2)}");
```

如果创建委托类型的实例，请使用简洁的语法。 在类中，定义委托类型和具有匹配签名的方法。


```csharp
public delegate void Del(string message);

public static void DelMethod(string str)
{
    Console.WriteLine("DelMethod argument: {0}", str);
}
```

创建委托类型的实例，然后调用该实例。 以下声明显示了紧缩的语法。


```csharp
Del exampleDel2 = DelMethod;
exampleDel2("Hey");
```

以下声明使用了完整的语法。


```csharp
Del exampleDel1 = new Del(DelMethod);
exampleDel1("Hey");
```

#### 异常处理中的`try-catch` 和 `using` 语句

-   对大多数异常处理使用 `try-catch`语句，一般要写到日志中。

    ```csharp
    static string GetValueFromArray(string[] array, int index)
    {
        try
        {
            return array[index];
        }
        catch (System.IndexOutOfRangeException ex)
        {
            Console.WriteLine("Index is out of range: {0}", index);
            throw;
        }
    }
    ```

-   非必要情况下，不在循环体内部进行`try-cache`操作

-   通过使用 C# `using`简化你的代码。 如果具有`try-finally`语句（该语句中 `finally`块的唯一代码是对 `Dispose`方法的调用），请使用 `using` 语句代替。

    在以下示例中，`try`-`finally` 语句仅在 `finally` 块中调用 `Dispose`。
    
    ```csharp
    Font font1 = new Font("Arial", 10.0f);
    try
    {
        byte charset = font1.GdiCharSet;
    }
    finally
    {
        if (font1 != null)
        {
            ((IDisposable)font1).Dispose();
        }
    }
    ```
    
    可以使用 `using` 语句执行相同的操作。
    
    ```csharp
    using (Font font2 = new Font("Arial", 10.0f))
    {
        byte charset2 = font2.GdiCharSet;
    }
    ```
    
    在 C# 8 及更高版本中，使用无需大括号的新的 语法：
    
    ```csharp
    using Font font3 = new Font("Arial", 10.0f);
    byte charset3 = font3.GdiCharSet;
    ```

#### `&&` 和 `||` 运算符

若要通过跳过不必要的比较来避免异常并提高性能，请在执行比较时使用 [`&&`](https://docs.microsoft.com/zh-cn/dotnet/csharp/language-reference/operators/boolean-logical-operators#conditional-logical-and-operator-)（而不是 [`&`](https://docs.microsoft.com/zh-cn/dotnet/csharp/language-reference/operators/boolean-logical-operators#logical-and-operator-)）和 [`||`](https://docs.microsoft.com/zh-cn/dotnet/csharp/language-reference/operators/boolean-logical-operators#conditional-logical-or-operator-)（而不是 [`|`](https://docs.microsoft.com/zh-cn/dotnet/csharp/language-reference/operators/boolean-logical-operators#logical-or-operator-)），如下面的示例所示。


```csharp
Console.Write("Enter a dividend: ");
int dividend = Convert.ToInt32(Console.ReadLine());

Console.Write("Enter a divisor: ");
int divisor = Convert.ToInt32(Console.ReadLine());

if ((divisor != 0) && (dividend / divisor > 0))
{
    Console.WriteLine("Quotient: {0}", dividend / divisor);
}
else
{
    Console.WriteLine("Attempted division by 0 ends up here.");
}
```

如果除数为 0，则 `if` 语句中的第二个子句将导致运行时错误。 但是，当第一个表达式为 false 时，&& 运算符将发生短路。 也就是说，它并不评估第二个表达式。 如果 `divisor` 为 0，则 & 运算符将同时计算这两个表达式，这会导致运行时错误。

#### `new` 运算符

-   使用对象实例化的简洁形式之一，如以下声明中所示。 第二个示例显示了从 C# 9 开始可用的语法。

    ```csharp
    var instance1 = new ExampleClass();
    ```

    ```csharp
    ExampleClass instance2 = new();
    ```
    前面的声明等效于下面的声明。
    ```csharp
    ExampleClass instance2 = new ExampleClass();
    ```


-   使用对象初始值设定项简化对象创建，如以下示例中所示。

    ```csharp
    var instance3 = new ExampleClass { Name = "Desktop", ID = 37414,
        Location = "Redmond", Age = 2.3 };
    ```

    下面的示例设置了与前面的示例相同的属性，但未使用初始值设定项。
    ```csharp
    var instance4 = new ExampleClass();
    instance4.Name = "Desktop";
    instance4.ID = 37414;
    instance4.Location = "Redmond";
    instance4.Age = 2.3;
    ```

#### 条件语句

-   分支语句不应该使用复杂长条件，应该将长条件封装成方法。

-   if / else / for / while / do语句中必须使用大括号。

-   在一个switch块内，每个case要么通过continue / break / return等来终止，要么注释说明程序将继续执行到哪一个case为止；在一个switch块内，都必须包含一个default语句并且放在最后，即使它什么代码也没有。

-   当switch括号内的变量类型为String并且此变量为外部参数时，必须先进行null判断。

-   在高并发场景中，避免使用“等于”判断作为中断或退出的条件。

-   表达异常的分支时，少用`if-else`方式，这种方式可以改写成：

    ```csharp
    if (condition) {
    ...
    return obj;
    }
    // 接着写else的业务逻辑代码;
    ```

#### 事件处理

如果你正在定义一个稍后不需要删除的事件处理程序，请使用 lambda 表达式。


```csharp
public Form2()
{
    this.Click += (s, e) =>
        {
            MessageBox.Show(
                ((MouseEventArgs)e).Location.ToString());
        };
}
```

Lambda 表达式缩短了以下传统定义。


```csharp
public Form1()
{
    this.Click += new EventHandler(Form1_Click);
}

void Form1_Click(object sender, EventArgs e)
{
    MessageBox.Show(((MouseEventArgs)e).Location.ToString());
}
```

#### 静态成员

使用类名调用 `static成员` ：ClassName.StaticMember。 这种做法通过明确静态访问使代码更易于阅读。 请勿使用派生类的名称来限定基类中定义的静态成员。 编译该代码时，代码可读性具有误导性，如果向派生类添加具有相同名称的静态成员，代码可能会被破坏。

#### LINQ 查询

-   对查询变量使用有意义的名称。 下面的示例为位于Seattle(西雅图)的客户使用 `seattleCustomers`。
    ```csharp
    var seattleCustomers = from customer in customers
                           where customer.City == "Seattle"
                           select customer.Name;
    ```

-   使用别名确保匿名类型的属性名称都使用 Pascal 大小写格式正确大写。
    ```csharp
    var localDistributors =
        from customer in customers
        join distributor in distributors on customer.City equals distributor.City
        select new { Customer = customer, Distributor = distributor };
    ```

-   如果结果中的属性名称模棱两可，请对属性重命名。 例如，如果你的查询返回客户名称和分销商 ID，而不是在结果中将它们保留为 `Name` 和 `ID`，请对它们进行重命名以明确 `Name` 是客户的名称，`ID` 是分销商的 ID。
    ```csharp
    var localDistributors2 =
        from customer in customers
        join distributor in distributors on customer.City equals distributor.City
        select new { CustomerName = customer.Name, DistributorID = distributor.ID };
    ```

-   在查询变量和范围变量的声明中使用隐式类型化。

    ```csharp
    var seattleCustomers = from customer in customers
                           where customer.City == "Seattle"
                           select customer.Name;
    ```

-   对齐 [`from`](https://docs.microsoft.com/zh-cn/dotnet/csharp/language-reference/keywords/from-clause) 子句下的查询子句，如上面的示例所示。

-   在其他查询子句前面使用 [`where`](https://docs.microsoft.com/zh-cn/dotnet/csharp/language-reference/keywords/where-clause) 子句，确保后面的查询子句作用于经过缩减和筛选的一组数据。

    ```csharp
    var seattleCustomers2 = from customer in customers
                            where customer.City == "Seattle"
                            orderby customer.Name
                            select customer;
    ```

-   使用多行 `from` 子句代替 [`join`](https://docs.microsoft.com/zh-cn/dotnet/csharp/language-reference/keywords/join-clause) 子句来访问内部集合。 例如，`Student` 对象的集合可能包含测验分数的集合。 当执行以下查询时，它返回高于 90 的分数，并返回得到该分数的学生的姓氏。

    ```csharp
    var scoreQuery = from student in students
                     from score in student.Scores
                     where score > 90
                     select new { Last = student.LastName, score };
    ```

####  ViewModel

-   每个域对象/实体都应该有自己的 ViewModel。
-   使用 DTO，每个实体/域对象应该拥有一个 DTO 类，其中名称可以以`DTO`结尾.
-   

#### 其他

-   除非需要派生，否则将所有内部和私有类型设为`static`或`sealed`。与任何实现细节一样，如果/当将来需要派生时，可以更改它们。
-   如果文件的样式碰巧与本准则不同（例如，私有成员被命名`m_member` 而不是`_member`），则该文件中的现有样式优先。
-   避免使用`this.`，除非绝对必要。
-   不要在其它表达式（尤其是条件表达式）中，插入赋值语句。
-   不要明确指定枚举类型或枚举值（位字段除外）
-   循环体中的语句要考量性能，以下操作尽量移至循环体外处理，如定义对象、变量、获取数据库连接，进行不必要的try-catch操作（这个try-catch是否可以移至循环体外）。
-   公开接口需要进行入参保护，尤其是批量操作的接口。
    -   调用频次低的方法。
    -   执行时间开销很大的方法。此情形中，参数校验时间几乎可以忽略不计，但如果因为参数错误导致中间执行回退，或者错误，那得不偿失。
    -   需要极高稳定性和可用性的方法。
    -   对外提供的开放接口，不管是RPC / API / HTTP接口。
    -   敏感权限入口。
-   尽可能和相关地使用`nameof(...)`而不是`"..."`。
-   在源代码中包含非 ASCII 字符时，使用 Unicode 转义序列 (\uXXXX) 而不是文字字符。文字的非 ASCII 字符偶尔会被工具或编辑器弄乱。

### 日志记录

​        日志记录是任何应用程序中的关键部分。当最终用户在应用程序中遇到错误或意外行为时，您希望有一个跟踪或减少一些指向错误的指针。使用堆栈跟踪记录异常在这方面非常有用，而对于一些复杂的操作，您可能决定记录额外的辅助信息以更好地理解程序流程。为了区分跟踪数据和错误数据，任何正规的日志框架会为每条消息分配一个级别，并允许您决定您实际想要存储的消息级别。

-   log4net

    您可以选择为不同的上下文（不同的类）创建不同的记录器，或者为整个应用程序创建一个记录器。在任何情况下，在运行单元测试时，您都不想将任何内容记录到磁盘或任何其他输出，因此建议注入日志接口而不是直接`Log`从代码中调用静态类。

    当使用每个类的上下文记录器时，您可以使用此方法：

    ```csharp
    class UserService
    {
        private readonly ILog _logger;
    
        public UserService(/* other required dependencies */, ILog logger = null)
        {
            _logger = logger ?? LogManager.GetLogger(typeof(UserService));
        }
    
        public void SomeMethod()
        {
            _logger.Warn("Some warning");
        }
    }
    ```

    这样，在运行时，依赖注入将不会注入记录器，并且将在构造函数中创建特定于上下文的记录器。在单元测试中，您可以传递一个模拟记录器，甚至可以验证某些记录行为。

### 安全编码准则


大多数应用程序代码只能使用 .NET 实现的基础结构。 在某些情况下，需要额外的应用程序特定的安全性，或通过扩展安全系统或通过使用全新临时方法构建。

如果在代码中使用 .NET 强制的权限和其他强制，则应建立屏障，以防止恶意代码访问不需要的信息或执行其他不需要的操作。 此外，必须在使用受信任代码的所有预期方案中平衡安全性和可用性。

本概述介绍代码可以用于处理安全系统的不同方式。

#### 保护资源访问

设计和编写代码时，尤其是在使用或调用来源未知的代码时，需要保护和限制该代码对资源的访问。 因此，请记住以下可确保代码安全的方法：

-   不使用代码访问安全性 (CAS)。
-   不使用部分信任的代码。
-   不要使用 [AllowPartiallyTrustedCaller](https://docs.microsoft.com/zh-cn/dotnet/api/system.security.allowpartiallytrustedcallersattribute) 特性 (APTCA) 。
-   不使用 .NET 远程处理。
-   不使用分布式组件对象模型 (DCOM)。
-   不使用二进制格式化程序。

不支持将代码访问安全性和 Security-Transparent 代码用作包含部分受信任代码的安全边界。 建议在未实施其他安全措施的情况下，不要加载和执行未知来源的代码。 其他安全措施包括：

-   虚拟化
-   AppContainers
-   操作系统 (OS) 用户和权限
-   Hyper-V 容器

#### 非特定于安全性的代码

非特定于安全性的代码不对任何安全系统进行显示处理。 它通过所接收的任何权限来运行。 虽然无法捕获与受保护操作（例如使用文件、网络等）关联的安全异常的应用程序 (如使用文件、网络等) 可能导致未经处理的异常，但非特定于安全性的代码仍利用 .NET 中的安全技术。

非特定于安全性的库具有你应了解的特殊性质。 假设你的库提供了使用文件或调用非托管代码的 API 元素。 如果你的代码没有相应的权限，则它不会如所述运行。 但是，即使代码具有权限，调用它的任何应用程序代码必须具有相同的权限才能正常运行。 如果调用代码没有正确的权限，则 [SecurityException](https://docs.microsoft.com/zh-cn/dotnet/api/system.security.securityexception) 将作为代码访问安全堆栈审核的结果显示。

#### 不是可重用组件的应用程序代码

如果您的代码是不由其他代码调用的应用程序的一部分，则安全性很简单，可能不需要特殊的编码。 但请记住，恶意代码可以调用你的代码。 代码访问安全性可能会阻止恶意代码访问资源，而此类代码仍然可以读取你的字段或可能包含敏感信息的属性的值。

此外，如果你的代码接受来自 Internet 或其他不可靠来源的用户输入，则务必要小心恶意输入。

#### 本机代码实现的托管包装

通常在这种情况下，某些有用功能是在你想要提供给托管代码的本机代码中实现的。 托管包装器可通过使用平台调用或 COM 互操作轻松写入。 但如果你这样做，包装器的调用方必须具有非托管代码权限才能成功。 在默认策略下，这意味着从 intranet 或 Internet 下载的代码将不能与包装一起使用。

最好仅向包装器代码提供这些权限，而不是向使用这些包装器的所有应用程序提供非托管代码权限。 如果基础功能没有公开任何资源，且实现同样也安全，则包装器只需断言其权限，这可使任何代码通过它进行调用。 当涉及资源时，安全编码应该与下一节中所述的库代码案例相同。 因为包装器可能对这些资源公开调用方，所以仔细验证本机代码的安全性是必要的，这是包装器的责任。

#### 用于公开受保护资源的库代码

以下方法是最强大的方法，因此，如果未正确地) 进行安全编码，则可能会造成潜在的危险 (：你的库作为其他代码访问某些不可用的资源的接口，就像 .NET 类强制执行所使用资源的权限一样。 只要公开资源，你的代码首先就必须要求相应资源的权限（也就是说，必须执行安全检查），然后通常断言其权限来执行实际的操作。

### WPF--XAML约定

-  使用 `X:Name` 而不是 `Name`

    虽然它们作用相同，但 `Name` 只能用于某些元素，其中 `x:Name` 将适用于任何元素。 此外，在查看元素的一长串属性时，以 `x: `开头的属性更容易发现。

-  将元素的第一个属性放在元素名称下方的行上

    ```xaml
    <!--反例-->
    <StackPanel DockPanel.Dock="Top" Orientation="Horizontal"
                Margin="3">
        <Button>
    ```

    更改它们以使第一个属性位于新行上，并将右尖括号也放在自己的行上，如下所示：

    ```xaml
    <StackPanel
        DockPanel.Dock="Top"
        Orientation="Horizontal"
        Margin="3"
        >
        <Button>
    ```

    这会将所有属性置于同一缩进级别。

-  例外：元素 `x:Name` 属性

    为了方便起见，这可以放在元素名称旁边：

    ```xaml
    <StackPanel x:Name="PanelName"
        DockPanel.Dock="Top"
        Orientation="Horizontal"
        Margin="3"
        >
        <Button>
    ```

-  如果元素没有内容，则自行关闭它

    在空元素上使用结束标签是浪费空间。 在这种情况下，请始终使用自闭合标签。

    ```xaml
    <!-- 正例 -->
    <Button x:Name="SomeButton"
        Text="Hello Sweet Mustard"
        />
    
    <!-- 反例 -->
    <Button x:Name="SomeButton"
        Text="Hello Sweet Mustard"
        ></Button>
    ```

-  带有类型指示的后缀 XAML

    元素的名称应始终在名称末尾包含有关元素类型的提示。

    ```xaml
    <!-- 正例 -->
    <Button x:Name="CheckoutButton"
        Text="Checkout"
        />
    
    <!-- 反例 -->
    <Button x:Name="Checkout"
        Text="Checkout"
        />
    ```

-  仅在隐藏代码或元素绑定中使用的名称节点

    ```xaml
    <!-- 如果在隐藏代码中引用了名称/键：: -->
    <StackPanel x:Name="ActionsPanel">
        <!-- ... -->
    </StackPanel>
    
    <!-- 如果隐藏代码中没有对节点的引用：: -->
    <StackPanel>
        <!-- ... -->
    </StackPanel>
    ```

-  User Controls与Templated Controls

    当需要 UI 元素时，您必须决定是使用*用户控件*（带有*ViewModel*的*视图*）还是*模板化控件*（UI 控件）。

    选择的一般规则：

    -   在使用视图模型和少量依赖属性实现应用程序特定视图时创建*用户控件。*这些视图应该位于*Views*目录中。
    -   在实现没有视图模型和/或直接绑定到 XAML 属性的大量依赖项属性的可重用 UI 控件时创建*模板化控件。*这些控件应位于*Controls*目录中。


### 附录

#### Allman风格

​        Allman 风格以Eric Allman命名。它有时也被称为*BSD* 风格，因为 Allman 为BSD Unix编写了许多实用程序（尽管这不应与不同的“BSD KNF 风格”混淆）。

​        这种风格将与控制语句关联的大括号放在下一行，缩进到与控制语句相同的级别。大括号内的语句缩进到下一级。

```c
while (x == y)
{
    something();
    somethingelse();
}

finalthing();
```

​        这种风格类似于Pascal语言和Transact-SQL使用的标准缩进，其中大括号相当于关键字`begin`和 `end`。

```pascal
(* Example Allman code indentation style in Pascal *)
procedure dosomething(x, y: Integer);
begin
    while x = y do
    begin
        something();
        somethingelse();
    end;
end;
```

​        这种风格的后果是，缩进的代码与包含语句清楚地分开，几乎所有的行都是**空格**，并且右大括号与左大括号在同一列中排列。有些人觉得这样可以很容易地找到匹配的牙套。阻塞样式还从关联的控制语句中描述代码块。注释掉或删除控制语句或代码块，或**代码重构**，都不太可能通过悬空或缺少大括号而引入语法错误。此外，它与外部功能块的大括号放置一致。

例如，以下在语法上仍然是正确的：

```c
// while (x == y)
{
    something();
    somethingelse();
}
```

就像这样：

```c
// for (int i=0; i < x; i++)
// while (x == y)
if (x == y)
{
    something();
    somethingelse();
}
```

即使这样，使用条件编译：

```c
    int c;
#ifdef HAS_GETCH
    while ((c = getch()) != EOF)
#else
    while ((c = getchar()) != EOF)
#endif
    {
        do_something(c);
    }
```

#### .editorconfig

```ini
# editorconfig.org

# top-most EditorConfig file
root = true

# Default settings:
# A newline ending every file
# Use 4 spaces as indentation
[*]
insert_final_newline = true
indent_style = space
indent_size = 4
trim_trailing_whitespace = true

# Generated code
[*{_AssemblyInfo.cs,.notsupported.cs,AsmOffsets.cs}]
generated_code = true

# C# files
[*.cs]
# New line preferences
csharp_new_line_before_open_brace = all
csharp_new_line_before_else = true
csharp_new_line_before_catch = true
csharp_new_line_before_finally = true
csharp_new_line_before_members_in_object_initializers = true
csharp_new_line_before_members_in_anonymous_types = true
csharp_new_line_between_query_expression_clauses = true

# Indentation preferences
csharp_indent_block_contents = true
csharp_indent_braces = false
csharp_indent_case_contents = true
csharp_indent_case_contents_when_block = true
csharp_indent_switch_labels = true
csharp_indent_labels = one_less_than_current

# Modifier preferences
csharp_preferred_modifier_order = public,private,protected,internal,static,extern,new,virtual,abstract,sealed,override,readonly,unsafe,volatile,async:suggestion

# avoid this. unless absolutely necessary
dotnet_style_qualification_for_field = false:suggestion
dotnet_style_qualification_for_property = false:suggestion
dotnet_style_qualification_for_method = false:suggestion
dotnet_style_qualification_for_event = false:suggestion

# Types: use keywords instead of BCL types, and permit var only when the type is clear
csharp_style_var_for_built_in_types = false:suggestion
csharp_style_var_when_type_is_apparent = false:none
csharp_style_var_elsewhere = false:suggestion
dotnet_style_predefined_type_for_locals_parameters_members = true:suggestion
dotnet_style_predefined_type_for_member_access = true:suggestion

# name all constant fields using PascalCase
dotnet_naming_rule.constant_fields_should_be_pascal_case.severity = suggestion
dotnet_naming_rule.constant_fields_should_be_pascal_case.symbols  = constant_fields
dotnet_naming_rule.constant_fields_should_be_pascal_case.style    = pascal_case_style
dotnet_naming_symbols.constant_fields.applicable_kinds   = field
dotnet_naming_symbols.constant_fields.required_modifiers = const
dotnet_naming_style.pascal_case_style.capitalization = pascal_case

# static fields should have s_ prefix
dotnet_naming_rule.static_fields_should_have_prefix.severity = suggestion
dotnet_naming_rule.static_fields_should_have_prefix.symbols  = static_fields
dotnet_naming_rule.static_fields_should_have_prefix.style    = static_prefix_style
dotnet_naming_symbols.static_fields.applicable_kinds   = field
dotnet_naming_symbols.static_fields.required_modifiers = static
dotnet_naming_symbols.static_fields.applicable_accessibilities = private, internal, private_protected
dotnet_naming_style.static_prefix_style.required_prefix = s_
dotnet_naming_style.static_prefix_style.capitalization = camel_case

# internal and private fields should be _camelCase
dotnet_naming_rule.camel_case_for_private_internal_fields.severity = suggestion
dotnet_naming_rule.camel_case_for_private_internal_fields.symbols  = private_internal_fields
dotnet_naming_rule.camel_case_for_private_internal_fields.style    = camel_case_underscore_style
dotnet_naming_symbols.private_internal_fields.applicable_kinds = field
dotnet_naming_symbols.private_internal_fields.applicable_accessibilities = private, internal
dotnet_naming_style.camel_case_underscore_style.required_prefix = _
dotnet_naming_style.camel_case_underscore_style.capitalization = camel_case

# Code style defaults
csharp_using_directive_placement = outside_namespace:suggestion
dotnet_sort_system_directives_first = true
csharp_prefer_braces = true:silent
csharp_preserve_single_line_blocks = true:none
csharp_preserve_single_line_statements = false:none
csharp_prefer_static_local_function = true:suggestion
csharp_prefer_simple_using_statement = false:none
csharp_style_prefer_switch_expression = true:suggestion
dotnet_style_readonly_field = true:suggestion

# Expression-level preferences
dotnet_style_object_initializer = true:suggestion
dotnet_style_collection_initializer = true:suggestion
dotnet_style_explicit_tuple_names = true:suggestion
dotnet_style_coalesce_expression = true:suggestion
dotnet_style_null_propagation = true:suggestion
dotnet_style_prefer_is_null_check_over_reference_equality_method = true:suggestion
dotnet_style_prefer_inferred_tuple_names = true:suggestion
dotnet_style_prefer_inferred_anonymous_type_member_names = true:suggestion
dotnet_style_prefer_auto_properties = true:suggestion
dotnet_style_prefer_conditional_expression_over_assignment = true:silent
dotnet_style_prefer_conditional_expression_over_return = true:silent
csharp_prefer_simple_default_expression = true:suggestion

# Expression-bodied members
csharp_style_expression_bodied_methods = true:silent
csharp_style_expression_bodied_constructors = true:silent
csharp_style_expression_bodied_operators = true:silent
csharp_style_expression_bodied_properties = true:silent
csharp_style_expression_bodied_indexers = true:silent
csharp_style_expression_bodied_accessors = true:silent
csharp_style_expression_bodied_lambdas = true:silent
csharp_style_expression_bodied_local_functions = true:silent

# Pattern matching
csharp_style_pattern_matching_over_is_with_cast_check = true:suggestion
csharp_style_pattern_matching_over_as_with_null_check = true:suggestion
csharp_style_inlined_variable_declaration = true:suggestion

# Null checking preferences
csharp_style_throw_expression = true:suggestion
csharp_style_conditional_delegate_call = true:suggestion

# Other features
csharp_style_prefer_index_operator = false:none
csharp_style_prefer_range_operator = false:none
csharp_style_pattern_local_over_anonymous_function = false:none

# Space preferences
csharp_space_after_cast = false
csharp_space_after_colon_in_inheritance_clause = true
csharp_space_after_comma = true
csharp_space_after_dot = false
csharp_space_after_keywords_in_control_flow_statements = true
csharp_space_after_semicolon_in_for_statement = true
csharp_space_around_binary_operators = before_and_after
csharp_space_around_declaration_statements = do_not_ignore
csharp_space_before_colon_in_inheritance_clause = true
csharp_space_before_comma = false
csharp_space_before_dot = false
csharp_space_before_open_square_brackets = false
csharp_space_before_semicolon_in_for_statement = false
csharp_space_between_empty_square_brackets = false
csharp_space_between_method_call_empty_parameter_list_parentheses = false
csharp_space_between_method_call_name_and_opening_parenthesis = false
csharp_space_between_method_call_parameter_list_parentheses = false
csharp_space_between_method_declaration_empty_parameter_list_parentheses = false
csharp_space_between_method_declaration_name_and_open_parenthesis = false
csharp_space_between_method_declaration_parameter_list_parentheses = false
csharp_space_between_parentheses = false
csharp_space_between_square_brackets = false

# License header
file_header_template = Licensed to the .NET Foundation under one or more agreements.\nThe .NET Foundation licenses this file to you under the MIT license.

# C++ Files
[*.{cpp,h,in}]
curly_bracket_next_line = true
indent_brace_style = Allman

# Xml project files
[*.{csproj,vbproj,vcxproj,vcxproj.filters,proj,nativeproj,locproj}]
indent_size = 2

[*.{csproj,vbproj,proj,nativeproj,locproj}]
charset = utf-8

# Xml build files
[*.builds]
indent_size = 2

# Xml files
[*.{xml,stylecop,resx,ruleset}]
indent_size = 2

# Xml config files
[*.{props,targets,config,nuspec}]
indent_size = 2

# YAML config files
[*.{yml,yaml}]
indent_size = 2

# Shell scripts
[*.sh]
end_of_line = lf
[*.{cmd,bat}]
end_of_line = crlf


```



#### 参考

[C#语言开发规范1.0]()

[C# 编码约定](https://docs.microsoft.com/zh-cn/dotnet/csharp/fundamentals/coding-style/coding-conventions)

[C# Coding Style](https://github.com/dotnet/runtime/blob/main/docs/coding-guidelines/coding-style.md)

[安全编码准则](https://docs.microsoft.com/zh-cn/dotnet/standard/security/secure-coding-guidelines)

[C# Coding Standards Best Practices](https://www.dofactory.com/csharp-coding-standards)

[C# Coding Standards and Naming Conventions](https://github.com/ktaranov/naming-convention/blob/master/C%23%20Coding%20Standards%20and%20Naming%20Conventions.md)

[C# at Google Style Guide](https://google.github.io/styleguide/csharp-style.html)

[C# Coding Conventions](https://code.sweetmustard.be/dotnet/coding-conventions/)

[Logging](https://code.sweetmustard.be/dotnet/logging/)

