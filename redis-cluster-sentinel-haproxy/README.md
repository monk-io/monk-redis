# Redis & Monk
This repository contains Monk.io template to deploy Mysql system either locally or on cloud of your choice (AWS, GCP, Azure, Digital Ocean).

# Prerequisites
- [Install Monk](https://docs.monk.io/docs/get-monk)
- [Register and Login Monk](https://docs.monk.io/docs/acc-and-auth)
- [Add Cloud Provider](https://docs.monk.io/docs/cloud-provider)
- [Add Instance](https://docs.monk.io/docs/multi-cloud)

#### Make sure monkd is running.
```bash
foo@bar:~$ monk status
daemon: ready
auth: logged in
not connected to cluster
```

## Clone Repository
```bash
git clone https://github.com/Burakhan/monk-redis
```

## Load Template
```bash
cd monk-redis/redis-cluster-sentinel-haproxy
monk load MANIFEST
```


#### Let's take a look at the themes I have installed.
```bash
foo@bar:~$ monk list monk-redis                                                                                                                    
✔ Got the list
Type      Template                              Repository  Version   Tags
runnable  monk-redis-cluster-haproxy/haproxy    local       1.000000  -
runnable  monk-redis-cluster-haproxy/rds1       local       1.000000  -
runnable  monk-redis-cluster-haproxy/rds2       local       1.000000  -
runnable  monk-redis-cluster-haproxy/rds3       local       1.000000  -
runnable  monk-redis-cluster-haproxy/sentinel1  local       1.000000  -
group     monk-redis-cluster-haproxy/stack      local       -         -
```

## Deploy Stack
```bash
foo@bar:~$  monk run monk-redis-cluster-haproxy/stack                                                                
✔ Starting the job: local/monk-redis-cluster-haproxy/stack... DONE
✔ Preparing nodes DONE
✔ Checking/pulling images...
✔ [================================================] 100% docker.io/library/haproxy:latest rds-4
✔ [================================================] 100% docker.io/library/redis:latest rds-1
✔ [================================================] 100% docker.io/library/redis:latest rds-4
✔ [================================================] 100% docker.io/library/redis:latest rds-3
✔ [================================================] 100% docker.io/library/redis:latest rds-2
✔ Checking/pulling images DONE
✔ Starting containers DONE
✔ Starting containers DONE
✔ Starting containers DONE
✔ Starting containers DONE
✔ Started local/monk-redis-cluster-haproxy/stack

🔩 templates/local/monk-redis-cluster-haproxy/stack
 ├─🧊 Peer rds-3
 │  └─🔩 templates/local/monk-redis-cluster-haproxy/rds3
 │     └─📦 f4eb51219592740a6654bd1253dec707-cluster-haproxy-rds3-monk-rds3
 │        ├─🧩 docker.io/library/redis:latest
 │        ├─💾 /var/lib/monkd/volumes/redis/redis3 -> /bitnami/redis/data
 │        └─🔌 open 16.170.209.25:6387 (0.0.0.0:6387) -> 6379
 ├─🧊 Peer rds-4
 │  ├─🔩 templates/local/monk-redis-cluster-haproxy/sentinel1
 │  │  └─📦 ff4ea0a9d79548909634a3b720171074--sentinel1-monk-rds-sentinel-1
 │  │     ├─🧩 docker.io/library/redis:latest
 │  │     └─💾 /var/lib/monkd/volumes/redis/sentinel -> /bitnami/redis/data
 │  └─🔩 templates/local/monk-redis-cluster-haproxy/haproxy
 │     └─📦 df07066edb5374f8bbed176d44d92d38-proxy-haproxy-monk-rds-haproxy
 │        ├─🧩 docker.io/library/haproxy:latest
 │        ├─🔌 open 13.48.124.173:6379 (0.0.0.0:6379) -> 6379
 │        └─🔌 open 13.48.124.173:9000 (0.0.0.0:9000) -> 9000
 ├─🧊 Peer rds-2
 │  └─🔩 templates/local/monk-redis-cluster-haproxy/rds2
 │     └─📦 3051227d5eb7edfffe60ab902c54c6d2-cluster-haproxy-rds2-monk-rds2
 │        ├─🧩 docker.io/library/redis:latest
 │        ├─💾 /var/lib/monkd/volumes/redis/redis2 -> /bitnami/redis/data
 │        └─🔌 open 13.53.137.101:6389 (0.0.0.0:6389) -> 6379
 └─🧊 Peer rds-1
    └─🔩 templates/local/monk-redis-cluster-haproxy/rds1
       └─📦 5c99ef930cf923a25d82e066c8ac558b-cluster-haproxy-rds1-monk-rds1
          ├─🧩 docker.io/library/redis:latest
          ├─💾 /var/lib/monkd/volumes/redis/redis1 -> /bitnami/redis/data
          └─🔌 open 16.170.210.56:6388 (0.0.0.0:6388) -> 6379

💡 You can inspect and manage your above stack with these commands:
	monk logs (-f) local/monk-redis-cluster-haproxy/stack - Inspect logs
	monk shell     local/monk-redis-cluster-haproxy/stack - Connect to the container's shell
	monk do        local/monk-redis-cluster-haproxy/stack/action_name - Run defined action (if exists)
💡 Check monk help for more!
```

## Variables
The variables are stack section in `redis.yml` file. You can quickly setup by editing the values here.

| Variable                     	    | Description                               	|
|------------------------------	    |-------------------------------------------	|
| redis_image_tag          	       | Docker image tag                           	|
| redis1_port 	                      | Redis expose port, Default: 6388             	|
| redis2_port 	                      | Redis expose port, Default: 6389             	|
| redis2_port 	                      | Redis expose port, Default: 6387             	|
| redis_empty_password               | Redis empyt password, Default: yes    	    |
| redis_io_thread              	    | Redis IO thread count, Default: 1       	    |
| redis_io_threads_do_reads          | Default: yes                              	|
| redis_disable_commands             | Redis disable commands, Default: FLUSHDB,FLUSHALL,CONFIG |
| rds_haproxy_port                   | HAProxy Port                                  |
| rds_haproxy_stats_port             | HAProxy Stats: <ip:port>/haproxy_stats |

## 

## Stop, remove and clean up workloads and templates

```bash
monk purge -x monk-redis/stack 
```