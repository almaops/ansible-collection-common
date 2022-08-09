almaops.common.pkg
=========

This role simply installs a list of packages.

Description
-----------

It was created because of 2 reasons. First, although there's [package module](https://docs.ansible.com/ansible/latest/modules/package_module.html) in Ansible distribution, it does not provide you an ability to specify cache validity time, because some package managers (f.e. yum and dnf) perform repository metadata update automatically, even when it's not necessary. Second, when it's wrapped into role, you can use it as a dependency in other roles.

Role Variables
--------------

`pkg_install_packages`: list of packages to install

`pkg_install_state`: state of package

It can be `present` (default), `latest` or `absent`

`pkg_install_update_cache`: by default set to `true`, enabling repository metadata update

`pkg_install_cache_valid_time`: how often you want to update repo (default: `600` seconds)

Example
-------
```
- hosts: all
  become: true
  roles:
    - role: almaops.common.pkg
      pkg_install_packages:
        - tmux
        - screen
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

This role was written by Dmitrii Kashin aka [freehck](https://github.com/freehck)
