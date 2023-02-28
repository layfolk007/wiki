---
title: "elk常用指令"
date: 2022-10-28T16:52:09+08:00
description: "elk常用指令"
draft: false
hideToc: false
enableToc: true
enableTocContent: false
tocFolding: false
tocPosition: inner
tocLevels: ["h2", "h3", "h4"]
tags: ["elk"]
series: ["elk"]
categories: ["elk"]
image:
---
```
查看memlock
curl -XGET -uelastic:redhat 'http://localhost:9200/_nodes?pretty&filter_path=**.mlockall'

创建index(需要create_index权限)
curl -XPUT -uwjx_els_es:redhat 'http://localhost:9200/productindex'

查询mapping(需要view_index_metadata权限)
curl -XGET -uwjx_els_es:redhat 'http://localhost:9200/productindex/_mapping?pretty'

设置mapping(需要write权限)
curl -XPOST -H "Content-Type: application/json" -uwjx_els_es:redhat 'http://localhost:9200/productindex/_mapping?pretty' -d '
{
    "properties": {
        "title": {
            "type": "text",
            "store": "true"
        },
        "description": {
            "type": "text",
            "index": "false"
        },
        "price": {
            "type": "double"
        },
        "onSale": {
            "type": "boolean"
        },
        "type": {
            "type": "integer"
        },
        "createDate": {
            "type": "date"
        }
    }
}'

查询settings(需要view_index_metadata权限)
curl -XGET -uwjx_els_es:redhat 'http://localhost:9200/productindex/_settings?pretty'

设置settings(需要manage权限，此为index的管理员权限，谨慎授权)
curl -XPUT -H "Content-Type: application/json" -uwjx_els_es:redhat 'http://localhost:9200/productindex/_settings?pretty' -d '
{
    "index.max_result_window": 1000000
}'
```

#### 创建role和user
```
curl -u elastic -X POST "localhost:9200/_security/role/wjx?pretty" -H 'Content-Type: application/json' -d '
{
  "cluster": [],
  "indices": [
    {
      "names": [ "*" ],
      "privileges": ["create_index","read","write","manage"]
    }
  ]
}'

curl -u elastic -X POST "localhost:9200/_security/user/wjx_els_es?pretty" -H 'Content-Type: application/json' -d '
{
  "password" : "",
  "roles" : [ "wjx" ],
  "full_name" : "",
  "email" : ""
}'
```
#### 按sendFixDate升序，显示最小的1条
```
curl -H'Content-Type:application/json' -d'{
    "query": {
        "match_all": {}
    },
    "sort": [
        {
            "sendFixDate": { "order": "asc"}
        }
    ],
    "size": 1
}' -u elastic 'http://localhost:9200/netty_gps_v7/_search?pretty'
```
#### 按sendFixDate查询180天前的总数
```
curl -H'Content-Type:application/json' -d'{
    "query": {
        "range": {
            "sendFixDate": {
                "lt": "now-180d",
                "format": "epoch_millis"
            }
        }
    }
}' -u elastic 'http://localhost:9200/netty_gps_v7/_count?pretty'
```
#### 删除100天前文档，只是标记删除，并没有物理删除，空间不会释放
```
curl -H'Content-Type:application/json' -d'{
    "query": {
        "range": {
            "sendFixDate": {
                "lt": "now-100d",
                "format": "epoch_millis"
            }
        }
    }
}' -u elastic -XPOST "http://localhost:9200/logstash_*/_delete_by_query?conflicts=proceed"
```
#### 强制merge，释放空间，会造成索引不可读写，只能等操作完成
```
curl -XPOST 'http://localhost:9200/_forcemerge?only_expunge_deletes=true&max_num_segments=1'
```