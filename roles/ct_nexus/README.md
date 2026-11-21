# almaops.common.ct_nexus

This ansible role installs a docker container with Nexus OSS

# Requirements

Docker engine running on target host with python3-docker

# Role Variables

Look into [./defaults/main.yml](./defaults/main.yml)

# Example Playbook

```
- name: Install Nexus container
  hosts: server_for_nexus
  roles:
    - role: almaops.common.docker
    - role: almaops.common.ct_nexus
```

# License
[MIT License](./LICENSE)


# Author Information
Valentin Gostev, <val@almaops.com>
