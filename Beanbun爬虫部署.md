### 1. 说明
#### 运行服务

`spider-slave-1` 爬虫节点1、日志服务

`spider-slave-2` 爬虫节点2、Redis服务

`spider-master` 管理后台、定时器

#### 流程

* 定时器队列 `TimerQueue` 和 `spiderQueue` 队列暂时放在 `slave-2` 节点，以后可以 `Redis` 集群。
* 日志处理统一放在`slave-1`，所以服务器通过 `TCP` > `Rsyslog` 提交日志到这里。
* 以后可以 `ES` 集群, `RsysLog` > `ES`, 可以参考`conf/logstash.conf`的配置, 能直接用。

* `master` 维护 TimerQueue，定时爬取首页或者列表页，并添加新任务到 spiderQueue
* `slave-*` 是爬虫节点，每个节点监控 `spiderQueue`，有任务则执行，将结果通过`API`提交到`master`的管理后台。
* 爬虫节点可随意添加, 把 `hostname` 命名成这种格式 `spider-slave-*`

### 2. 安装拓展

#### opencc 繁简体转换
`yum install poppler-utils`

`yum install doxygen`

```shell
git clone https://github.com/BYVoid/OpenCC.git --depth 1
cd OpenCC
make
sudo make install
```

`sudo ln -s /usr/lib/libopencc.so.2 /usr/lib64/libopencc.so.2`

```shell
git clone https://github.com/NauxLiu/opencc4php --depth 1
phpize
./configure --with-php-config=/www/server/php/71/bin/php-config
make && make install
```

#### Seaslog

```shell
git clone https://github.com/SeasX/SeasLog.git
phpize
./configure --with-php-config=/www/server/php/71/bin/php-config
make && make install
```

#### Event
`pecl install event`

### 3. 配置

#### Redis

##### 配置
```conf
bind 0.0.0.0
port 1234
requirepass 123456
```

##### 测试

`redis-cli -p 1234 -a 123456 -h 192.168.0.1`

#### SeasLog + Rsyslog

##### SeasLog 配置
```conf
;1File 2TCP 3UDP (Switch default 1)
seaslog.appender = 2

;If you use  Record TCP or UDP, configure this remote ip.
;Default "127.0.0.1"
seaslog.remote_host = "192.168.0.1"

;If you use Record TCP or UDP, configure this remote port.
;Default 514
seaslog.remote_port = 514
```
##### Rsyslog 配置
```conf
$ModLoad imtcp
$InputTCPServerRun 514

$template logfile,    "/var/log/tsspider_%$year%%$month%%$day%.log"
$template logformat,"%msg%\n"
:msg,contains,        "new-spider"            ?logfile;logformat
```
##### 测试
`tail -f /var/log/tsspider_20180501.log`

`php -r "SeasLog::info('new-spider szse this is a spider log');"`

##### 日志格式
`TIME HOSTNAME SPIDERNAME MSG`

`时间 主机名 网站名称 日志消息`

```txt
2018-05-01 20:58:04 spider-slave-1 szse new-spider this is a spider log
2018-05-01 21:01:14 spider-slave-2 szse new-spider this is a spider log
2018-05-01 21:03:16 spider-master szse new-spider this is a spider log
```

### 4. 运行 new_spider
```shell
git clone https://git.coding.net/zzwisdom/new_spider.git
修改 run.conf, 每行一个，最后留一个空行
sh run.sh start
sh run.sh stop
```