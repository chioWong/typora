```shell
# 创建Topic
kafka-topics --create --zookeeper localhost:2181 --replication-factor 2 --partitions 3 --topic second

# 列出所有Topic
kafka-topics --list --zookeeper localhost:2181

# 开启生产者,可以发送消息
/usr/local/Cellar/kafka/2.1.1/bin/kafka-console-producer --broker-list localhost:9092 --topic first

# 开启消费者,监听9092端口. 通过设置beginning,可以选择拉取该topic下的全部消息还是从某一个offset开始拉取
/usr/local/Cellar/kafka/2.1.1/bin/kafka-console-consumer --bootstrap-server localhost:9092 --topic first --from-beginning

# 读取kafka存储的log文件
kafka-run-class.sh kafka.tools.DumpLogSegments
# 用来指定要查看的 log 或 index 文件
--files <file1, file2, ...> 
# 这是个标记选项,如果带了这个参数,则当解析 log 文件时会将存储的message和这个message对应的key都打印出来
--print-data-log

# 读取.log或.index文件
kafka-run-class kafka.tools.DumpLogSegments --files /usr/local/var/lib/kafka-logs/second-1/00000000000000000000.log --print-data-log
kafka-run-class kafka.tools.DumpLogSegments --files /usr/local/var/lib/kafka-logs/second-0/00000000000000000000.index --print-data-log

# 以字符串的方式读取log文件
strings -e S "$LOG_FILE" | iconv -c -f "UTF-8" -t "UTF-8"
```

