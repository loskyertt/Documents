# 1.CMake使用

## 注释

### 注释行

`CMake`使用`#`进行行注释，可以放在任何位置。

```CMake
# 这是一个 CMakeLists.txt 文件
cmake_minimum_required(VERSION 3.0.0)
```

### 注释块

`CMake`使用`#[[ ]]`形式进行块注释。

```CMake
#[[ 这是一个 CMakeLists.txt 文件。
这是一个 CMakeLists.txt 文件
这是一个 CMakeLists.txt 文件]]
cmake_minimum_required(VERSION 3.0.0)
```
