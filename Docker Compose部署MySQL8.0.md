# Docker Compose部署MySQL8.0

对于单个服务容器的部署，可以使用Docker命令轻松实现，但是如果涉及的选项和参数比较多，则采用compose文件实现更为方便。

vim docker-compose.yml

```yaml
version: '3.7'
services:
  mysql:
    image: mysql:8
    container_name: mysql8
    ports:
    - 3306:3306
    command: 
      --default-authentication-plugin=mysql_native_password
      --character-set-server=utf8mb4
      --collation-server=utf8mb4_general_ci
      --explicit_defaults_for_timestamp=true
      --lower_case_table_names=1
    environment:
    - MYSQL_ROOT_PASSWORD=root
    volumes:
    - /etc/localtime:/etc/localtime:ro
    - mysql8-data:/var/lib/mysql
volumes:
  mysql8-data: null


```

docker-compose config 进行验证和查看



启动mysql服务 docker-compose up -d

关闭服务

docker-compose down --v