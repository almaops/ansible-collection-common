almaops.timezone
==========

[![License](https://img.shields.io/badge/license-MIT%20License-brightgreen.svg)](https://opensource.org/licenses/MIT)

Configure host's timezone

Role variables
--------------

Please refer to [defaults/main.yml](./defaults/main.yml) for full list of available variables. 

Dependencies
------------

 - [almaops.pkg_install](https://galaxy.ansible.com/almaops/pkg_install)

Example playbook
----------------

```
- hosts:
    - servers
  become: true
  roles:
    - role: almaops.timezone
      vars:
        timezone_tz: "Etc/UTC"
```

Install
-------

This role can be installed from [Ansible Galaxy](https://galaxy.ansible.com/almaops/timezone):

```
ansible-galaxy install almaops.timezone
```

License
-------

[MIT](./LICENSE)

Contributors
------------

Dmitrii Kashin, <freehck@freehck.com>
