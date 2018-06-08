#FreeRadius
##安装FreeRadius

以CentOS7 安装为例

```
yum install freeradius freeradius-utils
yum install openssl
```
radius server目录

```
[root@localhost raddb]# pwd
/etc/raddb
[root@localhost raddb]# ls
README.rst  clients.conf  hints       mods-available  mods-enabled  policy.d    radiusd.conf       sites-available  templates.conf  users
certs       dictionary    huntgroups  mods-config     panic.gdb     proxy.conf  radiusd.conf_evan  sites-enabled    trigger.conf
[root@localhost raddb]#
```

安装radius自测工具eapol_test  
eapol_test是WPA_supplicant中的一个软件

```
yum install WPA_supplicant
```
 

##添加raidus client
此处的client指的是与Radius server联动的设备，例如AP。

```
[root@localhost ~]# cd /etc/raddb/
[root@localhost raddb]# vi clients.conf
[root@localhost raddb]#
```
在/etc/raddb/clients.conf中添加readius client信息

```
client utopia_juniper_firewall {
        ipaddr          = 10.220.61.60  # AP的IP address
        secret          = Polycom123    # 与radius server协商的秘钥

}
```

##添加Radius server认证用户
在/etc/raddb/users文件中添加需要认证的用户信息

```
utopia  Cleartext-Password := "123456"
        Framed-IP-Address = 192.168.5.1,
        Framed-IP-Netmask = 255.255.255.0,
        Reply-Message := "Hello, it is from evan's radius server!"
```

##使用eapol_test进行测试
注意：配置文件中的字符区分大小写

- peap-gtp

```
eapol_test -c /evan/testFreeRadius/peap-gtp.cfg -s testing123
  3
  4
  5 network={
  6         ssid="example"
  7         key_mgmt=WPA-EAP
  8         eap=PEAP
  9         identity="test"
 10         anonymous_identity="anonymous"
 11         password="123456"
 12         phase2="autheap=GTC"
 13
 14
 15 }

```
 - peap-mschapv2 
 
```
 eapol_test -c /evan/testFreeRadius/peap-mschapv2.cfg -s testing123
  3
  4 network={
  5         ssid="example"
  6         key_mgmt=WPA-EAP
  7         eap=PEAP
  8         identity="test"
  9         anonymous_identity="anonymous"
 10         password="123456"
 11         phase2="autheap=MSCHAPV2"
 12 }
```
 
 - tls
 
```
 
  1 network={
  2     eap=TLS
  3     eapol_flags=0
  4     key_mgmt=IEEE8021X
  5     identity="test"
  6     password="123456"
  7
 13
 17     client_cert="/etc/raddb/certs/client.pem"
 18     private_key="/etc/raddb/certs/client.key"
 19     private_key_passwd="whatever"
 20
 26 }
~
~ 
```
- ttls-gtc

```
  5 network={
  6         ssid="example"
  7         key_mgmt=WPA-EAP
  8         eap=TTLS
  9         identity="test"
 10         anonymous_identity="anonymous"
 11         password="123456"
 12         phase2="autheap=GTC"
 13
 14
 15 }
```

- ttls-md5

```
  2 network={
  3         ssid="example"
  4         key_mgmt=WPA-EAP
  5         eap=TTLS
  6         identity="test"
  7         anonymous_identity="anonymous"
  8         password="123456"
  9         phase2="autheap=MD5"
 11 }
```
- ttls-mschap

```
  3 network={
  4         ssid="example"
  5         key_mgmt=WPA-EAP
  6         eap=TTLS
  7         identity="test"
  8         anonymous_identity="anonymous"
  9         password="123456"
 10         phase2="auth=MSCHAP"
 11 }
```
 
- ttls-mschapv2

```
  2 network={
  3         ssid="example"
  4         key_mgmt=WPA-EAP
  5 #        key_mgmt=IEEE8021X
  6         eap=TTLS
  7         identity="test"
  8         anonymous_identity="anonymous"
  9         password="123456"
 10         phase2="autheap=MSCHAPV2"
 11 #       ca_cert="/etc/raddb/certs/ca.pem"
 12 }
```

- ttl-pap

```
  2 network={
  3         ssid="example"
  4         key_mgmt=WPA-EAP
  5 #        key_mgmt=IEEE8021X
  6         eap=TTLS
  7         identity="test"
  8         anonymous_identity="anonymous"
  9         password="123456"
 10         phase2="autheap=MSCHAPV2"
 11 #       ca_cert="/etc/raddb/certs/ca.pem"
 12 }
```
 

##关闭防火墙 或是 iptables

```
[root@localhost ~]# systemctl stop firewalld.service            //停止firewall
[root@localhost ~]# systemctl disable firewalld.service        //禁止firewall开机启动
```

如果没有，会抓到如下报文

```
[root@localhost raddb]# tcpdump -i ens192 host 172.21.119.63. 
tcpdump: verbose output suppressed, use -v or -vv for full protocol decode. 
listening on ens192, link-type EN10MB (Ethernet), capture size 262144 bytes. 
02:04:27.789183 IP 172.21.119.63.51886 > localhost.localdomain.radius: RADIUS, Access-Request (1), id: 0x7f length: 169
02:04:27.789270 IP localhost.localdomain > 172.21.119.63: ICMP host localhost.localdomain unreachable - admin prohibited, length 205
02:04:32.476576 IP 172.21.119.63.51886 > localhost.localdomain.radius: RADIUS, Access-Request (1), id: 0x80 length: 169
02:04:32.476629 IP localhost.localdomain > 172.21.119.63: ICMP host localhost.localdomain unreachable - admin prohibited, length 205
```
##调试
1. 关闭radiusd server，通过 radius -X启动radius（radius debug mode），然后在尝试连接radius服务器，此时会打印出详细log。  
```
systemctl stop radiusd
radiusd -X
```
2. 通过抓包查看radius协商报文  
```
tcpdump -i [interface] host [APipaddress] 
```
3. 查看radius log文件  
```
cat /var/log/radius/radiusd.log
```

##证书
- andriod证书： client证书，只支持.p12格式的；CAroot证书没有要求. 

- 修改证书路径:

```
[root@localhost raddb]# pwd
/etc/raddb
[root@localhost raddb]# vi radiusd.conf


 69 certdir = /etc/raddb/certs
 70 cadir   = /etc/raddb/certs
```

- 修改证书文件名：

```
[root@localhost mods-enabled]# pwd
/etc/raddb/mods-enabled
[root@localhost mods-enabled]# vi eap

173         tls-config tls-common {
174                 private_key_password = whatever
175                 private_key_file = ${certdir}/server.pem
187                 certificate_file = ${certdir}/server.pem
200                 ca_file = ${cadir}/ca.pem

```

- freeradius 生成证书.

/etc/raddb/certs/，   
修改证书参数：   

```
vi ca.cnf
vi client.cnf
vi server.cnf
```
生成一个证书（以client证书为例）：

```
make ca.pem
or
make server.csr
```

生成一整套证书：  

```
./bootstrap
``` 
 
详细操作查看帮助文档，/etc/raddb/certs/README


##Note
- 每次修改完配置文件，最后重启服务systemctl restart radiusd。
- 如果systemctl start radiusd失败，通过radiusd -X查看失败log。
- 通过证书进行认证：首先，注意证书是否在有效期内；其次，注意设备的时间是否在证书的有效期内。
