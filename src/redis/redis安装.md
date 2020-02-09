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

## 3.安装redis开机自动启动
  
  ### 1. 拷贝 redis 安装目前下的 /usr/local/redis-4.0.8/utils/redis_init_script 到 /etc/init.d文件中
      cp /usr/local/redis-4.0.8/utils/redis_init_script /etc/init.d/
      
  ### 2.修改/etc/init.d/reids_init_script 文件
      [img]{https://github.com/longchenwen/mainshi/blob/master/src/redis/redis%E8%87%AA%E5%8A%A8%E5%AE%89%E8%A3%85.png}


  

