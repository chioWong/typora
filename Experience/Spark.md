```scala
// 保存文件时动态多分区

//1.实现类 RDDMultipleTextOutputFormat
import org.apache.hadoop.io.NullWritable
import org.apache.hadoop.mapred.lib.MultipleTextOutputFormat
class RDDMultipleTextOutputFormat extends MultipleTextOutputFormat[Any, Any] {
  override def generateFileNameForKeyValue(key: Any, value: Any, name: String): String = s"${key.toString}/r"

  override def generateActualKey(key: Any, value: Any): Any = NullWritable.get()
}

//2.以hadoop格式写出
val result:RDD[(String,String)] = sc.textfile("")...
result.partitionBy(new HashPartitioner(31))
   .saveAsHadoopFile(outpath, classOf[String], classOf[String], classOf[RDDMultipleTextOutputFormat], classOf[GzipCodec])

```

```scala
// RDD做成hive临时表
// 1.将RDD转为Row格式
 val rowRDD: RDD[Row] = inputRDD
    .map(item => {
      val res: Row = new GenericRowWithSchema(Array(item._1, item._2, item._3, item._4, item._5, item._6, item._7), CitySchema.applyMdnOut)
      res
    })
    .cache()

//2.将RDD做成DataFrame
 val outDF: DataFrame = hiveContext.createDataFrame(rowRDD, CitySchema.applyMdnOut)

//3.将DataFrame注册成临时表
outDF.registerTempTable("mdndata")
```

```scala
val sparkConf = new SparkConf.setAppName("sort").setMaster("local[*]")
val sc = new SparkConext.setConf(sparkConf)
sc.textFile(_)
  .map(item=>{
      val arr=item.split("\t")
      (arr(1),(arr(0),arr(2)))
  })
  .reduceByKey(item=>{
      val iter = Iter
  })
```

