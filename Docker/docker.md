# Docker 操作

## 经典flask 应用和conda 环境
```
my_flask_app/
│
├── app.py              # Flask 应用主文件
├── environment.yml     # Conda 环境文件
├── requirements.txt    # pip 依赖文件（可选）
└── Dockerfile          # Docker 配置文件
```

+ 生成environment.yml
```
打包：
conda env export --name my_env --file environment.yml
新建环境：
conda env create --file environment.yml
```
## Flask记得设置所有IP都可访问
+ 一个巨坑
```
if __name__ == '__main__':
    load_net_params()
    app.run(host='0.0.0.0', port=9003, debug=True) # 这里记得设置host
```
## 经典DockerFile Case
```
# 使用 Miniconda 基础镜像
# 这里还可以写成From Centos,决定了使用什么基础镜像

FROM continuumio/miniconda3  


# 复制本地的 Anaconda 环境到 Docker 镜像
# 这里 必须要将llm(从anaconda 下env中选出来的环境名为llm)

COPY llm /opt/conda/envs/llm


# 设置环境变量
# 容器中运行的命令行或脚本默认使用该环境路径下的可执行文件
ENV PATH /opt/conda/envs/llm/bin:$PATH

# 设置默认的工作目录
WORKDIR /app

# 复制应用代码到容器中
COPY . /app 

# 复制模型文件到容器中
# 这个是LLM需要从远端下载gpt2 我们提前下载到了本地
COPY gpt2 /root/.cache/modelscope/hub/AI-ModelScope/gpt2


# 配置 conda 使用清华镜像源
RUN conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/free/ && \
    conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/main/ && \
    conda config --set show_channel_urls yes

# 暴露应用端口
EXPOSE 9003

# 启动 Flask 应用
# 简单讲就是执行一条cmd命令
CMD ["conda", "run", "--no-capture-output", "-n", "llm", "python", "app.py"]
```


# Docker 基本操作

+ 进入一个创建好的docker容器
```
docker run -i -t [#镜像名称] /bin/bash
```
+ 停止一个容器
```
docker stop [容器id]
```
+ 查看正在使用的docker容器
```
$ docker ps
$ docker ps -a #查看全部容器 
```


+ 使用 docker images 来列出本地主机上的镜像。
```
docker images
```
+ 重启一个容器
```
docker restart <容器 ID>
```
+ 进入容器
```
docker exec -it 243c32535da7 /bin/bash
```

# 容器和镜像的区别
+ 镜像
```
Docker 镜像是一个轻量级、可执行的独立软件包，包含运行应用所需的所有依赖、库、基础设置和配置文件。镜像是不可变的，意味着一旦创建，它就不能被更改。镜像可以被视为应用程序和其依赖的模板，用于创建 Docker 容器。
```
+ 容器
```
Docker 容器是镜像的运行实例。当你从 Docker 镜像启动容器时，Docker 会在镜像顶部添加一个可写文件层。这个容器层允许你执行写操作，比如修改现有文件、创建新文件和安装软件，这些更改存在于容器中，不影响底层镜像。简而言之，如果镜像是类的定义，则容器是类的实例。
```
```
什么情况下应该打包镜像或容器？
打包镜像
分发和版本控制：当你需要共享你的应用程序或系统环境给其他开发者、团队成员或者部署到生产环境时，你应该打包 Docker 镜像。镜像提供了一个标准化的环境，确保无论在哪里部署，行为都是一致的。
CI/CD 管道：在持续集成和持续部署的流程中，自动化构建和测试通常会使用 Docker 镜像，因为它们提供了一个一致且可重复的环境。
多环境部署：如果你需要在多种环境（如开发、测试和生产环境）中部署相同的应用，打包和使用 Docker 镜像是理想选择。
打包容器（实际上是导出容器的当前状态）
调试和测试：如果你需要保存一个特定的应用状态，以便于后续进行调试或测试，你可以将这个容器的当前状态导出为一个新的镜像，或者直接保存容器的快照。
迁移：当需要将一个具体的容器状态从一个环境迁移到另一个环境时，例如从开发环境迁移到测试环境，可以使用 docker commit 创建一个新镜像，然后在另一个环境中启动这个镜像。
```


## Flask映射
**两步**
+ docker run -it -p [服务器端口]:[容器内端口] --gpus all [镜像名]
+ such as:docker run -it -p 8003:9003 --gpus all webshow6
+ ssh -L 8003:127.0.0.1:8003 LiYuxiang@10.126.62.90
