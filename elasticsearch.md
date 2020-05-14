GET _search
{
  "query": {
    "match_all": {}
  }
}
GET /_cat/repositories
GET /_cat/snapshots/aliyun_auto_snapshot
GET /_cat/nodes?v
GET /_cat/allocation?v

GET _cat/nodes?v&h=ip,j,port,heapPercent,heapMax

GET _cat/health
GET _cat/indices?v
GET _cat/nodes?v
GET _cat/indices?v&h=index,store.size
GET _cat/master
GET _cat/plugins?v&h=id,name,component,description
GET _cat/indices/nsp_pay_result_list_20190928?v

GET _cat/templates
GET _template/pt_nsp_settle_send_detail__template


DELETE nsp_settle_arrive_detail_202003*


#设置索引（customer）副本数
PUT /completeness-202003*/_settings
{
  "index": {
        "number_of_replicas": "0"
        }
}

#缓存清理
POST _cache/clear

#同步刷新
POST _flush/synced


#自动均衡开关

PUT /_cluster/settings
{
    "transient" : {
        "cluster.routing.allocation.enable" : "all"
    }
}


PUT /_all/_settings
{
  "settings": {
    "index.unassigned.node_left.delayed_timeout": "5m" 
  }
}

GET _cat/nodeattrs?s=attr


#调整索引监控日志保留时间（.monitoring-es-6-*）
PUT _cluster/settings
{"persistent": {"xpack.monitoring.history.duration":"0d"}}

#设置需要采集的监控索引
PUT _cluster/settings
{"persistent": {"xpack.monitoring.collection.indices": 
"*,-.*"
}
}


#剩余磁盘空间达到es最小值，添加数据被block
PUT _all/_settings
{"index.blocks.read_only_allow_delete": null}



###慢日志###
#查看集群设置
GET _settings

#单个索引
PUT /my_index/_settings
{
    "index.search.slowlog.threshold.query.warn" : "10s", 
    "index.search.slowlog.threshold.fetch.debug": "500ms", 
    "index.indexing.slowlog.threshold.index.info": "5s" 
}
####对集群设置####
#设置index慢日志时间
PUT  _settings
{
        "index.indexing.slowlog.threshold.index.debug" : "0ms",
        "index.indexing.slowlog.threshold.index.info" : "0ms",
        "index.indexing.slowlog.threshold.index.trace" : "0ms",
        "index.indexing.slowlog.threshold.index.warn" : "0ms"
}

#设置慢查询fetch：
PUT _settings
{
    "index.search.slowlog.threshold.fetch.warn":"1s",
    "index.search.slowlog.threshold.fetch.info":"800ms",
    "index.search.slowlog.threshold.fetch.debug":"500ms",
    "index.search.slowlog.threshold.fetch.trace":"200ms"
}
#设置慢查询query：
PUT _settings
{
     "index.search.slowlog.threshold.query.warn":"5s",
    "index.search.slowlog.threshold.query.info":"2s",
    "index.search.slowlog.threshold.query.debug":"1s",
    "index.search.slowlog.threshold.query.trace":"400ms"
}