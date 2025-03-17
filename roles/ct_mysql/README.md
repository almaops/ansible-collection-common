# almaops.common.ct_mysql
[![License](https://img.shields.io/badge/license-MIT%20License-brightgreen.svg)](https://opensource.org/licenses/MIT)

# Requirements
Docker and docker-py must be installed on target host (f.e. via almaops.common.docker)

# Role variables
Please refer to [defaults/main.yml](./defaults/main.yml) for full list of available variables. 

# Dependencies
None

# Example playbook
```
- hosts:
    - almaops_env_stg01_mysql
  become: true
  roles:
    - role: almaops.ct_mysql
      vars:
        ct_mysql_ct_image_tag: "8.4"
        ct_mysql_bind_addr: "127.0.0.1"
        ct_mysql_bind_port: "3306"
        ct_mysql_env:
          MYSQL_ROOT_PASSWORD: "AlmaOps112"
```

# License
[MIT](./LICENSE)

# Contributors
[Dmitrii Kashin](https://github.com/freehck)
[Valentin Gostev](https://github.com/ussrlongbow)
