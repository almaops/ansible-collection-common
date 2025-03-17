almaops.common.ct_keycloak
==========

Install keycloak in docker container. HA cluster using mysql JDBC PING is also possible.

Requirements
------------

Docker daemon installed (f.e. via almaops.common.docker)

Role Variables
--------------

Look into [./defaults/main.yml](./defaults/main.yml)

Example Playbook
----------------

```
- hosts: servers
  roles:
    - role: almaops.common.ct_keycloak
```

License
-------

[MIT License](./LICENSE)


Author Information
------------------
[Dmitrii Kashin](https://github.com/freehck)
