---
title: prometheus指南
date: 2019-09-11 17:00:59
tags: [proemtheus，k8s]
categories: basic
---
prometheus 的数据存储类型为时间序列，prometheus周期性的从应用或pushgateway(应用将metrics发送到pushgateway)中拉取metrics的当前value
##  一.监控指标(Metrics)：

####  1.组成

-  指标(metric)：metric name和描述当前样本特征的labelsets;
- 时间戳(timestamp)：一个精确到毫秒的时间戳(查询时timestamp不可见);
- 样本值(value)： 一个folat64的浮点型数据表示当前样本的值。
**metrics示例：**
```
<--metric  name--><-------------metrics labels------> <--timestamp -->     <--value-->
http_request_total{status="200", method="GET"}@1434417560938 => 94355
http_request_total{status="200", method="GET"}@1434417561287 => 94334
```
>注意：metric部分完全相同为同一个metric

####  2.四种类型：

-  Counter 计数器指标其工作方式和计数器一样，**只增不减**记录某些事件发生的次数。
-  Gauge表盘**可增可减**的类似于仪表盘，用于反映当前系统的状态
-  分析数据分布情况：
      -   Histogram 柱状图，用于统计一些数据分布的情况，用于计算在一定范围内的分布情况，同时还提供了度量指标值的总和
      -   Summary  摘要，计算在一定时间窗口范围内度量指标对象的总数以及所有对量指标值的总和。

**四种类型示例：**
```
# histogram 类型示例
<---------------------------metric name----------------------->  <value range> <count>
prometheus_tsdb_compaction_chunk_range_bucket{le="1000"}          0
prometheus_tsdb_compaction_chunk_range_bucket{le="4000"}          0
prometheus_tsdb_compaction_chunk_range_bucket{le="16000"}        100
prometheus_tsdb_compaction_chunk_range_bucket{le="64000"}        160
prometheus_tsdb_compaction_chunk_range_bucket{le="256000"}     260 # value 小于25600的个数统计，包含前一段的数据
prometheus_tsdb_compaction_chunk_range_bucket{le="1024000"}  780
prometheus_tsdb_compaction_chunk_range_bucket{le="+Inf"}           780 # prometheus 自添加的范围
prometheus_tsdb_compaction_chunk_range_sum 1.1540798e+09  #value 统计,prometheus自行处理
prometheus_tsdb_compaction_chunk_range_count 780  #个数总计，prometheus自行处理

# summary 类型示例
<---------metric name-----><quantile range>  <value>
go_gc_duration_seconds{quantile="0"}        3.1888e-05
go_gc_duration_seconds{quantile="0.25"} 3.6715e-05 # 25%小于3.6715e-05
go_gc_duration_seconds{quantile="0.5"}    5.1522e-05
go_gc_duration_seconds{quantile="0.75"} 8.1027e-05
go_gc_duration_seconds{quantile="1"}      0.000161162
go_gc_duration_seconds_sum 0.001803144 # gc的总时间，prometheus自行处理
go_gc_duration_seconds_count 27 #gc 的次数，prometheus自行处理
```

## 二.应用埋点：

#### 1.前提(监控的规范配置)：
prometheus支持自动发现机制，需要我们在应用部署的yaml添加配置支持，分为service和pod,某些部署应用只有pod没有service，这种情况只能在pod上加配置，通过pods采集metrics。如果有service，就无需在pod加配置，直接在service上加即可。否则，prometheus将采集两次数据，service-endpoints最终也会落到pod上。
#### pod
```yaml
 metadata:
     annotations:
         prometheus.io/scrape，为true则会将pod作为监控目标。
         prometheus.io/path，默认为/metrics
         prometheus.io/port , 端口
    labels:   # 添加额外的信息，在采集时填入metrics labels
```
> 注意：经过测试，一个POD只支持一个path的配置，当一个POD中有多个container的metrics,考虑使用pushgateway,此时应当在采集时区分来自应用，最佳的做法:分开部署。

#### service-endpoints
```yaml
 metadata:
     annotations:
        prometheus.io/scrape，为true则会将pod作为监控目标。
        prometheus.io/path，默认为/metrics
        prometheus.io/port , 端口
        prometheus.io/scheme 默认http，如果为了安全设置了https，此处需要改为https
    labels:   # 添加额外的信息，在采集时填入metrics labels
```
#### 2.开发
在应用中埋点，需要引入对应语言的[prometheus库](https://github.com/prometheus).
**埋点的原理：**埋点的产生的metrics将一直在内存中，依据应用的运行状态更新value,直到应用停止运行
适用于：
- 记录系统的资源使用情况，
- 应用的运行状态
- 错误发生的计数
...

 应用埋点产生可控的metrics,metrics不应当用于记录用户的行为，随着运行时长(用户增加)，产生大量metrics，占满内存。需要持久化与用户强相关的数据，不应当使用prometheus，metrics只用记录应用运行状态，存于内存中。



