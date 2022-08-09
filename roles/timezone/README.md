almaops.timezone
==========

[![License](https://img.shields.io/badge/license-MIT%20License-brightgreen.svg)](https://opensource.org/licenses/MIT)

Configure host's timezone

Role variables
--------------

Please refer to [defaults/main.yml](./defaults/main.yml) for full list of available variables. 


Example playbook
----------------

```
- hosts:
    - servers
  become: true
  roles:
    - role: almaops.common.timezone
      vars:
        timezone_tz: "Etc/UTC"
```

Install
-------

This role is part of [Ansible Galaxy collection](https://galaxy.ansible.com/almaops/common):

`ansible-galaxy collection install almaops.common`

License
-------

[MIT](./LICENSE)

Contributors
------------

Dmitrii Kashin, <freehck@freehck.com>
