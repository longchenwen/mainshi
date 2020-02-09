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
    ```
    # chkconfig: 2345 10 90
    # description: Start and Stop redis

    REDISPORT=6379
    EXEC=/usr/local/bin/redis-server
    CLIEXEC=/usr/local/bin/redis-cli  #这是安装的路径

    PIDFILE=/var/run/redis_${REDISPORT}.pid
    CONF="/usr/local/redis/redis.conf"  # 配置文件的位置

    ```
    
   ### 备注：
      上面的注释的意思是，redis服务必须在运行级2，3，4，5下被启动或关闭，启动的优先级是90，关闭的优先级是10。从redis 安装目录中拷贝过来，可能没有，需要添加 上去; 否则 chkconfig 命令 会失败;
      
  ### 3.开机启动设置，执行一下命令：
    添加redis服务：
        chkconfig --add redis
    设为开机启动 ：
        chkconfig redis on
        
  ### 4.auth password  设置密码


  

