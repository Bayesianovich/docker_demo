# 使用Docker Compose部署WordPress

1.创建一个项目目录，并切换到该项目目录

mkdir my_wordpress 

cd my_wordpress

2.在该目录下创建并编辑docker-compose.yml

```
version: '3.3'

services:
   db:
     image: mysql:5.7
     volumes:
       - db_data:/var/lib/mysql
     restart: always
     environment:
       MYSQL_ROOT_PASSWORD: wordpress
       MYSQL_DATABASE: wordpress
       MYSQL_USER: wordpress
       MYSQL_PASSWORD: wordpress

   wordpress:
     depends_on:
       - db
     image: wordpress:latest
     ports:
       - "9000:80"
     restart: always
     environment:
       WORDPRESS_DB_HOST: db:3306
       WORDPRESS_DB_USER: wordpress
       WORDPRESS_DB_PASSWORD: wordpress
       WORDPRESS_DB_NAME: wordpress
volumes:
    db_data: {}

```

3. 构建项目

docker-compose up -d


4. 在浏览器中打开WordPress

http://主机IP:9000

第一次打开需注册

![image-20231019182540612](https://raw.githubusercontent.com/huangshoucheng/picBed/master/iamgeimage-20231019182540612.png)

5.关闭docker-compose down 这会删除容器和默认网络，但会保留存储在卷中的WordPress数据库。要同时删除卷 添加 -volumes



💡 理解`docker-compose.yml`的基本组成部分和结构，逐步分解该文件：

1. **version**:
    - 这指的是你使用的`docker-compose.yml`的版本。不同的版本有不同的功能。
2. **services**:
    - 这是你要运行的所有容器的列表。在你的例子中，你有两个服务：**`db`**和**`wordpress`**。
3. **db**:
    - 这是你的数据库服务。
    - **`image: mysql:5.7`**: 使用MySQL 5.7镜像。
    - **`volumes`**: 将容器内的**`/var/lib/mysql`**目录映射到名为`db_data`的本地卷，以持久化数据库数据。
    - **`restart: always`**: 如果容器退出，始终重新启动它。
    - **`environment`**: 设置环境变量，这些变量是MySQL容器期望的，以便正确地初始化。
4. **wordpress**:
    - 这是你的WordPress服务。
    - **`depends_on`**: 它依赖`db`服务。
    - **`image: wordpress:latest`**: 使用最新的WordPress镜像。
    - **`ports`**: 将容器的80端口映射到主机的9000端口。
    - **`restart: always`**: 同样，如果容器退出，始终重新启动它。
    - **`environment`**: 设置WordPress容器期望的环境变量，以便连接到数据库。
5. **volumes**:
    - 这是一个定义数据卷的区域。在这种情况下，你只是定义了一个名为`db_data`的卷，而不是指定它在主机上的具体位置，所以它将在Docker的默认位置创建。
