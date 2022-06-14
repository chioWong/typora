```sql
#创建表
create EXTERNAL table IF NOT EXISTS TABLENAME
(
    name string,
    city string
)
PARTITIONED BY (p_mon string) #设置分区
STORED AS ORC;                #设置存储格式，默认为textFile
row format delimited fields terminated by ','; # 设置切分格式
```

```sql
# 彻底删除表
drop table TABLENAME purge;
```

```sql
# 添加数据进hive表
load data local inpath '/work/comobile/final_result20190116.csv' into table mobile.comobile PARTITION (d=20190119);
```

```sql
# 读取orc格式文件
hive --orcfiledump -d 
```

```sql
# hive表,写入动态多分区.写入的格式是textFile还是orc,是由写入表建表的时候确定的.
hive -e "SET hive.exec.dynamic.partition=true;
SET hive.exec.dynamic.partition.mode=nonstrict;
SET hive.exec.max.dynamic.partitions.pernode = 1000;
SET hive.exec.max.dynamic.partitions=1000;
INSERT INTO wqh.mdn_table2 PARTITION (op,province) SELECT phone,phone_md5,province_chinese,city,city_code,op,province from
    wqh.mdn_data;"
```

```sql
# hive 提高查询速度 !有待研究原理
set hive.vectorized.execution.enabled = true;
set hive.vectorized.execution.reduce.enabled = true;
set hive.cbo.enable=true;
set hive.compute.query.using.stats=true;
set hive.stats.fetch.column.stats=true;
set hive.stats.fetch.partition.stats=true;
```

```sql
hive -e"
set hive.vectorized.execution.enabled = true;
set hive.vectorized.execution.reduce.enabled = true;
set hive.cbo.enable=true;
set hive.compute.query.using.stats=true;
set hive.stats.fetch.column.stats=true;
set hive.stats.fetch.partition.stats=true;
SELECT a.phone,a.city,a.app_freq FROM databank.user_info_freq a join databank.user_info b ON a.phone=b.phone WHERE (a.city='深圳' or a.city='西安' or a.city='济南' or a.city='成都' or a.city='杭州') AND (a.app_freq['768']['freq']=1 or a.app_freq['1293']['freq']=1 or a.app_freq['1785']['freq']=1 or a.app_freq['478']['freq']=1) AND b.idfa!='' AND a.age>=18 AND a.age<=35 AND a.sex=0 AND a.op='liantong' AND a.m=201906;
">/home/wqh/work/temp/20190820/all.csv
```

```sql
// rdd写入hive表
val rowRDD: RDD[Row] = filtcb
      .map(item => {
        val res: Row = new GenericRowWithSchema(Array(item._1, item._2, item._3, item._4, item._5, item._6), CDRschema.apply_yd)
        res
      })
      .cache()

    val outDF: DataFrame = hiveContext.createDataFrame(rowRDD, CDRschema.apply_yd)

    outDF.registerTempTable("ydcdrTemp")

    val writesql = "INSERT INTO " + outputTable + " PARTITION (statis_day) SELECT phone,phone_md5,province_chinese,province,city,op,city_code from ydcdrTemp"

    hiveContext.sql(writesql)
```

```sql
# 新增表字段
alter table test.test_20180410 add columns(city string);
# 修改字段注释
alter table test.test_20180410 change column city city string COMMENT '城市';
# 删除字段(用新的schem替换原有的，
ALTER TABLE test.test_20180410 REPLACE COLUMNS(id BIGINT, city STRING COMMENT '城市');
```

```sql
-- 时间戳转日期
-- PLAN_START_TIME 为13位时间戳.
from_unixtime(CAST(CAST(PLAN_START_TIME AS BIGINT)/1000 AS BIGINT),'yyyy-MM-dd HH:mm:ss')
```

```sql
-- Hive内部表在元数据中会默认统计分区内数据行数，而外部表中由于没有insert overwrite操作，统计属性不会生效，需要用这个语句重新统计
ANALYZE TABLE dwd.dwd_hive_server2_audit_log_i_d COMPUTE STATISTICS;
```

