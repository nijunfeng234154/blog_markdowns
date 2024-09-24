## linux系统注意事项

在linux系统中我们报python os库error 没有写权限 但我们不是root权限时，

我们使用sudo mkdir 要写入的目录 

chomod 777 写入目录名 

然后在直接运行python代码就可以了

### 批量文件操作

今天遇到一个场景需要在文件夹里批量解压。

这时候需要用到shell语句，可以学习一下常用的循环：

`for i in $(ls *.zst);do unzstd $i;done`

相信这样的场景以后还会遇到，如果原生的组件没有写批量化功能的话。

#### 批量删除

使用`ls|xargs rm` 到目标文件夹下将所有文件删除

如何递归删除目录？

`ls|xargs rm -r`

### tar命令的权限问题

`*$ \*tar\* -\*c* -\*f* \*archive\*.*tar\* --\*owner\*=0 --\*group\*=0 --\*no-same-owner\* --\*no-same-permissions\* .*`

tar命令打包时会把原有的用户权限信息也给打包进去 

导致你解压之后的代码运行时可能会遇到权限问题

使用 --no-same-owner

--no-same-permissions 参数可以忽略这些权限信息

加参数不一定好使 最后解决办法是原来要压缩的目录全改为777权限，在打包压缩，之后解压运行正常

### find命令在文件中找字符串

- `find .`：从当前目录开始查找。
- `-type f`：只查找文件。
- `-name "*.txt"`：只匹配文件名以`.txt`结尾的文件。
- `-exec`：对找到的每个文件执行后面的命令。
- `grep -H "要搜索的字符串"`：使用`grep`搜索包含指定字符串的行，并使用`-H`选项打印出包含匹配行的文件名。
- `{}`：表示`find`命令找到的文件名。
- `+`：表示将所有找到的文件名作为参数一次性传递给`grep`，而不是逐个文件执行。

如果你只想查找包含完整单词的字符串，可以在`grep`命令中添加`-w`选项：

```linux
find . -type f -name "*.txt" -exec grep -wH "要搜索的字符串" {} +
```

### 查询特定修改时间的文件

使用管道命令结合grep

```
grep "01/APR/2014:16:3[5-9]" filename
```

```bash
ls -al | grep "Dec  1" | grep 02:00
```

linux系统时间和现实时间不一定一致

### linux系统时间对齐

对其标准服务器上的时间

使用工具ntpdate

`ntpdate -u cn.pool.ntp.org :网络时间同步命令`

也可以用别的服务器

### 删除指定名称的docker容器

需要注意正则表达式的语法

`docker rmi $(docker images|grep "(./*?)")`

### 查找特定时间的目录/文件

使用find命令

查找某一修改时间后的文件

```text
find /path/to/search -type f -newermt '2023-01-01 00:00:00'
```

这个时间是正常的你想的那个时间

指定递归目录级深度参数  -maxdepth 1

### crontab设置定时任务来定时校准服务器时间

`* * * * * ntpdate -u cn.pool.ntp.org`

### Linux screen来控制后台运行程序

screen常用命令：

创建一个虚拟终端:

`screen -R name`

进去运行程序

ctrl+a, 再按d 退出终端

`screen -ls` 来列出当前开的虚拟终端

继续连接到虚拟终端

`screen -R/-r name`

清除终端:

进入对应虚拟终端，然后输入exit

`screen -d name`
将终端设置为后台运行（detached）如果没这么做，screen -ls会显示 attached状态（前台运行）也可以用crtl+a,d

### linux系统密码找回/重置

ubuntu重启按shift键进入recovery mode

开个root terminal

然后passwd改密码

待更新（填坑）：使用linux系统镜像来重置密码

### ubuntu配置网卡配置文件，配置静态ip、网关等

vim /etc/netplan/xxxx.yaml

sudo netplan generate

sudo netplan apply

## 0.0.0.0和127.0.0.1的区别

0.0.0.0 表示出网ip 监听所有ip

127.0.0.1 表示本机回环地址

### Docker常见操作整理

docker run 参数：-p指定容器端口和服务映射宿主机端口

-v指定数据保存的文件路径

-d指定使用的镜像和版本 如mysql:8.2

-e指定一些变量的赋值

--name指定生成的container名称

例子：

`docker run --name test-mysql -e MYSQL_ROOT_PASSWORD=strong_password -d mysql`

进入运行的docker容器

`docker exec -it name bash`

ctrl+d从docker容器内环境退出

拉取docker镜像,指定版本

`docker pull mysql:8.2`

容器操作，停止/重启/删除（停止和删除容器的区别是什么？）

`docker stop name`

`docker restart name` 

`docker rm name`

查看运行中容器列表

`docker ps -a`

查看已下载的docker镜像

`docker images`

删除docker镜像

### 以mb为单位查看linux系统文件大小

`ls -l --block-size=M`

似乎有其他的命令行工具
