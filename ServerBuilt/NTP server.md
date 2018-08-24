# NTP server
## 安装 ntp 
```
yum install ntp
```

## 修改配置文件

```
[root@localhost etc]# vi /etc/ntp.conf
```

- 配置本地PC为NTP server

```
 24 server 127.127.1.0
 25 server 127.127.1.0 stratum 8
```

- 配置NTP客户端IP  

```
 14 restrict 10.220.62.102 255.255.255.255  
 15 restrict 10.230.37.185 255.255.255.255  
```
- 启动ntp服务

```
systemctl enable ntpd
systemctl start ntpd
```

## 测试
在ntp client 同步时间

```
EvanLi:~ evanli$ date
Mon Jun  4 11:06:03 CST 2018
EvanLi:~ evanli$ sudo ntpdate 10.220.200.49
 4 Jun 10:01:30 ntpdate[93421]: step time server 10.220.200.49 offset -3911.198421 sec
EvanLi:~ evanli$ date
Mon Jun  4 10:01:34 CST 2018
```
