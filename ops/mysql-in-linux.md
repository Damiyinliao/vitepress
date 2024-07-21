<center>在Linux上安装Mysql</center>

##### 准备
> 环境：CentOS 7

##### 一、安装前的环境检查

1、检查是否安装过mysql

```bash
ps ajx | grep mysql          // 检查 是否有 mysql 的进程
```

![image-20240715141052410](http://img.filmgallery.cn/md/image-20240715141052410.png)

如图所示没有关于mysql的进程

2、检查是否含有mariabd

```bash
ps ajx | grep mariabd
```

![image-20240715141255696](http://img.filmgallery.cn/md/image-20240715141255696.png)

同样也是，没有mariabd的相关进程

---

- 如果发现有进程那么就关闭进程

  ```bash
  bash systemctl stop mysqld  // 关闭进程
  ```

  

3、其次检查是否有mysql的安装包

```bash
rpm -qa | grep mysql    // 检查是否有安装包
```

![image-20240715141806685](http://img.filmgallery.cn/md/image-20240715141806685.png)

这就表示含有相关的安装包，如果没有那么就没有任何的反应

---

- 删除安装包

  ```bash
  rpm -qa | grep mysql  | xargs  yum -y remove  // 批量化 删除安装包
  ```

- 检查是否含有残留

  ```bash
  ls /etc/my.cnf    // 检查是否有 配置文件
  ```

  ![image-20240715142244184](http://img.filmgallery.cn/md/image-20240715142244184.png)

根据提示，没有。

---

- 如果有，则删除

  ```bash
  rm -rf /etc/my.cnf    // 删除配置文件
  ```

4、检查服务

```bash
which mysql         // ------------ 检查 是否有客户端
 
which mysqld        // ------------ 检查 是否有服务端
```

![image-20240715142501106](http://img.filmgallery.cn/md/image-20240715142501106.png)

至此，就可以安全安装了~~~

##### 二、下载官方的安装包

1、检查自己的系统版本

```bash
cat /etc/redhat-release
```

![image-20240715142646067](http://img.filmgallery.cn/md/image-20240715142646067.png)

2、进入官网找到适合自己系统的版本(<http://repo.mysql.com>)

![image-20240715143027078](http://img.filmgallery.cn/md/image-20240715143027078.png)

3、将下载好的安装包上传

输入命令rz打开文件窗口

```bash
rz
```

![image-20240715151105796](http://img.filmgallery.cn/md/image-20240715151105796.png)

找到对应的问价打开就行。

---

查看文件

```bash
ll
```

![image-20240715151520232](http://img.filmgallery.cn/md/image-20240715151520232.png)

4、解压上传的安装包

```bash
rpm -ivh mysql57-community-release-el7.rpm
```

![image-20240715151627179](http://img.filmgallery.cn/md/image-20240715151627179.png)

等待完成。

5、检查是否解压完成

```bash
ls /etc/yum.repos.d/ -l 
```

![image-20240715151907271](http://img.filmgallery.cn/md/image-20240715151907271.png)

##### 三、正式安装

1、下载

```bash
yum install -y mysql-community-server
```

![image-20240715152034351](http://img.filmgallery.cn/md/image-20240715152034351.png)

2、检查是否安装成功

```bash
ls /etc/my.cnf
which mysqld
shich mysql
```

![image-20240715152656300](http://img.filmgallery.cn/md/image-20240715152656300.png)

3、启动mysql程序

```bash
 systemctl start mysqld
```

4、检查是否成功

```bash
ps ajx | grep mysqld    // 检查进程是否启动成功
```

![image-20240715152831049](http://img.filmgallery.cn/md/image-20240715152831049.png)

##### 四、进入mysql

1、输入命令

```bash
mysql -u root -p
```

![image-20240715153802101](http://img.filmgallery.cn/md/image-20240715153802101.png)



提示没有密码，我们需要调整一下配置，采用无密码进入

打开配置文件

```bash
vim /etc/my.cnf
```

![image-20240715154555288](http://img.filmgallery.cn/md/image-20240715154555288.png)

代开文件后，按下A键输入

```bash
skip-grant-tables
```

随后按下esc 再如何:wq进行退出

2、重新启动mysql

```bash
systemctl restart mysqld   // 重新启动

mysql -u root -p    // 登录
```

![image-20240715154908505](http://img.filmgallery.cn/md/image-20240715154908505.png)

进入成功

3、更改密码

先刷新权限，不然会提示修改密码时报错：--skip-grant-tables option

```bash
flush privileges;
// 随后重新设置密码
set password for root@localhost = password('new_password');
```

4、退出去删除掉配置中的skip-grant-tables
