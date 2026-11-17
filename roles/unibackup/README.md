# almaops.common.unibackup

[![License](https://img.shields.io/badge/license-MIT%20License-brightgreen.svg)](https://opensource.org/licenses/MIT)

Role installs pre-configured backup script with pre/post hooks (like docker stop/start).
Script follows common scenario:
1. Optionally stop a running service (like `docker stop`)
2. Optionally invoke any commands (for example `pg_dump`)
3. Archive specified set of directories
4. Optionally encrypt backup
5. Start services back (like `docker compose up -d`)

## Important notes
- Simple rentention is handled during script run
- Logs go to stdout/stderr
- Script stores archive on mounted volumes only
- Script does not handle syncing/uploading backup archives to external storage
- Role does not handle any cron/timerd jobs
- One role run - one script - one app to backup - one resulting archive
- Optional encryption with `age`
- Created/tested/validated with AI/LLM asistance
- Backup script takes no params

## Role Variables

Please refer to [defaults/main.yml](./defaults/main.yml) for full list of available variables.

## Example Playbook
```
- name: Deploy backup script
  hosts:
    - mydockerhost
  become: true
  roles:
    - role: almaops.common.unibackup
      vars:
        unibackup_name: "myapp"
        unibackup_manage_stop: |
          docker compose -f /opt/myapp/docker-compose.yml stop
        unibackup_manage_start: |
          docker compose -f /opt/myapp/docker-compose.yml up -d
        unibackup_data_source_dirs:
          - /opt/myapp/data
        unibackup_age_encrypt: true
```
## Script usage
```bash
/opt/unibackup/myapp.sh >> /path/to/log/file
```
Backup stored in `{{ unibackup_backup_dir }}` (defaults to `"/srv/backups/{{ unibackup_name }}"`)


## License
[MIT](./LICENSE)
