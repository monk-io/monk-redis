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
cd monk-redis/redis
monk load MANIFEST
```

## Let's take a look at the themes I have installed

```bash
foo@bar:~$ monk list redis
✔ Got the list
Type      Template          Repository  Version   Tags
runnable  redis/base                       local       1.000000  -
runnable  redis/redis                      local       1.000000  -
```

## Deploy Stack

```bash
✔ Starting the run job: local/redis/redis... DONE
✔ Preparing nodes DONE
✔ Checking/pulling images DONE
✔ Starting containers DONE
✔ New container 00d298a4e10754a827ba85acde7938a2-local-redis-redis-redis created DONE
✔ Started local/redis/redis
🔩 templates/local/redis/redis
 └─🧊 Peer local
    └─🔩 templates/local/redis/redis
       └─📦 00d298a4e10754a827ba85acde7938a2-local-redis-redis-redis running
          ├─🧩 bitnami/redis:latest
          ├─💾 /var/lib/monkd/volumes/redis/master -> /bitnami/redis/data
          └─🔌 open 1.1.1.1:6379 (0.0.0.0:6379) -> 6379

💡 You can inspect and manage your above stack with these commands:
        monk logs (-f) local/redis/redis - Inspect logs
        monk shell     local/redis/redis - Connect to the container's shell
        monk do        local/redis/redis/action_name - Run defined action (if exists)
💡 Check monk help for more!
```

## Variables

The variables are stack section in `redis.yml` file. You can quickly setup by editing the values here.

| Variable                  | Description                                              |
| ------------------------- | -------------------------------------------------------- |
| image_tag                 | Docker image tag                                         |
| redis_port                | Redis expose port, Default: 6389                         |
| redis_password            | Redis password, Default: 123                             |
| redis_empty_password      | Redis empyt password, Default: yes                       |
| redis_io_thread           | Redis IO thread count, Default: 1                        |
| redis_io_threads_do_reads | Default: yes                                             |
| redis_disable_commands    | Redis disable commands, Default: FLUSHDB,FLUSHALL,CONFIG |

## Stop, remove and clean up workloads and templates

```bash
monk purge -x redis/redis
```
