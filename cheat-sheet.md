# Charter Ansible and AAP Command Cheat Sheet

> Run the course commands from the lab directory unless the command says otherwise.

```bash
cd ~/bootcamp/lab
```

## Table of Contents

1. [Lab Navigation and File Inspection](#1-lab-navigation-and-file-inspection)
2. [Environment and Version Checks](#2-environment-and-version-checks)
3. [Git Commands](#3-git-commands)
4. [SSH and Key Troubleshooting](#4-ssh-and-key-troubleshooting)
5. [Podman Lab Containers](#5-podman-lab-containers)
6. [Ansible Configuration](#6-ansible-configuration)
7. [Inventory Commands](#7-inventory-commands)
8. [Ad Hoc Ansible Commands](#8-ad-hoc-ansible-commands)
9. [Facts and Service Discovery](#9-facts-and-service-discovery)
10. [Playbook Commands](#10-playbook-commands)
11. [Variables, group_vars, and host_vars](#11-variables-group_vars-and-host_vars)
12. [Templates, Files, Loops, and Handlers](#12-templates-files-loops-and-handlers)
13. [Roles](#13-roles)
14. [Ansible Navigator](#14-ansible-navigator)
15. [Running Multiple Playbooks and Roles](#15-running-multiple-playbooks-and-roles)
16. [Cleanup Commands](#16-cleanup-commands)
17. [NetBox Dynamic Inventory](#17-netbox-dynamic-inventory)
18. [AAP API Commands](#18-aap-api-commands)
19. [Troubleshooting Commands](#19-troubleshooting-commands)
20. [Common Errors and Fast Fixes](#20-common-errors-and-fast-fixes)

---

# 1. Lab Navigation and File Inspection

## Enter the repository

```bash
cd ~/bootcamp
cd lab
```

Or directly:

```bash
cd ~/bootcamp/lab
```

## Confirm the current directory

```bash
pwd
```

## List files

```bash
ls
ls -la
```

## Show the lab structure

```bash
find . -maxdepth 2 -type f | sort
```

Show a role structure:

```bash
find roles/web_config -maxdepth 2 -type f | sort
```

Show all directories and files:

```bash
find .
```

## Read files

```bash
cat inventories/inventory.ini
cat group_vars/linux.yml
cat group_vars/web.yml
cat host_vars/server1.yml
cat ansible.cfg
```

Read only part of a file:

```bash
head -n 20 playbooks/module5_template_deploy.yml
tail -n 20 playbooks/module5_template_deploy.yml
```

Read a file one screen at a time:

```bash
less playbooks/module5_template_deploy.yml
```

## Search files

Search for a variable:

```bash
grep -R "web_message:" -n inventories group_vars host_vars playbooks roles
```

Search the complete lab:

```bash
grep -R "web_message:" -n .
```

Search for a role name:

```bash
grep -R "web_config" -n .
```

Search for a host or group:

```bash
grep -R "ubuntu_web" -n inventories
```

## Edit files

```bash
vi group_vars/web.yml
vim group_vars/web.yml
nano group_vars/web.yml
```

---

# 2. Environment and Version Checks

## Basic tool checks

```bash
git --version
ssh -V
python3 --version
ansible --version
ansible-playbook --version
ansible-navigator --version
podman --version
docker --version
code --version
```

## Ansible help

```bash
ansible --help
ansible-playbook --help
ansible-inventory --help
ansible-config --help
ansible-galaxy --help
ansible-navigator --help
```

## Install Ansible Navigator on macOS

Using pip:

```bash
brew install python
pip3 install ansible-navigator --user
```

Find the Python user installation directory:

```bash
python3 -m site --user-base
```

Add the Python user binary directory to the path:

```bash
echo 'export PATH="$HOME/Library/Python/3.13/bin:$PATH"' >> ~/.zprofile
source ~/.zprofile
```

Upgrade Navigator:

```bash
python3 -m pip install --upgrade ansible-navigator
```

Using pipx:

```bash
brew install pipx
pipx ensurepath
pipx install ansible-navigator
pipx upgrade ansible-navigator
```

Verify:

```bash
ansible-navigator --version
```

---

# 3. Git Commands

## Clone the training repository

```bash
git clone https://github.com/Ansible-workshop-ch/bootcamp.git
cd bootcamp
```

## Check repository state

```bash
git status
git branch
git log --oneline -10
```

## Pull the latest changes

```bash
git pull
```

Explicit branch:

```bash
git pull origin main
```

## Review changes

```bash
git diff
git diff --stat
```

Review staged changes:

```bash
git diff --cached
```

## Add, commit, and push

```bash
git add .
git commit -m "Update Ansible lab"
git push
```

Add specific files:

```bash
git add inventories/inventory.ini ansible.cfg
git commit -m "Fix inventory and Ansible configuration"
git push origin main
```

AAP-compatible inventory example:

```bash
git add inventories/inventory_aap.json ansible.cfg
git commit -m "Add AAP-compatible inventory"
git push
```

## Confirm the latest commit

```bash
git log -1 --oneline
```

## Show changed files

```bash
git status --short
```

---

# 4. SSH and Key Troubleshooting

## Fix private key permissions

```bash
chmod 600 ~/.ssh/ansible_lab_key
```

## Connect to a managed node

```bash
ssh ansible@rhel1
```

## Connect using a private key

```bash
ssh -i ~/.ssh/ansible_lab_key ansible@192.168.1.79
```

## Connect using a custom port

```bash
ssh \
  -o IdentitiesOnly=yes \
  -i ~/.ssh/ansible_lab_key \
  -p 2222 \
  ansible@192.168.1.79
```

Run a command without opening an interactive shell:

```bash
ssh \
  -o IdentitiesOnly=yes \
  -i ~/.ssh/ansible_lab_key \
  -p 2222 \
  ansible@192.168.1.79 \
  hostname
```

## Disable host key prompts for a lab environment

```bash
ssh \
  -o StrictHostKeyChecking=no \
  -o UserKnownHostsFile=/dev/null \
  -i ~/.ssh/ansible_lab_key \
  -p 2222 \
  ansible@192.168.1.79
```

## Verbose SSH troubleshooting

```bash
ssh -vvv \
  -i ~/.ssh/ansible_lab_key \
  -p 2222 \
  ansible@192.168.1.79
```

## Check identity files

```bash
ls -la ~/.ssh
ssh-add -l
```

Add a key to the agent:

```bash
ssh-add ~/.ssh/ansible_lab_key
```

## Test commands after login

```bash
hostname
whoami
cat /etc/os-release
exit
```

---

# 5. Podman Lab Containers

## List containers

```bash
podman ps
podman ps -a
```

Show names and port mappings:

```bash
podman ps --format "table {{.Names}}\t{{.Status}}\t{{.Ports}}"
```

## Start and stop containers

```bash
podman start container1
podman stop container1
podman restart container1
```

Start multiple containers:

```bash
podman start container1 container2 container3 rhel1 rhel2 rhel3
```

## View logs

```bash
podman logs container1
podman logs --tail 100 container1
```

Follow logs:

```bash
podman logs -f container1
```

## Enter a container

```bash
podman exec -it container1 bash
```

Run one command:

```bash
podman exec container1 hostname
podman exec container1 cat /etc/os-release
```

## Inspect networking and ports

```bash
podman port container1
podman inspect container1
```

Only display the IP address:

```bash
podman inspect -f '{{.NetworkSettings.IPAddress}}' container1
```

## Remove containers

```bash
podman rm -f container1
```

Remove stopped containers:

```bash
podman container prune
```

---

# 6. Ansible Configuration

## Show active configuration

```bash
ansible-config dump
```

Only show changed configuration:

```bash
ansible-config dump --only-changed
```

Show the configuration file being used:

```bash
ansible --version
```

## Check a configuration value

```bash
ansible-config dump | grep -i roles_path
ansible-config dump | grep -i inventory
ansible-config dump | grep -i host_key_checking
```

## Local lab ansible.cfg

From `bootcamp/lab/`:

```ini
[defaults]
inventory = inventories/inventory.ini
roles_path = ./roles
host_key_checking = False
retry_files_enabled = False
```

## Root ansible.cfg for AAP

From the repository root:

```ini
[defaults]
roles_path = ./lab/roles
```

## Confirm Ansible can find the role

```bash
ansible-config dump --only-changed | grep -i roles
```

---

# 7. Inventory Commands

## Display the inventory graph

```bash
ansible-inventory -i inventories/inventory.ini --graph
```

## List the full inventory

```bash
ansible-inventory -i inventories/inventory.ini --list
```

## Show one host and its resolved variables

```bash
ansible-inventory -i inventories/inventory.ini --host server1
```

## Validate inventory parsing

```bash
ansible-inventory -i inventories/inventory.ini --list >/dev/null
echo $?
```

A return code of `0` means the inventory parsed successfully.

## Target patterns

All hosts:

```bash
ansible all -i inventories/inventory.ini -m ping
```

One group:

```bash
ansible web -i inventories/inventory.ini -m ping
```

One host:

```bash
ansible server1 -i inventories/inventory.ini -m ping
```

Multiple groups or hosts:

```bash
ansible 'rhel_web:ubuntu_web' -i inventories/inventory.ini -m ping
```

Exclude a host:

```bash
ansible 'all:!server1' -i inventories/inventory.ini -m ping
```

Intersection:

```bash
ansible 'web:&rhel_web' -i inventories/inventory.ini -m ping
```

## Common inventory warning check

```bash
ansible-inventory -i inventories/inventory.ini --graph
```

If a parent group references a child group, the child group must exist.

Example:

```ini
[web:children]
rhel_web
ubuntu_web
```

Both groups must also be defined:

```ini
[rhel_web]
rhel1

[ubuntu_web]
container1
```

---

# 8. Ad Hoc Ansible Commands

## Connectivity

```bash
ansible all -i inventories/inventory.ini -m ansible.builtin.ping
```

Short module name:

```bash
ansible all -i inventories/inventory.ini -m ping
```

## Hostname and uptime

```bash
ansible all -i inventories/inventory.ini \
  -m ansible.builtin.command \
  -a "hostname"
```

```bash
ansible all -i inventories/inventory.ini \
  -m ansible.builtin.command \
  -a "uptime"
```

## Target one group

```bash
ansible web -i inventories/inventory.ini \
  -m ansible.builtin.command \
  -a "hostname"
```

## Install a package

Debian or Ubuntu:

```bash
ansible web -i inventories/inventory.ini \
  -m ansible.builtin.package \
  -a "name=apache2 state=present" \
  --become
```

Red Hat:

```bash
ansible rhel_web -i inventories/inventory.ini \
  -m ansible.builtin.package \
  -a "name=httpd state=present" \
  --become
```

## Start a service

Debian or Ubuntu:

```bash
ansible web -i inventories/inventory.ini \
  -m ansible.builtin.service \
  -a "name=apache2 state=started enabled=true" \
  --become
```

Red Hat:

```bash
ansible rhel_web -i inventories/inventory.ini \
  -m ansible.builtin.service \
  -a "name=httpd state=started enabled=true" \
  --become
```

## Restart a service

```bash
ansible web -i inventories/inventory.ini \
  -m ansible.builtin.service \
  -a "name=apache2 state=restarted" \
  --become
```

## Check service status

```bash
ansible web -i inventories/inventory.ini \
  -m ansible.builtin.command \
  -a "systemctl is-active apache2" \
  --become
```

## Run a shell command

Use `shell` only when shell behavior is required:

```bash
ansible all -i inventories/inventory.ini \
  -m ansible.builtin.shell \
  -a "ps aux | grep sshd"
```

## Read a remote file

```bash
ansible linux -i inventories/inventory.ini \
  -m ansible.builtin.command \
  -a "cat /etc/charter/web_config_role.txt" \
  --become
```

```bash
ansible linux -i inventories/inventory.ini \
  -m ansible.builtin.command \
  -a "cat /var/www/html/index.html" \
  --become
```

## Test a local web service from each host

```bash
ansible linux -i inventories/inventory.ini \
  -m ansible.builtin.uri \
  -a "url=http://localhost status_code=200"
```

## Copy a file

```bash
ansible all -i inventories/inventory.ini \
  -m ansible.builtin.copy \
  -a "content='Managed by Ansible\n' dest=/tmp/ansible-test.txt mode=0644"
```

## Remove a file

```bash
ansible all -i inventories/inventory.ini \
  -m ansible.builtin.file \
  -a "path=/tmp/ansible-test.txt state=absent"
```

## Use privilege escalation

```bash
ansible all -i inventories/inventory.ini -m ping --become
```

Ask for the sudo password:

```bash
ansible all -i inventories/inventory.ini -m ping --ask-become-pass
```

## Use a specific SSH user and key

```bash
ansible all -i inventories/inventory.ini \
  -m ping \
  -u ansible \
  --private-key ~/.ssh/ansible_lab_key
```

---

# 9. Facts and Service Discovery

## Gather all facts

```bash
ansible web -i inventories/inventory.ini \
  -m ansible.builtin.setup
```

## Filter facts

Operating system:

```bash
ansible all -i inventories/inventory.ini \
  -m ansible.builtin.setup \
  -a "filter=ansible_distribution*"
```

OS family:

```bash
ansible all -i inventories/inventory.ini \
  -m ansible.builtin.setup \
  -a "filter=ansible_os_family"
```

Memory:

```bash
ansible all -i inventories/inventory.ini \
  -m ansible.builtin.setup \
  -a "filter=ansible_memtotal_mb"
```

IP address:

```bash
ansible all -i inventories/inventory.ini \
  -m ansible.builtin.setup \
  -a "filter=ansible_default_ipv4"
```

## Gather service facts

```bash
ansible all -i inventories/inventory.ini \
  -m ansible.builtin.service_facts \
  --become
```

Show services containing `ssh`:

```bash
ansible all -i inventories/inventory.ini \
  -m ansible.builtin.shell \
  -a "systemctl list-units --type=service --state=running | grep -i ssh" \
  --become
```

List all running systemd services:

```bash
ansible all -i inventories/inventory.ini \
  -m ansible.builtin.command \
  -a "systemctl list-units --type=service --state=running --no-pager" \
  --become
```

---

# 10. Playbook Commands

## Run a playbook

```bash
ansible-playbook \
  -i inventories/inventory.ini \
  playbooks/module1_ping.yml
```

## Syntax check

```bash
ansible-playbook \
  -i inventories/inventory.ini \
  playbooks/module5_template_deploy.yml \
  --syntax-check
```

## Dry run

```bash
ansible-playbook \
  -i inventories/inventory.ini \
  playbooks/module5_template_deploy.yml \
  --check
```

Dry run with file differences:

```bash
ansible-playbook \
  -i inventories/inventory.ini \
  playbooks/module5_template_deploy.yml \
  --check \
  --diff
```

## Show targeted hosts

```bash
ansible-playbook \
  -i inventories/inventory.ini \
  playbooks/module5_template_deploy.yml \
  --list-hosts
```

## Show tasks

```bash
ansible-playbook \
  -i inventories/inventory.ini \
  playbooks/module5_template_deploy.yml \
  --list-tasks
```

## Limit execution

One host:

```bash
ansible-playbook \
  -i inventories/inventory.ini \
  playbooks/module5_template_deploy.yml \
  --limit container1
```

Multiple hosts:

```bash
ansible-playbook \
  -i inventories/inventory.ini \
  playbooks/module5_template_deploy.yml \
  --limit 'container1:container2'
```

One group:

```bash
ansible-playbook \
  -i inventories/inventory.ini \
  playbooks/module5_template_deploy.yml \
  --limit rhel_web
```

## Start at one task

```bash
ansible-playbook \
  -i inventories/inventory.ini \
  playbooks/module5_template_deploy.yml \
  --start-at-task "Deploy the web page"
```

## Run one task at a time

```bash
ansible-playbook \
  -i inventories/inventory.ini \
  playbooks/module5_template_deploy.yml \
  --step
```

## Tags

List tags:

```bash
ansible-playbook \
  -i inventories/inventory.ini \
  playbooks/module5_template_deploy.yml \
  --list-tags
```

Run selected tags:

```bash
ansible-playbook \
  -i inventories/inventory.ini \
  playbooks/module5_template_deploy.yml \
  --tags web
```

Skip selected tags:

```bash
ansible-playbook \
  -i inventories/inventory.ini \
  playbooks/module5_template_deploy.yml \
  --skip-tags packages
```

## Verbose output

```bash
ansible-playbook \
  -i inventories/inventory.ini \
  playbooks/module5_template_deploy.yml \
  -v
```

More detail:

```bash
ansible-playbook \
  -i inventories/inventory.ini \
  playbooks/module5_template_deploy.yml \
  -vvv
```

SSH-level detail:

```bash
ansible-playbook \
  -i inventories/inventory.ini \
  playbooks/module5_template_deploy.yml \
  -vvvv
```

## Run with a specific key

```bash
ansible-playbook \
  -i inventories/inventory.ini \
  playbooks/module5_template_deploy.yml \
  -u ansible \
  --private-key ~/.ssh/ansible_lab_key
```

---

# 11. Variables, group_vars, and host_vars

## Run the variables playbook

```bash
ansible-playbook \
  -i inventories/inventory.ini \
  playbooks/module4_variables.yml
```

## Pass an extra variable

```bash
ansible-playbook \
  -i inventories/inventory.ini \
  playbooks/module4_variables.yml \
  -e "web_message=Message from extra vars"
```

Pass multiple variables:

```bash
ansible-playbook \
  -i inventories/inventory.ini \
  playbooks/module4_variables.yml \
  -e "web_message='Runtime message' environment_name=training"
```

Pass variables from a file:

```bash
ansible-playbook \
  -i inventories/inventory.ini \
  playbooks/module4_variables.yml \
  -e @extra_vars.yml
```

## Inspect resolved variables

```bash
ansible-inventory \
  -i inventories/inventory.ini \
  --host server1
```

## Search all definitions of a variable

```bash
grep -R "web_message:" -n \
  inventories group_vars host_vars playbooks roles
```

## Example precedence test

Run the role normally:

```bash
ansible-playbook \
  -i inventories/inventory.ini \
  playbooks/module6_role_apply.yml
```

Override the role default:

```bash
ansible-playbook \
  -i inventories/inventory.ini \
  playbooks/module6_role_apply.yml \
  -e "web_message=Extra variable wins"
```

## Common variable locations

```text
group_vars/all.yml
group_vars/linux.yml
group_vars/web.yml
group_vars/rhel_web.yml
group_vars/ubuntu_web.yml
host_vars/server1.yml
roles/web_config/defaults/main.yml
roles/web_config/vars/main.yml
```

---

# 12. Templates, Files, Loops, and Handlers

## Syntax check Module 5

```bash
ansible-playbook \
  -i inventories/inventory.ini \
  playbooks/module5_template_deploy.yml \
  --syntax-check
```

## Run Module 5

```bash
ansible-playbook \
  -i inventories/inventory.ini \
  playbooks/module5_template_deploy.yml
```

## Run it again to test idempotency

```bash
ansible-playbook \
  -i inventories/inventory.ini \
  playbooks/module5_template_deploy.yml
```

Expected second-run behavior:

```text
Most tasks: ok
Handler: not triggered
changed=0
```

## Show template changes

```bash
ansible-playbook \
  -i inventories/inventory.ini \
  playbooks/module5_template_deploy.yml \
  --check \
  --diff
```

## Verify generated content

```bash
ansible linux -i inventories/inventory.ini \
  -m ansible.builtin.command \
  -a "cat /var/www/html/index.html" \
  --become
```

## Verify Apache configuration

Debian or Ubuntu:

```bash
ansible ubuntu_web -i inventories/inventory.ini \
  -m ansible.builtin.command \
  -a "apache2ctl configtest" \
  --become
```

Red Hat:

```bash
ansible rhel_web -i inventories/inventory.ini \
  -m ansible.builtin.command \
  -a "httpd -t" \
  --become
```

## Confirm the web page responds

```bash
ansible linux -i inventories/inventory.ini \
  -m ansible.builtin.uri \
  -a "url=http://localhost status_code=200"
```

---

# 13. Roles

## Create a role

```bash
ansible-galaxy role init web_config --init-path roles
```

Create another role:

```bash
ansible-galaxy role init clean-config --init-path roles
```

## Show the role structure

```bash
find roles/web_config -maxdepth 2 -type f | sort
```

## Validate the role playbook

```bash
ansible-playbook \
  -i inventories/inventory.ini \
  playbooks/module6_role_apply.yml \
  --syntax-check
```

## List role tasks

```bash
ansible-playbook \
  -i inventories/inventory.ini \
  playbooks/module6_role_apply.yml \
  --list-tasks
```

## Run the role

```bash
ansible-playbook \
  -i inventories/inventory.ini \
  playbooks/module6_role_apply.yml
```

## Run the role again

```bash
ansible-playbook \
  -i inventories/inventory.ini \
  playbooks/module6_role_apply.yml
```

## Override a role default

```bash
ansible-playbook \
  -i inventories/inventory.ini \
  playbooks/module6_role_apply.yml \
  -e "web_message=Charter Role Override Successful"
```

## Verify role-created files

```bash
ansible linux -i inventories/inventory.ini \
  -m ansible.builtin.command \
  -a "cat /etc/charter/web_config_role.txt" \
  --become
```

```bash
ansible linux -i inventories/inventory.ini \
  -m ansible.builtin.command \
  -a "cat /var/www/html/index.html" \
  --become
```

## Fix a broken role tasks file

A role task file must contain a YAML list of tasks.

Wrong:

```yaml
---
tasks:
  - name: Example
    ansible.builtin.debug:
      msg: Hello
```

Correct:

```yaml
---
- name: Example
  ansible.builtin.debug:
    msg: Hello
```

Validate after fixing:

```bash
ansible-playbook \
  -i inventories/inventory.ini \
  playbooks/cleanup.yaml \
  --syntax-check
```

---

# 14. Ansible Navigator

## Start Navigator

```bash
ansible-navigator
```

Welcome screen:

```bash
ansible-navigator welcome
```

## Check version and settings

```bash
ansible-navigator --version
ansible-navigator settings
```

## Inventory graph

```bash
ansible-navigator inventory \
  -i inventories/inventory.ini \
  --graph \
  --mode stdout
```

## Inventory list

```bash
ansible-navigator inventory \
  -i inventories/inventory.ini \
  --list \
  --mode stdout
```

## Run a playbook

```bash
ansible-navigator run \
  playbooks/module9_final_usecase.yml \
  -i inventories/inventory.ini \
  --mode stdout
```

## Run without an execution environment

```bash
ansible-navigator run \
  playbooks/module9_final_usecase.yml \
  -i inventories/inventory.ini \
  --mode stdout \
  --ee false
```

## Run with a specific user and key

```bash
ansible-navigator run \
  playbooks/module5_template_deploy.yml \
  -i inventories/inventory.ini \
  --mode stdout \
  -u ansible \
  --private-key ~/.ssh/ansible_lab_key
```

## Limit Navigator execution

```bash
ansible-navigator run \
  playbooks/module9_final_usecase.yml \
  -i inventories/inventory.ini \
  --mode stdout \
  --limit rhel1
```

## Run cleanup with Navigator

```bash
ansible-navigator run \
  playbooks/cleanup.yaml \
  -i inventories/inventory.ini \
  --mode stdout
```

Without an EE:

```bash
ansible-navigator run \
  playbooks/cleanup.yaml \
  -i inventories/inventory.ini \
  --mode stdout \
  --ee false
```

## Read module documentation

```bash
ansible-navigator doc ansible.builtin.service --mode stdout
ansible-navigator doc ansible.builtin.package --mode stdout
ansible-navigator doc ansible.builtin.template --mode stdout
ansible-navigator doc ansible.builtin.uri --mode stdout
```

## Execute a command through Navigator

```bash
ansible-navigator exec -- ansible --version
```

```bash
ansible-navigator exec -- ansible-inventory \
  -i inventories/inventory.ini \
  --graph
```

## Check execution environment images

```bash
ansible-navigator images
```

## Replay a saved artifact

```bash
ansible-navigator replay <artifact-file.json>
```

## macOS SSH_AUTH_SOCK fix

Resolve the real socket path:

```bash
export SSH_AUTH_SOCK="$(
  python3 -c 'import os; print(os.path.realpath(os.environ["SSH_AUTH_SOCK"]))'
)"
```

Run Navigator without the SSH agent variable:

```bash
env -u SSH_AUTH_SOCK ansible-navigator exec -- ansible --version
```

## Interactive Navigator commands

From the welcome screen:

```text
:inventory -i inventories/inventory.ini
:doc ansible.builtin.service
:q
```

---

# 15. Running Multiple Playbooks and Roles

## Run multiple playbooks in one command

```bash
ansible-playbook \
  -i inventories/inventory.ini \
  playbooks/module4_variables.yml \
  playbooks/module5_template_deploy.yml
```

Run three playbooks:

```bash
ansible-playbook \
  -i inventories/inventory.ini \
  playbooks/module4_variables.yml \
  playbooks/module5_template_deploy.yml \
  playbooks/module6_role_apply.yml
```

Important: Ansible runs them in the order listed.

## Run multiple roles from one playbook

Example playbook:

```yaml
---
- name: Apply multiple roles
  hosts: linux
  become: true
  gather_facts: true

  roles:
    - web_config
    - clean-config
```

Run it:

```bash
ansible-playbook \
  -i inventories/inventory.ini \
  playbooks/multiple_roles.yml
```

Navigator:

```bash
ansible-navigator run \
  playbooks/multiple_roles.yml \
  -i inventories/inventory.ini \
  --mode stdout
```

---

# 16. Cleanup Commands

## Syntax check cleanup

```bash
ansible-playbook \
  -i inventories/inventory.ini \
  playbooks/cleanup.yaml \
  --syntax-check
```

## Run cleanup

```bash
ansible-playbook \
  -i inventories/inventory.ini \
  playbooks/cleanup.yaml
```

## Run cleanup with Navigator

```bash
ansible-navigator run \
  playbooks/cleanup.yaml \
  -i inventories/inventory.ini \
  --mode stdout
```

## Limit cleanup to one host

```bash
ansible-playbook \
  -i inventories/inventory.ini \
  playbooks/cleanup.yaml \
  --limit container1
```

## Confirm files were removed

```bash
ansible linux -i inventories/inventory.ini \
  -m ansible.builtin.stat \
  -a "path=/etc/charter/web_config_role.txt"
```

---

# 17. NetBox Dynamic Inventory

## Set NetBox environment variables

```bash
export NETBOX_API="https://netbox.example.com"
export NETBOX_TOKEN="<netbox-token>"
```

Confirm they are set:

```bash
echo "$NETBOX_API"
test -n "$NETBOX_TOKEN" && echo "Token is set"
```

Do not print the token itself.

## Test the NetBox API

```bash
curl -sS \
  -H "Authorization: Bearer $NETBOX_TOKEN" \
  "$NETBOX_API/api/status/" \
  | jq
```

## Search NetBox devices

```bash
curl -sS \
  -H "Authorization: Bearer $NETBOX_TOKEN" \
  "$NETBOX_API/api/dcim/devices/?q=container1" \
  | jq '{count, names: [.results[].name]}'
```

## Search NetBox virtual machines

```bash
curl -sS \
  -H "Authorization: Bearer $NETBOX_TOKEN" \
  "$NETBOX_API/api/virtualization/virtual-machines/?q=container1" \
  | jq '{count, names: [.results[].name]}'
```

## Inspect the dynamic inventory file

From the repository root:

```bash
cat lab/inventories/netbox_inventory.yml
```

From `bootcamp/lab`:

```bash
cat inventories/netbox_inventory.yml
```

## Test NetBox inventory parsing

From the repository root:

```bash
ansible-inventory \
  -i lab/inventories/netbox_inventory.yml \
  --graph
```

From `bootcamp/lab`:

```bash
ansible-inventory \
  -i inventories/netbox_inventory.yml \
  --graph
```

List all data:

```bash
ansible-inventory \
  -i inventories/netbox_inventory.yml \
  --list
```

Inspect one NetBox host:

```bash
ansible-inventory \
  -i inventories/netbox_inventory.yml \
  --host container1
```

## Run the NetBox inventory test playbook

From the repository root:

```bash
ansible-playbook \
  -i lab/inventories/netbox_inventory.yml \
  lab/playbooks/netbox_dynamic_inventory.yml
```

From `bootcamp/lab`:

```bash
ansible-playbook \
  -i inventories/netbox_inventory.yml \
  playbooks/netbox_dynamic_inventory.yml
```

## Merge NetBox and static inventory

```bash
ansible-playbook \
  -i lab/inventories/netbox_inventory.yml \
  -i lab/inventories/inventory.ini \
  lab/playbooks/netbox_dynamic_inventory.yml \
  --limit 'container1:container2:container3'
```

From `bootcamp/lab`:

```bash
ansible-playbook \
  -i inventories/netbox_inventory.yml \
  -i inventories/inventory.ini \
  playbooks/netbox_dynamic_inventory.yml \
  --limit 'container1:container2:container3'
```

The later inventory can add or override connection variables for hosts already discovered from NetBox.

## Verify a host exists exactly once

```bash
curl -sS \
  -H "Authorization: Bearer $NETBOX_TOKEN" \
  "$NETBOX_API/api/dcim/devices/?name=container1" \
  | jq '.count'
```

```bash
curl -sS \
  -H "Authorization: Bearer $NETBOX_TOKEN" \
  "$NETBOX_API/api/virtualization/virtual-machines/?name=container1" \
  | jq '.count'
```

The combined count should equal `1`.

---

# 18. AAP API Commands

AAP 2.5 or 2.6 may expose the controller API through the platform gateway.

## Set variables

```bash
export AAP_URL="https://aap.example.com"
export AAP_TOKEN="<oauth-token>"
```

Check that variables exist:

```bash
echo "$AAP_URL"
test -n "$AAP_TOKEN" && echo "AAP token is set"
```

## Test the controller API

Gateway path:

```bash
curl -sk \
  -H "Authorization: Bearer $AAP_TOKEN" \
  "$AAP_URL/api/controller/v2/ping/" \
  | jq
```

Direct controller path:

```bash
curl -sk \
  -H "Authorization: Bearer $AAP_TOKEN" \
  "$AAP_URL/api/v2/ping/" \
  | jq
```

## List projects

```bash
curl -sk \
  -H "Authorization: Bearer $AAP_TOKEN" \
  "$AAP_URL/api/controller/v2/projects/" \
  | jq '.results[] | {id, name, status, scm_revision}'
```

## List inventories

```bash
curl -sk \
  -H "Authorization: Bearer $AAP_TOKEN" \
  "$AAP_URL/api/controller/v2/inventories/" \
  | jq '.results[] | {id, name}'
```

## List job templates

```bash
curl -sk \
  -H "Authorization: Bearer $AAP_TOKEN" \
  "$AAP_URL/api/controller/v2/job_templates/" \
  | jq '.results[] | {id, name, playbook}'
```

## Launch a job template

Example with job template ID `9`:

```bash
curl -sk \
  -X POST \
  -H "Authorization: Bearer $AAP_TOKEN" \
  -H "Content-Type: application/json" \
  "$AAP_URL/api/controller/v2/job_templates/9/launch/" \
  -d '{}' \
  | jq
```

Launch with extra variables:

```bash
curl -sk \
  -X POST \
  -H "Authorization: Bearer $AAP_TOKEN" \
  -H "Content-Type: application/json" \
  "$AAP_URL/api/controller/v2/job_templates/9/launch/" \
  -d '{
    "extra_vars": {
      "web_message": "Launched from the AAP API"
    }
  }' \
  | jq
```

## Save the job ID

```bash
JOB_ID="$(
  curl -sk \
    -X POST \
    -H "Authorization: Bearer $AAP_TOKEN" \
    -H "Content-Type: application/json" \
    "$AAP_URL/api/controller/v2/job_templates/9/launch/" \
    -d '{}' \
  | jq -r '.job'
)"
```

```bash
echo "$JOB_ID"
```

## Check job status

```bash
curl -sk \
  -H "Authorization: Bearer $AAP_TOKEN" \
  "$AAP_URL/api/controller/v2/jobs/$JOB_ID/" \
  | jq '{id, name, status, started, finished, failed}'
```

## Read job stdout

Text output:

```bash
curl -sk \
  -H "Authorization: Bearer $AAP_TOKEN" \
  "$AAP_URL/api/controller/v2/jobs/$JOB_ID/stdout/?format=txt"
```

JSON output:

```bash
curl -sk \
  -H "Authorization: Bearer $AAP_TOKEN" \
  "$AAP_URL/api/controller/v2/jobs/$JOB_ID/stdout/?format=json" \
  | jq
```

## List failed events

```bash
curl -sk \
  -H "Authorization: Bearer $AAP_TOKEN" \
  "$AAP_URL/api/controller/v2/jobs/$JOB_ID/job_events/?event=runner_on_failed" \
  | jq '.results[] | {
      host: .event_data.host,
      task: .event_data.task,
      result: .event_data.res
    }'
```

## List unreachable events

```bash
curl -sk \
  -H "Authorization: Bearer $AAP_TOKEN" \
  "$AAP_URL/api/controller/v2/jobs/$JOB_ID/job_events/?event=runner_on_unreachable" \
  | jq '.results[] | {
      host: .event_data.host,
      task: .event_data.task,
      result: .event_data.res
    }'
```

---

# 19. Troubleshooting Commands

## Confirm the current location

```bash
pwd
ls
```

## Confirm Ansible version and active config

```bash
ansible --version
ansible-config dump --only-changed
```

## Validate inventory first

```bash
ansible-inventory \
  -i inventories/inventory.ini \
  --graph
```

## Test one host

```bash
ansible container1 \
  -i inventories/inventory.ini \
  -m ping \
  -vvv
```

## Test SSH outside Ansible

```bash
ssh \
  -o IdentitiesOnly=yes \
  -i ~/.ssh/ansible_lab_key \
  -p 2222 \
  ansible@192.168.1.79 \
  hostname
```

## Check the exact inventory variables

```bash
ansible-inventory \
  -i inventories/inventory.ini \
  --host container1
```

## Confirm a playbook exists

```bash
ls -l playbooks/module5_template_deploy.yml
```

## Find a playbook

```bash
find . -name "module5_template_deploy.yml"
```

## Syntax check before execution

```bash
ansible-playbook \
  -i inventories/inventory.ini \
  playbooks/module5_template_deploy.yml \
  --syntax-check
```

## Show detailed failure output

```bash
ansible-playbook \
  -i inventories/inventory.ini \
  playbooks/module5_template_deploy.yml \
  -vvv
```

## Confirm role search path

```bash
ansible-config dump --only-changed | grep -i roles
```

## Confirm role files

```bash
find roles/web_config -maxdepth 2 -type f | sort
```

## Confirm variable definitions

```bash
grep -R "web_message:" -n \
  inventories group_vars host_vars playbooks roles
```

## Check YAML indentation

```bash
python3 - <<'PY'
import yaml
from pathlib import Path

path = Path("playbooks/module5_template_deploy.yml")
with path.open() as file:
    yaml.safe_load(file)

print(f"{path} parsed successfully")
PY
```

## Check ports

```bash
podman ps --format "table {{.Names}}\t{{.Ports}}"
```

## Check local port reachability

```bash
nc -vz 192.168.1.79 2222
```

Or:

```bash
telnet 192.168.1.79 2222
```

## Check DNS

```bash
getent hosts github.com
nslookup github.com
dig github.com
```

## Check network reachability

```bash
ping -c 4 github.com
curl -I https://github.com
```

## Check proxy settings

```bash
env | grep -i proxy
git config --global --get http.proxy
git config --global --get https.proxy
```

Remove Git proxy settings:

```bash
git config --global --unset http.proxy
git config --global --unset https.proxy
```

---

# 20. Common Errors and Fast Fixes

## Error: No inventory was parsed

Symptom:

```text
No inventory was parsed, only implicit localhost is available
```

Fix:

```bash
cd ~/bootcamp/lab
ansible-inventory -i inventories/inventory.ini --graph
```

Then run:

```bash
ansible all -i inventories/inventory.ini -m ping
```

## Error: Could not match supplied host pattern

Symptom:

```text
Could not match supplied host pattern, ignoring: rhel_web
```

Check the inventory:

```bash
grep -n "rhel_web" inventories/inventory.ini
ansible-inventory -i inventories/inventory.ini --graph
```

The group name in the command must exactly match the inventory group.

## Error: Undefined child group

Symptom:

```text
Section [web:children] includes undefined group: ubuntu_web
```

Fix the inventory so the child group exists:

```ini
[web:children]
rhel_web
ubuntu_web

[rhel_web]
rhel1

[ubuntu_web]
container1
```

Validate:

```bash
ansible-inventory -i inventories/inventory.ini --graph
```

## Error: Permission denied publickey

Test SSH manually:

```bash
ssh -vvv \
  -o IdentitiesOnly=yes \
  -i ~/.ssh/ansible_lab_key \
  -p 2222 \
  ansible@192.168.1.79
```

Fix key permissions:

```bash
chmod 600 ~/.ssh/ansible_lab_key
```

Run Ansible with the same key:

```bash
ansible all \
  -i inventories/inventory.ini \
  -m ping \
  -u ansible \
  --private-key ~/.ssh/ansible_lab_key
```

## Error: Role not found

Check configuration:

```bash
ansible-config dump --only-changed | grep -i roles
```

Check role files:

```bash
find roles/web_config -maxdepth 2 -type f
```

Local `ansible.cfg`:

```ini
[defaults]
roles_path = ./roles
```

AAP repository-root `ansible.cfg`:

```ini
[defaults]
roles_path = ./lab/roles
```

## Error: tasks/main.yml must contain a list

Wrong:

```yaml
---
tasks:
  - name: Example
```

Correct:

```yaml
---
- name: Example
```

Validate:

```bash
ansible-playbook \
  -i inventories/inventory.ini \
  playbooks/multiple_roles.yml \
  --syntax-check
```

## Error: Updated variable is not appearing

Find every definition:

```bash
grep -R "web_message:" -n \
  inventories group_vars host_vars playbooks roles
```

Remember the simplified precedence used in the course:

```text
role defaults
group_vars/all
group_vars/group
host_vars/host
play variables
extra variables
```

A host variable such as:

```text
host_vars/server1.yml
```

can override:

```text
group_vars/web.yml
```

Extra variables override both:

```bash
ansible-playbook \
  -i inventories/inventory.ini \
  playbooks/module6_role_apply.yml \
  -e "web_message=Runtime override"
```

## Error: Navigator cannot use the macOS SSH agent

```bash
export SSH_AUTH_SOCK="$(
  python3 -c 'import os; print(os.path.realpath(os.environ["SSH_AUTH_SOCK"]))'
)"
```

Or bypass it:

```bash
env -u SSH_AUTH_SOCK ansible-navigator run \
  playbooks/module9_final_usecase.yml \
  -i inventories/inventory.ini \
  --mode stdout \
  --ee false
```

## Error: NetBox host count equals zero

Search devices:

```bash
curl -sS \
  -H "Authorization: Bearer $NETBOX_TOKEN" \
  "$NETBOX_API/api/dcim/devices/?q=container1" \
  | jq '{count, names: [.results[].name]}'
```

Search virtual machines:

```bash
curl -sS \
  -H "Authorization: Bearer $NETBOX_TOKEN" \
  "$NETBOX_API/api/virtualization/virtual-machines/?q=container1" \
  | jq '{count, names: [.results[].name]}'
```

The host must first exist in NetBox before NetBox dynamic inventory can discover it.

---

# Fast Daily Workflow

Use this order when starting a lab:

```bash
cd ~/bootcamp/lab

git pull

ansible --version

ansible-inventory \
  -i inventories/inventory.ini \
  --graph

ansible all \
  -i inventories/inventory.ini \
  -m ping

ansible-playbook \
  -i inventories/inventory.ini \
  playbooks/<playbook>.yml \
  --syntax-check

ansible-playbook \
  -i inventories/inventory.ini \
  playbooks/<playbook>.yml
```

Navigator workflow:

```bash
cd ~/bootcamp/lab

ansible-navigator --version

ansible-navigator inventory \
  -i inventories/inventory.ini \
  --graph \
  --mode stdout

ansible-navigator run \
  playbooks/<playbook>.yml \
  -i inventories/inventory.ini \
  --mode stdout
```

If no execution environment is available:

```bash
ansible-navigator run \
  playbooks/<playbook>.yml \
  -i inventories/inventory.ini \
  --mode stdout \
  --ee false
```

---

# Most Important Commands to Memorize

```bash
cd ~/bootcamp/lab
```

```bash
ansible --version
```

```bash
ansible-inventory -i inventories/inventory.ini --graph
```

```bash
ansible all -i inventories/inventory.ini -m ping
```

```bash
ansible all -i inventories/inventory.ini -m command -a "hostname"
```

```bash
ansible-playbook -i inventories/inventory.ini playbooks/<playbook>.yml --syntax-check
```

```bash
ansible-playbook -i inventories/inventory.ini playbooks/<playbook>.yml
```

```bash
ansible-navigator inventory -i inventories/inventory.ini --graph --mode stdout
```

```bash
ansible-navigator run playbooks/<playbook>.yml -i inventories/inventory.ini --mode stdout
```

```bash
grep -R "variable_name:" -n inventories group_vars host_vars playbooks roles
```

```bash
ansible-inventory -i inventories/inventory.ini --host <hostname>
```

```bash
ssh -vvv -i ~/.ssh/ansible_lab_key -p <port> ansible@<host>
```
