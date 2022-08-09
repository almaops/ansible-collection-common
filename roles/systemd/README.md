almaops.systemd
=========

[![License](https://img.shields.io/badge/license-MIT%20License-brightgreen.svg)](https://opensource.org/licenses/MIT)

Wraps the systemd ansible module into the role

Description
-----------

This roles just wraps the systemd ansible module into the role.


Example
-------
```
- name: Update nginx upstreams and reload
  hosts: nginx
  roles:
    - role: almaops.common.template
      template_content: |
        upstream grafana {
            {% for host in groups["monitoring"] %}
            server {{ hostvars[host].backnet_ip }}:3000;
            {% endfor %}
        }
      template_dest: "/etc/nginx/conf.d/grafana_upstream.conf"
    - role: almaops.common.systemd
      systemd_service_name: nginx
      systemd_service_state: reloaded
      systemd_daemon_reload: false
      when: template_result is changed
```

Install
-------

This role is part of [Ansible Galaxy collection](https://galaxy.ansible.com/almaops/common):

`ansible-galaxy collection install almaops.common`

License
-------

[MIT](./LICENSE)

Author Information
------------------

Dmitrii Kashin, <freehck@freehck.com>
