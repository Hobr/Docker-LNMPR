# Docker-LNMPR

[![GitHub issues](https://img.shields.io/github/issues/Hobr/Docker-LNMPR.svg)](https://github.com/Hobr/Docker-LNMPR/issues)
[![GitHub stars](https://img.shields.io/github/stars/Hobr/Docker-LNMPR.svg)](https://github.com/Hobr/Docker-LNMPR/stargazers)
[![GitHub forks](https://img.shields.io/github/forks/Hobr/Docker-LNMPR.svg)](https://github.com/Hobr/Docker-LNMPR/network)

Docker + Alpine(Linux) + Nginx + MariaDB + PHP-FPM + Redis

## 快速使用

```bash
git clone https://github.com/Hobr/Docker-LNMPR.git
cd Docker-LNMPR
chmod 777 ./redis/redis.log
chmod -R 777 ./redis/data
docker-compose up -d
```

## PHP扩展安装

### 安装PHP官方源码包里的扩展

在php的Dockerfile中加入以下命令

```bash
RUN apk add libpng-dev \
    && docker-php-ext-install pdo_mysql mysqli pcntl \
```

### PECL 扩展安装

```bash
RUN pecl install memcached-2.2.0 \
    && docker-php-ext-enable memcached \
```

### 通过下载扩展源码，编译安装的方式安装

```bash
RUN cd ~ \
    && wget https://github.com/phpredis/phpredis/archive/5.0.2.tar.gz \
    && tar -zxvf 5.0.2.tar.gz \
    && mkdir -p /usr/src/php/ext \
    && mv phpredis-5.0.2 /usr/src/php/ext/redis \
    && docker-php-ext-install redis \

    && apk add libstdc++\
    && cd ~ \
    && wget https://github.com/swoole/swoole-src/archive/v4.4.2.tar.gz \
    && tar -zxvf v4.4.2.tar.gz \
    && mkdir -p /usr/src/php/ext \
    && mv swoole-src-4.4.2 /usr/src/php/ext/swoole \
    && docker-php-ext-install swoole \
```

## 常见问题处理

- MariaDB/Redis连接时IP不应为**127.0.0.1**,而应为 **db** / **redis**（因为我们使用Docker）
