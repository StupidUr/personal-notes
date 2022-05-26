---
title: '常用服务搭建' # 左侧标题
sidemenu: true # 左侧菜单
order: 1 # 排序
---

# 常用服务器搭建

## `Bitwarden`密码自建服务器

> Bitwarden_rs是用rust编写的兼容Bitwarden api的服务端，兼容Bitwarden客户端，运行起来更轻量

1. 安装
- docker安装
```bash
docker pull bitwardenrs/server:latest
docker run -d --name bitwarden -v /bw-data/:/data/ -p 80:80 bitwardenrs/server:latest
//如果要禁止别人注册,可以在自己创建之后,停止容器,再运行
docker run -d --name bitwarden -v /bw-data/:/data/ -p 80:80 -e SIGNUPS_ALLOWED=false  bitwardenrs/server:latest
```
`bw-data`目录为自建服务器持久化存储的内容的目录，80端口映射开放配置相应的SSL以及域名进行访问

- 官方命令安装

```bash
curl -Lso bitwarden.sh https://go.btwrdn.co/bw-sh && chmod +x bitwarden.sh
./bitwarden.sh install
./bitwarden.sh start
```
2. 使用

> 通过浏览器访问 [Bitwarden官网](https://bitwarden.com/download/) 下载，包括浏览器、客户端、移动端


**移动端需求配置`自动填充服务`为Bitwarden应用**


- 方法一：使用快捷键自动填充
    
  在Windows上： Ctrl + Shift + L

  在macOS上： Cmd + Shift + L

  在Linux上： Ctrl + Shift + L

  Safari浏览器：Cmd + \或Cmd + 8或Cmd + Shift + P


- 方法二：在插件中进行设置

  设置-选项-启用页面加载时的自动填充 

## Code-server

```

docker run -it -d --name code-server -p 443:8080 -v "/home/coder/.config:/root/.config" -v "/home/coder/project:/home/coder/project" -u "$(id -u):$(id -g)"   -e "DOCKER_USER=root" codercom/code-server:latest

```


