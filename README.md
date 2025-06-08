[![License](https://img.shields.io/badge/license-MIT%20License-brightgreen.svg)](./LICENSE)
[![ansible-lint](https://github.com/almaops/ansible-collection-common/actions/workflows/ansible-lint.yml/badge.svg)](https://github.com/almaops/ansible-collection-common/actions/workflows/ansible-lint.yml)
[![REUSE status](https://api.reuse.software/badge/github.com/almaops/ansible-collection-common)](https://api.reuse.software/info/github.com/almaops/ansible-collection-common)
# Ansible Collection - almaops.common

## Description
This collection provides base Almaops roles.

## Installation
With command line from Ansible Galaxy:
```
ansible-galaxy collection install almaops.common
```
With requirements file:
```
~# cat /path/to/your/requirements.yml
---
collections:
# from Ansible Galaxy
- name: almaops.common
# from Git repository
- name: https://github.com/almaops/ansible-collection-common.git
  type: git
...
~# ansible-galaxy install -r /path/to/your/requirements.yml
```

## Roles

|Role|Description|Notes|
|---|---|---|
|[almaops.common.age_encrypt](https://github.com/almaops/ansible-collection-common/tree/master/roles/age_encrypt)|Install [Age encryption](https://github.com/FiloSottile/age)||
|[almaops.common.bind9](https://github.com/almaops/ansible-collection-common/tree/master/roles/bind9)|Install and configure [Bind9](https://bind9.readthedocs.io/en/latest/reference.html) DNS server||
|[almaops.common.certbot](https://github.com/almaops/ansible-collection-common/tree/master/roles/certbot)|Install certbot and manage Let's Encrypt certificates|Forked from [geerlingguy.certbot](https://github.com/geerlingguy/ansible-role-certbot)|
|[almaops.common.cron](https://github.com/almaops/ansible-collection-common/tree/master/roles/cron)|Manage cron jobs||
|[almaops.common.ct_clickhouse](https://github.com/almaops/ansible-collection-common/tree/master/roles/ct_clickhouse)|Deploy [Clickhouse](https://clickhouse.com/) container||
|[almaops.common.ct_kafka](https://github.com/almaops/ansible-collection-common/tree/master/roles/ct_kafka)|Deploy Kafka cluster in containers||
|[almaops.common.ct_kafka_ui](https://github.com/almaops/ansible-collection-common/tree/master/roles/ct_kafka_ui)|Deploy Kafka UI container||
|[almaops.common.ct_keycloak](https://github.com/almaops/ansible-collection-common/tree/master/roles/ct_keycloak)|Deploy [Keycloak](https://www.keycloak.org/) container||
|[almaops.common.ct_minio](https://github.com/almaops/ansible-collection-common/tree/master/roles/ct_minio)|Deploy [Minio](https://min.io/) container||
|[almaops.common.ct_mongodb](https://github.com/almaops/ansible-collection-common/tree/master/roles/ct_mongodb)|Deploy [MongoDB](https://github.com/mongodb/mongo) container||
|[almaops.common.ct_mysql](https://github.com/almaops/ansible-collection-common/tree/master/roles/ct_mysql)|Deploy [MySQL](https://www.mysql.com/) container||
|[almaops.common.ct_postgres](https://github.com/almaops/ansible-collection-common/tree/master/roles/ct_postgres)|Deploy [PostgreSQL](https://www.postgresql.org/) container||
|[almaops.common.ct_redis](https://github.com/almaops/ansible-collection-common/tree/master/roles/ct_redis)|Deploy [Redis](https://github.com/redis/redis) container||
|[almaops.common.ct_rocketchat](https://github.com/almaops/ansible-collection-common/tree/master/roles/ct_rocketchat)|Deploy [RocketChat](https://www.rocket.chat/) container||
|[almaops.common.ct_youtrack](https://github.com/almaops/ansible-collection-common/tree/master/roles/ct_youtrack)|Deploy [JetBrains Youtrack](https://www.jetbrains.com/youtrack/) container||
|[almaops.common.ct_zookeeper](https://github.com/almaops/ansible-collection-common/tree/master/roles/ct_zookeeper)|Deploy ZooKeeper cluster in containers||
|[almaops.common.docker](https://github.com/almaops/ansible-collection-common/tree/master/roles/docker)|Install [Docker engine](https://docs.docker.com/engine/)||
|[almaops.common.flush_handlers](https://github.com/almaops/ansible-collection-common/tree/master/roles/flush_handlers)|Alias for [`ansible.builtin.meta`](https://docs.ansible.com/ansible/latest/collections/ansible/builtin/meta_module.html)||
|[almaops.common.gitlab](https://github.com/almaops/ansible-collection-common/tree/master/roles/gitlab)|Install and configure [Gitlab](https://about.gitlab.com/install/)|Forked from [geerlingguy.gitlab](https://github.com/geerlingguy/ansible-role-gitlab)|
|[almaops.common.htpasswd](https://github.com/almaops/ansible-collection-common/tree/master/roles/htpasswd)|Manage htpasswd passwords|Forked from [geerlingguy.htpasswd](https://github.com/geerlingguy/ansible-role-htpasswd)|
|[almaops.common.nginx](https://github.com/almaops/ansible-collection-common/tree/master/roles/nginx)|Install Nginx and manage vhosts|Forked from [geerlingguy.nginx](https://github.com/geerlingguy/ansible-role-nginx)|
|[almaops.common.pip](https://github.com/almaops/ansible-collection-common/tree/master/roles/pip)|Manage Python PIP packages||
|[almaops.common.pkg](https://github.com/almaops/ansible-collection-common/tree/master/roles/pkg)|Manage OS packages||
|[almaops.common.prefab](https://github.com/almaops/ansible-collection-common/tree/master/roles/prefab)|Variables for re-use||
|[almaops.common.systemd](https://github.com/almaops/ansible-collection-common/tree/master/roles/systemd)|Manage systemd units||
|[almaops.common.template](https://github.com/almaops/ansible-collection-common/tree/master/roles/template)|Create files from jinja2 or inline templates||
|[almaops.common.timezone](https://github.com/almaops/ansible-collection-common/tree/master/roles/timezone)|Set timezone||
|[almaops.common.user](https://github.com/almaops/ansible-collection-common/tree/master/roles/user)|Manage OS users||
|[almaops.common.var_loader](https://github.com/almaops/ansible-collection-common/tree/master/roles/var_loader)|Load variables from arbitrary directories||
|[almaops.common.virtualbox](https://github.com/almaops/ansible-collection-common/tree/master/roles/virtualbox)|Install [VirtualBox](https://www.virtualbox.org/)||

## Playbooks

|Playbook|Target variable|
|---|---|
|almaops.common.pkg_upgrade_all|`almaops_target_pkg_upgrade_all`|
