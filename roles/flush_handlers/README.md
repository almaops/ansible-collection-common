almaops.common.flush_handlers
=========

[![License](https://img.shields.io/badge/license-MIT%20License-brightgreen.svg)](https://opensource.org/licenses/MIT)

Flush ansible handlers

Description
-----------

This role flushes ansible handlers. You can just insert it between other roles, no need to create separate ansible plays when you need it.


Example
-------
```
- name: Flush handlers
  hosts: all
  roles:
    - role: almaops.common.flush_handlers
```

Install
-------

This role is part of [Ansible Galaxy collection](https://galaxy.ansible.com/almaops/common):

`ansible-galaxy collection install almaops.common`

License
-------
[MIT](./LICENSE)

Author Information
------------------

Dmitrii Kashin, <freehck@freehck.com>
