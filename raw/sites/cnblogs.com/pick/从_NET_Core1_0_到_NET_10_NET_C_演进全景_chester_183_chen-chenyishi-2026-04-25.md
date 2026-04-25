# 从 .NET Core1.0 到 .NET 10：.NET + C# 演进全景 - chester&#183;chen

> Author: chenyishi
> Date: 2025-11-14
> Source: https://www.cnblogs.com/chenyishi/p/19219260

---

本文回顾微软 .NET 与 C# 语言从跨平台起步到统一平台、再到现代化性能优化的全过程。每个版本都配有简明 Demo 代码，便于开发者快速掌握特性变化与实践。 一、.NET Core 时代：跨平台的开端
1. .NET Core 1.x（C# 7.0） 发布时间：.NET Core 1.0 于 2016-06-27 发布。 意义：标志 .NET 生态迈向真正跨平台、开源。 C# 7.0 核心特性： Out 变量内联声明 元组 (tuple) 返回多个值 模式匹配 (pattern matching) Demo 代码： if (int.TryParse("123", out int number)) // Out 变量内联
{ Console.WriteLine(number);
} (string name, int age) GetPerson() => ("Alice", 30); // 元组
var person = GetPerson();
Console.WriteLine($"Name: {person.name}, Age: {person.age}"); object obj = "Hello";
if (obj is string str) // 模式匹配
{ Console.WriteLine(str.Length);
}
2. .NET Core 2.x（C# 7.1～7.3） 发布时间：.NET Core 2.0 于 2017-08-14 发布。 意义：性能大幅提升，支持 .NET Standard 2.0，库生态更加丰富。 C# 7.1～7.3 核心特性： async Main 方法 默认表达式 (default literal) 元组投影初始值设定项（tuple element name inference） Demo 代码： public static async Task Main(string[] args) // async Main
{ await Task.Delay(100); Console.WriteLine("Async Main done");
} int number = default; // 默认表达式
var tuple = (name: "Alice", age: 30); // 元组投影初始值
Console.WriteLine($"Name: {tuple.name}, Age: {tuple.age}");
3. .NET Core 3.x（C# 8.0） 发布时间：.NET Core 3.0 于 2019-09-23 发布。 意义：首次将 Windows 桌面（WPF/WinForms）纳入 .NET Core 支持，并引入高性能结构如 Span。 C# 8.0 核心特性： 可空引用类型 (nullable reference types) 异步流 (async IAsyncEnumerable) 模式和索引 (indices & ranges) using 声明简化 (using var) Demo 代码： string? nullableString = null; // 可空引用类型 async IAsyncEnumerable<int> GetAsyncNumbers()
{ for (int i = 0; i < 5; i++) { await Task.Delay(50); yield return i; }
} await foreach (var n in GetAsyncNumbers())
{ Console.WriteLine(n);
} using var reader = new StreamReader("file.txt"); // using 声明
int[] arr = {1, 2, 3, 4};
int last = arr[^1]; // 索引操作
Console.WriteLine(last);
二、统一平台时代：.NET 5 到 .NET 10
4. .NET 5（C# 9.0） 发布时间：.NET 5 于 2020-11-10 发布。 意义：标志 “.NET Framework” + “.NET Core” 向统一 .NET 平台合并。 C# 9.0 核心特性： 记录类型 (record) 顶级语句 (top-level statements) 模式匹配增强（关系模式、逻辑模式） Demo 代码： public record Person(string FirstName, string LastName); // 记录类型 // 顶级语句
Console.WriteLine("Hello, World from C# 9!"); // 模式匹配增强
Person person = new("Alice", "Smith");
if (person is { LastName: "Smith" })
{ Console.WriteLine("Found Smith");
}
5. .NET 6（C# 10.0） 发布时间：.NET 6 于 2021-11-08 发布。 意义：LTS（长期支持）版，推进统一平台愿景，性能与开发体验进一步优化。 C# 10.0 核心特性： 全局 using 指令 (global using) 文件范围的命名空间 (file-scoped namespace) 记录结构 (record struct) 常量插值字符串 (constant interpolated strings) Demo 代码： // GlobalUsings.cs
global using System;
global using System.Collections.Generic; // Program.cs
namespace MyApp; // 文件范围命名空间 public readonly record struct Point(int X, int Y); // 记录结构 const string name = "World";
const string greeting = $"Hello, {name}!"; // 常量插值字符串 Console.WriteLine(greeting);
6. .NET 7（C# 11.0） 发布时间：.NET 7 于 2022-11-08 发布。 意义：专注于性能提升、云原生支持、AOT(前向编译)改进。 C# 11.0 核心特性： 原始字符串字面量 (raw string literals) 列表模式 (list patterns) 必需成员 (required members) 泛型数学 (generic math) Demo 代码： string xml = """
<person> <name>Alice</name> <age>30</age>
</person>
"""; // 原始字符串字面量 public class Person
{ public required string FirstName { get; set; } public required string LastName { get; set; }
} // 泛型数学 简化示例
static T Add<T>(T x, T y) where T : System.Numerics.INumber<T> => x + y; Console.WriteLine(Add<int>(3, 4));
7. .NET 8（C# 12.0） 发布时间：.NET 8，于 2023-11 发布（实际 2023-11-14） 意义：LTS 版，原生 AOT 正式版、进一步性能优化。 C# 12.0 核心特性： 主构造函数 (primary constructors) 支持所有 class/struct。(Microsoft Learn) 集合表达式 (collection expressions) 和扩展初始化语法。
-（还包括别名任意类型(using alias any type)、inline 数组等） Demo 代码： // 主构造函数示例
public class Person(string name, int age) // C# 12 主构造函数
{ public string Name => name; public int Age => age;
} // 集合表达式示例
int[] array = [1, 2, 3];
List<int> list = [1, 2, 3]; // 使用 spread 运算符 ..（假设已支持）
int[] other = [4, 5];
int[] combined = [1, 2, ..other, 6];
8. .NET 9（C# 13.0） 发布时间：.NET 9 于 2024-11-12 发布。 意义：继续推进性能优化、智能化开发（AI 集成等） C# 13.0 核心特性（你原文提及）包括： params 集合增强（支持任意集合类型而不仅是数组） field 关键字 简化属性访问器中字段引用 ref struct 实现接口 部分属性和索引器增强 说明：经校验发现，关于这些具体特性的官方资料仍较少、属于预览或提案阶段。建议在博客中注明 “预览/提案” 状态。 Demo 代码（按你原文）： public void ProcessItems(params ReadOnlySpan<int> items) // params 集合增强
{ foreach (var item in items) { Console.WriteLine(item); }
} public class Example
{ private int _backing; public string Name { get; set => field = value ?? throw new ArgumentNullException(nameof(value)); // field 关键字 }
}
9. .NET 10（C# 14.0） 发布时间：.NET 10 目前为最新里程碑版本。 意义：进一步提升开发者生产力、性能表现。 C# 14.0 核心特性： 扩展成员 (extension members)：新增扩展属性、静态扩展成员、用户定义运算符等。(Microsoft Learn) 空条件赋值 (null-conditional assignment)：可以在左侧使用 ?. 进行赋值。 nameof 支持未绑定泛型 Lambda 参数修饰符简化 Demo 代码： // 扩展成员示例（C# 14）
public static class EnumerableExtensions
{ extension<TSource>(IEnumerable<TSource> source) // 扩展块 { // 扩展属性 public bool IsEmpty => !source.Any(); // 扩展方法 public IEnumerable<TSource> WhereEven(Func<TSource, bool> predicate) => source.Where(predicate); } extension<TSource>(IEnumerable<TSource>) // 静态扩展成员 { public static IEnumerable<TSource> Combine(IEnumerable<TSource> first, IEnumerable<TSource> second) => first.Concat(second); public static IEnumerable<TSource> Identity => Enumerable.Empty<TSource>(); public static IEnumerable<TSource> operator + (IEnumerable<TSource> left, IEnumerable<TSource> right) => left.Concat(right); }
} // 空条件赋值示例
Person? person = null;
person?.Name = "Alice"; // 只有当 person 不为 null 时才赋值 // nameof 支持未绑定泛型
string typeName = nameof(List<>); // "List" // Lambda 参数修饰简化（示例）
delegate bool TryParse<T>(string s, out T value);
TryParse<int> parse = (s, out value) => int.TryParse(s, out value);
三、核心演进趋势总结
通过从 .NET Core 1.0 到 .NET 10、从 C# 7.0 到 C# 14 的演进，几个核心趋势十分明显： 跨平台与统一化：从 Windows 专属的 .NET Framework，到真正跨平台的 .NET Core，再到统一平台 .NET。 性能持续优化：运行时、垃圾回收 (GC)、JIT/AOT、结构 (Span) 等不断强化。 开发体验简化：语言特性持续减少样板代码（boilerplate），如顶级语句、全局 using、主构造函数、扩展成员等。 现代化／云原生：容器支持、微服务、AOT、云端运行优化。 智能化与扩展能力：后期语言版本引入扩展成员、泛型数学、AI 集成等，提升 “智能应用” 构建能力。 四、版本选择建议 新项目：推荐使用最新的 LTS 版本（如 .NET 10）以获得最新特性与性能拨优。 现有项目迁移：建议先升级到最近的 LTS（如 .NET 6、.NET 8），然后再考虑迁移至 .NET 10。迁移前需考虑：第三方库支持、语言特性兼容性、开发工具版本（Visual Studio／VS Code）、API 弃用情况等。 谨慎预览特性：对于尚在预览或提案阶段的语言特性（如 C# 13 的某些特性），应慎重使用于生产环境，并注明“预览中”。