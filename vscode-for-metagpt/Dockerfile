# 第一阶段：基于 linuxserver/codeserver 开始
# 加速源： docker.nju.edu.cn

# 当前为 linuxserver/code-server 4.89.1-ls214
FROM linuxserver/code-server:latest  as base
# FROM docker.nju.edu.cn/linuxserver/code-server:latest  as builder

# 更新并安装编译所需的工具和库
RUN apt update && \
    apt install -y --no-install-recommends \
        nodejs \
        bash \
        python3 \
        python3-pip \
        npm \
        git && \
    ln -s /usr/bin/python3 /usr/bin/python && \
    # 清理 apt 缓存
    apt-get clean && \
    rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/* && \
    # 配置 code-server 的工作目录
    mkdir -p /codeSpace && chmod 777 /codeSpace && cd /codeSpace && \
    # git 加速
    # git config --global url."https://gitclone.com/".insteadOf "https://" && \
    # 安装 metagpt
    ## 安装main分支
    #git clone https://github.com/geekan/MetaGPT.git && \
    ## 安装稳定发布分支 # 当前为v0.8.1
    git clone --branch "$(git ls-remote --tags --refs https://github.com/geekan/MetaGPT.git | grep -v '\^{}' | cut -d/ -f3 | sort -Vr | head -n 1)" https://github.com/geekan/MetaGPT.git && \
    cd MetaGPT && git checkout -b mytest && \
    pip install -e .

WORKDIR /codeSpace

# 暴露 code-server 端口
EXPOSE 8443

# 配置 # code-server 默认密码
# ENV PASSWORD="my_code_serv" 

# 启动 docker & code-server
# CMD bash -c "/usr/local/bin/dockerd-entrypoint.sh & exec node /usr/local/lib/node_modules/code-server/out/node/entry.js $RUN_PARAMETERS"

## 使用说明：
# 本地docker运行脚本：
# docker run --privileged -itd -p 8780:8443 -e PASSWORD="your_password" -e DEFAULT_WORKSPACE="/codeSpace" --name vscode-metagpt  loongel/tools:vscode_metagpt.v1.0

## 编译脚本：
# cd path_to_dockerfile
# docker build -t codeserv-for-metagpt .

## 启动测试 
# docker run --privileged -itd -p 8780:8443 --name vsc-metagpt codeserv-for-metagpt
# 测试异常调试
# 1. docker exec -it vsc-metagpt bash
# 2. docker run --privileged -itd -p 8780:8443 --name vsc-metagpt codeserv-for-metagpt sh

## 发布到 docker hub
# docker login
# docker tag codeserv-for-metagpt loongel/tools:vscode_metagpt.v1.0
# docker push loongel/tools:vscode_metagpt.v1.0



