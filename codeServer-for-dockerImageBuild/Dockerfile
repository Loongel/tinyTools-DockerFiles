# 第一阶段：编译阶段 当前的code-server仅能通过alpine:3.16（node18.18）编译成功
FROM alpine:3.16 as builder

# 更新并安装编译所需的工具和库
RUN apk update && apk --no-cache add \
    alpine-sdk \
    bash \
    libstdc++ \
    libc6-compat \
    python3 \
    npm \
    krb5-dev

# 安装 code-server
RUN npm install --global code-server --unsafe-perm

# 第二阶段：运行阶段
#FROM docker:dind
FROM docker:dind-rootless

USER root

# 安装运行所需的库
RUN apk update && apk --no-cache add \
    bash \
    libstdc++ \
    #libc6-compat \
    #krb5-libs \
    nodejs \
    sudo

# 从编译阶段复制 code-server
#COPY --from=builder /usr/local/bin/code-server /usr/local/bin/code-server
COPY --from=builder /usr/local/lib/node_modules /usr/local/lib/node_modules

# Create a non-root user and give it necessary permissions
#RUN adduser -D coderuser && echo "coderuser ALL=(ALL) NOPASSWD: ALL" > /etc/sudoers.d/coderuser \
#    && echo "rootless ALL=(coderuser) NOPASSWD: ALL" > /etc/sudoers.d/rootless

# 给予rootless sudo root 权限,用于 vs code 控制台安装软件
RUN echo "rootless ALL=(ALL) NOPASSWD: ALL" > /etc/sudoers.d/rootless \
&& sudo chmod 771 /home/rootless -R

# Switch to the non-root user

USER rootless

# 配置 code-server 的工作目录
RUN sudo mkdir -p /codeSpace && sudo chown -R rootless:rootless /codeSpace
WORKDIR /codeSpace
#WORKDIR /home/rootless

# 暴露 code-server 端口
EXPOSE 8443

# 配置 # code-server 默认密码
ENV PASSWORD="my_code_serv" 
# 配置 dockerd
ENV DOCKER_HOST="unix:///run/user/1000/docker.sock"
# code-server运行参数配置 (--cert "" 代表使用自签名证书)
ENV RUN_PARAMETERS="--bind-addr 0.0.0.0:8443 --auth password --cert"

# 启动 docker & code-server
CMD bash -c "/usr/local/bin/dockerd-entrypoint.sh & exec node /usr/local/lib/node_modules/code-server/out/node/entry.js $RUN_PARAMETERS"

## 使用说明：
# 虽然本镜像应该支持非特权运行，但目前swarm集群未按rootless的环境运行，所以此镜像仍需要本地docker特权运行，无法在swarm中运行（已测试，增加cap_admin,cap_net也无法运行）。
# 本地docker运行脚本：
# docker run --privileged -itd -p 8780:8443 -e PASSWORD="your_password" -e DEFAULT_WORKSPACE="/codeSpace" --name codeserv  loongel/tools:codesrv_imageBuild.v1.0

## 编译脚本：
#cd path_to_dockerfile
# docker build -t docker-in-codeserver .

## 启动测试 
# docker run --privileged -itd -p 8780:8443 --name codeserv docker-in-codeserver
# 测试异常调试
# 1. docker exec -it codeserv sh
# 2. docker run --privileged -itd -p 8780:8443 --name codeserv docker-in-codeserver sh

## 发布到 docker hub
# docker login
# docker tag docker-in-codeserver loongel/tools:codesrv_imageBuild.v1.0
# docker push loongel/tools:codesrv_imageBuild.v1.0



