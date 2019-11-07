# redis的安装
前言:在安装redis前必须先安装java jdk,安装jdk成功->安装redis

## 1. 在源码里面安装
* 下载redid centos版本的安装包
* 解压安装包
* 进入解压的安装 make命令 编辑
* 进入源码src make install 安装
* 启动服务在src文件里面

## 2.不在源码路径安装
* 前两部相同
* 先创建一个文件,在安装时使用命令 make install PREFIX=/usr/local/redis 指定安装到路径
* 将配置文件复制到这个安装的路径

