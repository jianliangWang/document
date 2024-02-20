## Linux 防火墙

### 防火墙状态

启动： systemctl start firewalld

查看状态： systemctl status firewalld

禁用，禁止开机启动： systemctl disable firewalld

停止运行： systemctl stop firewalld

### 配置firewalld-cmd

查看版本： firewall-cmd --version

查看帮助： firewall-cmd --help

显示状态： firewall-cmd --state

查看所有打开的端口： firewall-cmd --zone=public --list-ports

更新防火墙规则： firewall-cmd --reload

更新防火墙规则，重启服务： firewall-cmd --completely-reload



查看已激活的Zone信息: firewall-cmd --get-active-zones
查看指定接口所属区域： firewall-cmd --get-zone-of-interface=eth0
拒绝所有包：firewall-cmd --panic-on
取消拒绝状态： firewall-cmd --panic-off
查看是否拒绝： firewall-cmd --query-panic

### 开启关闭端口

添加：
firewall-cmd --zone=public --add-port=80/tcp --permanent （--permanent永久生效，没有此参数重启后失效）
重新载入：
firewall-cmd --reload
查看：
firewall-cmd --zone=public --query-port=80/tcp
删除：
firewall-cmd --zone=public --remove-port=80/tcp --permanent

### 管理服务

以smtp服务为例， 添加到work zone
添加：
firewall-cmd --zone=work --add-service=smtp
查看：
firewall-cmd --zone=work --query-service=smtp

删除：
firewall-cmd --zone=work --remove-service=smtp

### 配置 IP 地址伪装

查看：
firewall-cmd --zone=external --query-masquerade
打开：
firewall-cmd --zone=external --add-masquerade
关闭：
firewall-cmd --zone=external --remove-masquerade

### 端口转发

打开端口转发，首先需要打开IP地址伪装 ：
firewall-cmd --zone=external --add-masquerade
转发 tcp 22 端口至 3753：
firewall-cmd --zone=external --add-forward-port=22:porto=tcp:toport=3753
转发端口数据至另一个IP的相同端口：
firewall-cmd --zone=external --add-forward-port=22:porto=tcp:toaddr=192.168.1.112
转发端口数据至另一个IP的 3753 端口：
firewall-cmd --zone=external --add-forward-port=22:porto=tcp:：toport=3753:toaddr=192.168.1.112



## 配置静态IP地址：

vim /etc/sysconfig/network-scripts/ifcfg-ens3

```pro
TYPE="Ethernet"
PROXY_METHOD="none"
BROWSER_ONLY="no"
BOOTPROTO="static"  // 这里改成静态
DEFROUTE="yes"
IPV4_FAILURE_FATAL="no"
IPV6INIT="yes"
IPV6_AUTOCONF="yes"
IPV6_DEFROUTE="yes"
IPV6_FAILURE_FATAL="no"
IPV6_ADDR_GEN_MODE="stable-privacy"
NAME="ens33"
UUID="6c04285f-3769-475c-8787-71b3c1c1f44e"
DEVICE="ens33"
ONBOOT="yes"
IPADDR=192.168.12.131 // 这里是静态IP地址
NETMASK=255.255.255.0  // 子网掩码
GATEWAY=192.168.12.2  // 这里要注意，一般vm8的网关地址是这个，这里要是错了就不行了，如果不知道是什么先将ip设置为动态看下，再改成一样的
```

重启网络：systemctl restart network

如果不能访问外网：

配置：vim /etc/resolv.conf

```pro
nameserve 192.168.2.2  
```

和网关地址一样