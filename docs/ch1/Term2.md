# 📖 条款 02：尽量以 const, enum, inline 替换 \#define

_Prefer consts, enums, and inlines to \#defines._

## 📖 为什么避免使用 `#define`

虽然 `#define` 常常被用来定义常量和宏，但它并不属于语言的一部分。

更准确地说，`#define` 是预处理器指令，在编译器开始处理源码之前，预处理器会将所有宏展开。这就带来了一些问题：

- **宏名称可能不被编译器看到**：例如，`#define ASPECT_RATIO 1.653` 在编译器看到它之前就被预处理器移走，导致 `ASPECT_RATIO` 并没有进入符号表。这样可能会让你在调试时迷惑，因为编译错误可能提到常量值 `1.653` 而不是宏名称 `ASPECT_RATIO`。
    
- **调试困难**：如果宏定义在第三方的头文件中，你可能根本不清楚常量值 `1.653` 是从哪里来的，浪费时间去追踪它。
    

## 📖 解决方法：使用 `const` 常量替代 `#define`

将 `#define` 替换为常量：

```cpp
const double AspectRatio = 1.653;
```

这样做的好处：

- **符号表可见性**：作为常量的 `AspectRatio` 会被编译器看到，确保它进入符号表，调试时可以明确显示。
- **避免重复**：浮点常量使用常量代替宏，可以避免宏展开后出现多个相同的常量值。常量 `AspectRatio` 只会在内存中存在一次。

### 📖 特殊情况：常量指针和类内常量

1. **常量指针**： 如果要在头文件中定义一个常量的指针（比如 `const char*` 字符串），需要声明为 `const` 两次：
    ```cpp
    const char* const authorName = "Scott Meyers";
    ```
    **更好的做法**：考虑使用 `std::string` 替代 `char*`，因为它通常更安全且易于管理：
    ```cpp
    const std::string authorName = "Scott Meyers";
    ```
2. **类内常量**： 为了将常量限定在类的作用域内，可以定义为 `static const`：
    ```cpp
    class GamePlayer {
    private:
        static const int NumTurns = 5;
        int scores[NumTurns]; // 使用常量
    };
    ```
    需要注意，若要取地址或编译器要求定义式时，可能还需要提供定义式：
    ```cpp
    const int GamePlayer::NumTurns = 5; // 定义
    ```
    对于某些旧编译器，可能需要将定义式放在实现文件中。

## 📖 另一种选择：`enum` 的使用

有时候，`enum` 可以作为常量的替代，尤其是当你希望将值限制在某个枚举中：
```cpp
class GamePlayer {
private:
    enum { NumTurns = 5 }; // 使用枚举定义常量
    int scores[NumTurns];
};
```

`enum` 的优势：
- **不允许取地址**：和 `const` 常量一样，`enum` 也不会导致额外的内存分配，且禁止取地址，这有助于避免意外引用。
- **与 `#define` 类似**：`enum` 和 `#define` 在某些方面相似，但它更具类型安全性。

## 📖 宏的替代：使用 `inline` 函数

宏看起来像函数，但却没有函数调用带来的额外开销。为了避免宏带来的潜在问题（如参数重复计算），可以使用 `inline` 函数替代宏：

```cpp
template<typename T>
inline void callWithMax(const T& a, const T& b) {
    f(a > b ? a : b);
}
```

优点：
- **类型安全**：`inline` 函数是类型安全的，避免了宏可能引发的问题。
- **不会重复计算参数**：和宏不同，`inline` 函数不会多次计算传入的参数。

## 📖 小结
- **使用 `const` 和 `enum` 替代 `#define`**：对于常量，优先考虑使用 `const` 或 `enum`，它们能带来更好的调试和类型安全。
- **使用 `inline` 函数替代宏**：对于宏定义的函数，使用 `inline` 函数既能保证效率，也能避免宏引起的潜在问题。
- 预处理器并不完全过时，但使用它的场合已经大大减少。

---

## ✅ 请记住

> **优先使用编译器替代预处理器**，用 `const`、`enum` 和 `inline` 函数替代 `#define`，不仅能让代码更加安全、清晰，还能减少调试时的困扰。