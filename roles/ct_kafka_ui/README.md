# almaops.common.ct_kafka_ui
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
    - kafka-ui-server
  become: true
  roles:
    - role: almaops.common.ct_kafka_ui
      ct_kafka_ui_bind_addr: "10.1.2.3"
      ct_kafka_ui_ct_env:
        KAFKA_CLUSTERS_0_NAME: local
        KAFKA_CLUSTERS_0_BOOTSTRAPSERVERS: "kafka:9092"
```

# License
[MIT](./LICENSE)

# Contributors
[Valentin Gostev](https://github.com/ussrlongbow)
