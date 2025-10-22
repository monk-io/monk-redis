# Redis & Monk

This repository contains Monk.io template to deploy Mysql system either locally or on cloud of your choice (AWS, GCP, Azure, Digital Ocean).

## Prerequisites

- [Install Monk](https://docs.monk.io/docs/get-monk)
- [Register and Login Monk](https://docs.monk.io/docs/acc-and-auth)
- [Add Cloud Provider](https://docs.monk.io/docs/cloud-provider)
- [Add Instance](https://docs.monk.io/docs/multi-cloud)

## Make sure monkd is running

```bash
foo@bar:~$ monk status
daemon: ready
auth: logged in
not connected to cluster
```

## Clone Repository

```bash
git clone https://github.com/monk-io/monk-redis
```

## Load Template

```bash
cd monk-redis/redis-cluster-sentinel-haproxy
monk load MANIFEST
```

## Let's take a look at the themes I have installed

```bash
foo@bar:~$ monk list redis-cluster-haproxy
✔ Got the list
Type      Template                               Repository  Version  Tags
runnable  redis-cluster-haproxy/base             local       -        -
runnable  redis-cluster-haproxy/haproxy          local       -        -
runnable  redis-cluster-haproxy/redis-leader     local       -        -
runnable  redis-cluster-haproxy/redis-follower-1 local       -        -
runnable  redis-cluster-haproxy/redis-follower-2 local       -        -
runnable  redis-cluster-haproxy/sentinel         local       -        -
group     redis-cluster-haproxy/stack            local       -        -
```

## Deploy Stack

```bash
foo@bar:~$  monk run redis-cluster-haproxy/stack
✔ Starting the job: local/redis-cluster-haproxy/stack... DONE
✔ Preparing nodes DONE
✔ Checking/pulling images...
✔ [================================================] 100% bitnami/redis:latest local
✔ [================================================] 100% haproxy:latest local
✔ [================================================] 100% bitnami/redis-sentinel:latest local
✔ Checking/pulling images DONE
✔ Starting containers DONE
✔ New container 83c00af62bb27380b3a0d5c2c43cf52d-ter-haproxy-redis-leader-redis created DONE
✔ New container b29a8e8293e7a0ab753bba2c027cf959-er-haproxy-redis-follower-1-redis created DONE
✔ New container 11b6f7a1bf583da1115f12a3522daf5a-er-haproxy-redis-follower-2-redis created DONE
✔ New container d82529b141164d4bb01531a2b244fbf0-ster-haproxy-sentinel-sentinel created DONE
✔ New container b6c8bce840625b0dc00d32bd1d388698-luster-haproxy-haproxy-haproxy created DONE
✔ Started local/redis-cluster-haproxy/stack
🔩 templates/local/redis-cluster-haproxy/stack
 └─🧊 Peer local
    ├─🔩 templates/local/redis-cluster-haproxy/redis-leader
    │  └─📦 83c00af62bb27380b3a0d5c2c43cf52d-ter-haproxy-redis-leader-redis running
    │     ├─🧩 bitnami/redis:latest
    │     ├─💾 /var/lib/monkd/volumes/redis/leader -> /bitnami/redis/data
    │     └─🔌 open 1.1.1.1:6387 (0.0.0.0:6387) -> 6379
    ├─🔩 templates/local/redis-cluster-haproxy/redis-follower-1
    │  └─📦 b29a8e8293e7a0ab753bba2c027cf959-er-haproxy-redis-follower-1-redis running
    │     ├─🧩 bitnami/redis:latest
    │     ├─💾 /var/lib/monkd/volumes/redis/follower-1 -> /bitnami/redis/data
    │     └─🔌 open 1.1.1.1:6388 (0.0.0.0:6388) -> 6379
    ├─🔩 templates/local/redis-cluster-haproxy/sentinel
    │  └─📦 d82529b141164d4bb01531a2b244fbf0-ster-haproxy-sentinel-sentinel running
    │     └─🧩 bitnami/redis-sentinel:latest
    ├─🔩 templates/local/redis-cluster-haproxy/haproxy
    │  └─📦 b6c8bce840625b0dc00d32bd1d388698-luster-haproxy-haproxy-haproxy running
    │     ├─🧩 haproxy:latest
    │     ├─🔌 open 1.1.1.1:6379 -> 6379
    │     └─🔌 open 1.1.1.1:9000 -> 9000
    └─🔩 templates/local/redis-cluster-haproxy/redis-follower-2
       └─📦 11b6f7a1bf583da1115f12a3522daf5a-er-haproxy-redis-follower-2-redis running
          ├─🧩 bitnami/redis:latest
          ├─💾 /var/lib/monkd/volumes/redis/follower-2 -> /bitnami/redis/data
          └─🔌 open 1.1.1.1:6389 (0.0.0.0:6389) -> 6379

💡 You can inspect and manage your above stack with these commands:
        monk logs (-f) local/redis-cluster-haproxy/stack - Inspect logs
        monk shell     local/redis-cluster-haproxy/stack - Connect to the container's shell
        monk do        local/redis-cluster-haproxy/stack/action_name - Run defined action (if exists)
💡 Check monk help for more!
```

## Variables

The variables are stack section in `redis.yml` file. You can quickly setup by editing the values here.

| Variable               | Description            | Default  |
| ---------------------- | ---------------------- | -------- |
| leader_port            | Redis leader           | 6388     |
| follower-1_port        | Redis follower-1       | 6389     |
| follower-2_port        | Redis follower-2       | 6387     |
| empty_password         | Redis empyt password   | no       |
| io_thread              | Redis IO thread count  | 1        |
| io_threads_do_reads    | Enable multi threading | yes      |
| disable_commands       | Redis disable commands | FLUSHALL |
| password               | Redis leader password  | bitnami  |
| rds_haproxy_port       | HAProxy Port           | 6379     |
| rds_haproxy_stats_port | HAProxy Stats Port     | 9000     |
## Stop, remove and clean up workloads and templates

```bash
monk purge -x redis-cluster-haproxy/stack
```
