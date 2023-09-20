# Linux swap 分区配置


# Linux swap分区配置

## 创建swap文件

### 创建swap文件夹
```bash
cd /usr
mkdir swap
cd swap/
```

### 创建swapfile文件
```bash
dd if=/dev/zero of=/usr/swap/swapfile bs=1M count=4096
```

### 查看swap文件
```bash
du -sh /usr/swap/swapfile
```

## 设置swap分区

### 将目标设置为swap分区文件
```bash
mkswap /usr/swap/swapfile
swapon /usr/swap/swapfile
```

### 查看现在的内存
```bash
free -m
```

### 设置开机自动启用虚拟内存
在etc/fstab文件中加入命令
```bash
vim /etc/fstab
```
末尾添加
```txt
/usr/swap/swapfile swap swap defaults 0 0
```

### 重启服务器
```bash
reboot
```
重启之后查看现在的内存
```bash
free -m
```
