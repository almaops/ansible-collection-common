
[![License](https://img.shields.io/badge/license-MIT%20License-brightgreen.svg)](./LICENSE)
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

### almaops.common.certbot

### almaops.common.clickhouse_backup

### almaops.common.cron
Configure of cron jobs. [README](./roles/cron/README.md)

### almaops.common.ct_kafka
Deploy Kafka cluster in containers. [README](./roles/ct_kafka/README.md)

### almaops.common.ct_kafka_ui
Deploy Kafka UI container. [README](./roles/ct_kafka_ui/README.md)

### almaops.common.ct_minio
Deploy Minio container. [README](./roles/ct_minio/README.md)

### almaops.common.ct_mongodb
Deploy MongoDB container. [README](./roles/ct_mongodb/README.md)

### almaops.common.ct_redis
Deploy Redis container. [README](./roles/ct_redis/README.md)

### almaops.common.ct_rocketchat
Deploy [Rocket.Chat](https://www.rocket.chat) container. [README](./roles/ct_rocketchat/README.md)

### almaops.common.ct_youtrack
Deploy [YouTrack container](https://hub.docker.com/r/jetbrains/youtrack/), no configuration/customization.

### almaops.common.ct_zookeeper
Deploy Zookeeper cluster in containers. [README](./roles/ct_zookeeper/README.md)

### almaops.common.docker
Installation/configuration for Docker daemon and Docker client userspace configuration. [README](./roles/pip/README.md)

### almaops.common.flush_handlers
Flushing Ansible handlers between other roles. [README](./roles/flush_handlers/README.md)

### almaops.common.gitlab
Deploy Gitlab CI/CD. Forked from [Jeff Geerling](https://github.com/geerlingguy/ansible-role-gitlab)

### almaops.common.htpasswd
Install and configure htpasswd. Forked from [Jeff Geerling](https://github.com/geerlingguy/ansible-role-htpasswd)

### almaops.common.nginx
Install and configure nginx. Forked from [Jeff Geerling](https://github.com/geerlingguy/ansible-role-nginx)

### almaops.common.pip
Install Python PIP packages. [README](./roles/pip/README.md)

### almaops.common.pkg
Install OS packages. [README](./roles/pkg/README.md)

### almaops.common.systemd
Setup and configure systemd units. [README](./roles/systemd/README.md)

### almaops.common.template
Templating from file or in-place. [README](./roles/template/README.md)

### almaops.common.timezone
Setup host's time zone. [README](./roles/timezone/README.md)

### almaops.common.user
Manage OS users. [README](./roles/user/README.md)

### almaops.common.var_loader
Load Ansible variables files from directory. [README](./roles/var_loader/README.md)

### almaops.common.virtualbox
Install and configure VirtualBox. [README](./roles/virtualbox/README.md)

## Playbooks

### almaops.common.pkg_upgrade_all
Upgrade all packages on target host.
```
# Upgrades all packages on all inventory hosts
~$ ansible-playbook almaops.common.pkg_upgrade_all -e almaops_target_pkg_upgrade_all=all
```