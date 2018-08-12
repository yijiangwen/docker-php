#  Docker化PHP环境

## 此构建参考学习：https://github.com/opso-code/docker-php

## 为合适自己的工作和开发环境而打造，for ninja911

使用 **docker-compose** 快速搭建php环境

## 环境构成

将 `php-fpm` 和 `nginx` 容器分开，通过 `php:9000` 端口通信

### php

php镜像来自官方 `php:fpm`，目前最新稳定版本是 `7.2.8`

在此基础上添加了以下等扩展：

- swoole-4.0.3
- redis/hiredis
- mysqli
- pdo_mysql
- mongodb
- GD
- memcached
- ...

手动添加了 `composer` 并替换了国内源，修改了时区（`Asia/Shanghai`）

### nginx

直接使用的 `nginx:latest` 镜像,需要挂载自己的PHP项目工作目录，并配置nginx/conf.d里各个站点

### mongodb

直接使用的 `mongodb:latest` 镜像，根据具体情况修改 `/data/mongodb` 本地映射的数据库文件夹，如不需要可注释掉，其他数据库同理。
Windows 磁盘是NTFS/FAT32，不支持Ext4大文件，不能挂载，需要注释挂载， Windows下无解

## 运行

```sh
$ cd docker-php/
// 后台运行
$ docker-compose up -d
// 进入php容器bash环境
$ docker-compose exec php bash
```
