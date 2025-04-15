# 📖 让自己适应C++，而非让C++适应自己

_Accustoming Yourself to C++_

不论你的编程背景是什么，**C++ 都可能让你觉得有点儿熟悉**。

它是一个威力强大的语言，带着众多特性，但在你可以驾驭这些威力并有效运用其特性之前，**你必须先习惯 C++ 的办事方式**。

👉 本书的目标便是帮助你做到这一点。

本章列出的就是其中**最基本的一些内容**。

---

## 📌 条款 01：C++其实是一个语言裁缝

_View C++ as a federation of languages._

---

### 📖 C++ 的起源

一开始，C++ 只是 C 加上一些面向对象特性。

当时它的名字是 **C with Classes**，清楚反映了它的血缘关系。

---

### 📖 C++ 的演变

随着语言逐渐成熟，C++：

- 变得更活跃、更无拘束
- 更大胆、更冒险
- 开始接受不同于 C with Classes 的各种观念、特性和编程策略

例如：

- **Exceptions（异常）** 改变了函数结构化的方式（见条款 29）
- **Templates（模板）** 带来了新的设计思考方式（见条款 41）
- **STL** 则定义了一个前所未见的伸展性做法
    

---

### 📖 C++ 是多重范型语言

今天的 C++ 是一个 **多重范型编程语言（multiparadigm programming language）**，它同时支持：

- 📌 过程形式（procedural）
- 📌 面向对象形式（object-oriented）
- 📌 函数形式（functional）
- 📌 泛型形式（generic）
- 📌 元编程形式（metaprogramming）

这种能力和弹性：

- 让 C++ 成为无可匹敌的工具
- 也可能引发迷惑：**所有“适当用法”似乎都有例外**

---

### 📖 最简单的理解方式

👉 **将 C++ 视为由相关语言组成的集合，而非单一语言。**

在每个“次语言（sublanguage）”中：

- 守则与通例倾向于**简单、直观、容易记住**
- 但当你从一个次语言切换到另一个，守则可能改变

---

## 📌 C++ 的四大次语言

为了理解 C++，你必须认识它的四个主要次语言：

---

### 1️⃣ C

说到底，C++ 仍以 C 为基础。

C++ 中的：

- 区块（blocks）
- 语句（statements）
- 预处理器（preprocessor）
- 内置数据类型（built-in data types）
- 数组（arrays）
- 指针（pointers）

👉 全都源自 C。

很多时候，C++ 对问题的解法就是“较高级的 C 解法”。

例如：

- 📄 条款 2：谈预处理器之外的替代方案
- 📄 条款 13：谈用对象管理资源

**但在 C++ 内的 C 成分工作时，仍受 C 的局限**

- 没有 templates
- 没有 exceptions
- 没有 overloading  
    ...

---

### 2️⃣ Object-Oriented C++

这部分正是 **C with Classes** 所诉求的：

- classes（包含构造函数和析构函数）
- 封装（encapsulation）
- 继承（inheritance）
- 多态（polymorphism）
- virtual 函数（动态绑定） ...

👉 是**面向对象设计古典守则**在 C++ 上的直接实现。

---

### 3️⃣ Template C++

这是 C++ 的 **泛型编程（generic programming）** 部分，  
也是大多数程序员**经验最少的部分**。

👉 Template 相关的考虑与设计已经渗透整个 C++。

良好编程守则中，“惟 template 适用”的条款并不罕见：

- 📄 条款 46：调用 template functions 时如何协助类型转换

由于 templates 的威力强大，甚至催生了新的编程范型：

- **Template Metaprogramming（TMP，模板元编程）**

📄 条款 48 提供了 TMP 的概述，但除非你是 template 激进团队的核心成员，大可不必太担心这些。TMP 的规则对 C++ 主流编程影响不大。

---

### 4️⃣ STL

STL 虽是个 **template 程序库**，但它非常特殊。

它对：

- 容器（containers）
- 迭代器（iterators）
- 算法（algorithms）
- 函数对象（function objects）

有极佳的协调和规约。  
**但 templates 和程序库也可以用其他方式建构**。

👉 STL 有自己独特的办事方式，  
**当你与 STL 一起工作时，必须遵守它的规约。**

---

## 📌 小结：四个语言，四套规约

记住这四个次语言。  
当你从一个次语言切换到另一个，  
如果高效编程守则要求你改变策略，**不要感到惊讶**。

例如：

- **对内置（C-like）类型**
    - `pass-by-value` 通常比 `pass-by-reference` 高效
- **在 Object-Oriented C++ 中**
    - 由于有用户自定义构造和析构函数，`pass-by-reference-to-const` 更好
- **在 Template C++ 中**
    - 因为你甚至不知道对象类型，更倾向 `pass-by-reference-to-const`
- **在 STL 中**
    - 迭代器和函数对象本质上是基于 C 指针
    - 旧式的 `pass-by-value` 规则又再次适用

（具体传参方式详见 📄 条款 20）

---

## ✅ 请记住

> C++ 高效编程守则**视状况而变化**，取决于你使用 C++ 的哪一部分。
