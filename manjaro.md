# 一、linux下的miniconda

## 1.集成conda环境

```
 $ conda activate base                                                   
Error while loading conda entry point: conda-content-trust (OpenSSL 3.0's legacy provider failed to load. This is a fatal error by default, but cryptography supports running without legacy algorithms by setting the environment variable CRYPTOGRAPHY_OPENSSL_NO_LEGACY. If you did not expect this error, you have likely made a mistake with your OpenSSL configuration.)

CondaError: Run 'conda init' before 'conda activate'
```

根据该的错误信息，问题可能与 OpenSSL 版本有关。OpenSSL 3.0 引入了一些变化，可能导致与某些软件包的兼容性问题。在此情况下，设置环境变量`CRYPTOGRAPHY_OPENSSL_NO_LEGACY`可能会解决问题。

### 1).详细解决步骤

1. **编辑 `~/.zshrc` 文件**：
   
   添加以下行到 `~/.zshrc` 文件：
   
   ```.conf
   export PATH=/opt/miniconda3/bin:$PATH
   export CRYPTOGRAPHY_OPENSSL_NO_LEGACY=1
   ```
   
   保存并退出编辑器。
   
   终端输入这行可以防止conda自动激活环境（建议加上）：
   
   ```sh
   conda config --set auto_activate_base false
   ```

2. **重新加载 `~/.zshrc` 文件**：
   
   在终端中运行以下命令重新加载配置文件：
   
   ```sh
   source ~/.zshrc
   ```

3. **运行 `conda init`**：
   
   初始化 conda 环境：
   
   ```sh
   # 这是conda安装的位置
   /opt/miniconda3/bin/conda init zsh
   ```

4. **再次重新加载 `~/.zshrc` 文件**：
   
   再次运行以下命令重新加载配置文件：
   
   ```sh
   source ~/.zshrc
   ```

5. **激活 conda 环境**：
   
   现在尝试激活 conda 环境：
   
   ```sh
   conda activate base
   ```

### 2).验证

验证 conda 是否配置好：

```bash
conda --version
```

### 3).一步到位操作

把段复制到`.zshrc`中，注意安装miniconda的路径：

```bash
# >>> conda initialize >>>
# !! Contents within this block are managed by 'conda init' !!
__conda_setup="$('/opt/miniconda3/bin/conda' 'shell.zsh' 'hook' 2> /dev/null)"
if [ $? -eq 0 ]; then
    eval "$__conda_setup"
else
    if [ -f "/opt/miniconda3/etc/profile.d/conda.sh" ]; then
        . "/opt/miniconda3/etc/profile.d/conda.sh"
    else
        export PATH="/opt/miniconda3/bin:$PATH"
    fi
fi
unset __conda_setup
# <<< conda initialize <<<
export CRYPTOGRAPHY_OPENSSL_NO_LEGACY=1
```

# 二、Linux下的docker

### 1. 启动 Docker 服务

```sh
# 启动docker服务
sudo systemctl start docker


# 关闭docker服务
sudo systemctl stop docker
# 关闭docker的socket端口
sudo systemctl stop docker.socket
```

```sh
# 设置开机自启动
sudo systemctl enable docker
sudo systemctl enable docker.socket

# 关闭开机自启动
sudo systemctl disable docker
sudo systemctl disable docker.socket
```

### 2. 检查 Docker 服务状态

确保 Docker 服务正在运行：

```sh
sudo systemctl status docker
```

### 3. 添加用户到 `docker` 组（可选）

为了避免每次运行 Docker 命令时都需要使用 `sudo`，可以将当前用户添加到 `docker` 组：

```sh
sudo usermod -aG docker $USER
```

然后，重新登录以使更改生效，或者重新加载用户组信息：

```sh
# 登录
newgrp docker

# 退出
exit
```

### 4. 运行测试容器

运行一个简单的容器来测试 Docker 安装：

```sh
docker run hello-world
```

### 5. 常用 Docker 命令

以下是一些常用的 Docker 命令：

- **查看已拉取的镜像**:
  
  ```sh
  docker images
  ```

- **拉取镜像**：
  
  ```sh
  docker pull <image_name>
  ```
  
  例如，拉取最新的 Ubuntu 镜像：
  
  ```sh
  docker pull ubuntu
  ```

- **运行容器**：
  
  ```sh
  docker run -it <image_name> /bin/bash
  ```
  
  例如，运行一个交互式的 Ubuntu 容器：
  
  ```sh
  docker run -it ubuntu /bin/bash
  ```

- **列出正在运行的容器**：
  
  ```sh
  docker ps
  ```

- **列出所有容器**：
  
  ```sh
  docker ps -a
  ```

- **停止容器**：
  
  ```sh
  docker stop <container_id>
  ```

- **启动已停止的容器**：
  
  ```sh
  docker start <container_id>
  ```

- **删除容器**：
  
  ```sh
  docker rm <container_id>
  ```

- **删除镜像**：
  
  ```sh
  docker rmi <image_id>
  ```

### 6. Docker Compose（可选）

如果需要使用 Docker Compose，可以通过以下命令安装：

```sh
sudo pacman -S docker-compose
```

然后，可以使用 `docker-compose` 命令来管理多容器应用。

# 三、manjaro常用指令

## 1.初始配置

- **配置镜像源**
  
  ```sh
  sudo pacman-mirrors -m rank -c China
  ```

- **安装依赖包**
  
  ```sh
  sudo pacman -Syu base-devel
  ```

## 2.安装或更新软件包

- **介绍：**
  在 Manjaro（以及其他基于 Arch 的发行版）中，`pacman` 是一个用于软件包管理的命令行工具。下面是 `pacman` 命令的详细解释：
1. **`pacman -Syyu`**:
   
   - `-S`：用于安装软件包。
   - `-y`：强制刷新软件包数据库。
   - `-yy`：强制刷新所有的软件包数据库。通常只需要一个 `-y` 就足够，但 `-yy` 用于解决某些情况下可能出现的数据库同步问题。
   - `-u`：更新所有已安装的软件包。
   - 组合在一起，`pacman -Syyu` 的作用是强制刷新所有软件包数据库，然后更新所有已安装的软件包。这通常用于确保软件包数据库是最新的，然后进行全面的系统更新。

2. **`pacman -Sy`**：
   
   - `-S`：用于安装软件包。
   - `-y`：刷新软件包数据库。
   - 组合在一起，`pacman -Sy` 的作用是刷新软件包数据库。这是用来更新数据库，但不进行任何软件包的安装或升级。

3. **`pacman -Syy`**：
   
   - `-S`：用于安装软件包。
   - `-yy`：强制刷新所有的软件包数据库。
   - 组合在一起，`pacman -Syy` 的作用是强制刷新所有软件包数据库。这用于解决某些情况下可能出现的数据库同步问题，但不会进行任何软件包的安装或升级。

4. **`pacman -Su`**：
   
   - `-S`：用于安装软件包。
   - `-u`：更新所有已安装的软件包。
   - 组合在一起，`pacman -Su` 的作用是更新所有已安装的软件包。在运行此命令前，建议先运行 `pacman -Sy` 以确保数据库是最新的。

总结：

- `pacman -Syyu`：强制刷新所有数据库，然后更新所有已安装的软件包。

- `pacman -Sy`：刷新数据库，但不进行任何安装或升级。

- `pacman -Syy`：强制刷新所有数据库，但不进行任何安装或升级。

- `pacman -Su`：更新所有已安装的软件包，前提是数据库已经是最新的。

- **要更新指定的软件包:**

```bash
sudo pacman -S 包名
```

这样，`pacman` 会检查你指定的软件包是否有新版本，如果有的话，就会下载并安装更新后的版本。

假设你想更新 `firefox` 软件包，你可以运行：

```bash
sudo pacman -S firefox
```

这个命令会更新 `firefox` 到最新版本。如果 `firefox` 已经是最新的，那么它会告诉你没有更新可用。

- **`pacman -Sy 包名`**：
  刷新软件包数据库，然后安装或更新指定的软件包。例如：
  
  ```bash
  sudo pacman -Sy firefox
  ```

- **`pacman -Si 包名`**：
  显示指定软件包的信息，但不会安装或更新。这个选项可以用来查看某个软件包的详细信息，包括它的版本、依赖关系等。例如：
  
  ```bash
  pacman -Si firefox
  ```

- **查看可升级的包/库：**
  
  ```bash
    # 查看包
  sudo pacman -Qu
  
  # 查看库
  sudo pacman -Q
  ```

## 3.卸载

### 卸载单个软件包

要卸载单个软件包，可以使用以下命令：

```sh
sudo pacman -R package_name
```

### 卸载软件包及其未使用的依赖

有时卸载一个包后，它的一些依赖包可能不再被其他软件包使用。要卸载软件包及其未使用的依赖，可以使用以下命令：

```sh
sudo pacman -Rns package_name
```

解释：

- `-R`：卸载指定的包。
- `-n`：从系统中删除安装包的所有配置文件。
- `-s`：递归地卸载未使用的依赖包。

### 强制卸载（不推荐）

在极少数情况下，可能需要强制卸载一个包，即使这可能会破坏系统的依赖关系。请谨慎使用此选项：

```sh
sudo pacman -Rdd package_name
```

解释：

- `-R`：卸载指定的包。
- `-d`：忽略依赖关系检查。

### 清理未使用的孤立包

有时，系统中可能会有一些未使用的孤立包，这些包是作为依赖安装的，但现在没有任何包依赖它们。可以使用以下命令清理这些孤立包：

```sh
sudo pacman -Rns $(pacman -Qtdq)
```

解释：

- `pacman -Qtdq`：列出所有未使用的孤立包。
- `-Rns`：递归地卸载未使用的包及其配置文件。

### 使用 AUR 助手卸载包

如果您使用 `yay` 或 `pamac` 等 AUR 助手工具来安装包，它们也提供了卸载功能。例如：

#### 使用 `yay`：

```sh
yay -R package_name
```

#### 使用 `pamac`：

```sh
pamac remove package_name
```

### 示例

假设要卸载 Google Chrome，可以使用以下命令：

```sh
sudo pacman -R google-chrome
```

如果您想要卸载 Google Chrome 及其未使用的依赖，可以使用：

```sh
sudo pacman -Rns google-chrome
```

## 4.查看开机启动项

### 方法一：使用 `systemctl` 查看 systemd 服务

使用 `systemctl` 命令来列出所有启用的（开机自启动的）systemd 服务：

```sh
systemctl list-unit-files --type=service --state=enabled
```

这个命令将列出所有设置为开机自启动的服务。如果你想查看当前运行的服务，可以使用以下命令：

```sh
systemctl list-units --type=service --state=running
```

### 方法二：查看用户级别的开机自启动应用

用户级别的开机自启动应用通常在 `~/.config/autostart` 目录下。你可以使用以下命令查看该目录中的内容：

```sh
ls ~/.config/autostart
```

### 方法三：使用 `crontab` 查看定时任务

你还可以使用 `crontab` 查看是否有任何定时任务设置为在启动时运行。使用以下命令查看当前用户的 `crontab` 条目：

```sh
crontab -l
```

如果有需要在系统启动时运行的任务，它们通常会使用 `@reboot` 时间标志。

### 方法四：检查 `/etc/rc.local` 文件

虽然 `rc.local` 文件在现代系统中不再常用，但有时仍然会使用它来设置开机自启动任务。你可以检查这个文件（如果存在）来查看是否有任何任务设置为在启动时运行：

```sh
cat /etc/rc.local
```

### 方法五：使用 `chkconfig`（仅适用于部分系统）

在一些 Linux 发行版中，`chkconfig` 工具可以用来查看和管理开机自启动服务。首先安装 `chkconfig`：

```sh
sudo pacman -S chkconfig
```

然后使用以下命令查看所有服务的状态：

```sh
chkconfig --list
```

### 方法六：使用 `systemctl` 查看所有启用的单元文件

你也可以使用 `systemctl` 查看所有启用的单元文件，包括服务、套接字、目标等：

```sh
systemctl list-unit-files --state=enabled
```

## 5.文件操作

- **创建文件**

```sh
touch <filename>

# 例：touch test.txt
```

- **创建文件夹**

```sh
mkdir <foldername>

# 例：mkdir build
```

- **删除文件/文件夹**

```sh
rm <filename1> <filename2>

# "/"表示删除该文件夹下的所有子文件
rm <foldername>/ -rf

# 删除当前文件夹的所有文件
rm * -rf

# 注：后缀的"-rf"是指强制删除（不会有警告），"-r"是指普通删除（若与其它文件有链接，会提出警告）
```

- **复制文件**

```sh
cp <foldername1> <foldername2> -r

# 例：把v1文件夹下的内容复制到v2文件夹，如果v2没有，会自动创建
cp v1 v2 -r

# 复制文件到文件夹可以不加"-r"，例：
cp lib.so lib1
```

## 6.软件包管理

可以使用包管理工具 `pacman` 和 `pamac` 来查看已安装的软件包。以下是具体的指令和方法：

### 方法一：使用 `pacman`

`pacman` 是 Manjaro 和 Arch Linux 的默认包管理工具，可以列出所有已安装的软件包。

1. **列出所有已安装的软件包**：
   
   ```bash
   pacman -Q
   ```

2. **列出所有已安装的软件包及其版本（包括用户手动安装的）**：
   
   ```bash
   pacman -Qe
   ```

3. **列出所有的外部软件包（即非官方仓库安装的包，如 AUR 软件包）**：
   
   ```bash
   pacman -Qm
   ```

### 方法二：使用 `pamac`

`pamac` 是 Manjaro 提供的图形化包管理工具，但也可以通过命令行使用。

1. **列出所有已安装的软件包**：
   
   ```bash
   pamac list --installed
   ```

2. **列出所有已安装的软件包及其版本**：
   
   ```bash
   pamac list --installed --quiet
   ```

3. **列出所有的外部软件包（即非官方仓库安装的包，如 AUR 软件包）**：
   
   ```bash
   pamac list --foreign
   ```

# 四、终端代理设置

```bash
kate ~/.zshrc
```

- **在`.zshrc`中添加以下内容：**

```txt
# where proxy
proxy(){
  export http_proxy="http://127.0.0.1:2334"
  export https_proxy="http://127.0.0.1:2334"
  echo "HTTP Proxy on by hiddify"
}

# where noproxy
noproxy(){
  unset http_proxy
  unset https_proxy
  echo "HTTP Proxy off"
}
```

`2334`为Hiddify-next的端口

```bash
source ~/.zshrc
```

- **效果：**

```bash
╭─ ~                                                                                 
╰─❯ proxy_h
HTTP Proxy on by hiddify

╭─ ~                                                                      
╰─❯ noproxy
HTTP Proxy off

╭─ ~                                                                                 
╰─❯ proxy_n
HTTP Proxy on by nekoray
```

# 五、N卡驱动安装

- **全面更新系统**
  
  ```bash
  sudo pacman -Syu
  ```

- **查看显卡类型**
  
  ```bash
  lspci -vnn | grep VGA
  
  # 或者
  lspci -k | grep -EA3 'VGA|3D'
  ```

然后从官网下载对应型号的驱动，一定要把下载好的文件放到主文件夹（`~`）下！

- **查看内核**
  
  ```bash
  uname -r
  ```

- **下载内核头文件**
  
  ```bash
  sudo pacman -S linux61-headers
  ```
  
  注：`linux61`表示是`6.1.x`系列的内核，如果是`4.19.x`系列的，那么就是`linux419`。

- **安装相应的依赖工具**
  
  ```bash
  sudo pacman -S base-devel dkms
  ```
  
  - **自动安装推荐的开源驱动程序**
    
    ```bash
    sudo mhwd -a pci free 0300
    ```
  
  - **自动安装推荐的闭源驱动程序：**
    
    ```bash
    sudo mhwd -a pci nonfree 0300
    ```

# 六、zsh美化

- **在`~/.zshrc`文件中进行配置：**
  
  | Code   | Info                              |
  | ------ |:---------------------------------:|
  | %T     | 系统时间（时：分）                         |
  | %*     | 系统时间（时：分：秒）                       |
  | %D     | 系统日期（年-月-日）                       |
  | %n     | 用户名称（即：当前登陆终端的用户的名称，和whami命令输出相同） |
  | %B     | 开始到结束使用粗体打印                       |
  | %b     | 开始到结束使用粗体打印                       |
  | %U     | 开始到结束使用下划线打印                      |
  | %u     | 开始到结束使用下划线打印                      |
  | %d     | 你当前的工作目录                          |
  | %~     | 你目前的目录相对于～的相对路径                   |
  | %M     | 计算机的主机名                           |
  | %m     | 计算机的主机名（在第一个句号之前截断）               |
  | %l     | 你当前的tty                           |
  | %F{色码} | 用来设定某个颜色的开始                       |
  | %f     | 用来设定成预设的样式， 也可以说是设定好的颜色结束         |

- **换行示例：**
  
  ```
  NEWLINE=$'\n'
  PROMPT="%{$fg[green]%}%d %t %{$reset_color%}%# ${NEWLINE}"
  ```

- **推荐配置：**
  
  ```
  # PROMPT
  NEWLINE=$'\n' # 换行
  PS1="%B%F{2}%D%f %F{60}%*%f%b [%F{184}%n%f@%F{30}%~%f]${NEWLINE}%F{111}$%f "
  ```
  

# 七、中文输入法配置

  ## 1.推荐方式

  ```bash
  yay -Sy fcitx-im fcitx-configtool
  ```

  ```bash
  kate ~/.xprofile
  ```
  输入以下内容：
  ```
  export GTK_IM_MODULE=fcitx
  export QT_IM_MODULE=fcitx
  export XMODIFIERS="@im=fcitx"
  ```

  ## 2.其它（待定）

  **方式一：**
  ```bash
  # 安装fcitx5
  sudo pacman -S fcitx5-im fcitx5-chinese-addons  fcitx5-rime
  
  # 配置
  kate /etc/environment
  ```
  输入以下内容：
  ```
  GTK_IM_MODULE=fcitx
  QT_IM_MODULE=fcitx
  XMODIFIERS=@im=fcitx
  SDL_IM_MODULE=fcitx
  ```

  **方式二：**
  ```bash
  yay -S fcitx fcitx-configtool
  
  # 配置
  kate ~/.pam_environment
  ```
  输入以下内容：
  ```
  GTK_IM_MODULE DEFAULT=fcitx
  QT_IM_MODULE  DEFAULT=fcitx
  XMODIFIERS    DEFAULT=\@im=fcitx
  SDL_IM_MODULE DEFAULT=fcitx
  ```


# 八、EndeavourOS配置镜像源

```bash
#激活
sudo pacman -Sy archlinuxcn-keyring haveged
```

添加以下内容：
```
[archlinuxcn]
Server = https://mirrors.aliyun.com/archlinuxcn/$arch
```
