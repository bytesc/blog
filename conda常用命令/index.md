# conda 常用命令


# conda 常用命令

## linux 安装
```bash
wget https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh
bash Miniconda3-latest-Linux-x86_64.sh
# 一直输入yes
```

## 查看环境
```bash
conda env list 
conda config --set auto_activate_base false
```

## 环境管理
```bash
# 创建环境
conda create -n env_name python=x.x
# 删除环境
conda remove -n env_name --all
# 激活环境
conda activate env_name
# 关闭环境
conda deactivate
```

## package 管理
```bash
conda list
# 安装某个package
conda install [package]
# 删除某个package
conda remove [package]
# 更新某个package
conda update [package]
# 更新conda，保持conda最新
conda update conda
```
