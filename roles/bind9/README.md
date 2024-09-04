# almaops.common.bind9

[![License](https://img.shields.io/badge/license-MIT%20License-brightgreen.svg)](https://opensource.org/licenses/MIT)

Role installs and configures Bind9 DNS server.  
Reference documentation - https://bind9.readthedocs.io/en/latest/reference.html

## Role Variables

Please refer to [defaults/main.yml](./defaults/main.yml) for full list of available variables.

### `bind9_systemd_unit`
- type: string
- default value: `named`

Used by handler to reload/restart bind9.  

### `bind9_enable_root_hint`
- type: boolean
- default value: `true`

Flag controls enabling root hint zone in configuration. When set to `true` downloads root hint from `{{ bind9_url_root_hint }}` and saves to `{{ bind9_path_root_hint }}`.  

### `bind9_url_root_hint`
- type: string
- default value: `https://www.internic.net/domain/named.root`

Link to root hint zone file.  

### `bind9_template_*`
Variables containing template names of configuration files included in `/etc/bind/named.conf`
|Variable|Type|Default value|Comment|
|--------|----|-------------|-------|
|`bind9_template_config_acl`|string|`named.conf.acl.j2`|Template for acl configs|
|`bind9_template_config_options`|string|`named.conf.options.j2`|Template for options block|
|`bind9_template_config_local`|string|`named.conf.local.j2`|Template for local config (zones, custom options)|
|`bind9_template_config_default_zones`|string|`named.conf.default-zones.j2`|Template for default zones definition|

### `bind9_path_*`
Variables containing path to configuration files
|Variable|Type|Default value|Comment|
|--------|----|-------------|-------|
|`bind9_path_root_hint`|string|`/etc/bind/named.root`|Root hint zone, if `{{ bind9_enable_root_hint }}` enabled, included from `{{ bind9_path_config_default_zones }}`|
|`bind9_path_config_main`|string|`/etc/bind/named.conf`|Main bind9 config|
|`bind9_path_config_acl`|string|`/etc/bind/named.conf.acl`|Included from main config|
|`bind9_path_config_options`|string|`/etc/bind/named.conf.options`|Included from main config|
|`bind9_path_config_local`|string|`/etc/bind/named.conf.local`|Included from main config|
|`bind9_path_config_default_zones`|string|`/etc/bind/named.conf.default-zones`|Included from main config|

### `bind9_packages`
- type: list
- default value: `[bind9, bind9-dnsutils, bind9-host, bind9-utils]`
List if bind9 packages to install.  

### `bind9_config_acl`
- type: list
- default value: `[]`

List of ACL definitions for `{{ bind9_path_config_acl }}`.  
Example:  
```
bind9_config_acl:
  - name: acl_one
    cidr:
      - "10.10.10.0/24"
      - "10.20.0.0/16"
  - name: acl_two
    cidr:
      - "192.168.1.0/24"
      - "192.168.2.0/24"
```
See https://bind9.readthedocs.io/en/latest/reference.html#acl-block-grammar  

### `bind9_config_options_listen_on`
- type: list
- default value: `[]`

List of interface bindings, configures IPv4 addresses and ports bind9 will listen to.  
Example:
```
bind9_config_options_listen_on:
  - addresses:
      - "10.10.10.10"
  - options: "port 1234 proxy plain tls ephemeral http myserver"
    addresses:
      - "10.20.20.20"
```
See https://bind9.readthedocs.io/en/latest/reference.html#namedconf-statement-listen-on  

### `bind9_config_options_listen_on_v6`
- type: list
- default value: `[addresses=[none]]`

List of interface bindings, configures IPv4 addresses and ports bind9 will listen to. 
Same syntax as for `{{ bind9_config_options_listen_on }}`.  
See https://bind9.readthedocs.io/en/latest/reference.html#namedconf-statement-listen-on-v6  

### `bind9_config_options_directory`
- type: string
- default value: `/var/cache/bind`
Server's working directory
See https://bind9.readthedocs.io/en/latest/reference.html#namedconf-statement-directory  

### `bind9_config_options_recursion`
- type: enum
- default value: `yes`
- allowed values: `yes`, `no`

Defines whether recursion and caching are allowed.
See https://bind9.readthedocs.io/en/latest/reference.html#namedconf-statement-recursion  

### `bind9_config_options_allow_query`
- type: list
- default value: `[]`

Specifies which hosts (an IP address list) are allowed to send queries to this resolver.


### `bind9_config_options_allow_recursion`
- type: list
- default value: `[]`


### `bind9_config_options_forwarders`
- type: list
- default value: `[]`

### `bind9_config_options_response_policy`
- type: list
- default value: `[]`

List of reponse policy zones. Zones must be defined in `{{ bind9_zones }}`.  
Example:  
```
bind9_config_options_response_policy:
  - zone: "rpz.adblock"
    options: "log false"
  - zone: "rpz.override"
```
See https://bind9.readthedocs.io/en/latest/reference.html#namedconf-statement-response-policy  

### `bind9_config_options_raw`
- type: string
- default value: `""`

Raw text block included into options section

### `bind9_config_local_raw`
- type: string
- default value: `""`

Raw text block included into main config

### `bind9_zones`
- type: list
- default value: `[]`

List of zones definitions.  
Example:
```
bind9_zones:
  - zone: "test.example.com"
    type: master
    file: "/etc/bind/zone.myzone"
    source: "zone-template.j2" # source file
    source_type: "template" # if "template" - zone file will be created as jinja2 template, if "copy" will be created as is
    create: true # if false, zone file will not be created on target
```
- `zone` - zone name as defined in configuration file.
- `type` - zone type. See https://bind9.readthedocs.io/en/latest/reference.html#namedconf-statement-type
- `file` - zone file name.
- `source` - source file for zone, either jinja2 template or raw text file.
- `source_type` - `template` or `copy`, if value is `template`, `source` will be processed as jinja2 template, if `copy` - will be copied to target as is.
- `create` - `true/false` - if `false`, zone file will not be created on target.

## Example Playbook

```
- name: Install and configure Bind9
  hosts: my_dns_servers
  become: true
  roles:
    - role: almaops.common.bind9
      vars:
        bind9_config_options_listen_on_v6:
          - addresses:
            - none
        bind9_config_acl:
          - name: acl_one
            cidr:
              - "10.10.10.0/24"
              - "10.20.0.0/16"
          - name: acl_two
            cidr:
              - "192.168.1.0/24"
              - "192.168.2.0/24"
```

## License
[MIT](./LICENSE)

## Author Information
[Valentin Gostev](https://bind9.readthedocs.io/en/latest/reference.html), <val@le.lc>
