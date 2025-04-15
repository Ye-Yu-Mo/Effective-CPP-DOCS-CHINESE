# 📖 导读

学习一门编程语言的基础是一回事，**用这种语言设计和实现高效程序**又是另一回事。特别是 C++，它以**强大的功能和丰富的表达能力**而著称。

---

### 🎯 C++：既是利器，也是陷阱

如果用得当，C++ 可以成为你工作中的**得力助手**。  
通过**精心设计的类、函数和模板**，程序可以变得：

- 简单
- 直观
- 高效
- 少犯错

但如果没有良好的训练，C++ 也可能让你的代码：

- 难以理解
- 难以维护
- 难以扩展
- 性能低下
- 错误频发

---

### 📚 本书的目的

本书的目的是教你**如何有效地使用 C++**。

我假设你：

- 已经知道 C++ 是什么
- 并且有一些使用经验

这里会帮你学会编写：

- **易读**
- **易维护**
- **可移植**
- **可扩展**
- **高效**
- **行为符合预期**

的 C++ 程序。

---

### 📌 建议的分类

本书中的建议主要分两类：

1. **通用设计策略**
2. **具体语言特性技巧**

设计相关的讨论集中在「**两种做法里选哪种更好**」。

例如：

- 继承（inheritance） vs 模板（templates）
- public 继承 vs private 继承
- private 继承 vs 组合（composition）
- 成员函数 vs 非成员函数
- 传值（pass-by-value） vs 传引用（pass-by-reference）

这些决定非常重要，因为一个糟糕的决策可能**初期没事，后期爆雷**，而那个时候再修，**既麻烦又耗时，还代价高昂**。

---

### ⚠️ 程序细节不可忽视

即便你知道大方向，C++ 程序要写好也不容易。  
一些细节要是忽略了，程序就容易出现：

- 奇怪现象
- 难以定位的 bug

比如：

- 赋值操作符的正确返回值？
- 析构函数要不要 virtual？
- operator new 分配不到内存时怎么办？

本书会帮你**避开这些坑**。

---

### 📖 这本书不是……

1. **全面的 C++ 教科书**  
    而是整理了 **55 条写好 C++ 的建议**（称为“条款”）。
2. **C++ 入门教材**  
    假设你已经知道基础概念，或者知道去哪里查。
3. **平台相关技巧合集**  
    只讲 **标准 C++**，注重**移植性**。
4. **绝对真理的“C++福音书”**  
    没有万能公式，每条建议背后都有解释。软件开发复杂，很多时候要看具体情况。

---

### 📌 怎么使用这本书？

- 每条建议**相对独立**，可以从感兴趣的地方看起。
- 建议彼此有关联，看着看着会串起来。

---

### 📌 使用建议

这本书的最好用法是：

- **理解 C++ 的行为和原因**
- **知道如何利用这些行为做出优势设计**
- 不要盲目套用，但也别轻易违背，除非有充分理由

---

### ✅ 总结

这本书最重要的作用是：

> **帮你搞清楚 C++ 是怎么运作的、为什么这样运作，以及怎么利用这些特性，写出稳定、高效、易维护的程序。**

## 📖 C++ 术语（Terminology）

下面是每位程序员都应该了解的一份 C++ 词汇表。这些术语十分重要，必须确认彼此都同意它们的意义。

---

### 📌 声明式（declaration）

告诉编译器某个东西的**名称和类型**，但略去细节。  
例子：

```cpp
extern int x;                          // 对象声明式
std::size_t numDigits(int number);     // 函数声明式
class Widget;                          // 类声明式
template<typename T> class GraphNode;  // 模板声明式
```

- **对象**（object）：即便是内置类型如 `int`，我也称其为对象。
- **std::size_t**：位于命名空间 `std` 中，C89 程序库符号有可能存在于全局作用域或 `std` 内，取决于头文件。

说明：我在范例代码里会写明 `std::`，但文本里常省略，你需要知道 `size_t`、`vector`、`cout` 这类都是在 `std` 里。

> 📌 `size_t` 是个 typedef，表示无符号类型，通常用于计数字符个数、容器元素数量，`vector`、`deque` 和 `string` 的 `operator[]` 参数类型。

---

### 📌 签名式（signature）

函数的声明揭示其签名，也就是**参数和返回类型**。  
例如：

```cpp
std::size_t numDigits(int
```

C++ 官方定义中，**签名不包括返回类型**，但为了方便，本书将返回类型也视为签名的一部分。

---

### 📌 定义式（definition）

提供声明式所遗漏的细节。

- 对**对象**：编译器为其分配内存
- 对**函数/模板**：提供代码本体
- 对**类/模板类**：列出成员定义
    

例子：

```cpp
int x;                                // 对象定义式
std::size_t numDigits(int number) {   // 函数定义式
  std::size_t digitsSoFar = 1;
  while ((number /= 10) != 0) ++digitsSoFar;
  return digitsSoFar;
}
class Widget {
public:
  Widget();
  ~Widget();
};
template<typename T>
class GraphNode {
public:
  GraphNode();
  ~GraphNode();
};
```

---

### 📌 初始化（Initialization）

**给予对象初值**的过程。  
用户自定义类型由构造函数执行。

- **default 构造函数**：无参数或所有参数有默认值。

```cpp
class A {
public:
  A();        // default 构造函数
};

class B {
public:
  explicit B(int x = 0, bool b = true);  // default 构造函数
};

class C {
public:
  explicit C(int x);    // 不是 default 构造函数
};
```

- `explicit`：防止隐式类型转换，只允许显式类型转换。

例子：

```cpp
void doSomething(B bObject);

B b1;
doSomething(b1);         // OK
B b2(28);
doSomething(b2);         // OK
doSomething(28);         // 错误！int → B 没有隐式转换
doSomething(B(28));      // OK，显式转换
```

> ⚠️ 除非有理由，建议构造函数加 `explicit`。

---

### 📌 Copy 构造函数 和 Copy assignment 操作符（赋值重载）

**copy 构造函数**：以同型对象初始化自身  
**copy assignment 操作符**：将另一个同型对象的值赋给自身

例子：

```cpp
class Widget {
public:
  Widget();                               // default 构造函数
  Widget(const Widget& rhs);              // copy 构造函数
  Widget& operator=(const Widget& rhs);   // copy assignment
};

Widget w1;              // default 构造
Widget w2(w1);          // copy 构造
w1 = w2;                // copy assignment
```

**注意**：

```cpp
Widget w3 = w2;   // 也是调用 copy 构造函数
```

- 新对象定义 → 构造
- 非新对象赋值 → assignment

**pass-by-value** 会调用 copy 构造。

例子：

```cpp
bool hasAcceptableQuality(Widget w);
Widget aWidget;
if (hasAcceptableQuality(aWidget)) { ... }
// aWidget 会被复制给 w
```

> ⚠️ 传值传递用户自定义类型通常是坏主意，推荐 pass-by-reference-to-const。

---

### 📌 STL（Standard Template Library）

C++ 标准库一部分，提供：

- **容器**：`vector`, `list`, `set`, `map`
- **迭代器**：`vector<int>::iterator`
- **算法**：`for_each`, `find`, `sort`
- **函数对象**（function objects）：重载 `operator()` 的 class

如果你还不熟，建议备一本参考书，因为 STL 超实用。

---

### 📌 Undefined Behavior（未定义行为）

某些 C++ 构件的行为未定义，运行期行为不可预期，甚至不稳定。

例子：

```cpp
int* p = 0;
std::cout << *p;   // 解引用 null 指针

char name[] = "Darla";
char c = name[10]; // 越界访问
```

> ⚠️ 经验丰富的 C++ 程序员都会极力避免 undefined behavior。因为他有可能给你去楼下买一份麦当劳（bushi）

---

### 📌 接口（interface）

虽然 C++ 没有 Java/.NET 中的 `interface` 语言元素，但一般指：

- 函数签名
- class 的 public / protected / private 接口
- template 类型参数要求的表达式合法性

**客户（client）**：使用你代码的人或代码。  
比如函数的客户是调用它的部分。  
**好接口让客户少踩坑**，你自己迟早也会成为自己代码的客户。

---

### 📌 术语缩写

- `ctor` ：构造函数（constructor）
- `dtor` ：析构函数（destructor）

---

### 📌 补充说明

- 本书内容有时不区分 **functions 与 function templates**，以及 **classes 与 class templates**，除非必要。

## 📖 命名习惯（Naming Conventions）

我尝试挑选**有意义的名称**用于 `objects`、`classes`、`functions`、`templates` 等等身上，但某些隐藏于名称背后的意义可能不是那么显而易见。比如我最喜爱的两个参数名称：

- `lhs`：_left-hand side（左手端）_
- `rhs`：_right-hand side（右手端）

我常常将它们作为**二元操作符（binary operators）函数**，例如 `operator==` 和 `operator*` 的参数名称。

---

### 📌 示例

假设 `a` 和 `b` 表示两个有理数对象（`Rational` 对象），如果该对象可被一个 **non-member operator*** 函数执行乘法（如条款 24 所言），那么下面表达式：

```cpp
a * b
```

等价于以下的函数调用：

```cpp
operator*(a, b)
```

在条款 24 中，我声明此一 `operator*` 如下：

```cpp
const Rational operator*(const Rational& lhs, const Rational& rhs);
```

如你所见：

- **左操作数** `a` → 函数内的 `lhs`
- **右操作数** `b` → 函数内的 `rhs`

---

### 📌 成员函数中的命名

对于成员函数，左侧实参由 `this` 指针表现出来，所以有时我只使用参数名称 `rhs`。你可能已经在第 5 页的若干 `Widget` 成员函数声明中注意到了这一点。

---

### 📦 Widget 命名说明

对了，我经常以 `Widget` class 示例，`Widget` 并不代表任何东西，它只是当我需要一个示范用的 class 名称时偶尔采用的名称，它和 **GUI toolkits 的 widgets 完全无关**。

---

### 📌 指针命名习惯

我常将“**指向一个 T 型对象的指针**”命名为 `pT`，意思是 _pointer to T_。例如：

```cpp
Widget* pw;       // pw = "ptr to Widget"
class Airplane;
Airplane* pa;     // pa = "ptr to Airplane"
class GameCharacter;
GameCharacter* pgc;  // pgc = "ptr to GameCharacter"
```

---

### 📌 引用命名习惯

对于 references，我使用类似习惯：

- `rw` → _reference to Widget_
- `ra` → _reference to Airplane_

---

### 📌 成员函数命名习惯

当我讨论成员函数时，偶尔会以 `mf` 作为名称。

## 📖 关于线程（Threading Consideration）

作为一门语言，**C++ 对线程（threads）没有任何概念** —— 事实上，它对任何并发（concurrency）相关的事物都没有概念。  
同样，**C++ 标准程序库**也没有。

---

### 📌 C++ 与多线程的时代背景

当 C++ 初次受到全世界关注时，**多线程程序（multithreaded programs）还不存在**。

但现在它们已经成为现实。

---

### 📌 本书的处理方式

本书的焦点放在**标准、可移植的 C++**，但我不能忽略一个事实：

> **线程安全性（thread safety）** 是许多程序员面对的重要议题。

我对 “**标准 C++ 和真实世界之间这个缺口**” 的处理方式是：

- 如果我所检验的 C++ 构件在多线程环境中**有可能引发问题**，就会特别指出来。
- 这并不能构成一本 C++ 多线程编程专著，但能让本书**即便大量限制自身处于单线程背景下，仍承认多线程的存在**，并指出：
    - **有线程概念的程序员**在评估本书提供的建议时，需要特别小心留意的地方。
---

### 📌 给不同读者的建议

- 如果你**不熟悉多线程**，或你的程序**无需考虑多线程问题**，可以**忽略本书中与线程相关的讨论**。
- 但如果你正在编写一个**与线程有关的应用程序或程序库**，请记住：
    - 本书的注释，或许会比一般“以 C++ 解决问题时需注意……”的讨论多出一些些。
## 📖 TR1 和 Boost

你会发现，本书中**处处提到 TR1 和 Boost**。

它们各自有一个专门的条款进行详细说明：

- 📄 条款 54：TR1
- 📄 条款 55：Boost
    

不过有点遗憾，这两部分被安排在全书末尾（放在那里是因为**那样的结构安排最合理**。真的，我试过其他许多摆法😅）。

---

### 📌 如果你想先了解

- 👉 你可以**现在直接翻到后面阅读**它们。
- 👉 或者如果你喜欢**按顺序阅读**，也没关系。下面这段**实施摘要**能帮你提前搞清楚它们是什么，助你“飞渡难关”。

---

### 📌 TR1 概览

**TR1**（_Technical Report 1_）是一份规范，描述了一些准备加入 C++ 标准程序库的新功能。

这些新功能主要以：

- **class templates**
- **function templates**

的形式出现，针对的内容包括：

- `hash tables`
- `reference-counting smart pointers`
- `regular expressions`
- 以及更多内容。

📌 **命名空间**

- 所有 TR1 组件都放在命名空间 `tr1` 中，后者嵌套在命名空间 `std` 内。

---

### 📌 Boost 概览

**Boost** 是：

- 一个组织
- 也是一个网站 👉 [http://boost.org](http://boost.org/)

它提供：

- **可移植**
- **同行复审**
- **源码开放**

的 C++ 程序库。

📌 **TR1 与 Boost 的关系**

- 大多数 **TR1 功能** 都是以 **Boost 的成果** 为基础。
- 在编译器厂商把 TR1 功能整合进自家 C++ 标准库之前，开发者若要寻找 TR1 实现，**Boost 网站**通常是首选之地。

📌 **Boost 的附加价值**

- Boost 提供的内容**远多于 TR1**，无论你是否关注 TR1，都很值得去了解 Boost。
