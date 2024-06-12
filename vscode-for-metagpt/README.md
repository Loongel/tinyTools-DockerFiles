### Vscode For MetaGPT

#### 介绍

该项目建立一个通过 vscode server 访问的 metagpt 的 webui 环境，开发人员可以在这个环境下运行、调试、修改、分析metagpt的工作过程。

#### 核心功能

- **web-ui**：通过 vscode 运行、修改和调试 metagpt 代码生成的整个过程。

#### 环境变量

- **code-server**：[linuxserver/code-server](https://docs.linuxserver.io/images/docker-code-server/)

- **metagpt**：[geekan/MetaGPT](https://github.com/geekan/MetaGPT)

- **关键变量**：
  ```bash
  SUDO_PASSWORD=yoursudocode
  ```

#### 使用方法

- **本地 docker**： 
  ```bash
  docker run --privileged -itd -p 8780:8443 -e PASSWORD="your_password" -e DEFAULT_WORKSPACE="/codeSpace" --name vscode-metagpt  loongel/tools:vscode_metagpt.v1.0
  ```

- **本地 docker compose**： (待)
  ```bash
  ```

  - **docker swarm stack compose**： (待)
  ```bash
  ```
