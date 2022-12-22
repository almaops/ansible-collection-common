
# almaops.common.ct_rocketchat
[![License](https://img.shields.io/badge/license-MIT%20License-brightgreen.svg)](https://opensource.org/licenses/MIT)

# Requirements
Docker and docker-py must be installed on the target host.

# Role variables
Please refer to [defaults/main.yml](./defaults/main.yml) for full list of available variables. 

# Dependencies
None

# Example playbook

```
- name: Deployment of the Rocket.Chat
  hosts: almaops.common.ct_rocketchat
  become: yes
  vars_files:
    - "{{ playbook_dir }}/.../ct_bitnami_mongodb.yml"
    - "{{ playbook_dir }}/.../nginx.yml"
    - "{{ playbook_dir }}/.../ct_rocketchat.yml"
  pre_tasks:
    - name: "Creating a network, adding containers to the network"
      docker_network:
        name: "{{ ct_rocketchat_ct_network_mode }}"
  roles:
    - role: "almaops.common.ct_bitnami_mongodb"
    - role: "nginx"
    - role: "almaops.common.ct_rocketchat"
```

# License
[MIT](./LICENSE)

# Contributors
[Aleksandr Vzhesnevskiy](https://github.com/hDw1z)
