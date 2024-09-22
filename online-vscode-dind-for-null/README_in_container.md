### Vscode For Metagpt

#### 介绍

该项目建立一个通过 vscode server webui 访问的 metagpt 环境，可以在这个环境下运行、调试、修改、观察、metagpt的工作

#### 核心功能

- **web-ui**：通过 vscode 运行、修改和调试 metagpt 代码生成的整个过程。

#### 目录结构(容器内部)

- **/config/workspace**：vscode 默认工作空间。
- **.metagpt/**：包含metagpt的配置文件夹，如: LLM_api等。
- **MetaGPT/**：metagpt源码。
- **MetaGPT/workspace**：存放metagpt生成的各个项目，供metagpt agents读写。
- **run_ui.sh**：metagpt部分功能控制的shell-ui，目前只有两个功能：获取MetaGPT文件夹的写入权限；链接MetaGPT到MetaGPT自己的workspace。

#### 使用方法(容器内部)

1. 使用 `bash run_ui.sh `开启 "MetaGPT文件夹的写入权限"。注意：sudo密码默认为 `yoursudocode`

2. 配置metagpt的LLM_API,修改`.metagpt/config2.yaml` 文件，如：
```conf
llm:
  api_key: "sk-33faeed3a8754eefb939c1b2abba14fe"  #这是一个示意的key
  base_url: "https://api.deepseek.com"
  model: "deepseek-chat"  # or gpt-3.5-turbo-1106 / gpt-4-1106-previewgit

# mermaid:
#   engine: "nodejs"
#   path: "mmdc"
#   puppeteer_config: "/app/metagpt/config/puppeteer-config.json"
#   pyppeteer_path: "/usr/bin/chromium
```

3.在vscode的终端执行，比如开发一个"2048 game"游戏：` metagpt "Create a 2048 game" ` ，具体metagpt的使用方法可查阅 `MetaGPT/README.md`,以及 `metagpt -h`
