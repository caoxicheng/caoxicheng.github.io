---
title: 安装并使用Verdaccio部署私有NPM仓库
date: 2022-02-07 10:32
tags:
---

前言

需要搭建私有的NPM仓库，社区提供了两种方法，我们思考了一下，采用了Verdaccio「主要还是轻量吧」

---

## 使用nodejs 安装

```
yum install -y gcc-c++ make
```

```
curl -sL https://rpm.nodesource.com/setup_14.x | sudo -E bash -
```

> 这里我们一开始安装了10.x版本的node，但是发现Cerdaccio不支持10.x的版本，但是yum已经有了缓存
> 需要清除缓存，不然会404

```
yum install -y nodejs
```

```
npm install -g verdaccio
```

```
nerdaccio --listen 0.0.0.0:4873
```

---

## 使用docker 安装

1. 拉取最新的docker镜像

```
docker pull verdaccio/verdaccio
```

2. 在服务器`/data/verdaccio/conf`下创建一个config.yaml放置配置文件

```
#
# This is the config file used for the docker images.
# It allows all users to do anything, so don't use it on production systems.
#
# Do not configure host and port under `listen` in this file
# as it will be ignored when using docker.
# see https://verdaccio.org/docs/en/docker#docker-and-custom-port-configuration
#
# Look here for more config file examples:
# https://github.com/verdaccio/verdaccio/tree/master/conf
#

# path to a directory with all packages
storage: /verdaccio/storage/data
# path to a directory with plugins to include
plugins: /verdaccio/plugins

web:
  # WebUI is enabled as default, if you want disable it, just uncomment this line
  #enable: false
  title: Verdaccio
  # comment out to disable gravatar support
  # gravatar: false
  # by default packages are ordercer ascendant (asc|desc)
  # sort_packages: asc
  # darkMode: true
  # logo: http://somedomain/somelogo.png
  # favicon: http://somedomain/favicon.ico | /path/favicon.ico
  # rateLimit:
  #   windowMs: 1000
  #   max: 10000

# translate your registry, api i18n not available yet
# i18n:
# list of the available translations https://github.com/verdaccio/ui/tree/master/i18n/translations
#   web: en-US

max_body_size: 20mb

auth:
  htpasswd:
    file: /verdaccio/storage/htpasswd
    # Maximum amount of users allowed to register, defaults to "+infinity".
    # You can set this to -1 to disable registration.
    # max_users: 1000

# a list of other known repositories we can talk to
uplinks:
  npmjs:
    url: https://registry.npmjs.org/

packages:
  '@*/*':
    # scoped packages
    access: $all
    publish: $authenticated
    unpublish: $authenticated
    proxy: npmjs

  '**':
    # allow all users (including non-authenticated users) to read and
    # publish all packages
    #
    # you can specify usernames/groupnames (depending on your auth plugin)
    # and three keywords: "$all", "$anonymous", "$authenticated"
    access: $all

    # allow all known users to publish/publish packages
    # (anyone can register by default, remember?)
    publish: $authenticated
    unpublish: $authenticated

    # if package is not available locally, proxy requests to 'npmjs' registry
    proxy: npmjs

# You can specify HTTP/1.1 server keep alive timeout in seconds for incoming connections.
# A value of 0 makes the http server behave similarly to Node.js versions prior to 8.0.0, which did not have a keep-alive timeout.
# WORKAROUND: Through given configuration you can workaround following issue https://github.com/verdaccio/verdaccio/issues/301. Set to 0 in case 60 is not enough.
server:
  keepAliveTimeout: 60

middlewares:
  audit:
    enabled: true

# log settings
logs: { type: stdout, format: pretty, level: http }
#experiments:
#  # support for npm token command
#  token: false
#  # enable tarball URL redirect for hosting tarball with a different server, the tarball_url_redirect can be a template string
#  tarball_url_redirect: 'https://mycdn.com/verdaccio/${packageName}/${filename}'
#  # the tarball_url_redirect can be a function, takes packageName and filename and returns the url, when working with a js configuration file
#  tarball_url_redirect(packageName, filename) {
#    const signedUrl = // generate a signed url
#    return signedUrl;
#  }

# This affect the web and api (not developed yet)
#i18n:
#web: en-US
```

3.运行命令

```shell
V_PATH=/data/verdaccio; docker run -it -d --rm --name verdaccio \
  -p 4873:4873 \
  -v $V_PATH/conf:/verdaccio/conf \
  -v $V_PATH/storage:/verdaccio/storage \
  -v $V_PATH/plugins:/verdaccio/plugins \
  verdaccio/verdaccio
```

4.授予文件权限

```
sudo chown -R 10001:65533 /data/verdaccio
```
