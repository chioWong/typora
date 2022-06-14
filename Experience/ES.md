```json
## 新建es索引，设置分词器
{
  "settings": {
    "analysis": {
      "analyzer": {
        "db_analyzer": {
          "lowercase": "true",
          "pattern": "\\W|_",
          "type": "pattern"
        },
        "tag_analyzer": {
          "lowercase": "true",
          "pattern": ",",
          "type": "pattern"
        },
        "case_sensitive_analyzer": {
          "lowercase": "true",
          "type": "standard"
        }
      }
    }
  },
  "mappings": {
    "properties": {
      "tbl_id": {
        "type": "long"
      },
      "tbl_name": {
        "type": "text",
        "analyzer": "db_analyzer"
      },
      "db_id": {
        "type": "long"
      },
      "db_name": {
        "type": "text",
        "analyzer": "db_analyzer"
      },
      "tbl_comment": {
        "type": "text",
        "analyzer": "case_sensitive_analyzer"
      },
      "tbl_tag": {
        "type": "text",
        "analyzer": "tag_analyzer"
      },
      "tbl_score": {
        "type": "long"
      },
      "tbl_dev_owner": {
        "type": "text",
        "analyzer": "case_sensitive_analyzer"
      },
      "tbl_prod_owner": {
        "type": "text",
        "analyzer": "case_sensitive_analyzer"
      },
      "tbl_column_name": {
        "type": "text",
        "analyzer": "db_analyzer"
      },
      "tbl_column_comment": {
        "type": "text",
        "analyzer": "case_sensitive_analyzer"
      }
    }
  }
}
```

```json
## 添加别名
{
  "actions": [
    {
      "add": {
        "index": "meta_data_tbl_1",
        "alias": "meta_data_tbl"
      }
    }
  ]
}
```

