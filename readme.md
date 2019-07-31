# ELK note
运行ELK镜像需要vm.max_map_count至少需要262144内存

切换到root用户修改配置sysctl.conf
vi /etc/sysctl.conf
在尾行添加以下内容
vm.max_map_count=262144
并执行命令
sysctl -p

# Install elastalert
`sudo apt install python-pip`
`git clone https://github.com/Yelp/elastalert.git`
See [elastalert](https://github.com/Yelp/elastalert.git)
...
# flatline.yaml
``` yml
# Alert when the rate of events less then a threshold for one instance

name: Email Title!

type: flatline

index: metricbeat-*

timeframe:
  minutes: 1
threshold: 10

filter:
- query:
    query_string:
      query: "container.name: containerName"
realert:
  minutes: 5

smtp_host: smtpHost
smtp_port: 25

smtp_auth_file: /home/user/docker-elk/elastalert/smtp_auth_file.yaml
email_reply_to: email@test.com
from_addr: email@test.com

alert:
- "email"

email:
- "email@test.com"
```

#smtp_auth_file.yaml

``` yml
user: email@test.com
password: password
```

# 后台运行elastalert
```
nohup python -m elastalert.elastalert --verbose --rule flatline.yaml > flatline.log 2>&1 &
```

# Stop

```
jobs

[1]+  Running                 nohup python -m elastalert.elastalert --verbose --rule flatline.yaml > flatline.log &  (wd: ~/docker-elk/elastalert)

kill %1
```
if nothing, try `ps -aux | grep pms` then `kill {pid}`
```

```




