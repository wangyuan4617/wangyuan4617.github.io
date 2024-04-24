---
title: vlmcsd 部署
date: 2024-04-24 22:36:43
tags: [kms, windows, linux]
---

### 在 Debian 中部署

#### 1、首先从 Github 中下载构建好的二进制文件

https://github.com/Wind4/vlmcsd

#### 2、在目录中找到适合运行环境的文件

```
例如：\binaries\binaries\Linux\intel\static\vlmcsd-x64-musl-static
```

#### 3、通过 SFTP 上传到 Debian 的目录中，再复制到合适的位置

```
cp /temp/vlmcsd-x64-musl-static /mnt/vlmscd/
```

#### 4、给文件添加可执行权限

```
chmod 755 /mnt/vlmscd/vlmcsd-x64-musl-static
```

#### 5、创建系统服务配置

```
// 新建一个文件 vlmcsd.service
[Unit]
Description=kms激活服务
After=network.target
[Service]
Type=forking
PIDFile=/var/run/vlmcsd.pid
ExecStart=/mnt/vlmcsd/vlmcsd-x64-musl-static -p /var/run/vlmcsd.pid
ExecStop=/bin/kill -HUP $MAINPID
PrivateTmp=true
[Install]
WantedBy=multi-user.target
```

#### 6、复制文件到 systemctl 的配置文件目录下

```
cp /mnt/vlmcsd.service /lit/systemd/system/
```

#### 7、重新加载一下 systemctl,并启动服务

```
// 重新加载systemctl
systemctl daemon-reload

// 启动vlmcsd

systemctl start vlmcsd.service
```

#### 8、其他

记得打开防火墙端口，默认 1688
