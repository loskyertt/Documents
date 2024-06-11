# 一、docker-MySQL

## 1.基本操作

1. **拉取 MySQL 镜像：**
   通过运行 `docker pull mysql[:版本号]`，版本号可选，默认是latest。

2. **创建并运行 MySQL 容器（推荐）：**
   使用以下命令来运行 MySQL 容器。这里假设想在主机上绑定3306端口，并设置一个root用户的密码：
   
   ```bash
   docker run \
   --name some-mysql \
   -e MYSQL_ROOT_PASSWORD=1234 \
   -p 3306:3306 \
   -v ~/docker_volume/mysql/data:/var/lib/mysql \
   -v ~/docker_volume/mysql/init:/docker-entrypoint-initdb.d \
   -v ~/docker_volume/mysql/conf:/etc/mysql/conf.d \
   -d mysql:<版本号>
   ```
   
   以上命令的说明：
   
   - `--name some-mysql`：给容器指定一个名称，比如 `some-mysql`。
   - `-e MYSQL_ROOT_PASSWORD=1234`：设置root用户的密码为 `1234`。
   - `mysql:latest`：使用最新版本的 MySQL 镜像。
   - `p`：设置端口，`3306:3306`中，前者是宿主机的端口，可以自定义，后者是映射到容器的端口。
   - `-v`：挂载容器里的文件（`~/docker_volume/mysql/data`为本地目录）。

3. **查看运行中的容器：**
   使用以下命令来查看容器：
   
   ```bash
   # 查看运行中的容器
   docker ps
   
   # 查看所用容器
   docker ps -a 
   
   # 格式化查看容器
   docker ps --format "table {{.ID}}\t{{.Image}}\t{{.Ports}}\t{{.Status}}\t{{.Names}}"
   
   # 命令简化（推荐加到.bashrc或者.zshrc中）
   alias dps='docker ps --format "table {{.ID}}\t{{.Image}}\t{{.Ports}}\t{{.Status}}\t{{.Names}}"'
   ```
   
   注：`-v`这里要先创建好对应的文件路径！！

4. **连接到 MySQL 容器：**
   
   ```bash
   # 进入到容器内部
   docker exec -it some-mysql bash
   
   # 连接到容器
   docker exec -it some-mysql mysql -uroot -p
   ```
   
   这会启动一个终端会话，并要求输入上面设置的root用户的密码 (`my-secret-pw`)。
   
   以上命令的说明：
   
   1. **`docker exec`**：
   - **含义**：在一个已经运行的容器内执行命令。
   
   - **用途**：允许你在运行的容器中执行新的命令。
   2. **`-it`**：
   - **`-i` (interactive)**：保持标准输入（stdin）打开，使得你可以与容器中的进程进行交互。
   
   - **`-t` (tty)**：分配一个伪终端，提供一个终端会话环境。这两个选项一起使用可以进入容器并交互。
   3. **`some-mysql`**：
   - **含义**：容器的名称或 ID。在这个例子中，`some-mysql` 是你之前启动的 MySQL 容器的名称。
   4. **`mysql`**：
   - **含义**：这是在容器内执行的命令。在这个例子中，是启动 MySQL 客户端。
   
   - **用途**：连接到 MySQL 数据库。
   5. **`-uroot`**：
   - **`-u`**：指定 MySQL 客户端的用户名。
   
   - **`root`**：MySQL 数据库的用户名。在这个例子中，使用的是 `root` 用户。
   6. **`-p`**：
   - **含义**：提示输入 MySQL 用户的密码。
   
   - **用途**：在执行命令后，你会被提示输入 `root` 用户的密码。这个密码是在运行容器时通过 `MYSQL_ROOT_PASSWORD` 环境变量设置的。
     
     以上命令的 `-p 3306:3306` 会将主机的3306端口映射到容器的3306端口，这样可以从主机上的MySQL客户端连接到容器中的MySQL服务。

## 2.更改容器信息

### 方法一、停止并删除现有容器

首先，停止并删除现有的 `mysql-test` 容器：

```bash
docker start mysql-test        # 运行
docker stop mysql-test        # 停止
docker rm mysql-test        # 删除
```

然后再重新创建`mysql-test`即可。

### 方法二、不同的容器名称

可以使用不同的名称来运行容器，这样就不需要删除现有的容器。例如：

```bash
docker run --name mysql-test-2 -e MYSQL_ROOT_PASSWORD=0403 -p 3307:3306 -d mysql:latest
```

这样会启动一个新容器，名称为 `mysql-test-2`，并绑定主机的3307端口到容器的3306端口。

## 3.查看容器信息

```
docker inspect <容器名>
```

可以查看容器内部信息（如端口，ip地址，挂载卷等）。

# 二、docker-PHP

## 1.基本操作

1. **拉取 nginx 镜像**
   
   ```bash
   docker pull nginx
   ```

2. **创建并运行 nginx 容器**
   
   ```bash
   docker run --name nginx -p 8083:80 -d \
   -v ~/docker_volume/nginx/www:/usr/share/nginx/html:ro \
   -v ~/docker_volume/nginx/conf.d:/etc/nginx/conf.d:ro \
   --link php-fpm:php \
   nginx
   ```
   
   这里的`ro`表示只读卷的意思，不能对挂载卷进行修改（可以不要）！
   浏览器输入：`localhost:8080`检查是否运行成功。

3. **拉取 php 镜像**
   
   ```bash
   docker pull php:7.4-fpm
   ```

4. **创建并运行 php 容器**
   
   ```bash
   docker run --name php-fpm -v ~/docker_volume/nginx/www:/www -d php:7.4-fpm
   ```

# 三、docker-ubuntu

- **运行并创建容器：**
  
  ```bash
  docker run -itd --name ubuntu-test ubuntu
  ```

- **进入容器：**

```bash
docker exec -it ubuntu-test bash 
```

  要退出正在交互模式下的容器终端，可以执行以下操作：

1. 按下 `Ctrl + D` 快捷键。
2. 或者在容器终端中键入 `exit` 命令并按回车键。

# 补充

## 1.配置代理

1.创建`dockerd`相关的`systemd`目录，这个目录下的配置将覆盖`dockerd`的默认配置：

```bash
sudo mkdir -p /etc/systemd/system/docker.service.d
```

2.新建配置文件`/etc/systemd/system/docker.service.d/http-proxy.conf`，这个文件中将包含环境变量：

```
[Service]
Environment="HTTP_PROXY=http://127.0.0.1:2334"
Environment="HTTPS_PROXY=http://127.0.0.1:2334"
```

这里的`2334`是`hiddify-next`的默认端口。

3.如果你自己建了私有的镜像仓库，需要`dockerd`绕过代理服务器直连，那么配置`NO_PROXY`变量：

```
[Service]
Environment="HTTP_PROXY=http://proxy.example.com:80"
Environment="HTTPS_PROXY=https://proxy.example.com:443"
Environment="NO_PROXY=your-registry.com,10.10.10.10,*.example.com"
```

多个`NO_PROXY`变量的值用逗号分隔，而且可以使用通配符（*），极端情况下，如果`NO_PROXY=*`，那么所有请求都将不通过代理服务器。

4.重新加载配置文件，重启`dockerd`

```bash
sudo systemctl daemon-reload
sudo systemctl restart docker
```

5.检查确认环境变量已经正确配置：

```bash
sudo systemctl show --property=Environment docker
```

像这样就成功了：

```
╰─❯ sudo systemctl show --property=Environment docker        
[sudo] sky 的密码：
Environment=HTTP_PROXY=http://127.0.0.1:2334 HTTPS_PROXY=http://127.0.0.1:2334
```
