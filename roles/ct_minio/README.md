# almaops.ct_minio
[![License](https://img.shields.io/badge/license-MIT%20License-brightgreen.svg)](https://opensource.org/licenses/MIT)


# Requirements
Docker and docker-py must be installed on target host

# Role variables
Please refer to [defaults/main.yml](./defaults/main.yml) for full list of available variables. 

# Dependencies
None

# Example playbook
```
- hosts:
    - minio-server
  become: true
  roles:
    - role: almaops.ct_minio
      ct_minio_bind_addr: "127.0.0.1"
      ct_minio_env_minio_root_user: root
      ct_minio_env_minio_root_password: "123qwe123qwe"
```

# License
[MIT](./LICENSE)

# Contributors
[Valentin Gostev](https://github.com/ussrlongbow)
