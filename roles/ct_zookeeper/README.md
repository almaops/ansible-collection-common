# almaops.common.ct_zookeeper
This role deploys Docker containers with [Apache Zookeeper](https://zookeeper.apache.org)

# Requirements
Docker engine and Python's Docker module are required on target server for this role to operate.  

# Role variables
All Ansible variables specific to this role are prefixed with `ct_zookeeper_`  

| Variable | Default value | Description |
|----------|---------------|-------------|
| `ct_zookeeper_task_prefix` | `=== ZOOKEEPER CT ===` | Prefix in task names to ease reasdability of playbook's output |
| `ct_zookeeper_persist_data` | `true` | Set flag to `false` to disable persistent volumes mounted to container |
| `ct_zookeeper_template_env` | `env.j2` | Jinja2 template for env file used by container |
| `ct_zookeeper_file_env` | `{{ ct_zookeeper_ct_name }}.env` | Env file filename on host |
| `ct_zookeeper_path_config_base` | `/etc/ct_zookeeper` | Directory where env files are stored, one per container |
| `ct_zookeeper_path_config_file` | `{{ ct_zookeeper_path_config_base }}/{{ ct_zookeeper_file_env }}` | Full path to container's env file |
| `ct_zookeeper_path_base` | `/srv/ct_zookeeper` | Directory for persistent volumes mounted to container |
| `ct_zookeeper_path_zk_data` | `{{ ct_zookeeper_path_base }}/{{ ct_zookeeper_ct_name }}/data` | Persistent volume for Zookeeper data |
| `ct_zookeeper_path_zk_logs` | `{{ ct_zookeeper_path_base }}/{{ ct_zookeeper_ct_name }}/logs` | Persistent volume for Zookeeper logs |
| `ct_zookeeper_path_ct_zk_data` | `/var/lib/zookeeper/data` | Path to data directory inside container |
| `ct_zookeeper_path_ct_zk_logs` | `/var/lib/zookeeper/log` | Path to transaction logs directory inside container |
| `ct_zookeeper_bind_addr` | `127.0.0.1` | IP address Zookeeper binds to |
| `ct_zookeeper_bind_port_peer` | `2888` | Peer port |
| `ct_zookeeper_bind_port_leader` | `3888` | Leader election port |
| `ct_zookeeper_bind_port_client` | `2181` | Client connections port |
| `ct_zookeeper_ct_name` | `zookeeper-{{ ct_zookeeper_server_id }}` | Container name |
| `ct_zookeeper_ct_image` | `confluentinc/cp-zookeeper` | Docker image |
| `ct_zookeeper_ct_version` | `latest` | Docker image version |
| `ct_zookeeper_ct_restart_policy` | `always` | `restart_policy` property passed to Ansible's `docker_container` module |
| `ct_zookeeper_ct_state` | `started` | `state` property passed to Ansible's `docker_container` module |
| `ct_zookeeper_ct_restart` | `no` | `restart` property passed to Ansible's `docker_container` module |
| `ct_zookeeper_ct_pull` | `no` | `pull` property passed to Ansible's `docker_container` module |
| `ct_zookeeper_ct_recreate` | `no` | `recreate` property passed to Ansible's `docker_container` module |
| `ct_zookeeper_ct_volumes_data` | `- "{{ ct_zookeeper_path_zk_data }}:{{ ct_zookeeper_path_ct_zk_data }}"`<br/>`- "{{ ct_zookeeper_path_zk_logs }}:{{ ct_zookeeper_path_ct_zk_logs }}"` | Persistent volumes mounted by default when `ct_zookeeper_persist_data` is true |
| `ct_zookeeper_ct_volumes_extra` | `[]` | Variable allows to mount arbitrary volumes to container |
| `ct_zookeeper_ct_volumes` | `[]` | List of volumes mounted to container, if `ct_zookeeper_persist_data` is true equals to sum of `ct_zookeeper_ct_volumes_data` and `ct_zookeeper_ct_volumes_extra` |
| `ct_zookeeper_env_zookeeper_tick_time` | `2000` | Value of environment variable `ZOOKEEPER_TICK_TIME` passed to container in env file |
| `ct_zookeeper_env_zookeeper_init_limit` | `5` | Value of environment variable `ZOOKEEPER_INIT_LIMIT` passed to container in env file |
| `ct_zookeeper_env_zookeeper_sync_limit` | `2` | Value of environment variable `ZOOKEEPER_SYNC_LIMIT` passed to container in env file |
| `ct_zookeeper_env_zookeeper_admin_enable_server` | `false` | Value of environment variable `ZOOKEEPER_ADMIN_ENABLE_SERVER` passed to container in env file |
| `ct_zookeeper_zookeeper_nodes` |  | List of zookeeper nodes to form a cluster, see example section |
| `ct_zookeeper_server_id` | Host specific value inherited from `{{ ct_zookeeper_zookeeper_nodes }}` | Zookeeper server ID |

# Example
Consider you want to deploy a 3-node Zookeeper cluster, you have these hosts in your inventory:  
```
zk1 backnet_ip=10.0.0.1
zk2 backnet_ip=10.0.0.2
zk3 backnet_ip=10.0.0.3
```
Playbook to deploy Zookeeper cluster on these host will be:
```
- hosts: [zk1, zk2, zk3]
  become: yes
  roles:
    - role: almaops.common.ct_zookeeper
      ct_zookeeper_zookeeper_nodes:
        - host: zk1
          bind_ip: "{{ hostvars['zk1'].backnet_ip }}"
          id: 1
          peerport: "2888" # if not set defaults to {{ ct_zookeeper_bind_port_peer }}
          leaderport: "3888" # if not set defaults to {{ ct_zookeeper_bind_port_leader }}
        - host: zk2
          bind_ip: "{{ hostvars['zk2'].backnet_ip }}"
          id: 2
          peerport: "2888" # if not set defaults to {{ ct_zookeeper_bind_port_peer }}
          leaderport: "3888" # if not set defaults to {{ ct_zookeeper_bind_port_leader }}
        - host: zk3
          bind_ip: "{{ hostvars['zk13'].backnet_ip }}"
          id: 3
          peerport: "2888" # if not set defaults to {{ ct_zookeeper_bind_port_peer }}
          leaderport: "3888" # if not set defaults to {{ ct_zookeeper_bind_port_leader }}
```