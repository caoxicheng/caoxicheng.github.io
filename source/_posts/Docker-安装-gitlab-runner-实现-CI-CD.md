---
title: Docker 安装 gitlab-runner 实现 CI/CD
date: 2022-01-29 15:02
tags:
---

# Docker install gitlab-runner

1. 拉取gitlab-runner镜像

```
sudo docker pull gitlab/gitlab-runner:latest
```

2. 添加gitlab-runner container

```
sudo docker run -d --name gitlab-runner --restart always \
  -v /srv/gitlab-runner/config:/etc/gitlab-runner \
  -v /var/run/docker.sock:/var/run/docker.sock \
  gitlab/gitlab-runner:latest
```

3. 注册runner

```
sudo docker exec -it gitlab-runner gitlab-ci-multi-runner register
```

```
说明： 
 1、gitlab ci的地址以及token，从你要配置该runner到哪个项目，就去gitlab下该项目首页右侧设置—》CI/CD Pipelines—》Specific Runners下可以找到。 
 2、gitlab-ci tags这个很重要，在项目构建流程yaml文件里面指定tag，就是匹配使用哪个tag的runner，这里我定义了fone-ui，回头再配置文件里面就指定这个tag。 
 3、executor：执行者可以有很多种，这里我们使用docker，方便构建执行。 
 4、Docker image：构建Docker image时填写的image名称，根据项目代码语言不同，指定不同的镜像。我这里项目是node语言的，所以我使用官方node:16.13.2镜像。
```

4. 设置运行脚本

```
cache:
  paths:
    - node_modules/

image: node:16.13.2

build:
    stage: build
    tags:
        - fone-ui
    script:
        - echo "show npm registry & proxy..."
        - npm get registry
        - npm config get proxy
        - echo "Start building App..."
        - npm ci
        - npm run build
        - echo "Build successfully!"
    only:
        refs:
            - merge_requests
        variables:
            - $CI_MERGE_REQUEST_TARGET_BRANCH_NAME == "develop" || $CI_MERGE_REQUEST_TARGET_BRANCH_NAME == "master"
```

## 如何删除呢？

1. gitlab设置页面，删除runner
2. docker容器中的gitlab-runner中删除

第一个在注册步骤中的页面可以一键删除

第二个使用命令

1. 进入runner容器内

```
$ docker exec -it gitlab-runner bash
```

2. 查看runner列表

```
$ gitlab-runner list
```

3. 使用指定的id和url删除验证

```
gitlab-runner verify --delete -t [你的token，即第一步中的对应的token] -u http://git.xxxx.com/
```

4. 退出容器

```
exit
```

环境参数

> CentOs 7.9
> 
> docker 20.20.10
> 
> NodeJs 16.13.2
> 
> GitLab 社区版 13.12.0
