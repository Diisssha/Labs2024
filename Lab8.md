1. Запустите контейнер и посотрите информацию о состоянии docker на хосте. Выведите информацию о типе операционной системы на которой установлен docker в файл, этом используйте командку docker info.
```
Server:
 Containers: 1
  Running: 1
  Paused: 0
  Stopped: 0
 Images: 4
 Server Version: 26.1.1
 Storage Driver: overlay2
  Backing Filesystem: extfs
  Supports d_type: true
  Using metacopy: false
  Native Overlay Diff: true
  userxattr: false
 Logging Driver: json-file
 Cgroup Driver: cgroupfs
 Cgroup Version: 1
 Plugins:
  Volume: local
  Network: bridge host ipvlan macvlan null overlay
  Log: awslogs fluentd gcplogs gelf journald json-file local splunk syslog
 Swarm: inactive
 Runtimes: io.containerd.runc.v2 runc
 Default Runtime: runc
 Init Binary: docker-init
 containerd version: e377cd56a71523140ca6ae87e30244719194a521
 runc version: v1.1.12-0-g51d5e94
 init version: de40ad0
 Security Options:
  seccomp
   Profile: unconfined
 Kernel Version: 5.15.146.1-microsoft-standard-WSL2
 Operating System: Docker Desktop
 OSType: linux
 Architecture: x86_64
 CPUs: 8
 Total Memory: 9.176GiB
 Name: docker-desktop
 ID: 69aa57e8-e65f-4ee2-a941-5fa99e4c6608
 Docker Root Dir: /var/lib/docker
 Debug Mode: false
 HTTP Proxy: http.docker.internal:3128
 HTTPS Proxy: http.docker.internal:3128
 No Proxy: hubproxy.docker.internal
 Labels:
  com.docker.desktop.address=npipe://\\.\pipe\docker_cli
 Experimental: false
 Insecure Registries:
  hubproxy.docker.internal:5555
  127.0.0.0/8
 Live Restore Enabled: false
```

 2. Запустите произвольный контейнер. Выполните тестирование производительности в течении 3 минут. Оцените общую производительность во время выполения теста системы. Заморозьте контейнер. Разморозьте контейнер.
```
docker run -itd --name apache24 -p 8080:80 --network srv httpd:2.4
44c8dafef9b9ded7d43b1bd22af5f542f830863c02df6a6ee8f8a8f19d65f5b8
docker run --rm --network srv jordi/ab -k -c 100 -n 18000 http://apache24:80/
This is ApacheBench, Version 2.3 <$Revision: 1826891 $>
Copyright 1996 Adam Twiss, Zeus Technology Ltd, http://www.zeustech.net/
Licensed to The Apache Software Foundation, http://www.apache.org/

Benchmarking apache24 (be patient)
Completed 1800 requests
Completed 3600 requests
Completed 5400 requests
Completed 7200 requests
Completed 9000 requests
Completed 10800 requests
Completed 12600 requests
Completed 14400 requests
Completed 16200 requests
Completed 18000 requests
Finished 18000 requests


Server Software:        Apache/2.4.62
Server Hostname:        apache24
Server Port:            80

Document Path:          /
Document Length:        45 bytes

Concurrency Level:      100
Time taken for tests:   0.961 seconds
Complete requests:      18000
Failed requests:        861
   (Connect: 0, Receive: 0, Length: 839, Exceptions: 22)
Keep-Alive requests:    17138
Total transferred:      5577170 bytes
HTML transferred:       772245 bytes
Requests per second:    18735.34 [#/sec] (mean)
Time per request:       5.338 [ms] (mean)
Time per request:       0.053 [ms] (mean, across all concurrent requests)
Transfer rate:          5668.96 [Kbytes/sec] received

Connection Times (ms)
              min  mean[+/-sd] median   max
Connect:        0    0   0.7      0       9
Processing:     0    5   6.5      4      86
Waiting:        0    5   6.5      3      86
Total:          0    5   6.6      4      86

Percentage of the requests served within a certain time (ms)
  50%      4
  66%      6
  75%      7
  80%      8
  90%     11
  95%     15
  98%     23
  99%     31
 100%     86 (longest request)
```

 3. Посмотрите логи веб сервера apache, когда контейнер находится под тестированием 
 производительности.
```
docker logs apache24
```

часть логов:
```
172.18.0.3 - - [11/Oct/2024:20:19:46 +0000] "GET / HTTP/1.0" 200 45
172.18.0.3 - - [11/Oct/2024:20:19:46 +0000] "GET / HTTP/1.0" 200 45
172.18.0.3 - - [11/Oct/2024:20:19:46 +0000] "GET / HTTP/1.0" 200 45
172.18.0.3 - - [11/Oct/2024:20:19:46 +0000] "GET / HTTP/1.0" 200 45
172.18.0.3 - - [11/Oct/2024:20:19:46 +0000] "GET / HTTP/1.0" 200 45
172.18.0.3 - - [11/Oct/2024:20:19:46 +0000] "GET / HTTP/1.0" 200 45
172.18.0.3 - - [11/Oct/2024:20:19:46 +0000] "GET / HTTP/1.0" 200 45
172.18.0.3 - - [11/Oct/2024:20:19:46 +0000] "GET / HTTP/1.0" 200 45
172.18.0.3 - - [11/Oct/2024:20:19:46 +0000] "GET / HTTP/1.0" 200 45
172.18.0.3 - - [11/Oct/2024:20:19:46 +0000] "GET / HTTP/1.0" 200 45
172.18.0.3 - - [11/Oct/2024:20:19:46 +0000] "GET / HTTP/1.0" 200 45
172.18.0.3 - - [11/Oct/2024:20:19:46 +0000] "GET / HTTP/1.0" 200 45
172.18.0.3 - - [11/Oct/2024:20:19:46 +0000] "GET / HTTP/1.0" 200 45
172.18.0.3 - - [11/Oct/2024:20:19:46 +0000] "GET / HTTP/1.0" 200 45
172.18.0.3 - - [11/Oct/2024:20:19:46 +0000] "GET / HTTP/1.0" 200 45
172.18.0.3 - - [11/Oct/2024:20:19:46 +0000] "GET / HTTP/1.0" 200 45
172.18.0.3 - - [11/Oct/2024:20:19:46 +0000] "GET / HTTP/1.0" 200 45
172.18.0.3 - - [11/Oct/2024:20:19:46 +0000] "GET / HTTP/1.0" 200 45
172.18.0.3 - - [11/Oct/2024:20:19:46 +0000] "GET / HTTP/1.0" 200 45
172.18.0.3 - - [11/Oct/2024:20:19:46 +0000] "GET / HTTP/1.0" 200 45
172.18.0.3 - - [11/Oct/2024:20:19:46 +0000] "GET / HTTP/1.0" 200 45
172.18.0.3 - - [11/Oct/2024:20:19:46 +0000] "GET / HTTP/1.0" 200 45
172.18.0.3 - - [11/Oct/2024:20:19:46 +0000] "GET / HTTP/1.0" 200 45
172.18.0.3 - - [11/Oct/2024:20:19:46 +0000] "GET / HTTP/1.0" 200 45
172.18.0.3 - - [11/Oct/2024:20:19:46 +0000] "GET / HTTP/1.0" 200 45
172.18.0.3 - - [11/Oct/2024:20:19:46 +0000] "GET / HTTP/1.0" 200 45
172.18.0.3 - - [11/Oct/2024:20:19:46 +0000] "GET / HTTP/1.0" 200 45
172.18.0.3 - - [11/Oct/2024:20:19:46 +0000] "GET / HTTP/1.0" 200 45
172.18.0.3 - - [11/Oct/2024:20:19:46 +0000] "GET / HTTP/1.0" 200 45
172.18.0.3 - - [11/Oct/2024:20:19:46 +0000] "GET / HTTP/1.0" 200 45
172.18.0.3 - - [11/Oct/2024:20:19:46 +0000] "GET / HTTP/1.0" 200 45
```

4. Запустите еще один контейнер с mongodb. Посмотрите статистику всех конетйнеров, используя (--format), не включая столбец PIDS. Запишите все результаты в файл.
```
docker stats --format "table {{.Name}}\t{{.CPUPerc}}\t{{.MemUsage}}\t{{.NetIO}}" --no-stream
NAME       CPU %     MEM USAGE / LIMIT     NET I/O
mymongo    0.60%     97.81MiB / 9.176GiB   946B / 0B
apache24   0.01%     43.77MiB / 9.176GiB   3.26MB / 6.97MB
```

5/6.