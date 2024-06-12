# 第一阶段：基于 linuxserver/codeserver 开始
# 加速源： docker.nju.edu.cn

# 当前为 linuxserver/code-server 4.89.1-ls214
FROM linuxserver/code-server:latest  as base
# FROM docker.nju.edu.cn/linuxserver/code-server:latest  as builder

# 更新并安装编译所需的工具和库
RUN apt update && \
    apt install -y --no-install-recommends \
        bash wget xz-utils \
        python3 \
        python3-pip \
        npm \
        git && \
    ln -s /usr/bin/python3 /usr/bin/python && \
    sudo bash -c "mkdir -p /usr/local/nodejs-custom && wget -qO- https://nodejs.org/dist/v18.20.3/node-v18.20.3-linux-x64.tar.xz    | tar -xJ -C /usr/local/nodejs-custom --strip-components=1 &&  ln -sfn /usr/local/nodejs-custom/bin/node /usr/bin/node && ln -sfn /usr/local/nodejs-custom/bin/npm /usr/bin/npm" && \
    # 清理 apt 缓存
    apt-get clean && \
    rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/* 

FROM base as b1
    # 配置 code-server 的工作目录
RUN mkdir -p /config/workspace && cd /config/workspace  && \
    ln -sf /config/.metagpt /config/workspace/.metagpt && \
    # git 加速
    # git config --global url."https://gitclone.com/".insteadOf "https://" && \
    # 安装 metagpt
    ## 安装main分支
    #git clone https://github.com/geekan/MetaGPT.git && \
    ## 安装稳定发布分支 # 当前为v0.8.1
    git clone --branch "$(git ls-remote --tags --refs https://github.com/geekan/MetaGPT.git | grep -v '\^{}' | cut -d/ -f3 | sort -Vr | head -n 1)" https://github.com/geekan/MetaGPT.git && \
    cd MetaGPT && git checkout -b mytest && \
    pip install --no-cache-dir -e . && \
    pip install --no-cache-dir -e .[rag] && \
    metagpt --init-config
    
# 暴露 code-server 端口
EXPOSE 8443

# 配置 # code-server 默认密码/sudo密码
ENV SUDO_PASSWORD=yoursudocode
# ENV PASSWORD="my_code_serv" 

FROM b1 as b2
# 通过传递一个每次都变化的值给 CACHEBUST 来确保该阶段总是被重新执行。
# docker build --build-arg CACHEBUST=$(date +%s)
# 这里使用 date +%s 生成一个基于当前时间的秒数的唯一值，从而使得每次构建都视为不同，触发 b3 阶段的重新构建
ARG CACHEBUST=1 
RUN apt update && apt install dialog && \
    cd /config/workspace && git clone https://github.com/Loongel/dialog-shell-ui.git && \
    for file in dialog-shell-ui/conf/*.conf.template; do mv "$file" "${file%.template}"; done && \
    chmod +x dialog-shell-ui/*.sh dialog-shell-ui/lib/*.sh && \
    cp dialog-shell-ui/run_ui.sh.copy_to_parents_folder run_ui.sh && chmod +x run_ui.sh && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/* 

COPY README_in_container.md /config/workspace/README.md

## 使用说明：
# 线上镜像docker运行脚本：
# docker run --privileged -itd -p 8780:8443 -e PASSWORD="your_password" -e DEFAULT_WORKSPACE="/codeSpace" --name vscode-metagpt  loongel/tools:vscode_metagpt.v1.0

## 编译脚本：
# cd path_to_dockerfile
# docker build -t codeserv-for-metagpt .
# 分步编译
# docker build --target base -t codeserv-for-metagpt.base .
# docker build --target b1 -t codeserv-for-metagpt.b1 .

## 本地启动测试 
# docker run --privileged -itd -p 8780:8443 --name vsc-metagpt codeserv-for-metagpt
# 测试异常调试
# 1. docker exec -it vsc-metagpt bash
# 2. docker run --privileged -itd -p 8780:8443 --name vsc-metagpt codeserv-for-metagpt bash
# 分步测试调试
# docker run --privileged -itd -p 8780:8443 --name vsc-metagpt codeserv-for-metagpt.base bash

## 发布到 docker hub
# docker login
# docker tag codeserv-for-metagpt loongel/tools:vscode_metagpt.v1.0
# docker push loongel/tools:vscode_metagpt.v1.0



