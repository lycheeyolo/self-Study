# self-Study
## 预训练大模型下载
- 大文件git 下载需要lfs支持，运行git lfs install 查看是否已安装lfs；
- ubuntu 通过 apt-get install git-lfs 安装lfs.
- 

## linux
- 查看指定id的进程使用：ps -f -p 进程id

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

# vllm deploy
```docker
version: "0.1"

services:
  fastapi-server:
    restart: always
    image: jcr.ronds.cn/library/ailab/lzy_rondsgpt_stream_server:vllm0.10.2
    container_name: lzy_rpm_harmonic_server
    ports:
      - "5015:8000"
      - "5014:22"
    runtime: nvidia
    volumes:
      - /rondsai/lab/lizhiyuan/pretrain_models/pretrain_models:/models
      - /rondsai/wave:/rondsai/wave
    working_dir: /app
    command: >
      bash -c "CUDA_VISIBLE_DEVICES=7 VLLM_CONFIGURE_LOGGING=0 vllm serve /models/Qwen2.5-7B-Instruct --enable-lora --lora-modules rpm=/models/lora_rpm_signalsummary workfrequencyfault=/models/lora_workfrequencyfault --disable-log-stats"
    tty: true
    stdin_open: true
```
