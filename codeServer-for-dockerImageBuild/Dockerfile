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
FROM docker:dind

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
RUN adduser -D coderuser && echo "coderuser ALL=(ALL) NOPASSWD: ALL" > /etc/sudoers.d/coderuser

# Switch to the non-root user

USER coderuser

# 配置 code-server 的工作目录
WORKDIR /home/coderuser
#WORKDIR /home/rootless

# 暴露 code-server 端口
EXPOSE 8443

ENV PASSWORD="my_code_serv" 

# 启动 code-server
CMD bash -c "sudo /usr/local/bin/dockerd-entrypoint.sh & exec node /usr/local/lib/node_modules/code-server/out/node/entry.js --bind-addr 0.0.0.0:8443 --auth password"



