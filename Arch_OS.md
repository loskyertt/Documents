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

```
export PATH=/opt/miniconda3/bin:$PATH
export CRYPTOGRAPHY_OPENSSL_NO_LEGACY=1
```

保存并退出编辑器。
终端输入这行可以防止conda自动激活环境（建议加上）：

```bash
conda config --set auto_activate_base false
```

2. **重新加载 `~/.zshrc` 文件**：
   

在终端中运行以下命令重新加载配置文件：

```sh
source ~/.zshrc
```

3. **运行 `conda init`**：

初始化 conda 环境：

```bash
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
  
```bash
docker images
```

- **拉取镜像**：
  
```bash
docker pull <image_name>
```

例如，拉取最新的 Ubuntu 镜像：

```bash
docker pull ubuntu
```

- **运行容器**：
  
```bash
docker run -it <image_name> /bin/bash
```

例如，运行一个交互式的 Ubuntu 容器：

```bash
docker run -it ubuntu /bin/bash
```

- **列出正在运行的容器**：
  
```bash
docker ps
```

- **列出所有容器**：
  
```bash
docker ps -a
```

- **停止容器**：
  
```bash
docker stop <container_id>
```

- **启动已停止的容器**：
  
```bash
docker start <container_id>
```

- **删除容器**：
  
```bash
docker rm <container_id>
```

- **删除镜像**：
  
```bash
docker rmi <image_id>
```

### 6. Docker Compose（可选）

如果需要使用 Docker Compose，可以通过以下命令安装：

```bash
sudo pacman -S docker-compose
```

然后，可以使用 `docker-compose` 命令来管理多容器应用。

# 三、常用指令

## 1.初始配置

- **配置镜像源**
```bash
sudo pacman-mirrors -m rank -c China
```

- **安装依赖包**
```bash
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

**总结：**

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

- **查看包名信息**：
  显示指定软件包的信息，但不会安装或更新。这个选项可以用来查看某个软件包的详细信息，包括它的版本、依赖关系等。例如：
```bash
pacman -Si <pkgname>
```
其它
```bash
# 查看包的简略信息
yay -Qs <pkgname>

# 查看包的详细信息
yay -Qi <pkgname>
```

- **查看可升级的包/库：**
```bash
# 查看包
sudo pacman -Qu
  
# 查看库
sudo pacman -Q
```

## 3.卸载

### 1）卸载单个软件包

要卸载单个软件包，可以使用以下命令：

```bash
sudo pacman -R package_name
```

### 2）卸载软件包及其未使用的依赖

有时卸载一个包后，它的一些依赖包可能不再被其他软件包使用。要卸载软件包及其未使用的依赖，可以使用以下命令：

```bash
sudo pacman -Rns package_name
```

解释：

- `-R`：卸载指定的包。
- `-n`：从系统中删除安装包的所有配置文件。
- `-s`：递归地卸载未使用的依赖包。

### 3）强制卸载（不推荐）

在极少数情况下，可能需要强制卸载一个包，即使这可能会破坏系统的依赖关系。请谨慎使用此选项：

```bash
sudo pacman -Rdd package_name
```

解释：

- `-R`：卸载指定的包。
- `-d`：忽略依赖关系检查。

### 4）清理未使用的孤立包

有时，系统中可能会有一些未使用的孤立包，这些包是作为依赖安装的，但现在没有任何包依赖它们。可以使用以下命令清理这些孤立包：

```bash
sudo pacman -Rns $(pacman -Qtdq)
```

解释：

- `pacman -Qtdq`：列出所有未使用的孤立包。
- `-Rns`：递归地卸载未使用的包及其配置文件。



## 4.查看开机启动项

### 1）使用 `systemctl` 查看 systemd 服务

使用 `systemctl` 命令来列出所有启用的（开机自启动的）systemd 服务：

```sh
systemctl list-unit-files --type=service --state=enabled
```

查看当前运行的服务：

```sh
systemctl list-units --type=service --state=running
```

### 2）查看用户和系统级别的开机自启动应用

用户级别的开机自启动应用通常在 `~/.config/autostart` 目录下。你可以使用以下命令查看该目录中的内容：

```sh
# 用户
ls ~/.config/autostart

# 系统
ls /etc/xdg/autostart
```

### 3）使用 `crontab` 查看定时任务

你还可以使用 `crontab` 查看是否有任何定时任务设置为在启动时运行。使用以下命令查看当前用户的 `crontab` 条目：

```sh
crontab -l
```

如果有需要在系统启动时运行的任务，它们通常会使用 `@reboot` 时间标志。

### 4）检查 `/etc/rc.local` 文件

虽然 `rc.local` 文件在现代系统中不再常用，但有时仍然会使用它来设置开机自启动任务。你可以检查这个文件（如果存在）来查看是否有任何任务设置为在启动时运行：

```sh
cat /etc/rc.local
```

### 5）使用 `chkconfig`（仅适用于部分系统）

在一些 Linux 发行版中，`chkconfig` 工具可以用来查看和管理开机自启动服务。首先安装 `chkconfig`：

```sh
sudo pacman -S chkconfig
```

然后使用以下命令查看所有服务的状态：

```sh
chkconfig --list
```

### 6）使用 `systemctl` 查看所有启用的单元文件

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

## 6.赋予文件或文件夹权限

1. 使用`chmod`命令递归地更改文件夹内所有文件和子文件夹的权限：
```bash
chmod -R 777 /path/to/your/folder
```
`-R`表示递归。

2. 更改文件的权限而不更改文件夹的权限，可以结合`find`命令来实现：
```bash
find /path/to/your/folder -type f -exec chmod 777 {} +
```

这将只更改所有文件的权限，而不影响文件夹的权限。

3. 单独更改文件夹的权限（例如，给文件夹777权限，而文件保持不变）：
```bash
find /path/to/your/folder -type d -exec chmod 777 {} +
```

## 7.软件包管理

可以使用包管理工具 `pacman` 和 `pamac` 来查看已安装的软件包。以下是具体的指令和方法：

### 1）使用 `pacman`

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

### 2）使用 `pamac`

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

输入下面指令，可以查看终端代理地址：
```
env | grep -i proxy
```



# 五、N卡驱动

## 1.安装

- **全面更新系统**
  
```bash
sudo pacman -Syu
```

- **查看显卡类型**
  
```bash
lspci -vnn | grep VGA

# 或者
lspci -k | grep -EA3 'VGA|3D'

# 或者
inxi -G
```

手动安装的话，从官网下载对应型号的驱动，一定要把下载好的驱动文件放到主文件夹（`~`）下！

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

- **建议安上：**

```bash
# 必须安装
sudo pacman -S nvidia nvidia-settings lib32-nvidia-utils
```

## 2.补充 
- **`nvidia-hook`, `nvidia-inst`, `nvidia-utils` 的用途：**

### 1). `nvidia-hook`
`nvidia-hook` 是一个 Pacman 钩子脚本，用于在内核更新时自动重新生成 NVIDIA 内核模块。这可以确保你的 NVIDIA 驱动程序在内核更新后继续正常工作，而无需手动干预。

**功能：**
- 自动在内核更新后重新生成 NVIDIA 内核模块。
- 提高系统更新的便利性，减少因驱动兼容性问题导致的系统启动问题。

### 2). `nvidia-inst`
`nvidia-inst` 是 EndeavourOS 提供的一个脚本，用于安装和配置 NVIDIA 驱动程序。它简化了在 EndeavourOS 系统上安装 NVIDIA 驱动的过程。

**功能：**
- 自动检测你的显卡型号。
- 安装合适版本的 NVIDIA 驱动程序。
- 配置系统以使用新的驱动程序。

### 3). `nvidia-utils`
`nvidia-utils` 包含 NVIDIA 驱动程序的核心用户空间组件和实用工具。这个包提供了运行 NVIDIA 驱动程序所需的库和命令行工具。

**功能：**
- 提供核心的 NVIDIA 驱动库。
- 包含用于查询和配置 NVIDIA 显卡的命令行工具，如 `nvidia-smi`。
- 包含 OpenGL 库和 Vulkan 库，用于支持高性能图形渲染。

 `nvidia-hook`, `nvidia-inst`, `nvidia-utils` 不能完全替代 `nvidia-settings` 和 `lib32-nvidia-utils`。

### 4). `nvidia-settings`
`nvidia-settings` 是一个独立的图形化配置工具，用于调整和配置 NVIDIA 显卡的各种设置。它提供了一个图形界面，允许用户进行以下操作：
- 配置显示器布局（多显示器设置）。
- 调整显卡性能参数（如风扇速度、功率模式）。
- 配置 OpenGL 设置。
- 管理显示器色彩和亮度设置。

**替代情况：**
- `nvidia-utils` 提供了一些命令行工具，但没有图形界面和用户友好的配置选项。
- `nvidia-settings` 提供了更丰富和直观的配置选项，尤其适用于需要频繁调整显卡设置的用户。

### 5). `lib32-nvidia-utils`
`lib32-nvidia-utils` 是 32 位 NVIDIA 驱动程序库的集合，用于在 64 位系统上运行 32 位应用程序（如一些旧版游戏和软件）。这些库对于兼容 32 位应用程序至关重要。

**替代情况：**
- `nvidia-utils` 包含 64 位库，不包括 32 位库。
- 如果你需要运行 32 位应用程序，仍然需要安装 `lib32-nvidia-utils`。

### 6).总结
- **`nvidia-hook`**：自动重新生成 NVIDIA 内核模块的 Pacman 钩子脚本，用于内核更新后确保驱动正常工作。
- **`nvidia-inst`**：EndeavourOS 的 NVIDIA 驱动安装和配置脚本，简化驱动安装过程。
- **`nvidia-utils`**：包含 NVIDIA 驱动程序的核心用户空间组件和实用工具。

- **`nvidia-settings`**：提供图形化界面，用于配置和调整 NVIDIA 显卡设置，不可被上述包完全替代。
- **`lib32-nvidia-utils`**：32 位 NVIDIA 驱动程序库，用于支持 32 位应用程序，不可被上述包完全替代。

因此，如果需要使用图形化配置工具和运行 32 位应用程序，建议安装 `nvidia-settings` 和 `lib32-nvidia-utils`。



# 六、zsh配置

## 1.样式配置（prompt/PS1）

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

- **显示git分支：**
```
parse_git_branch() {
  git branch 2> /dev/null | sed -e '/^[^*]/d' -e 's/* \(.*\)/ (\1)/'
}
```

- **推荐配置：**
```
# PROMPT
parse_git_branch() {
  git branch 2> /dev/null | sed -e '/^[^*]/d' -e 's/* \(.*\)/ (\1)/'
}
NEWLINE=$'\n' # 换行
PS1="%B%F{2}%D%f %F{60}%*%f%b [%F{184}%n%f@%F{30}%~%f]$(parse_git_branch)${NEWLINE}%F{111}$%f "
```
或者：
```
# prompt
ZSH_NEWLINE=$'\n'
export PROMPT=" %F{46}%F %(?.%F{green}√.%F{red}?%?)%f  %B%F{69}%~ ${ZSH_NEWLINE} %F{119}==>%f%b "
```

```
NEWLINE=$'\n' # 换行
PS1="%B%F{2}%D%f %F{60}%*%f%b [%F{184}%n%f@%F{30}%~%f]${NEWLINE}%F{111}╰─❯%f "
```

## 2.插件配置

- **备份：**
```bash
cp ~/.zshrc ~/.zshrc.backup
```

- **创建配置文件夹：**
```bash
mkdir -p .zsh/plugins
cp .zshrc .zsh/
mv .zsh_history .zsh/
```
注：若没有`.zsh_history`，那么需要用`touch`指令创建。

- **然后编辑`.zshrc`Copy, 加上：**
```
### ZSH HOME
export ZSH=$HOME/.zsh

### ---- history config ----------
export HISTFILE=$ZSH/.zsh_history

# How many commands zsh will load to memory.
export HISTSIZE=10000

# How maney commands history will save on file.
export SAVEHIST=10000

# History won't save duplicates.
setopt HIST_IGNORE_ALL_DUPS

# History won't show duplicates on search.
setopt HIST_FIND_NO_DUPS
```

- **安装插件：**
```bash
cd .zsh/plugins

git clone  https://github.com/zdharma-continuum/fast-syntax-highlighting.git

git clone https://github.com/zsh-users/zsh-autosuggestions.git

git clone https://github.com/zsh-users/zsh-completions.git
```

- **在.zshrcCopy中添加：**
```
source $ZSH/plugins/fast-syntax-highlighting/fast-syntax-highlighting.plugin.zsh
fpath=($ZSH/plugins/zsh-completions/src $fpath)

# zsh-autosuggestions:config
source $ZSH/plugins/zsh-autosuggestions/zsh-autosuggestions.zsh
ZSH_AUTOSUGGEST_HIGHLIGHT_STYLE="fg=#ff00ff,bg=cyan,bold,underline"
ZSH_AUTOSUGGEST_STRATEGY=(history completion)
ZSH_AUTOSUGGEST_BUFFER_MAX_SIZE=20

# end config
```

- **链接：**
最后就是创建符号链接，这样我们就可以通过更改`~/.zshrc`Copy来同步更改`.zsh/.zshrc`Copy配置文件了。首先需要确认`~`目录下没有`.zshrc`文件，如果有，就`rm .zshrc`。此时可以开始创建符号链接了.
```bash
ln -s ~/.zsh/.zshrc ~/.zshrc

source ~/.zshrc
```
可以通过`ls -la`来查看是否链接成功。

**注意**：安装"oh my zsh"可能会出现的问题

发现安装完 oh my zsh 后终端中有些命令不能使用：
编辑 .zshrc 发现里面内容都被替换掉了，之前的配置内容都被转移到一个叫 .zshrc.pre-oh-my-zsh 文件中。


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

## 2.其它（没成功过）

**方式一：**
```bash
# 安装fcitx5
sudo pacman -S fcitx5-im #基础包组
sudo pacman -S fcitx5-chinese-addons #官方中文输入引擎
sudo pacman -S fcitx5-anthy #日文输入引擎
yay -S fcitx5-pinyin-moegirl #萌娘百科词库（挂梯子）
sudo pacman -S fcitx5-pinyin-zhwiki #中文维基百科词库
sudo pacman -S fcitx5-material-color #主题

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



# 八、提高编译速度


## 1.增加编译并行度
可以通过设置更多的并行编译任务来加速编译过程。使用与 CPU 核心数量相等或更多的并行任务数。设置 MAKEFLAGS 来实现这一点：

编辑 /etc/makepkg.conf 文件，找到以下行并进行修改：
```
MAKEFLAGS="-j$(nproc)"
```
`$(nproc)`会自动检测 CPU 核心数，并设置相同数量的并行任务。包括在执行`make`指令时，可以通过加`-j<核心数>`来手动指定编译时用CPU的核心数。



# 九、问题汇总

## 1.证书安装问题
有时候可能无法安装`ca-certificates`这一系列证书，排除掉网络问题后，大概率就是时间问题。解决办法：

1. **同步系统时间**：
运行以下命令以确保系统时间是准确的：
```bash
sudo ntpd -qg
sudo hwclock --systohc
```

2. **手动更新CA证书**：
- 将信任的CA证书复制到`/etc/pki/ca-trust/source/anchors/`。确保证书是PEM或DER格式。
- 然后，运行以下命令来更新系统中的CA证书：
```bash
sudo update-ca-trust
```
------------一般到这里就能解决了-------------

3. **更换软件源**：
- 编辑`/etc/pacman.d/mirrorlist`文件，选择一个新的软件源。
- 可以使用`reflector`来自动选择最快的软件源：
```bash
sudo reflector --latest 5 --sort rate --save /etc/pacman.d/mirrorlist
```
- 更新软件包数据库：
```bash
sudo pacman -Syy
```

4. **重新安装软件包**：
可以尝试重新安装`ca-certificates`和`ca-certificates-utils`：
```bash
sudo pacman -S ca-certificates ca-certificates-utils
```

## 2.历史命令问题
出现这种情况`zsh: corrupt history file /home/sky/.zsh/.zsh_history`。有的时候系统因为默写原因强行启动的时候会破坏zsh的历史文件。
解决办法：
```bash
cp ~/.zsh_history ~/.zsh_history_backup
rm ~/.zsh_history
strings -eS ~/.zsh_history_backup > ~/.zsh_history
fc -R ~/.zsh_history
```
如果上述步骤没有解决问题，可能是因为.zsh_history文件严重损坏。在这种情况下，需要放弃旧的历史记录并创建一个新的文件。
