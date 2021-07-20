# Apache Pulsar

**学习参考**

[TGIP-CN 032 Apache Pulsar 上手实战_哔哩哔哩_bilibili](https://www.bilibili.com/video/BV1oK4y1A76N)

[直播回顾｜TGIP-CN 032：Apache Pulsar 快速上手实战 - SegmentFault 思否](https://segmentfault.com/a/1190000040118231?utm_source=tag-newest)



**pulsar支持的数据格式**

![QQ截图20210719145014](img\QQ截图20210719145014.png)

![QQ截图20210719145225](img\QQ截图20210719145225.png)

![QQ截图20210719145250](img\QQ截图20210719145250.png)

![QQ截图20210719145403](img\QQ截图20210719145403.png)

![QQ截图20210719145603](img\QQ截图20210719145603.png)

**下载地址**

```
https://pulsar.apache.org/en/download/
# Mirror
https://mirrors.tuna.tsinghua.edu.cn/apache/pulsar/
```

- Pulsar Manager

  pulsar的UI界面。



## Standalone本地开发模式

```
# 开启pulsar
# 前台开启
bin/pulsar standalone

# 后台开启
bin/pulsar-daemon standalone

# 展示pulsar集群
# "standalone"
bin/pulsar-admin clusters list

# 展示pulsar集群的broker
# "localhost:8080"
bin/pulsar-admin brokers list test

# 展示集群的topic
# "persistent://public/default/my-topic"
# "persistent://public/default/test-partitioned-4-partition-2"
# "persistent://public/default/test-partitioned-4-partition-3"
bin/pulsar-admin topics list public/default

# 为某个topic生产一条消息
bin/pulsar-client produce my-topic --messages "hello-xxx"
# 为某个topic消费一条信息 -s：指定消费名称
bin/pulsar-client consume my-topic -s "first-subscription"
```



## Cluster



## 监控

### Pulsar Manager

![QQ截图20210719155503](img\QQ截图20210719155503.png)

```
# 解压manager的bin包
tar zxvf apache-pulsar-manager-0.2.0-bin.tar.gz
# 进入目录
cd pulsar-manager
# 解压目录下的压缩包
tar xvf pulsar-manager.tar
# 移动dist文件夹到上一步解压出的文件夹下 并命名为ui
mv dist pulsar-manager/ui
# 启动manager
pulsar-manager/pulsar-manager/pulsar-manager/bin/pulsar-manager

# 配置建议
# application.properties
server.port=7750
# 默认false，打开了可以用集成的bkvm工具，看bookeeper底层存储逻辑
bookie.enable=true
# 默认false，打开了可以看到topic内消息内容
pulsar.peek.message=true
# bkvm.conf
bookie.enable=true

# 为pulsar-manager初始化账号密码0
CSRF_TOKEN=$(curl http://localhost:7750/pulsar-manager/csrf-token)

curl \
   -H 'X-XSRF-TOKEN: $CSRF_TOKEN' \
   -H 'Cookie: XSRF-TOKEN=$CSRF_TOKEN;' \
   -H "Content-Type: application/json" \
   -X PUT http://localhost:7750/pulsar-manager/users/superuser \
   -d '{"name": "admin", "password": "apachepulsar", "description": "test", "email": "username@test.org"}'
   
# 浏览器访问manager
# host:port/(index.html的目录)
localhost:7750/ui/index.html

# New Environment
# ServiceUrl:http://host:port （port为broker暴露端口
http://localhost:8080

# 压力测试
# 每秒100条 2个生产者 每条数据1024字节 topic为test-perf
bin/pulsar-perf produce -r 100 -n 2 -s 1024 test-perf 
# 消费test-perf的数据
bin/pulsar-perf consume test-perf
```

