**操作**

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

**创建role和user**

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



