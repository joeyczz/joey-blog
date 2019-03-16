---
title: 免费SSL
date: 2019-03-16 18:23:26
tags: [SSL]
---

## Let’s Encrypt 免费ssl

### 使用Certbot

使用推荐的Certbot方式

#### 安装certbot 

```
yum install certbot
```

#### 推荐使用certonly命令

```
<!--通配符   这样能支持 www.example.com、example.com、a.example.com-->
certbot certonly  -d *.example.com --manual --preferred-challenges dns --server https://acme-v02.api.letsencrypt.org/directory
```

第一次会需要输入email，生成文件保存在/etc/letsencrypt/目录下
其他默认错了，输会中断
当看到以下内容需要注意
```
- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
Please deploy a DNS TXT record under the name
_acme-challenge.example.com with the following value:

xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx

Before continuing, verify the record is deployed.
- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
Press Enter to Continue
```
这个时候需要去你的域名服务商配置DNS解析

选择记录类型TXT
主机记录输 _acme-challenge
记录值TXT值 输入value    xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
配置完保存

通过下面的命令查看DNS是否生效
看到对应的TXT值（value）后继续回车
```
dig  -t txt  _acme-challenge.example.com
```

#### 生成ssl文件
在/etc/letsencrypt/live/example.com目录下 pullchain.pem privkey.pem

效验证书信息是否正确
```
certbot certificates
or
openssl x509 -in  /etc/letsencrypt/archive/example.com/cert1.pem -noout -text 
```

**证书有效期为3个月**

若需要续期可以手动执行
```
certbot renew
```
也可以配置 crontab 定时任务

### Nginx 配置

```
server {
    server_name www.example.com
    listen 442  ssl http2;
    charset utf-8;
    ssl_certificate         /etc/letsencrypt/live/example.com/pullchain.pem
    ssl_certificate_key     /etc/letsencrypt/live/example.com/privkey.pem
    localtion / {
        root    /www
        index   index.html
    }
}

```
 