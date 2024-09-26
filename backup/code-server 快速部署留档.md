```Dockerfile
FROM debian:12

# 在这里填写密码，将同时作用于sudo密码和code-server
ENV PASSWORD=example

RUN apt-get update && \
    apt-get install -y sudo wget build-essential git && \
    useradd -m dev -s /bin/bash && \
    echo "dev:$PASSWORD" | chpasswd && \
    usermod -aG sudo dev && \
    wget https://github.com/coder/code-server/releases/download/v4.93.1/code-server_4.93.1_amd64.deb && \
    apt-get install -y ./code-server_4.93.1_amd64.deb && \
    rm code-server_4.93.1_amd64.deb

EXPOSE 8080

USER dev
CMD ["code-server", "--bind-addr", "0.0.0.0:8080"]
```
使用命令`podman build -t codeserver:latest .`构建映像（个人更喜欢Podman）  
需要映射容器内端口 8080 为服务器的 任意端口（记得开防火墙  