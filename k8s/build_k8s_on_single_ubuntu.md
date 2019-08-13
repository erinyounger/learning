## 在Ubuntu上建立单机版K8s

###　环境准备
１．　翻墙并设置代理(过程略过)　
export http_proxy=http://127.0.0.1:19181
export https_proxy=http://127.0.0.1:19181
export no_proxy=192.168.1.118 # 你电脑的ip地址　

### 安装

１．　安装Ｄｏｃｋｅｒ　
- step 1: 安装必要的一些系统工具　
sudo apt-get update
sudo apt-get -y install apt-transport-https ca-certificates curl software-properties-common　
- step 2: 安装GPG证书　
curl -fsSL http://mirrors.aliyun.com/docker-ce/linux/ubuntu/gpg | sudo apt-key add -　
- Step 3: 写入软件源信息　
sudo add-apt-repository "deb [arch=amd64] http://mirrors.aliyun.com/docker-ce/linux/ubuntu $(lsb_release -cs) stable"　
- Step 4: 更新并安装 Docker-CE　
sudo apt-get -y update　
sudo apt-get -y install docker-ce　

2. 安装kubeadm和kubelet等依赖　
```
sudo apt-get update
sudo apt-get install -y kubelet kubeadm kubectl kubernetes-cni
```
3. 配置并启动Ｄｏｃｋｅｒ　
```
sudo vim /etc/systemd/system/multi-user.target.wants/docker.service
增加
[Service]
Environment="HTTP_PROXY=http://127.0.0.1:19181/"
Environment="HTTPS_PROXY=http://127.0.0.1:19181/"
```
