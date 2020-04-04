---
title: stress testing
date: 2017-03-29 15:11:23
categories: blog
tags: 后端
---

# 压测

## ab 工具压测

均使用使用长连接
使用资源：均使用单核 cpu （nginx 绑定 单核 cpu 或 docker 限制使用 --cpus=1）
配置
> worker_processes  2;
> worker_cpu_affinity 0001;

### openresty 配置 content_by_lua_file 只输出 ngx.say

ab -k -c 100 -n 10000 http://localhost:60000/lua-file

```
Concurrency Level:      100
Time taken for tests:   0.155 seconds
Complete requests:      10000
Failed requests:        0
Keep-Alive requests:    10000
Total transferred:      1700000 bytes
HTML transferred:       30000 bytes
Requests per second:    64321.92 [#/sec] (mean)
Time per request:       1.555 [ms] (mean)
Time per request:       0.016 [ms] (mean, across all concurrent requests)
Transfer rate:          10678.44 [Kbytes/sec] received
```

### openresty 配置 content_by_lua_block 只输出 ngx.say

ab -k -c 100 -n 10000 http://localhost:60000/lua

```
Concurrency Level:      100
Time taken for tests:   0.157 seconds
Complete requests:      10000
Failed requests:        0
Keep-Alive requests:    10000
Total transferred:      1700000 bytes
HTML transferred:       30000 bytes
Requests per second:    63688.59 [#/sec] (mean)
Time per request:       1.570 [ms] (mean)
Time per request:       0.016 [ms] (mean, across all concurrent requests)
Transfer rate:          10573.30 [Kbytes/sec] received
```

### openresty 配置 nana

ab -k -c 100 -n 10000 http://localhost:60000/index

```
Concurrency Level:      100
Time taken for tests:   0.303 seconds
Complete requests:      10000
Failed requests:        0
Keep-Alive requests:    10000
Total transferred:      2170000 bytes
HTML transferred:       420000 bytes
Requests per second:    32985.77 [#/sec] (mean)
Time per request:       3.032 [ms] (mean)
Time per request:       0.030 [ms] (mean, across all concurrent requests)
Transfer rate:          6990.15 [Kbytes/sec] received
```

### golang 原生 http 压测

ab -k -c 100 -n 10000 http://localhost:60002/bar

```
Concurrency Level:      100
Time taken for tests:   0.328 seconds
Complete requests:      10000
Failed requests:        0
Keep-Alive requests:    10000
Total transferred:      1780000 bytes
HTML transferred:       370000 bytes
Requests per second:    30494.68 [#/sec] (mean)
Time per request:       3.279 [ms] (mean)
Time per request:       0.033 [ms] (mean, across all concurrent requests)
Transfer rate:          5300.83 [Kbytes/sec] received
```

### golang gin 框架压测

ab -k -c 100 -n 10000 http://localhost:60002/ping

```
Concurrency Level:      100
Time taken for tests:   0.462 seconds
Complete requests:      10000
Failed requests:        0
Keep-Alive requests:    10000
Total transferred:      1650000 bytes
HTML transferred:       180000 bytes
Requests per second:    21648.77 [#/sec] (mean)
Time per request:       4.619 [ms] (mean)
Time per request:       0.046 [ms] (mean, across all concurrent requests)
Transfer rate:          3488.33 [Kbytes/sec] received
```

## wrk 工具压测

### openresty 配置 content_by_lua_file 只输出 ngx.say

wrk -t1 -c 100 -d10s http://localhost:60000/lua-file

```
Running 10s test @ http://localhost:60000/lua-file
  1 threads and 100 connections
  Thread Stats   Avg      Stdev     Max   +/- Stdev
    Latency     3.01ms    3.91ms  24.57ms   83.61%
    Req/Sec    67.62k     4.07k   73.21k    80.00%
  672346 requests in 10.01s, 121.19MB read
Requests/sec:  67143.94
Transfer/sec:     12.10MB
```

### openresty 配置 content_by_lua_block 只输出 ngx.say

wrk -t1 -c 100 -d10s http://localhost:60000/lua

```
Running 10s test @ http://localhost:60000/lua
  1 threads and 100 connections
  Thread Stats   Avg      Stdev     Max   +/- Stdev
    Latency     3.09ms    4.08ms  25.10ms   83.45%
    Req/Sec    69.08k     3.72k   75.70k    73.00%
  686708 requests in 10.02s, 123.78MB read
Requests/sec:  68558.83
Transfer/sec:     12.36MB
```

### openresty 配置 nana

wrk -t1 -c 100 -d10s http://localhost:60000/index

```
Running 10s test @ http://localhost:60000/index
  1 threads and 100 connections
  Thread Stats   Avg      Stdev     Max   +/- Stdev
    Latency     4.19ms    4.44ms  29.00ms   82.53%
    Req/Sec    34.98k     2.18k   37.34k    84.00%
  348020 requests in 10.03s, 78.33MB read
Requests/sec:  34713.01
Transfer/sec:      7.81MB
```

### golang 原生 http 压测

wrk -t1 -c 100 -d10s http://localhost:60002/bar

```
Running 10s test @ http://localhost:60002/bar
  1 threads and 100 connections
  Thread Stats   Avg      Stdev     Max   +/- Stdev
    Latency     6.45ms   10.80ms 127.35ms   88.51%
    Req/Sec    30.82k     5.73k   47.39k    80.00%
  307005 requests in 10.02s, 45.09MB read
Requests/sec:  30626.31
Transfer/sec:      4.50MB
```

### golang gin 框架压测

wrk -t1 -c 100 -d10s http://localhost:60002/ping

```
Running 10s test @ http://localhost:60002/ping
  1 threads and 100 connections
  Thread Stats   Avg      Stdev     Max   +/- Stdev
    Latency     8.05ms   10.04ms  78.14ms   85.71%
    Req/Sec    20.39k     3.19k   26.53k    68.00%
  203091 requests in 10.02s, 27.31MB read
Requests/sec:  20260.14
Transfer/sec:      2.72MB
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
