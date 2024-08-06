# self-Study
## 预训练大模型下载
- 大文件git 下载需要lfs支持，运行git lfs install 查看是否已安装lfs；
- ubuntu 通过 apt-get install git-lfs 安装lfs.
- 


# git
## 本地代码，在远程新建项目，关联本地代码上传远程
```shell
git init
git remote add origin gitlab-url
git add .
git commit -m "info"
git push -u origin master
```
master是远程分支

# docker 
## 在docker容器中启用ssh，允许远程进入
1. 安装ssh
   ``` shell
   apt-get update
   apt-get install -y openssh-server
   ```
2. 配置SSH服务
   ``` shell
   vim /etc/ssh/sshd_config
   ```
   确认以下内容启用
   ```shell
   PermitRootLogin yes
   PasswordAuthentication yes
   ```
3. 配置密码
   ``` shell
   passwd
   ```
4. 重启ssh
   ```shell
   service ssh restart
   ```
