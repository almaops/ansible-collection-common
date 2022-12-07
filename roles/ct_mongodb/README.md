
# almaops.common.ct_mongodb
[![License](https://img.shields.io/badge/license-MIT%20License-brightgreen.svg)](https://opensource.org/licenses/MIT)

# Requirements
Docker and docker-py must be installed on target host.

# Role variables
Please refer to [defaults/main.yml](./defaults/main.yml) for full list of available variables. 

# Dependencies
None

# Example playbook
```
- name: Deployment of the MongoDB
  hosts: almaops.common.ct_mongodb
  become: yes
  vars_files:
    - "{{ playbook_dir }}/.../.yml" 
  roles:
    - role: "almaops.common.ct_mongodb"
```

# License
[MIT](./LICENSE)

# Contributors
[Aleksandr Vzhesnevskiy](https://github.com/hDw1z)
