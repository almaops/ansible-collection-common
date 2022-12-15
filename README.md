
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

### almaops.common.cron
Configuration of cron jobs. Please consult with role's [README](./roles/cron/README.md)

### almaops.common.ct_bitnami_mongodb
Deployment of the "MongoDB" container based on the "bitnami/mongodb" docker image from VMWare (https://hub.docker.com/r/bitnami/mongodb). Please consult with role's [README](./roles/ct_bitnami_mongodb/README.md)

### almaops.common.docker
Installation/configuration for Docker daemon and Docker client userspace configuration. Please consult with role's [README](./roles/pip/README.md)

### almaops.common.flush_handlers
Flushing Ansible handlers between other roles. Please consult with role's [README](./roles/flush_handlers/README.md)

### almaops.common.pip
Python pip packages installation. Please consult with role's [README](./roles/pip/README.md)

### almaops.common.pkg
Packages installation. Please consult with role's [README](./roles/pkg/README.md)

### almaops.common.systemd
Manipulating systemd services between other roles. Please consult with role's [README](./roles/systemd/README.md)

### almaops.common.template
Templating from file or in-place. Please consult with role's [README](./roles/template/README.md)

### almaops.common.timezone
Setup host's time zone. Please consult with role's [README](./roles/timezone/README.md)
