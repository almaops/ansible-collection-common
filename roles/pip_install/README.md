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
    - role: almaops.pip_install
      pip_install_packages:
        - docker
      pip_install_state: "present"
```

Install
-------

This role can be installed from [Ansible Galaxy](https://galaxy.ansible.com/almaops/pip_install):

`ansible-galaxy install almaops.pip_install`

License
-------

[MIT](./LICENSE)

Author Information
------------------

Dmitrii Kashin, <freehck@freehck.com>, aka [freehck](https://github.com/freehck)
