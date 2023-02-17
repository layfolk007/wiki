按sendFixDate升序，显示最小的1条

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

按sendFixDate查询180天前的总数

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

删除100天前文档，只是标记删除，并没有物理删除，空间不会释放

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

强制merge，释放空间，会造成索引不可读写，只能等操作完成

```
curl -XPOST 'http://localhost:9200/_forcemerge?only_expunge_deletes=true&max_num_segments=1'
```
