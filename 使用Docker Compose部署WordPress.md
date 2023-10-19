# 使用Docker Compose部署WordPress

1.创建一个项目目录，并切换到改项目目录

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

构建项目

docker-compose up -d



在浏览器中打开WordPress

http://主机IP:9000

第一次打开需注册

![image-20231019182540612](https://raw.githubusercontent.com/huangshoucheng/picBed/master/iamgeimage-20231019182540612.png)