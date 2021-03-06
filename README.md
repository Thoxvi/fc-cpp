# fc-C++11

[![Build Status](https://travis-ci.org/GrayFlow-Institute/fc-cpp.svg?branch=master)](https://travis-ci.org/GrayFlow-Institute/fc-cpp)

一个在 C++11 上使用高阶函数链式调用的库。

- [C++11 版本](https://github.com/GrayFlow-Institute/fc-cpp)
- [Python 版本](https://github.com/Riparo/fc-python)

## 用法

### 基于 forward_list(单链表)

一般的，我们使用 C++ STL 的 std::forward_list 来作为动态数组传递给 Fc 模板类，例如

```cpp
#include <fc/fc.h>
#include <forward_list>

using namespace fc;
using namespace std;

int main(){
  forward_list<int> l = {
      1, 2, 3, 4, 5, 6
  };

  auto v = Fc<int>(l).map([](int x) {
    return x + 1;
  }).filter([](int x) {
    return x > 3;
  }).reduce([](int x, int y) {
    return x + y;
  });

  assert(v==22);
}
```

### 基于迭代器

当然，Fc 也支持使用迭代器来**拷贝**容器到内建的 forward_list，以提高**灵活性**和**兼容**更多容器

```cpp
#include <fc/fc.h>
#include <forward_list>

using namespace fc;
using namespace std;

int main(){
  forward_list<int> l = {
      1, 2, 3, 4, 5, 6
  };

  auto v = Fc<int>(l.begin(),l.end()).map([](int x) {
    return x + 1;
  }).filter([](int x) {
    return x > 3;
  }).reduce([](int x, int y) {
    return x + y;
  });

  assert(v==22);
}
```

### 函数使用一览

```cpp
#include <fc/fc.h>
#include <forward_list>

using namespace fc;
using namespace std;

int main(){
  forward_list<int> test_list = {
      1, 2, 3
  };

  auto v = Fc<int>(test_list)
    .filter([](int x) { return x > 1; })
    .print([](int x) { return to_string(x); }) // 2, 3
    .map<string>([](int x) {
      return to_string(x);
    })
    .print() // "2","3"
    .to_vector(); // ["2","3"]
}
```

## 已完成

## 待扩展

- [x] 优化性能
- [x] 增加灵活性
- [x] 支持迭代器
- [x] 构建安装包
- [ ] **可扩展地**支持更多的功能性函数

## 更新历史

- 2019-05-15 v0.0.9
  - 修复迭代器初始化顺序为倒序的 Bug
  - 增加无参数版本 `print`(针对 `std::string`)
  - 增加测试覆盖范围，并增加新功能对应测试
- 2019-05-15 v0.0.8
  - 增加 `print` 功能
  - 增加 `tamplate reduce`
  - 给大部分功能函数加上 `inline`
- 2019-05-14 v0.0.7
  - 增加 `namespace fc;` 作用域
  - 增加 `to_vector` 功能
- 2019-05-13 v0.0.6
  - 增加复制、移动构造函数，同时重载 `=` 号
  - 增加 `map` 返回新类型返回值的重载，使用例如 `fc` 内嵌 `int` 类型的链表：`fc.map<std::string>([](int x)->std::string{return std::to_string(x);}).done();` 即可返回一个内嵌 `std::string` 类型链表的 fc
- 2019-05-12 v0.0.5 修改目录结构，使用删除原来的 lib 改用 hpp
- 2018-07-05 v0.0.4 修复了一个智障未返回引用的性能优化错误
- 2018-06-27 v0.0.3 让 Fc 支持迭代器，即类似于 `for(auto v:fc){...}` 格式的操作
- 2018-06-27 v0.0.2 修改 vector 为 forward_list，以减少扩容 vector 性能损失
- 2018-06-23 v0.0.1 文档编写