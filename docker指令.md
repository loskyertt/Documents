# 一、docker-MySQL

## 1.基本操作

1. **拉取 MySQL 镜像：**
   通过运行 `docker pull mysql[:版本号]`，版本号可选，默认是latest。

2. **运行 MySQL 容器：**
   使用以下命令来运行 MySQL 容器。这里假设想在主机上绑定3306端口，并设置一个root用户的密码：
   
   ```bash
   docker run --name some-mysql -e MYSQL_ROOT_PASSWORD=my-secret-pw -d mysql:latest
   ```
   
   以上命令的说明：
   
   - `--name some-mysql`：给容器指定一个名称，比如 `some-mysql`。
   - `-e MYSQL_ROOT_PASSWORD=my-secret-pw`：设置root用户的密码为 `my-secret-pw`。
   - `-d`：让容器在后台运行。
   - `mysql:latest`：使用最新版本的 MySQL 镜像。

3. **查看运行中的容器：**
   使用以下命令来查看运行中的容器：
   
   ```bash
   docker ps
   ```

4. **连接到 MySQL 容器：**
   你可以通过以下命令连接到容器中的 MySQL 实例：
   
   ```bash
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

5. **绑定端口（可选）：**
    如果想要在主机上绑定3306端口，可以运行以下命令：
   
   ```bash
   docker run --name some-mysql -e MYSQL_ROOT_PASSWORD=my-secret-pw -p 3306:3306 -d mysql:latest
   ```
   
    以上命令的 `-p 3306:3306` 会将主机的3306端口映射到容器的3306端口，这样可以从主机上的MySQL客户端连接到容器中的MySQL服务。

## 2.更改容器信息

### 方法一、停止并删除现有容器

首先，停止并删除现有的 `mysql-test` 容器：

```bash
docker stop mysql-test
docker rm mysql-test
```

然后，重新运行绑定端口的命令：

```bash
docker run --name mysql-test -e MYSQL_ROOT_PASSWORD=0403 -p 3306:3306 -d mysql:latest
```

### 方法二、不同的容器名称

你可以使用不同的名称来运行容器，这样就不需要删除现有的容器。例如：

```bash
docker run --name mysql-test-2 -e MYSQL_ROOT_PASSWORD=0403 -p 3306:3306 -d mysql:latest
```

这样会启动一个新容器，名称为 `mysql-test-2`，并绑定主机的3306端口到容器的3306端口。
