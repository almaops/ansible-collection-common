# Ansible Role: Redis Container
This role creates Redis container on target host

# Requirements
Target host must be running Docker enginer and have Docker PIP module installed.

# Role variables
Please refer to [defaults/main.yml](./defaults/main.yml) for full list of available variables. 

## Network settings

| Variable | Default | Description |
|----------|---------|-------------|
|`ct_redis_enable_ports`|`true`|Flag that controls if default Redis port is proxied|
|`ct_redis_bind_addr`|`"127.0.0.1"`|IP address Redis container binds to|
|`ct_redis_bind_port`|`"6379"`|Port Redis container binds to|
|`ct_redis_bind_ct_port`|`"6379"`|Port Redis server binds to|
|`ct_redis_ct_ports_default`|`[]`|Default Redis port proxied to container<br />If `{{ ct_redis_enable_ports }}` is `false`, this list is empty<br />if `true` list is set to `["{{ ct_redis_bind_addr }}:{{ ct_redis_bind_port }}:{{ ct_redis_bind_ct_port }}"]` |
|`ct_redis_ct_ports_extra`|`[]`|User defined arbitrary ports proxied to container|
|`ct_redis_ct_ports`|`{{ ct_redis_ct_ports_default + ct_redis_ct_ports_extra }}`|Resulting set of ports proxied to container<br />passed to `ports` parameter of `docker_container` module|

## Volume settings
| Variable | Default | Description |
|----------|---------|-------------|
|`ct_redis_path_config_base`|`/etc/ct_redis`|Directory where custom configuration files are placed|
|`ct_redis_path_config_file`|`{{ ct_redis_path_config_base }}/{{ ct_redis_ct_name }}.conf`|Configuration file name, defaults to container's name|
|`ct_redis_path_ct_config_file`|`/usr/local/etc/redis/redis.conf`|Path to custom config inside container|
|`ct_redis_path_data_base`|`/srv/ct_redis`|Base directory for data volumes|
|`ct_redis_path_data_volume`|`{{ ct_redis_path_data_base }}/{{ ct_redis_ct_name }}`|Data directory mounted to container|
|`ct_redis_path_ct_data`|`/data`|Data directory inside container|
|`ct_redis_ct_volumes_data`|`["{{ ct_redis_path_data_volume }}:{{ ct_redis_path_ct_data }}"]`|Data directory mount|
|`ct_redis_ct_volumes_config`|`[]`|Custom config mount, <br />if `{{ ct_redis_enable_custom_config }}` is `true`,<br /> value is set to `["{{ ct_redis_path_config_file }}:{{ ct_redis_path_ct_config_file }}:ro"]`|
|`ct_redis_ct_volumes_extra`|`[]`|User defined arbitrary mounts to container,<br /> in format `["/source:/dest"]`|
|`ct_redis_ct_volumes`|`{{ ct_redis_ct_volumes_data + ct_redis_ct_volumes_config + ct_redis_ct_volumes_extra }}`|Resulting set of mounts,<br />passed to `volumes` parameter of `docker_container` module|

## Custom config
The role provides a simple flag to enable custom configuration file for Redis container.

| Variable | Default | Description |
|----------|---------|-------------|
|`ct_redis_enable_custom_config`|`false`|Flag controls mounting custom config file to container<br />If value set to `true`, variables `{{ ct_redis_ct_volumes_config }}` and `{{ ct_redis_ct_command_custom_config }}` are overrriden from [vars](./vars/enable_custom_config.yml), once flag enabled redis configuration file is created from template on target host and mounted to container, path to config file is appended to container's `CMD`|
|`ct_redis_cfg_extra`|`{}`|Key-value pairs of Redis configuration parameters for default custom config template|

## Command
| Variable | Default | Description |
|----------|---------|-------------|
|`ct_redis_ct_command_custom_config`|`[]`|Path to custom config file inside container that is appended to container's `CMD`,<br /> if `{{ ct_redis_enable_custom_config }}` is true, value set to `["{{ ct_redis_path_ct_config_file }}"]`|
|`ct_redis_ct_command_options`|`[]`|User defined arbitrary options passed to `redis-server` executable|
|`ct_redis_ct_command`|`{{ [\"redis-server\"] + ct_redis_ct_command_custom_config + ct_redis_ct_command_options }}`|Resulting command passed to container as `command` parameter of `docker_container` volume|

# Dependencies
None. 

# Example playbook
```
# Creates Redis container binded to custom IP address and port
- hosts:
    - redis_server
  become: true
  roles:
    - role: almaops.common.ct_redis
      ct_redis_bind_addr: "192.168.0.10"
      ct_redis_bind_port: "26379"

# Creates Redis container with custom config from your own redis.conf template
- hosts:
    - redis_server
  become: true
  roles:
    - role: almaops.common.ct_redis
      ct_redis_enable_custom_config: true
      ct_redis_template_config: "/path/to/custom/redis.conf.j2"
```      

# License
[MIT](./LICENSE)

# Contributors
[Valentin Gostev](https://github.com/ussrlongbow). 
