# almaops.common.ct_youtrack

[![License](https://img.shields.io/badge/license-MIT%20License-brightgreen.svg)](https://opensource.org/licenses/MIT)

Pulls and runs YouTrack official container. This role DOES NOT handle YouTrack configuration and/or customization in any way.

# Requirements
Docker and docker-py must be installed on the target host.

# Role variables
Please refer to [defaults/main.yml](./defaults/main.yml) for full list of available variables. 


# Example playbook
```
- hosts:
    - my-youtrack-server
  become: true
  roles:
    - role: almaops.common.docker
    - role: almaops.common.ct_youtrack
```

# Install
This role is part of [Ansible Galaxy collection](https://galaxy.ansible.com/almaops/common):

`ansible-galaxy collection install almaops.common`

# License
[MIT](./LICENSE)

# Contributors
[Valentin Gostev](https://github.com/ussrlongbow)
