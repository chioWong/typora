```shell
## 本地jar添加到MAVEN库
mvn install:install-file 
-Dfile=D:\Intellij\bahir-flink-master\flink-connector-redis\target\flink-connector-redis_2.11-1.1-SNAPSHOT.jar 
-DartifactId=bahir-flink-parent_2.11 -DgroupId=org.apache.flink.streaming.connectors.redis  -Dversion=1.1-SNAPSHOT -Dpackaging=jar

# 然后在pom.xml正常添加依赖即可.编译打包时，本地jar也会被打进去

```

