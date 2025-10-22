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
cd monk-redis/redis-cluster-sentinel
monk load MANIFEST
```

## Let's take a look at the themes I have installed

```bash
$ monk list redis-cluster
✔ Got the list
Type      Template                       Repository  Version  Tags
runnable  redis-cluster/base             local       -        -
runnable  redis-cluster/redis-leader     local       -        -
runnable  redis-cluster/redis-follower-1 local       -        -
runnable  redis-cluster/redis-follower-2 local       -        -
runnable  redis-cluster/sentinel         local       -        -
group     redis-cluster/stack            local       -        -
```

## Deploy Stack

```bash
foo@bar:~$  monk run redis-cluster/stack
? Select which 'redis-cluster/stack' to run local/redis-cluster/stack
✔ Starting the run job: local/redis-cluster/stack... DONE
✔ Preparing nodes DONE
✔ Checking/pulling images...
✔ [================================================] 100% bitnami/redis:latest local
✔ [================================================] 100% bitnami/redis-sentinel:latest local
✔ Checking/pulling images DONE
✔ Starting containers DONE
✔ New container 7ab9415e3ca4288468a2ce2d0b6f9882-dis-cluster-redis-leader-redis created DONE
✔ New container 524c95d78a90e4a4cd2656efa22d6297-edis-cluster-sentinel-sentinel created DONE
✔ New container 0a597b55e626e104a9be54e12c946dd8-is-cluster-redis-follower-1-redis created DONE
✔ New container 2b2759878f277f0afdabea4bc6386132-is-cluster-redis-follower-2-redis created DONE
✔ Started local/redis-cluster/stack
🔩 templates/local/redis-cluster/stack
 └─🧊 Peer local
    ├─🔩 templates/local/redis-cluster/redis-leader
    │  └─📦 7ab9415e3ca4288468a2ce2d0b6f9882-dis-cluster-redis-leader-redis running
    │     ├─🧩 bitnami/redis:latest
    │     ├─💾 /var/lib/monkd/volumes/redis/leader -> /bitnami/redis/data
    │     └─🔌 open 1.1.1.1:6387 (0.0.0.0:6387) -> 6379
    ├─🔩 templates/local/redis-cluster/redis-follower-1
    │  └─📦 0a597b55e626e104a9be54e12c946dd8-is-cluster-redis-follower-1-redis running
    │     ├─🧩 bitnami/redis:latest
    │     ├─💾 /var/lib/monkd/volumes/redis/follower-1 -> /bitnami/redis/data
    │     └─🔌 open 1.1.1.1:6388 (0.0.0.0:6388) -> 6379
    ├─🔩 templates/local/redis-cluster/redis-follower-2
    │  └─📦 2b2759878f277f0afdabea4bc6386132-is-cluster-redis-follower-2-redis running
    │     ├─🧩 bitnami/redis:latest
    │     ├─💾 /var/lib/monkd/volumes/redis/follower-2 -> /bitnami/redis/data
    │     └─🔌 open 1.1.1.1:6389 (0.0.0.0:6389) -> 6379
    └─🔩 templates/local/redis-cluster/sentinel
       └─📦 524c95d78a90e4a4cd2656efa22d6297-edis-cluster-sentinel-sentinel running
          └─🧩 bitnami/redis-sentinel:latest

💡 You can inspect and manage your above stack with these commands:
        monk logs (-f) local/redis-cluster/stack - Inspect logs
        monk shell     local/redis-cluster/stack - Connect to the container's shell
        monk do        local/redis-cluster/stack/action_name - Run defined action (if exists)
💡 Check monk help for more!
```

## Variables

The variables are stack section in `redis.yml` file. You can quickly setup by editing the values here.

| Variable            | Description            | Default  |
| ------------------- | ---------------------- | -------- |
| leader_port         | Redis leader           | 6388     |
| follower-1_port     | Redis follower-1       | 6389     |
| follower-2_port     | Redis follower-2       | 6387     |
| empty_password      | Redis empyt password   | no       |
| io_thread           | Redis IO thread count  | 1        |
| io_threads_do_reads | Enable multi threading | yes      |
| disable_commands    | Redis disable commands | FLUSHALL |
| password            | Redis leader password  | bitnami  |

## Stop, remove and clean up workloads and templates

```bash
monk purge -x redis-cluster/stack
```
