---
title: MQ学习笔记
date: 2022-11-16 13:58:30
tags:
top: true
---

## RabbitMQ

*<!-- more -->*

##### 1、安装配置RabbitMQ

- 安装依赖

  ```xml
   yum install socat -y
  ```

- 安装Erlang，因为RabbitMQ是使用Erlang语言开发的，需要依赖Erlang的环境

  erlang-23.0.2-1.el7.x86_64.rpm下载地址： https://github.com/rabbitmq/erlang-rpm/releases/download/v23.0.2/erlang-23.0.2-1.el7.x86_64.rpm 

  首先将erlang-23.0.2-1.el7.x86_64.rpm上传至服务器，然后执行下述命令：

  ```xml
  rpm -ivh erlang-23.0.2-1.el7.x86_64.rpm
  ```

- 安装RabbitMQ

  rabbitmq-server-3.8.4-1.el7.noarch.rpm下载地址：

   https://github.com/rabbitmq/rabbitmq-server/releases/download/v3.8.5/rabbitmq-server-3.8.5-1.el7.noarch.rpm 

  首先将rabbitmq-server-3.8.4-1.el7.noarch.rpm上传至服务器，然后执行下述命令：

  ```xml
  rpm -ivh rabbitmq-server-3.8.4-1.el7.noarch.rpm
  ```

- 启用RabbitMQ的管理插件

  进入到 /usr/lib/rabbitmq/lib/rabbitmq_server-3.8.5/sbin，启动管理插件

  ```xml
  rabbitmq-plugins enable rabbitmq_management
  ```

- 启动RabbitMQ

  进入到 /usr/lib/rabbitmq/lib/rabbitmq_server-3.8.5/sbin，启动RabbitMQ

  ```xml
  systemctl start rabbitmq-server
  或者
  rabbitmq-server
  或者后台启动
  rabbitmq-server -detached
  ```

- 添加用户

  ```xml
   rabbitmqctl add_user root 123456
  ```

- 给用户设置标签

  ```xml
  rabbitmqctl set_user_tags root administrator
  ```

  用户的标签和权限

  | tag           | 权限                                                         |
  | ------------- | ------------------------------------------------------------ |
  | (None)        | 没有访问management插件的权限                                 |
  | management    | 可以使用消息协议做任何操作的权限，加上：<br />1. 可以使用AMQP协议登录的虚拟主机的权限 <br />2. 查看它们能登录的所有虚拟主机中所有队列、交换器和绑定的权限 <br />3. 查看和关闭它们自己的通道和连接的权限 <br />4. 查看它们能访问的虚拟主机中的全局统计信息，包括其他用户的活动 |
  | policymaker   | 所有management标签可以做的，加上： <br />1. 在它们能通过AMQP协议登录的虚拟主机上，查看、创建和删除策略以及虚 拟主机参数的权限 |
  | monitoring    | 所有management能做的，加上： <br />1. 列出所有的虚拟主机，包括列出不能使用消息协议访问的虚拟主机的权限 <br />2. 查看其他用户连接和通道的权限 <br />3. 查看节点级别的数据如内存使用和集群的权限 <br />4. 查看真正的全局所有虚拟主机统计数据的权限 |
  | administrator | 所有policymaker和monitoring能做的，加上： <br />1. 创建删除虚拟主机的权限 <br />2. 查看、创建和删除用户的权限 <br />3. 查看、创建和删除权限的权限 <br />4. 关闭其他用户连接的权限 |

  

  

- 给用户设置权限

  给root用户在虚拟主机"/"上的配置、写、读的权限

  ```xml
  rabbitmqctl set_permissions root -p / ".*" ".*" ".*"
  ```

##### 2、RabbitMQ的常用操作命令

- ```xml
  # 前台启动rabbitmq
  rabbitmq-server
  
  # 关闭rabbitmq
  rabbitmqctl stop
  
  # 后台启动rabbitmq
  rabbitmq-server -detached
  
  # 停止RabbitMQ和Erlang VM
  rabbitmqctl stop
  
  # 查看所有队列
  rabbitmqctl list_queues
  
  # 查看所有虚拟主机
  rabbitmqctl list_vhosts
  
  # 在Erlang VM运行的情况下启动RabbitMQ应用
  rabbitmqctl start_app
  rabbitmqctl stop_app
  
  # 查看节点状态
  rabbitmqctl status
  
  # 查看所有可用的插件
  rabbitmq-plugins list
  
  # 启用插件
  rabbitmq-plugins enable <plugin-name>
      
  # 停用插件
  rabbitmq-plugins disable <plugin-name>
      
  # 添加用户
  rabbitmqctl add_user username password
      
  # 列出所有用户：
  rabbitmqctl list_users
      
  # 删除用户：
  rabbitmqctl delete_user username
      
  # 清除用户权限：
  rabbitmqctl clear_permissions -p vhostpath username
      
  # 列出用户权限：
  rabbitmqctl list_user_permissions username
      
  # 修改密码：
  rabbitmqctl change_password username newpassword
      
  # 设置用户权限：
  rabbitmqctl set_permissions -p vhostpath username ".*" ".*" ".*"
      
  # 创建虚拟主机:
  rabbitmqctl add_vhost vhostpath
      
  # 列出所以虚拟主机:
  rabbitmqctl list_vhosts
      
  # 列出虚拟主机上的所有权限:
  rabbitmqctl list_permissions -p vhostpath
      
  # 删除虚拟主机:
  rabbitmqctl delete_vhost vhost vhostpath
      
  # 移除所有数据，要在 rabbitmqctl stop_app 之后使用:
  rabbitmqctl reset
  ```
  
- 
