### docker安装mysql

1. ##### 下载镜像文件

```
docker pull mysql:5.7
```

#####   2. 创建实例并启动

```
docker run -p 3306:3306 --name mysql \
-v /mydata/mysql/log:/var/log/mysql \
-v /mydata/mysql/data:/var/lib/mysql \
-v /mydata/mysql/conf:/etc/mysql \
-e MYSQL_ROOT_PASSWORD=root \
-d mysql:5.7
```

3. ##### Mysql 配置

```
vi /mydata/mysql/conf/my.cnf
[client]
default-character-set=utf8

[mysql]
default-character-set=utf8

[mysqld]
init_connect='SET collation_connection=utf8_unicode_ci'
init_connect='SET NAMES utf8'
character-set-server=utf8
collation-server=utf8_unicode_ci
skip-character-set-client-handshake
skip-name-resolve
```

[^注意]: 解决Mysql连接慢的问题，在配置文件中加入如下，并重启mysql ，命令： docker restart mysql

[mysqld]

skip-name-resolve

解释：

skip-name-resolve：跳过域名解析

**4. 跟随docker重启**

```
# sudo docker update mysql --restart=always
```

