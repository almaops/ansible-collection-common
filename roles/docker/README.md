# almaops.common.docker

This ansible role installs docker engine on target host from your distro's repository, not an omnibus daemon from upstream.  

## Role Variables
Full list of available variables -  [./defaults/main.yml](./defaults/main.yml)

### `docker_enable_pkg_install`
- type: boolean  
- default value: `true`  

If `false` no packages are installed on target host.  

### `docker_enable_pip_install`
- type: boolean  
- default value: `false`  

If `true` installs docker python library via pip.  

### `docker_enable_configure`
- type: boolean  
- default value: `true`  

If `false` daemon.json is untouched by role.  

### `docker_enable_docker_compose`
- type: boolean  
- default value: `false`  

If `true` installs docker-compose v1.  

### `docker_enable_docker_compose_v2`
- type: boolean  
- default value: `false`  

If `true` installs docker-compose v2.  

### `docker_enable_docker_python`
- type: boolean  
- default value: `true`  

If `true` installs docker python library via package manager.  

### `docker_template_daemon_json`
- type: string  
- default value: `daemon.json.j2`  

Template name for daemon.json.  

### `docker_path_daemon_json`
- type: string  
- default value: `/etc/docker/daemon.json`  

Path to daemon.json on target host.  

### `docker_config`
- type: dict

Docker engine configuration, converted to json, see https://docs.docker.com/reference/cli/dockerd/#daemon-configuration-file for available options.  

### `docker_registry_credentials`
- type: list
- default value: `[]`

List of registried the docker daemon should be logged in. See example playbook for details.  

# Example Playbook

```
- name: Install Docker engine
  hosts: servers
  become: true
  roles:
    - role: almaops.common.docker
      vars:
        docker_config:
            log-driver: "json-file"
            log-opts:
              max-size: "150m"
              max-file: "10"
              compress: "true"
            metrics-addr: "0.0.0.0:9323"
        docker_registry_credentials:
          - registry: "registry.example.com"
            username: "registry-user"
            password: "registry-password"
        docker_enable_docker_compose_v2: true
```

## License
[MIT License](./LICENSE)

## Author Information
Dmitrii Kashin, <freehck@freehck.com>
[Valentin Gostev](https://github.com/ussrlongbow), <val@le.lc>
