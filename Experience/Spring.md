```java
@Entity
@Table(name = "doc_user")
@Data
public class User implements Serializable {
    ...
}
// Entity注解过的是实体类,没有get,set 方法 
// 解决:Intellij Idea,下载安装Lombok plugin插件并重启Idea.
```

```java
// Field userRulePepository in com.yunheit.service.RuleService required a bean of type 'com.yunheit.dao.UserRulePepository' that could not be found.
/**
 * 启动项目时,显示dao目录下接口找不到
 * 这是因为在spring扫描被注解过的类时,不会扫描接口，需要在启动类上加上注解
 * 指定必须要全部扫描的目录
 */
@EnableJpaRepositories("com.yunheit.dao")
public class Application {
    public static void main(String[] args) {
        SpringApplication.run(Application.class, args);
    }
}
```

```java
//Failed to configure a DataSource: 'url' attribute is not specified and no embe......

// 该错误是因为Spring数据库配置存在问题。一般来说，可能是application.properties或yml文件没有被读取到
// 解决：
// 1.在pom.xml添加<resource><\resources>来确保文件读到了
// 2.在/config路径下，添加专门用于配置DataSource的类
@Configuration
public class DataSourceConfig {
    @Bean
    public DataSource datasource(){
        return DataSourceBuilder.create()
                .driverClassName("com.mysql.cj.jdbc.Driver")
                .url("jdbc:mysql://localhost:3306/doc")
                .username("root")
                .password("112358")
                .build();
    }
}
```

```java
// 数据库中有json格式数据，无法写入
// 解决：转成string格式再写入
// dao
@Query(value = "UPDATE doc_customer SET status= :status,customer_name= :customer_name,customer_info=  :customer_info  WHERE user_id= :user_id ", nativeQuery = true)
void editCurrentlyCustomer(@Param("status") Integer status,@Param("customer_name") String customer_name,@Param("customer_info")String customer_info,@Param("user_id") Integer user_id);
// service
customerRepository.editCurrentlyCustomer(status, customer_name, customer_info.toString(), user_id);
```

