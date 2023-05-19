# almaops.common.virtualbox

[![License](https://img.shields.io/badge/license-MIT%20License-brightgreen.svg)](https://opensource.org/licenses/MIT)

Install VirtualBox

# Role variables

Please refer to [defaults/main.yml](./defaults/main.yml) for full list of available variables. 


# Example playbook

```
- hosts:
    - servers
  become: true
  roles:
    - role: almaops.common.virtualbox
```

# Install

This role is part of [Ansible Galaxy collection](https://galaxy.ansible.com/almaops/common):

`ansible-galaxy collection install almaops.common`

# License

[MIT](./LICENSE)

# Contributors

[Valentin Gostev](https://github.com/ussrlongbow)
