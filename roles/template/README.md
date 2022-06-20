almaops.template
=========

[![License](https://img.shields.io/badge/license-MIT%20License-brightgreen.svg)](https://opensource.org/licenses/MIT)

Create file from template

Description
-----------

You can think about this role as about a wrapper for the template ansible module.

Just provide it with a template and get a result written to some file on the target system.

You can provide a template either by putting its content into `template_content` or by specifying the template file in `template_src`. The result will be present in the `template_dest` file on a target server. The result of template job will be present after this role in the variable `template_result`, so you can use it later for ansible conditions.

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
      tags:
        - upstream

    - role: almaops.systemd
      systemd_service_name: nginx
      systemd_service_state: reloaded
      systemd_daemon_reload: false
      when: template_result is changed
      tags:
        - upstream


Install
-------

This role can be installed from [Ansible Galaxy](https://galaxy.ansible.com/almaops/template):

`ansible-galaxy install almaops.template`

License
-------

[MIT](./LICENSE)

Author Information
------------------

Dmitrii Kashin, <freehck@freehck.com>
