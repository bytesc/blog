# squid 安装配置


# squid安装配置

## 安装
```bash
sudo apt update
sudo apt install squid
```

## 服务
```bash
sudo systemctl status squid
netstat -an | grep 3145
# 默认3145端口
sudo systemctl start squid
sudo systemctl stop squid
```

## 配置
```bash
sudo vim /etc/squid/squid.conf
```
文件内容 16% 处
```txt
# http_access deny all
http_access allow all
```

22%处
```txt
# Squid normally listens to port 3128
http_port 3145
```

## 开机启动

```bash
sudo systemctl enable squid.service
sudo systemctl disable squid.service
```

