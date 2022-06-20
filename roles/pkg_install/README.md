almaops.pkg_install
=========

This role simply installs a list of packages.

Description
-----------

It was created because of 2 reasons. First, although there's [package module](https://docs.ansible.com/ansible/latest/modules/package_module.html) in Ansible distribution, it does not provide you an ability to specify cache validity time, because some package managers (f.e. yum and dnf) perform repository metadata update automatically, even when it's not necessary. Second, when it's wrapped into role, you can use it as a dependency in other role's meta file.

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
    - role: almaops.pkg_install
      pkg_install_packages:
        - tmux
        - screen
```

Install
-------

This role can be installed from [Ansible Galaxy](https://galaxy.ansible.com/almaops/pkg_install):

`ansible-galaxy install almaops.pkg_install`

License
-------

[MIT](./LICENSE)

Author Information
------------------

This role was written by Dmitrii Kashin aka [freehck](https://github.com/freehck)
