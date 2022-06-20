almaops.cron
=========

[![License](https://img.shields.io/badge/license-MIT%20License-brightgreen.svg)](https://opensource.org/licenses/MIT)

Manage cron jobs

Role Variables
--------------

Please refer to [defaults/main.yml](./defaults/main.yml) for full list of available variables.


Example Playbook
----------------

```
- role: almaops.cron
  cron_default_file: "backups"
  cron_jobs:
    - name: "backup database"
      hour: "12"
      minute: "0"
      command: "/opt/scripts/mysql-backup-all.sh"
      user: "root"
    - name: "backup gitlab"
      schedule: "0 0 * * *"
      user: "root"
      command: "gitlab-backup create CRON=1"
      skip: true
```

License
-------
[MIT](./LICENSE)

Author Information
------------------
Dmitrii Kashin, <freehck@freehck.com>
