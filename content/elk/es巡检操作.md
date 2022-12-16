---
title: "es巡检操作"
date: 2022-11-04T09:58:06+08:00
description: es巡检操作
draft: false
hideToc: false
enableToc: true
enableTocContent: false
tocFolding: false
tocPosition: inner
tocLevels: ["h2", "h3", "h4"]
tags: []
series:
categories: []
image:
---
### 集群状态
`GET _cluster/health`  
关注status值，green为健康，yellow为有索引副本分片丢失，red为有索引主分片丢失，会出现查询异常状况
### 任务状态
`GET _cat/pending_tasks?v`  
查看集群中等待的任务状态，insertOrder：任务插入顺序；timeInQueue：任务已进入队列多长时间；priority：任务优先级；source：任务来源
### 节点状态
`GET _cat/nodes?h=ip,name,node.role,master,uptime,cpu&v`
### 磁盘状态
`GET _cat/allocation?v`  
disk.indices：索引占用空间；disk.used：磁盘已使用空间；disk.avail：磁盘空闲空间；disk.total：磁盘总空间；disk.percent：磁盘使用率
### 分片状态
`GET _cat/shards?v&s=store:desc`  
节点上的分片数，官方建议jvm 1G对应20个分片；分片大小，官方建议10G-65G，不应过大
### 索引状态
`GET {index_name}/_settings?include_defaults&flat_settings`  
index.number_of_replicas：副本数；index.refresh_interval：refresh频率；index.max_result_window：单次最大返回数
### jvm状态
`GET _nodes/stats/indices,jvm?human`  
jvm.mem.heap_used_percent：堆内存使用率；jvm.mem.gc.old.collection_count：full gc次数
### threadpool
`GET _cat/thread_pool?v&s=rejected:desc`  
bulk reject写入拒绝，关注各节点下name为write的rejected值；search reject查询拒绝，关注各节点下name为search的rejected值