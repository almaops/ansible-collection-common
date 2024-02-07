# almaops.common.ct_clickhouse

[![License](https://img.shields.io/badge/license-MIT%20License-brightgreen.svg)](https://opensource.org/licenses/MIT)

Pulls and runs [Clickhouse Server](https://hub.docker.com/r/clickhouse/clickhouse-server) container.

# Requirements
Docker and docker python module must be installed on the target host.

# Role variables
Please refer to [defaults/main.yml](./defaults/main.yml) for full list of available variables. 


# Example playbook
```
- hosts:
    - clickhouse_servers
  become: true
  roles:
    - role: almaops.common.docker
    - role: almaops.common.ct_clickhouse
```

# Install
This role is part of [Ansible Galaxy collection](https://galaxy.ansible.com/almaops/common):

`ansible-galaxy collection install almaops.common`

# License
[MIT](./LICENSE)

# Contributors
[Valentin Gostev](https://github.com/ussrlongbow)
