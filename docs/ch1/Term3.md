## 📌 条款 03：尽可能使用 `const`

### 🎯 为什么用 `const`？

- **表明意图**：明确告诉编译器、自己和其他程序员，“这东西不会被改动”。
- **防止意外修改**：编译器帮你守住不该动的东西。
- **提升代码可读性和维护性**：一眼看出哪些地方会改动对象，哪些地方绝对安全。
- **提高接口健壮性**：比如防止运算结果被赋值覆盖，防止函数里把参数无意间改了。

---

### 📌 `const` 的用法

#### 👉 普通变量 / 对象

```cpp
const int a = 10;
```

#### 👉 指针相关的 const（关键点！）

```cpp
const char* p;        // 指向 const 数据的指针（不能通过 p 改变 *p）
char* const p;        // const 指针，指向的数据可改，但不能改 p 的指向
const char* const p;  // 都 const，指针和指向的内容都不能改
```

#### 👉 函数参数和返回值

**参数加 const**

```cpp
void print(const std::string& s);
```

- 防止参数在函数内被改动
- 避免不必要的拷贝，提高性能

**返回值加 const**

```cpp
const Rational operator*(const Rational& lhs, const Rational& rhs);
```

- 防止结果对象被赋值  
    👉 `(a * b) = c;` 这种写法就会编译不过。

---

### 📌 const 成员函数

**作用**：保证这个成员函数**不会修改对象的成员变量**。

**语法**

```cpp
class Widget {
public:
    int getValue() const;
};
```

- 只能调用其他 const 成员函数
- 不能修改非 mutable 成员变量

---

### 📌 const 和重载

你可以根据 this 指针的 const 性，重载成员函数：

```cpp
class TextBlock {
public:
    const char& operator[](size_t pos) const;
    char& operator[](size_t pos);
};
```

- const 版本用于 const 对象
- 非 const 版本用于非 const 对象

---

### 📌 STL 迭代器和 const

- `const_iterator`：迭代器所指内容不能改
- `const iterator`：迭代器本身不能改，但可以改内容

```cpp
std::vector<int>::const_iterator cit = vec.begin();
```

---

### 📌 bitwise const vs. logical const

这里我们详细来讲一下

#### 📌 const 成员函数本质：**bitwise const**

```cpp
size_t length() const;
```

这个 `const` 表示 **该成员函数在调用时，不能修改类的非 `mutable` 成员变量**。也就是 bitwise const：  
👉 **从“内存物理层面”保证对象状态不变。**

#### 📌 mutable 的作用：**突破 bitwise const 限制**

但是有时候你希望在 `const` 成员函数里修改一些“对外不可见、只用于内部缓存或者计数”的数据。

比如：

- 缓存函数的计算结果
- 记录访问次数
- 缓存中间状态

这些数据的变化**不会改变对象对外的 observable state**，这就是 **logical const**： 👉 **从“逻辑意义上”对象状态是没变的。**

#### 📌 怎么做？

C++ 允许你通过 `mutable` 修饰成员变量，告诉编译器：

> “这个变量即便在 const 成员函数里，我也要能改，不算破坏 const 性质。”


- **bitwise const**：物理上不改动对象的任何非 static 成员
- **logical const**：只要对外“看起来没变”，哪怕内部缓存一下数据，也是合理的

一句话总结就是

**C++ 默认遵循 bitwise const**，但你可以用 `mutable` 解除对某些成员变量的 const 限制。

例如：
```cpp
class CTextBlock {
public:
    size_t length() const {
        if (!lengthIsValid) {
            textLength = strlen(pText);
            lengthIsValid = true;
        }
        return textLength;
    }

private:
    char* pText;
    mutable size_t textLength;
    mutable bool lengthIsValid;
};
```

- `length()` 是个 const 成员函数，正常不能改对象成员。
- 但 `textLength` 和 `lengthIsValid` 都是 `mutable`，所以即便在 const 函数里，也能改。
- 这样就能**缓存计算结果**，避免多次调用 `strlen`，提高性能。
- 对外来说，CTextBlock 的文字内容 `pText` 没变，调用 `length()` 返回值也一致，对象的**逻辑状态**没有变。
---
## 📌 总结口诀 🚀

> 能加 const 就加 const，  
> 指针看星号左右，  
> 成员函数该 const 就 const，  
> 内部缓存靠 mutable。
