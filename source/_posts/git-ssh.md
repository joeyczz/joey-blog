---
title: git ssh config
date: 2019-03-15 14:28:25
tags:
---

生成rsa
```
ssh-keygen -t rsa -b 4096 -C "mail@mail.xxx"
```

id_rsa 是私钥（需要保密）	id_rsa.pub 公钥

可以命名 => github_rsa 生成如下

github_rsa	github_rsa.pub


ssh默认只读取id_rsa

```
ssh-add ~/.ssh/id_rsa
ssh-add ~/.ssh/github_rsa
```

.pub 内容设置在线上 