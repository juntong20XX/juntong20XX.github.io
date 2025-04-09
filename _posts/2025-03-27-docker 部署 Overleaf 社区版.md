---
layout: mypost
title: "Docker 部署 Overleaf 社区版"
categories: [homelab]
lang: zh-cn
---



# Docker 部署 Overleaf 社区版

也许这篇可以作为 homelab 系列的第一篇？

写这篇的原因是我在更新我的 TexLive 到 2025后，发现部署的 Overleaf 显示`Sorry, something went wrong and your project could not be compiled. Please try again in a few moments.`，反正无法编译文件。

解决方式很无语，索性写一篇博客。

**TODO: 目录**

## 安装

### 配置 docker compose

我并没有按照[官方教程](https://github.com/overleaf/toolkit/blob/master/doc/docker-compose.md)使用 `toolkit` 。我在 all in boom 上部署了好几个服务，手动运维。如果对一个服务使用脚本而不是 docker 原生命令太特立独行了。

```yaml
services:
    overleaf-redis:
        image: redis:7.4.0
        hostname: overleaf-redis # PORT: 6379
        container_name: overleaf-redis
        networks:
            - jsudot-tex
        restart: 'unless-stopped'
    overleaf:
        image: sharelatex/sharelatex:5.3
        hostname: overleaf # port: 80
        container_name: overleaf
        networks:
            - reverse-proxy
            - jsudot-tex
        restart: 'unless-stopped'
        depends_on:
            mongo:
                condition: service_healthy
            overleaf-redis:
                condition: service_started
        links:
            - mongo
            - overleaf-redis
        stop_grace_period: 60s
        volumes:
            - "./overleaf-data:/var/lib/overleaf"
            - "./texlive:/usr/local/texlive"
        environment:
            OVERLEAF_APP_NAME: Overleaf Community Edition
            OVERLEAF_SITE_URL: ${OVERLEAF_SITE_URL}
            OVERLEAF_MONGO_URL: mongodb://mongo/sharelatex
            OVERLEAF_REDIS_HOST: overleaf-redis
            REDIS_HOST: overleaf-redis
            ENABLED_LINKED_FILE_TYPES: 'project_file,project_output_file'
            ENABLE_CONVERSIONS: 'true'
            EMAIL_CONFIRMATION_DISABLED: 'true'
            TEXMFVAR: /var/lib/overleaf/tmp/texmf-var
            ENABLE_CRON_RESOURCE_DELETION: 'true'
            # https://github.com/overleaf/overleaf/wiki/HTTPS-reverse-proxy-using-Nginx#configuration-for-overleaf-to-run-with-ssl
            OVERLEAF_SECURE_COOKIE: true
            OVERLEAF_BEHIND_PROXY: true
    overleaf-mongo:
        image: mongo:5.0
        hostname: overleaf-mongo
        container_name: overleaf-mongo # 27017
        networks:
            - jsudot-tex
        restart: 'unless-stopped'
        healthcheck:
            test: echo 'db.stats().ok' | mongo localhost:27017/test --quiet
            interval: 10s
            timeout: 10s
            retries: 5
        command: "--replSet overleaf"
        volumes:
            - "./mongo-data:/data/db"
    mongoinit:
        # 复制自以下链接
        # https://blog-exception0x0194.pages.dev/2024/07/23/build-local-overleaf-server-through-docker-compose/
        # 试过把命令在 setup 脚本里执行, 但不知道怎么把配置更新到 MongoDB 的配置文件里, 就只好这样了.
        image: mongo:5.0
        hostname: mongoinit
        container_name: mongoinit
        networks:
            - jsudot-tex
        # this container will exit after executing the command
        restart: "no"
        depends_on:
            mongo:
                condition: service_healthy
        entrypoint: [ "mongo", "--host", "overleaf-mongo:27017", "--eval", 'rs.initiate({ _id: "overleaf", members: [ { _id: 0, host: "overleaf-mongo:27017" } ] })' ]
```

### 配置



### 故障排除



