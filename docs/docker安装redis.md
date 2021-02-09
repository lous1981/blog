**1. 下载镜像文件**

`docker pull redis`

**2. 创建实例并启动**

```
mkdir -p /mydata/redis/conf
touch /mydata/redis/conf/redis.conf
```

```
docker run -p 6379:6379 --name redis -v /mydata/redis/data:/data \
-v /mydata/redis/conf/redis.conf:/etc/redis/redis.conf \
-d redis redis-server /etc/redis/redis.conf
```

**3. 检查**

`docker ps`

`docker exec -it redis redis-cli`

**4. 数据持久化**

```
#pwd
#ls
#vi /mydata/redis/conf/redis.conf
按键I添加
appendonly yes
:wq保存
```

**5. 跟随docker重启**

```
# sudo docker update redis --restart=always
```

