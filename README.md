# CentOS7
#ssh-keygen -t rsa -C "admin@redname.com"
git add --all
#
git commit -m 'first commit'
#
git remote add origin git@github.com:wangyonglin/Tengine.git
#
git push -u origin master

#SELinux 安全配置
查看SELinux状态：

1、/usr/sbin/sestatus -v      ##如果SELinux status参数为enabled即为开启状态

SELinux status:                 enabled

2、getenforce                 ##也可以用这个命令检查

关闭SELinux：

1、临时关闭（不用重启机器）：

setenforce 0                  ##设置SELinux 成为permissive模式

 ##setenforce 1 设置SELinux 成为enforcing模式

2、修改配置文件需要重启机器：

修改/etc/selinux/config 文件

将SELINUX=enforcing改为SELINUX=disabled

#使用logrotate管理nginx日志文件
描述：linux日志文件如果不定期清理，会填满整个磁盘。这样会很危险，因此日志管理是系统管理员日常工作之一。我们可以使用"logrotate"来管理linux日志文件，它可以实现日志的自动滚动，日志归档等功能。下面以nginx日志文件来讲解下logrotate的用法。

配置：
1、在/etc/logrotate.d目录下创建一个nginx的配置文件"nginx"配置内容如下
#vim /etc/logrotate.d/nginx
/usr/local/nginx/logs/*.log {
daily
rotate 5
missingok
notifempty
sharedscripts
postrotate
    if [ -f /usr/local/nginx/logs/nginx.pid ]; then
        kill -USR1 `cat /usr/local/nginx/logs/nginx.pid`
    fi
endscript
}

保存退出。

2、执行logrotate
#/usr/sbin/logrotate -f /etc/logrotate.d/nginx

在/usr/local/nginx/logs目录中会产生
error.log
error.log.1
说明logrotate配置成功。

3、让logrotate每天进行一次滚动,在crontab中添加一行定时脚本。
#crontab -e
59 23 * * *  /usr/sbin/logrotate -f /etc/logrotate.d/nginx

每天23点59分进行日志滚动

4、配置文件说明
daily：日志文件每天进行滚动
rotate：保留最5次滚动的日志
notifempty：日志文件为空不进行滚动
sharedscripts：运行postrotate脚本
下面是一个脚本
postrotate
    if [ -f /usr/local/nginx/logs/nginx.pid ]; then
        kill -USR1 `cat /usr/local/nginx/logs/nginx.pid`
    fi
endscript

脚本让nginx重新生成日志文件。
centos7 开启80端口
最近在虚拟机上安装了centos7，安装nginx之后虚拟机内能访问，真机不能访问，修改iptables配置也不起作用，最后上网查找了资料后才发现centos的防火墙改成了firewall,不再叫iptables,开放端口的方法如下：
[plain] view plain copy
firewall-cmd --zone=public --add-port=80/tcp --permanent  

命令含义：
 
--zone #作用域
 
--add-port=80/tcp  #添加端口，格式为：端口/通讯协议
 
--permanent   #永久生效，没有此参数重启后失效

重启防火墙：
[plain] view plain copy
systemctl stop firewalld.service  
systemctl start firewalld.service  
