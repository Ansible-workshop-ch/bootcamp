# web_config

The `web_config` role installs and configures an Apache web server on supported Red Hat and Debian systems.

## Requirements

* Ansible facts must be gathered.
* Privilege escalation is required.
* The managed host must use the `RedHat` or `Debian` operating system family.

## Default Variables

| Variable | Default | Purpose |
| --- | --- | --- |
| `company` | `Example Organization` | Organization displayed in generated files |
| `environment_name` | `default` | Environment displayed in generated files |
| `web_message` | `Managed by the web_config role` | Website heading |
| `web_owner` | `Platform Engineering` | Team displayed on the website |
| `web_port` | `80` | Port used to validate the website |
| `web_document_root` | `/var/www/html` | Website destination directory |
| `web_index_file` | `index.html` | Website index filename |
| `web_config_filename` | `charter-module6.conf` | Apache configuration filename |
| `web_role_info_path` | `/etc/charter/web_config_role.txt` | Role information file |

## Optional Inventory Overrides

The role can use these inventory variables when they are defined:

| Variable | Purpose |
| --- | --- |
| `package_name` | Operating-system-specific Apache package |
| `service_name` | Operating-system-specific Apache service |
| `common_packages` | Packages installed by the role |

## Example Playbook

```yaml
---
- name: Apply the web_config role
  hosts: web
  become: true
  gather_facts: true

  roles:
    - web_config