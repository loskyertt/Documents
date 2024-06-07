# 一、OpenGL库

- **已安装的:**
  
  ```bash
  sudo pacman -Sy mesa mesa-demos
  sudo pacman -Sy glfw-wayland glew
  sudo pacman -Sy freeglut
  ```

CMakeLists.txt配置：

```make
cmake_minimum_required(VERSION 3.10)
project(OpenGLTest)

# 设置项目名称为 OpenGLTest

# 设置 OpenGL 库偏好
if(POLICY CMP0072)
  cmake_policy(SET CMP0072 NEW)  # 使用 GLVND 库
endif()

# 寻找所需的 OpenGL 库
find_package(OpenGL REQUIRED)

# 寻找所需的 GLEW 库
find_package(GLEW REQUIRED)

# 寻找所需的 GLFW 库
find_package(PkgConfig REQUIRED)
pkg_search_module(GLFW REQUIRED glfw3)

# 添加可执行文件，并指定源文件为 main.cpp
add_executable(OpenGLTest main.cpp)

# 链接 OpenGL, GLEW, 和 GLFW 库到可执行文件
target_link_libraries(OpenGLTest OpenGL::GL GLEW::GLEW ${GLFW_LIBRARIES})

# 包含 GLFW 库的头文件目录
include_directories(${GLFW_INCLUDE_DIRS})
```
