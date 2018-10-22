[root@localhost ~]# cat /etc/redhat-release
CentOS Linux release 7.5.1804 (Core)

# apt search openjdk
# apt install -y openjdk-8-jre

# yum search openjdk
# yum install -y java-1.8.0-openjdk.x86_64

用ulimit -a可以看到

解除 Linux 系统的最大文件打开数限制
       说明：* 代表针对所有用户
            noproc 是代表最大进程数
            nofile 是代表最大文件打开数 

max file descriptors [4096] for elasticsearch process is too low, increase to at least [65536]

```
# vi /etc/security/limits.conf
* soft nofile 65536
* hard nofile 131072
```

解除 Linux 系统的最大进程数限制
max number of threads [1024] likely too low, increase to at least [2048]

```
# vi /etc/security/limits.conf
* soft nproc 2048
* hard nproc 4096
```

max virtual memory areas vm.max_map_count [65530] is too low, increase to at least [262144]

```
# vi /etc/sysctl.conf
vm.max_map_count=262144
#保存退出后，执行：
sysctl -p
```


error='Cannot allocate memory'
java虚拟机使用的内存
    # vim config/jvm.options  
    -Xms2g  --》修改为512m
    -Xmx2g  --》修改为512m

以普通用户运行


但不能访问【公网IP:9200】如何解决？

修改配置文件 config/elasticsearch.yml

# 允许外部访问
network.host: 0.0.0.0
http.port: 9200

# 允许跨源 REST 请求 设置参数的时候:后面要有空格
http.cors.enabled: true
http.cors.allow-origin: "*"

后台运行

bin/elasticsearch -d



yum install -y nodejs


https://nodejs.org/en/download/
Linux Binaries (x64)

wget https://nodejs.org/dist/v8.12.0/node-v8.12.0-linux-x64.tar.xz
tar -xvJf node-v8.12.0-linux-x64.tar.xz

mv node-v8.12.0-linux-x64 /usr/local/node

```
# vi /etc/profile
export NODE_HOME=/usr/local/node
export PATH=$NODE_HOME/bin:$PATH

# source /etc/profile
```

npm install -g cnpm --registry=https://registry.npm.taobao.org

[root@localhost elk]# node -v
v8.12.0
[root@localhost elk]# npm -v
6.4.1


yum install -y git

https://github.com/mobz/elasticsearch-head

cd elasticsearch-head

cnpm install


bzip2：无法 exec: 没有那个文件或目录
yum install -y bzip2

npm run start


默认只能本机连接，加入 hostname

```
# vi Gruntfile.js
connect: {
        server: {
                options: {
                        port: 9100,
                        hostname:'*',
                        base: '.',
                        keepalive: true
                }
        }
}
```

http://192.168.88.152:9100/
Elasticsearch 填入 http://192.168.88.152:9200/
如果连不上，检查es是否配置了cors

hohup npm run start &>/dev/null




```
# vi /Users/joe/Desktop/elk/test.log
{"api":"/user/get", "device":"android", "ip":"127.0.0.1"}
{"api":"/user/get", "device":"android", "ip":"127.0.0.1"}
{"api":"/user/get", "device":"ipad", "ip":"192.168.88.1"}
{"api":"/user/update", "device":"ipad", "ip":"192.168.88.1"}
```

```
# vi config/test.conf
input{
    file{
        path => ["/Users/joe/Desktop/elk/test.log"]
        codec => json{
            charset => "UTF-8"
        }
    }
}

output{
    elasticsearch{
        hosts => "192.168.88.152"
        index => "logstash-%{+YYYY.MM.dd}"
        document_type => "test"
    }
}
```

bin/logstash -f config/test.conf



tar -zxf kibana-6.4.2-linux-x86_64.tar.gz

bin/kibana

http://192.168.88.162:5601

```
# 允许远程访问
vi config/kibana.yml
server.host: "0.0.0.0"
```

Management->Kibana->Index Patterns

```
logstash-*   下一步  选 @timestamp
```

Discover 中就可以搜索了，例如: android，注意选择Time Range


