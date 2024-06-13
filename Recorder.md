# 一、OpenGL库

- **已安装的:**
  
  ```bash
  sudo pacman -Sy mesa mesa-demos
  sudo pacman -Sy glfw-wayland glew
  sudo pacman -Sy freeglut
   sudo pacman -S freeglut3-dev 
  ```

CMakeLists.txt配置：

```cmake
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

# 二、1Panel安装：

```
[1Panel Log]: ======================= 开始安装 ======================= 
设置 1Panel 安装目录（默认为/opt）：
[1Panel Log]: 您选择的安装路径为 /opt 
[1Panel Log]: 检测到 Docker 已安装，跳过安装步骤 
[1Panel Log]: 启动 Docker  
[1Panel Log]: 检测到 Docker Compose 已安装，跳过安装步骤 
设置 1Panel 端口（默认为10664）：
[1Panel Log]: 您设置的端口为：10664 
设置 1Panel 安全入口（默认为1ca4623473）：
[1Panel Log]: 您设置的面板安全入口为：未设置
设置 1Panel 面板用户（默认为087e1c3fae）：loskyertt
[1Panel Log]: 您设置的面板用户为：loskyertt 
设置 1Panel 面板密码（默认为66e5f6d330）：
错误：面板密码仅支持字母、数字、特殊字符（!@#$%*_,.?），长度 8-30 位
设置 1Panel 面板密码（默认为66e5f6d330）：
[1Panel Log]: 配置 1Panel Service 
Created symlink /etc/systemd/system/multi-user.target.wants/1panel.service → /etc/systemd/system/1panel.service.
[1Panel Log]: 启动 1Panel 服务 
[1Panel Log]: 1Panel 服务启动成功! 
[1Panel Log]:  
[1Panel Log]: =================感谢您的耐心等待，安装已经完成================== 
[1Panel Log]:  
[1Panel Log]: 请用浏览器访问面板: 
[1Panel Log]: 外网地址: http://113.57.44.219:10664/1ca4623473 
[1Panel Log]: 内网地址: http://172.27.35.198:10664/1ca4623473 
[1Panel Log]: 面板用户: loskyertt 
[1Panel Log]: 面板密码: 1118tian 
[1Panel Log]:  
[1Panel Log]: 项目官网: https://1panel.cn 
[1Panel Log]: 项目文档: https://1panel.cn/docs 
[1Panel Log]: 代码仓库: https://github.com/1Panel-dev/1Panel 
[1Panel Log]:  
[1Panel Log]: 如果使用的是云服务器，请至安全组开放 10664 端口 
[1Panel Log]:  
[1Panel Log]: 为了您的服务器安全，在您离开此界面后您将无法再看到您的密码，请务必牢记您的密码。 
[1Panel Log]:  
[1Panel Log]: ================================================================ 
```

# 三、安装"oh my zsh"

   安装完 oh my zsh 后终端中有些命令不能使用：

  编辑 .zshrc 发现里面内容都被替换掉了，之前的命令都被转移到一个叫 .zshrc.pre-oh-my-zsh 文件中。