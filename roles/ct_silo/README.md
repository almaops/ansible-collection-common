# almaops.ct_silo
[![License](https://img.shields.io/badge/license-MIT%20License-brightgreen.svg)](https://opensource.org/licenses/MIT)


# Requirements
Docker and python3-docker must be installed on target host

# Role variables
Please refer to [defaults/main.yml](./defaults/main.yml) for full list of available variables. 

# Dependencies
None

# Example playbook
```
- hosts:
    - server
  become: true
  roles:
    - role: almaops.common.ct_silo
      ct_silo_env_minio_root_user: root
      ct_silo_env_minio_root_password: "123qwe123qwe"
```

# License
[MIT](./LICENSE)

# Contributors
[Valentin Gostev](https://github.com/ussrlongbow)
