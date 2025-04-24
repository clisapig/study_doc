## docker方式零基础安装rustdesk(Debian)

### 1.准备工作

1.1更新系统

```
sudo apt update && sudo apt upgrade -y
```

1.2安装必要工具

```
sudo apt install -y curl wget
```

## 2.安装Docker

2.1一键安装Docker

```
curl -fsSL https://get.docker.com | sh
```

2.2启动Docker并设置开机自启

```
sudo systemctl start docker
sudo systemctl enable docker
```

2.3验证安装

```
sudo docker --version
```

## 3.安装RustDesk服务器

3.1创建数据目录

```
mkdir -p ~/rustdesk-server/data
cd ~/rustdesk-server
```

3.2下载`docker-compose.yml`

```
wget https://raw.githubusercontent.com/rustdesk/rustdesk-server/master/docker-compose.yml
```

3.3修改配置（可选）

编辑 `docker-compose.yml`，按需修改以下字段（默认可直接使用）：

```
environment:
  - RELAY=rustdesk.example.com  # 改为你的域名或服务器IP
  - ENCRYPTED_ONLY=1            # 仅加密连接（建议启用）
```

3.4 启动服务

```
sudo docker compose up -d
```

## 4.开放防火墙端口

```
sudo ufw allow 21115:21119/tcp
sudo ufw allow 8000/tcp
sudo ufw allow 21116/udp
sudo ufw reload
```

## 5.验证服务

5.1 检查容器状态

```
sudo docker ps

应看到 hbbs 和 hbbr 两个容器运行中
```

5.2 查看关键文件

```
cat ~/rustdesk-server/data/id_ed25519.pub
```

记录输出的公钥（客户端配置时需要）

## 6客户端配置

1. **下载客户端**：从 [RustDesk 官网](https://rustdesk.com/) 下载对应版本。
2. **设置服务器地址**：
   - ID 服务器：你的服务器IP或域名
   - 中继服务器：同上
   - Key：填写步骤 5.2 获取的公钥