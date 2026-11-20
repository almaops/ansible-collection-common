# almaops.common.avars

Loads all variable files from a specified directory into the current host's scope, with built-in auditing for overwritten and newly defined variables.

## Idea

`avars` is the streamlined successor to `almaops.common.var_loader`. Instead of splitting the target path across multiple parameters, it takes a single `avars_dir` path and loads every valid YAML/JSON file inside that directory via `ansible.builtin.include_vars`.

Because variables loaded via `include_vars` have high precedence (just below `--extra-vars`), layering multiple directories sequentially makes it easy to compose host configurations from modular profile folders. To keep layered configurations transparent and safe, `avars` includes toggleable auditing to report overwritten variables, log newly introduced variables, mask values in CI logs, or fail the play if an unexpected override occurs.

## Role Variables

| Variable | Default | Description |
| :--- | :--- | :--- |
| `avars_dir` | `""` | **Required.** Path to the directory containing variable files to include. |
| `avars_debug` | `false` | When `true`, prints a diff whenever an existing host variable is overwritten by files in `avars_dir`. |
| `avars_debug_new` | `false` | When `true`, prints every brand-new variable introduced by `avars_dir`. |
| `avars_debug_show_values` | `false` | Controls whether variable values are printed in debug output. Set to `false` in CI pipelines to print only variable names and source paths without exposing secrets. |
| `avars_fail_on_override` | `false` | When `true`, fails the play immediately if `avars_dir` overwrites any previously defined host variable. |
| `avars_ignore_regex` | `"^(_\|ansible_\|avars_)"` | Regular expression of variable names excluded from snapshot comparisons and diff auditing. |

## Example Playbook

### Load a Single Variable Directory

```yaml
- hosts: all
  tasks:
    - name: Load common environment variables
      ansible.builtin.include_role:
        name: almaops.common.avars
      vars:
        avars_dir: "{{ inventory_dir | dirname }}/vars/common"
```

### Enforce Zero Overrides in Strict Mode

```yaml
- hosts: all
  tasks:
    - name: Load profile and block any variable collisions
      ansible.builtin.include_role:
        name: almaops.common.avars
      vars:
        avars_dir: "{{ inventory_dir | dirname }}/vars/00-profiles/nginx"
        avars_fail_on_override: true
```

## CLI Auditing & CI Usage

You can toggle auditing on the fly from the command line without modifying your playbooks:

```bash
# Inspect overwritten variables with full values on a single host
ansible-playbook deploy.yml --limit nginx_hosts -e avars_debug=true

# Inspect both new and overwritten variables
ansible-playbook deploy.yml --limit nginx_hosts -e avars_debug=true -e avars_debug_new=true

# Audit variable changes in CI without printing values (safe for Vault secrets)
ansible-playbook deploy.yml -e avars_debug=true -e avars_debug_show_values=false

# Fail the run if any profile overwrites an existing variable
ansible-playbook deploy.yml --check -e avars_fail_on_override=true
```

## License
MIT
