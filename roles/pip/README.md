almaops.pip_install
=========

Install pip packages.

Description
-----------

This role simply installs a list of pip packages.  
As it's a pip module wrapped into role, you can use it as a dependency in other role's meta.  

Role Variables
--------------

`pip_install_packages`: list of packages to install.  
`pip_install_state`: state of package, `"present"` (default), `"latest"` or `"absent"`.  

Example
-------
```
- hosts: all
  become: true
  roles:
    - role: almaops.common.pip
      pip_install_packages:
        - docker
      pip_install_state: "present"
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

Dmitrii Kashin, <freehck@freehck.com>, aka [freehck](https://github.com/freehck)
