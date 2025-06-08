[![License](https://img.shields.io/badge/license-MIT%20License-brightgreen.svg)](https://opensource.org/licenses/MIT)
# almaops.common.prefab

Role contains data and mappings for re-use in other roles. Role can be imported/included in play/role/task or specified as dependency to make its variables available further in play.

## Example usage
First [install](https://github.com/almaops/ansible-collection-common?tab=readme-ov-file#installation) collection `almaops.common` to your project or specify it as dependency in your collection's `galaxy.yml`.

### As dependency
in role's `meta/main.yml`:
```
---
dependencies:
  - almaops.common.prefab
galaxy_info:
  author: "..."
  description: "..."
  role_name: ...
  namespace: ...
  license: ...
  min_ansible_version: "2.15"
  galaxy_tags:
    - ...
allow_duplicates: true
...
```

### In play, role, or task
```
---
- name: Example play
  hosts: localhost
  pre_tasks:
    - name: Include
      ansible.builtin.import_role:
        name: almaops.common.prefab
```

## License
MIT

## Author Information
[Valentin Gostev](https://github.com/ussrlongbow), <val@almaops.com>
