# 一.DNS信息收集 -NSLOOKUP



#### 1.1使用nslookup查看域名

```linux
┌──(root💀kali)-[~]
└─# nslookup www.baidu.com
Server:         192.168.121.2
Address:        192.168.121.2#53

Non-authoritative answer:
www.baidu.com   canonical name = www.a.shifen.com.
Name:   www.a.shifen.com
Address: 163.177.151.110
Name:   www.a.shifen.com
Address: 163.177.151.109	(解析到的ip地址)
```

#### 1.2 使用dig查看DNS（利用ip反查）

```
(root💀kali)-[~]
└─# dig -x 144.144.144.144 xuegod.cn any

; <<>> DiG 9.16.15-Debian <<>> -x 144.144.144.144 xuegod.cn any
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NXDOMAIN, id: 4120
;; flags: qr rd ra ad; QUERY: 1, ANSWER: 0, AUTHORITY: 0, ADDITIONAL: 0

;; QUESTION SECTION:
;144.144.144.144.in-addr.arpa.  IN      PTR

;; Query time: 8 msec
;; SERVER: 192.168.121.2#53(192.168.121.2)
;; WHEN: 六 11月 27 02:01:41 EST 2021
;; MSG SIZE  rcvd: 46

;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 30869
;; flags: qr rd ra; QUERY: 1, ANSWER: 4, AUTHORITY: 0, ADDITIONAL: 1

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 1280
; COOKIE: 92be0f3f309d9162d6bc3ce061a1d7b5a15a9192c0551e02 (good)
;; QUESTION SECTION:
;xuegod.cn.                     IN      ANY

;; ANSWER SECTION:
xuegod.cn.              600     IN      MX      10 mxbiz2.qq.com.
xuegod.cn.              600     IN      MX      5 mxbiz1.qq.com.
xuegod.cn.              86379   IN      NS      dns7.hichina.com.
xuegod.cn.              86379   IN      NS      dns8.hichina.com.

;; Query time: 256 msec
;; SERVER: 192.168.121.2#53(192.168.121.2)
;; WHEN: 六 11月 27 02:01:41 EST 2021
;; MSG SIZE  rcvd: 164

```



#### 1.3利用dig查询bind版本号

模板：

```
dig txt chaos VERSION.BIND
```



#### 1.4利用whois查询域名信息

```
┌──(root💀kali)-[~]
└─# whois xuegod.cn 
Domain Name: xuegod.cn
ROID: 20140908s10001s72166376-cn
Domain Status: ok
Registrant: 北京跳动未来科技有限公司
Registrant Contact Email: jianmingbasic@163.com
Sponsoring Registrar: 阿里云计算有限公司（万网）
Name Server: dns7.hichina.com
Name Server: dns8.hichina.com
Registration Time: 2014-09-08 10:52:31
Expiration Time: 2022-09-08 10:52:31
DNSSEC: unsigned
```



二.









































