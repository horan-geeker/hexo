---
title: ab stress testing for laravel thinkPHP and openresty
date: 2017-03-29 15:11:23
categories: blog
tags: 后端
---
# 压测

## ab
均使用使用长连接
硬件配置：单核 1G 阿里云ECS

#### openresty
访问一次redis
```
Concurrency Level:      100
Time taken for tests:   1.843 seconds
Complete requests:      10000
Failed requests:        0
Keep-Alive requests:    9900
Total transferred:      1699500 bytes
HTML transferred:       170000 bytes
Requests per second:    5425.49 [#/sec] (mean)
Time per request:       18.431 [ms] (mean)
Time per request:       0.184 [ms] (mean, across all concurrent requests)
Transfer rate:          900.45 [Kbytes/sec] received

```
访问一次mysql
```
Concurrency Level:      100
Time taken for tests:   3.348 seconds
Complete requests:      10000
Failed requests:        0
Keep-Alive requests:    9926
Total transferred:      1709630 bytes
HTML transferred:       180000 bytes
Requests per second:    2986.84 [#/sec] (mean)
Time per request:       33.480 [ms] (mean)
Time per request:       0.335 [ms] (mean, across all concurrent requests)
Transfer rate:          498.67 [Kbytes/sec] received
```

#### laravel(不适合并发场景，提供小数据参考)
小数据测试
```
Concurrency Level:      5
Complete requests:      100
Requests per second:    140.32 [#/sec] (mean)
Time per request:       35.632 [ms] (mean)

Concurrency Level:      10
Complete requests:      100
Requests per second:    109.62 [#/sec] (mean)
Time per request:       91.227 [ms] (mean)

Concurrency Level:      15
Complete requests:      100
Requests per second:    138.24 [#/sec] (mean)
Time per request:       108.510 [ms] (mean)

Concurrency Level:      20
Complete requests:      100
Requests per second:    140.74 [#/sec] (mean)
Time per request:       142.101 [ms] (mean)
```
细腻的数据表明laravel在超过5个请求的时候，就开始成倍增长了

访问一次mysql
```
Concurrency Level:      100
Time taken for tests:   73.146 seconds
Complete requests:      10000
Failed requests:        9877
   (Connect: 0, Receive: 0, Length: 9877, Exceptions: 0)
Non-2xx responses:      9877
Keep-Alive requests:    0
Total transferred:      3168103 bytes
HTML transferred:       194145 bytes
Requests per second:    136.71 [#/sec] (mean)
Time per request:       731.455 [ms] (mean)
Time per request:       7.315 [ms] (mean, across all concurrent requests)
Transfer rate:          42.30 [Kbytes/sec] received

```

#### ThinkPHP
访问一次mysql
```
Concurrency Level:      100
Time taken for tests:   22.863 seconds
Complete requests:      10000
Failed requests:        0
Keep-Alive requests:    0
Total transferred:      6840000 bytes
HTML transferred:       3640000 bytes
Requests per second:    437.39 [#/sec] (mean)
Time per request:       228.631 [ms] (mean)
Time per request:       2.286 [ms] (mean, across all concurrent requests)
Transfer rate:          292.16 [Kbytes/sec] received

```
