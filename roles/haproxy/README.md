# almaops.common.ct_mysql
[![License](https://img.shields.io/badge/license-MIT%20License-brightgreen.svg)](https://opensource.org/licenses/MIT)

# Requirements
None

# Role variables
Please refer to [defaults/main.yml](./defaults/main.yml) for full list of available variables. 

# Example playbook
```
- hosts:
    - almaops_env_stg01_haproxy
  become: true
  roles:
    - role: almaops.common.haproxy
```

# License
[MIT](./LICENSE)

# Contributors
[Dmitrii Kashin](https://github.com/freehck)
