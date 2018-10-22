docker elk

```
chmod -R 777 elasticsearch data logstash
apt install -y apache2-utils
htpasswd -c -b kibana.passwd admin a123456
docker-compose build
docker-compose up
docker-compose up -d
```

类似这样的报错：
max virtual memory areas vm.max_map_count [65530] is too low
需要修改物理机相关配置



filebeat.yml

```
filebeat.inputs:

- type: log
  enabled: true
  paths:
    - /data/webroot/kushu/app/runtime/logs/kushu-*.log
  json.keys_under_root: true
  json.overwrite_keys: true

  fields:
    kind: kushu
  fields_under_root: true

- type: log
  enabled: true
  paths:
    - /data/webroot/kushu/app/runtime/logs/logtodb-*.log
  json.keys_under_root: true
  json.overwrite_keys: true

  fields:
    kind: logtodb
  fields_under_root: true

output.logstash:
  hosts: ["192.168.88.88:5044"]
```