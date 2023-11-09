# Git 常用命令


# Git常用命令

## 基本设置

### 设置用户信息

```bash
git config --global user.name " "
git config --global user.email " "
git config --list
```

### 创建仓库
```bash
git init
```

### 取消NTFS格式保护
```bash
git config core.protectNTFS false
```

### 改缓存大小

```bash
git config --global http.postBuffer 524288000
git config --global sendpack.sideband false

git config --global http.postBuffer 1048576000
git config --global https.postBuffer 1048576000
```

### 遇到 TLS 验证问题

```bash
git config --global http.sslVerify false
git config --global https.sslVerify false
```

### 遇到 DNS 问题

```bash
nslookup github.com 8.8.8.8
# nslookup github.com

ssh -v -T git@[nslookup 获取的ip地址]
```


## 基础操作指令

### 添加和提交

```bash
git clone [仓库地址] -b [分支名]
```

```bash
git add .
# (在.gitignore文件中添加文件名即可忽略文件  eg: “*.txt”  “.idea”)
git commit -m " "
```

### 状态和日志

```bash
git status
git log --pretty=oneline --abbrev-commit --all --
git log --decorate
git log --graph
git reflog
```

### 版本回退

```bash
git reset --hard commitID (版本回退)
#--soft模式会撤消当前提交，但保留更改。
#这意味着，已撤消的提交的更改将重新出现在暂存区中，可以重新提交。
#例如，要回退到上一个提交并保留更改，可以运行以下命令：
git reset --soft HEAD~
#--mixed模式是默认的模式，它会撤销当前提交并取消对暂存区的更改。
#这意味着，已撤消的提交的更改将保留在工作目录中，但不会出现在暂存区中。
#例如，要回退到上一个提交并取消对暂存区的更改，可以运行以下命令：
git reset --mixed HEAD~
#--hard模式是最强大的模式，它会彻底删除已撤消的提交的更改。
#这意味着，已撤消的提交的更改将不再存在于工作目录或暂存区中。请注意，使用--hard模式将永久删除更改，无法恢复。
#例如，要回退到上一个提交并彻底删除所有更改，可以运行以下命令：
git reset --hard HEAD~

git restore --staged index.html #恢复已经删除的文件
```


## 分支操作

### 分支基本操作
```bash
git branch [分支名]
# -vv:查看远程分支 --all所有分支
git branch -D [分支名]
git checkout [分支名]
git switch [分支名]
git merge [分支名]
#(git fetch + git merge == git pull)
```

### 撤销某一次提交
```bash
git revert [commitid] #(删除某次提交)
git revert -m 1 [commitid] #(删除merge, -m 1表示保留1号)
git revert HEAD    # 撤销前一次的 commit 
git revert HEAD^ # 撤销前前一次的 commit
#1.手动解决冲突，然后提交
#2.git revert --abort 取消撤回，不解决冲突了
```

### 标签

```bash
git tag -l
git tag -a v0.1.1 -m "  "
git tag -d [tagname]
git push origin :refs/tags/[tagname]
git checkout -b [分支名称] [tag标签名称]
git show [tagname]
git push origin [tagname]
git push origin --tags
```


## 远程仓库

### 关联远程仓库

```bash
git remote -v
git remote add origin git@gitee.com:bytesc/git_test03.git
git remote set-url origin git@gitee.com:bytesc/git_test03.git
git remote rm origin
# origin 可以替换为任何仓库名
```

### 关联远程分支

```bash
git branch --set-upstream-to=origin/[远程分支名]
git push -u origin [远程分支名]:[本地分支名]

git push origin [远程分支名]  #(新建远程分支)
git push origin :[远程分支名] #(删除远程分支)
git push origin --delete [远程分支名] #(删除远程分支)
git push [-f] --set-upstream origin [本地分支名]:[远程分支名]
# -f:强制覆盖  --set-upstream:关联当前分支与远程分枝  
git pull [-u] origin [远程分支名]:[本地分支名]

git remote prune origin #(删除本地存在但是远程没有的分支)
```


## 常见操作流程

### 解决冲突

```txt
先fetch远程仓库，再merge远程分支到本地分支
merge后修改冲突文件，重新add，commit，push
对于fork的他人仓库，可以通过add remote他人仓库再同步
```

### 撤销误commit

```bash
git reset --mixed HEAD^
```

## 其它配置

### .gitignore

从 git 缓存中删除误提交的大文件等
```bash
git rm -r --cached [文件夹/文件名称] 
git rm -r --cached .idea
```


```txt
*.txt  ，*.xls  表示过滤某种类型的文件
target/ ：表示过滤这个文件夹下的所有文件
/test/a.txt ，/test/b.xls  表示指定过滤某个文件下具体文件
!*.java , !/dir/test/     !开头表示不过滤
*.[ab]    支持通配符：过滤所有以.a或者.b为扩展名的文件
/test  仅仅忽略项目根目录下的 test 文件，不包括 child/test等非根目录的test目录
以”#”号开头表示注释；
以斜杠“/”开头表示目录；
以星号“*”通配多个字符；
以问号“?”通配单个字符
以方括号“[]”包含单个字符的匹配列表；
以叹号“!”表示不忽略(跟踪)匹配到的文件或目录；

把已经脱管的文件ignore
git rm -r --cached Front_page/.idea
```

### 别名文件

```bash
touch ~/.bashrc #(在用户目录下)
# 内容："alias ll='ls -al'"
```

### 手动DNS

```txt
查询服务器ip：
https://myssl.com/dns_check.html#dns_check
github.com
raw.githubusercontent.com
添加域名解析到hosts：
vim /etc/hosts
末尾添加
20.205.243.166 github.com
185.199.109.133 raw.githubusercontent.com
```

