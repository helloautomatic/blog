 **在C++中，写一段代码，把某个enum中的所有元素放入到vector中，方便遍历**

假设有以下的枚举类型：

```cpp
// c++
enum class Fruit {
  Apple,
  Banana,
  Cherry,
  Durian
};
```

可以按照以下的方式将枚举类型中的所有元素放入到 vector 中：

```cpp
// c++
#include <vector>

int main() {
  std::vector<Fruit> fruits = {Fruit::Apple, Fruit::Banana, Fruit::Cherry, Fruit::Durian};
  for (const auto& fruit : fruits) {
    // 遍历 vector 中的所有枚举值
  }
  return 0;
}
```

上述代码使用了 C++11 中的初始化列表语法，将所有枚举值依次添加到了 vector 中。这样做的好处是代码简洁明了，方便阅读和维护。如果要新增或删除某个枚举值，只需要修改初始化列表即可。





**如果我的enum类型中有一万个元素，那么刚才写得代码就太冗余了，怎么办？**
确实，如果枚举类型中有很多元素，手动在代码中写入所有元素就会变得非常冗长。幸运的是，C++11 提供了一种更方便的方式来解决这个问题，即使用“枚举类”的特殊属性。枚举类允许使用枚举器来表示枚举值，枚举器是通过 enum class 关键字定义的。

下面是使用枚举类来定义一个 Fruit 枚举类型的示例：

```cpp
//c++
enum class Fruit : std::uint16_t {
  Apple,
  Banana,
  Cherry,
  Durian,
  // ... 还有很多其他的枚举值
};
```
这里，我们使用了 enum class 关键字来定义了一个枚举类，这个枚举类的底层类型为 std::uint16_t，即无符号 16 位整型。在枚举值前面加上了枚举类的名称，即 Fruit::，表示这些值属于 Fruit 枚举类。

现在，我们可以使用以下的代码将枚举类型中的所有元素放入到 vector 中：
```cpp
//c++
#include <vector>

int main() {
  std::vector<Fruit> fruits;
  for (std::uint16_t i = 0; i < static_cast<std::uint16_t>(Fruit::Count); ++i) {
    fruits.push_back(static_cast<Fruit>(i));
  }
  for (const auto& fruit : fruits) {
    // 遍历 vector 中的所有枚举值
  }
  return 0;
}
```
在这个代码中，我们定义了一个空的 vector，然后使用循环将枚举类型中的所有元素添加到 vector 中。循环的终止条件是枚举类中的一个特殊枚举值 Count，它表示枚举类型中所有枚举值的数量。由于枚举器是以 0 开始递增的整数序列，所以我们可以使用整型变量 i 从 0 到 Count - 1 循环，将枚举器的值强制转换为枚举类型，并添加到 vector 中。

使用枚举类的好处是代码简洁，不需要手动写入所有枚举值。同时，它还提供了类型安全性，可以避免将不同枚举类型的值混淆在一起。
