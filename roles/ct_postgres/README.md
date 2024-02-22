# Ansible Role: PostgreSQL Container
This role creates PostgreSQL container on target host

# Requirements
Target host must be running Docker enginer and have Docker PIP module installed.

# Role variables
Please refer to [defaults/main.yml](./defaults/main.yml) for full list of available variables. 

## Network settings

| Variable | Default | Description |
|----------|---------|-------------|
|`ct_postgres_enable_ports`|`true`|Flag that controls if PostgreSQL ports are proxied by container engine|
|`ct_postgres_bind_addr`|`"127.0.0.1"`|IP address binding for proxying by container engine|
|`ct_postgres_bind_port`|`"5432"`|PostgreSQL container port exposed by container engine|
|`ct_postgres_bind_ct_port`|`"5432"`|Port PostgreSQL server binds to inside container|
|`ct_postgres_ct_ports_extra`|`[]`|User defined arbitrary ports proxied to container|
|`ct_postgres_ct_networks`|`[]`|List of networks container will be connected to|

## Volume settings
| Variable | Default | Description |
|----------|---------|-------------|
|`ct_postgres_path_base`|`/srv/ct_postgres`|Base directory for files used by PostgreSQL container|
|`ct_postgres_path_data`|`"{{ ct_postgres_path_base }}/{{ ct_postgres_ct_name }}/data`|PostgreSQL data directory|
|`ct_postgres_path_env`|`{{ ct_postgres_path_base }}/{{ ct_postgres_ct_name }}/.env`|ENV file for container|
|`ct_postgres_path_ct_data`|`/var/lib/postgresql/data`|Data directory inside container|
|`ct_postgres_template_env`|`ct_postgres.env.j2`|ENV file template|
|`ct_postgres_ct_mount_extra`|`[]`|User defined arbitrary mounts to container,<br /> in format `["/source:/dest"]`|

## Command
| Variable | Default | Description |
|----------|---------|-------------|
|`ct_postgres_ct_command`|`["postgres"]`|Container's command|

# Dependencies
None. 

# Example playbook
```
# Creates PostgreSQL container binded to custom IP address and port
- hosts:
    - postgresql_server
  become: true
  roles:
    - role: almaops.common.ct_postgres
      vars:
        ct_postgres_bind_addr: "192.168.0.10"
        ct_postgres_bind_port: "26379"
        ct_postgres_env_postgres_password: "SuperSecret"
```      

# License
[MIT](./LICENSE)

# Contributors
[Valentin Gostev](https://github.com/ussrlongbow). 
