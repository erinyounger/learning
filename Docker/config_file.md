### 配置类操作

#### 1. Docker代理文件配置位置

```shell
[root@eeric docker]# cat /etc/systemd/system/docker.service.d/proxy.conf 
[Service]
Environment="HTTP_PROXY=http://192.168.3.6:41091/"
Environment="HTTPS_PROXY=http://192.168.3.6:41091/"
Environment="NO_PROXY=localhost,127.0.0.1,.example.com"
```

#### 2. Docker配置tcp端口远程控制

```shell
[root@eeric wechat-spider]# cat /lib/systemd/system/docker.service
ExecStart=/usr/bin/dockerd -H fd:// --graph /home/docker --containerd=/run/containerd/containerd.sock -H tcp://0.0.0.0:2375
```

#### 3. Docker配置生效重启 

```shell
systemctl daemon-reload
systemctl restart docker
```