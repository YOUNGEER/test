# Nginx

>- 什么事Nginx？

## 相关概念

- 正向代理
- 反向代理
- 负载均衡
- 动静分离



## 常用命令

- nginx命令路径：./usr/local/nginx/sbin
- 查看nginx版本号：./nginx -v
- 关闭nginx：./nginx -s stop
- 启动nginx：./nginx
- 重新加载：修改了配置文件后，./nginx -s reload ，无需重启服务器，直接生效

## 配置文件 

- 位置：./usr/local/nginx/conf/nginx.conf

#### 配置组成

- 全局块
  - 设置一些影响nginx服务器整体运行的配置指令
  - 比如 worker processes 1,值越大，并发越大，但是会受限于硬件
- event块
  - 设置服务器与用户的网络连接
  - 比如 wroker connections 1024,表示最大连接数1024个
- http块
  - 