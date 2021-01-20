### 1. 安装linux虚拟机

1. 下载并安装VirtualBox https://www.virtualbox.org/，并且要开启CPU虚拟化；
2. 下载并安装Vagrant https://www.vagrantup.com/

3. 打开cmd窗口，运行 vagrant init centos/7 即可初始化一个centos7系统；

4. 运行vagrant up 即可启动虚拟机，系统root用户的密码是vagrant

5. vagrant其他命令：

   - vagrant ssh ：自动使用vagrant用户连接虚拟机
   - vagrant upload source [destination] [name|id]：上传文件
   - https://www.vagrantup.com/docs/cli

6. 默认虚拟机的ip地址不是固定的ip，开发并不方便

   修改vagrantfile

   config.vm.network "private_network",ip:"192.168.56.10"

   这里的ip需要在物理机下使用ipconfig命令找到VirtualBox Host-Only Network的ip地址。重新使用vagrant up启动机器即可。然后再vagrant ssh连接机器

7. 默认只允许ssh登录方式，围了后来操作方便，文件上传等，我们可以配置允许账号密码登录

8. vagrant ssh进去系统之后

   vi /etc/ssh/sshd config

   修改 PasswordAuthentication yes/no

   重启服务 service sshd restart

9. 以后可以使用提供的ssh连接工具直接连接。

   ```
   vagrant 内存不足的问题
   解决办法：
    
   一开始找到的是 C:\Users\nioth\Vagrantfile 文件里有一行配置（默认是注释掉的）：
     #config.vm.synced_folder ".", "/vagrant_data"
    
   于是修改这个文件，改成下面（MyVagrantSyncFolder是自己新建得文件夹，空的，放在C:\Users\nioth\下面）：
    
     config.vm.synced_folder "./MyVagrantSyncFolder", "/vagrant_data"
     再用vagrant reload 重启， 发现没有用，心里一紧，不会吧， 再用Everything软件搜一遍，发现另外还有一个地方有一个vagrant配置文件：
     C:\Users\nioth\.vagrant.d\boxes\centos-VAGRANTSLASH-7\2004.01\virtualbox\Vagrantfile
    
    打开发现有如下配置：
    
   Vagrant.configure("2") do |config|
     config.vm.base_mac = "5254004d77d3"
     config.vm.synced_folder ".", "/vagrant", type: "rsync"
   end
    
    
    
   终于找到你了！ 
   于是，修改为：
    
   Vagrant.configure("2") do |config|
     config.vm.base_mac = "5254004d77d3"
     config.vm.synced_folder "./MyVagrantSyncFolder", "/vagrant", type: "rsync"
   end
    
    
    
   （原来那个文件里的配置还给它注释掉，恢复原样。）
    
   保存文件，再用vagrant reload， 大功告成！
   现在是 ./MyVagrantSyncFolder 文件夹与 虚拟机中的/vagrant文件夹进行映射， 这样就不会将大量主机文件复制到虚拟机了。
   ```

   

### 2.安装docker

Docker安装文档：https://docs.docker.com/engine/install/centos/

1. 卸载系统之前的docker

```
$ sudo yum remove docker \
                  docker-client \
                  docker-client-latest \
                  docker-common \
                  docker-latest \
                  docker-latest-logrotate \
                  docker-logrotate \
                  docker-engine
```

```
$ sudo yum install -y yum-utils

$ sudo yum-config-manager \
    --add-repo \
    https://download.docker.com/linux/centos/docker-ce.repo
```

1. #### 安装docker

```
$ sudo yum install docker-ce docker-ce-cli containerd.io
```

2. #### 启动docker

```
$ sudo systemctl start docker
```

3. #### 验证docker是否生效

```
$ sudo docker run hello-world
```

4. #### 开机自启动

```
$ sudo systemctl enable docker
```

5. #### docker配置镜像加速

阿里云镜像加速地址：https://cr.console.aliyun.com/cn-hangzhou/instances/mirrors

```
sudo mkdir -p /etc/docker
sudo tee /etc/docker/daemon.json <<-'EOF'
{
  "registry-mirrors": ["https://yi2ktbf4.mirror.aliyuncs.com"]
}
EOF
sudo systemctl daemon-reload
sudo systemctl restart docker
```

