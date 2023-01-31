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
runnable  redis/rds    local       1.000000  database
group     redis/stack  local       -         -
```

## Deploy Stack

```bash
foo@bar:~$ monk run redis/stack
✔ Starting the job: local/redis/stack... DONE
✔ Preparing nodes DONE
✔ Checking/pulling images...
✔ [================================================] 100% docker.io/bitnami/redis:latest QmVNxpEZyJ2eStXEQunPqMpr9AsRVnvQxRejyRiHQhNqdC
✔ Checking/pulling images DONE
✔ Starting containers DONE
✔ Started local/redis/stack

🔩 templates/local/redis/stack
 └─🧊 Peer QmVNxpEZyJ2eStXEQunPqMpr9AsRVnvQxRejyRiHQhNqdC
    └─🔩 templates/local/redis/rds
       └─📦 local-bb68707c72eace348495b2787c-local-redis-rds-rds
          ├─🧩 docker.io/bitnami/redis:latest
          ├─💾 /Users/burakhan/.monk/volumes/redis/mysql -> /bitnami/redis/data
          └─🔌 open 31.206.6.31:6389 (0.0.0.0:6389) -> 6379

💡 You can inspect and manage your above stack with these commands:
 monk logs (-f) local/redis/stack - Inspect logs
 monk shell     local/redis/stack - Connect to the container's shell
 monk do        local/redis/stack/action_name - Run defined action (if exists)
💡 Check monk help for more!
```

## Variables

The variables are stack section in `redis.yml` file. You can quickly setup by editing the values here.

| Variable                  | Description                                              |
|---------------------------|----------------------------------------------------------|
| image_tag                 | Docker image tag                                         |
| redis_port                | Redis expose port, Default: 6389                         |
| redis_password            | Redis password, Default: 123                             |
| redis_empty_password      | Redis empyt password, Default: yes                       |
| redis_io_thread           | Redis IO thread count, Default: 1                        |
| redis_io_threads_do_reads | Default: yes                                             |
| redis_disable_commands    | Redis disable commands, Default: FLUSHDB,FLUSHALL,CONFIG |

## Stop, remove and clean up workloads and templates

```bash
monk purge -x redis/stack
```
