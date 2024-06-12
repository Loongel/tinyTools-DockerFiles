## tinyTools-DockerFiles

The docker files for some mirco tools

注意：如果仅克隆对应子项目，需确保git版本高于2.25，或者执行下面命令升级：
```bash
git --version | grep -o '[0-9.]*' | awk -F. '{exit (($1 * 100 + $2 * 10 + $3) >= 225)}' || echo "NO need to updating Git..."
```

 - **codeServer-for-dockerImageBuild**
    ```bash
    git clone --filter=blob:none --sparse https://github.com/Loongel/tinyTools-DockerFiles && cd tinyTools-DockerFiles && git sparse-checkout set codeServer-for-dockerImageBuild/
    ```

 - **vscode-for-metagpt**
    ```bash
    git clone --filter=blob:none --sparse https://github.com/Loongel/tinyTools-DockerFiles && cd tinyTools-DockerFiles && git sparse-checkout set vscode-for-metagpt/
    ```

 
