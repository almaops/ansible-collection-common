almaops.systemd
=========

[![License](https://img.shields.io/badge/license-MIT%20License-brightgreen.svg)](https://opensource.org/licenses/MIT)

Wraps the systemd ansible module into the role

Description
-----------

This roles just wraps the systemd ansible module into the role.


Example
-------

    - role: almaops.template
      template_content: |
        upstream grafana {
            {% for host in groups["monitoring"] %}
            server {{ hostvars[host].backnet_ip }}:3000;
            {% endfor %}
        }
      template_dest: "/etc/nginx/conf.d/grafana_upstream.conf"
    
    - role: almaops.systemd
      systemd_service_name: nginx
      systemd_service_state: reloaded
      systemd_daemon_reload: false
      when: template_result is changed

Install
-------

This role can be installed from [Ansible Galaxy](https://galaxy.ansible.com/almaops/systemd):

`ansible-galaxy install almaops.systemd`

License
-------

[MIT](./LICENSE)

Author Information
------------------

Dmitrii Kashin, <freehck@freehck.com>
