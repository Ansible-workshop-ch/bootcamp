# Lab Code

The runnable Ansible code for every CLI lab in this bootcamp.

**Run all commands from this directory.**

```bash
cd bootcamp/lab
```

---

## Quick Start

```bash
# 1. Review your targets
$EDITOR inventories/inventory.ini

# 2. Verify connectivity
ansible all -m ping

# 3. Run a module playbook
ansible-playbook playbooks/module3_webserver.yml
```

`ansible.cfg` points at:

```text
./inventories/inventory.ini
./roles
```

So no `-i` flag is needed as long as you are running commands from this directory.

---

## Layout

```text
lab/
├── README.md
├── ansible.cfg
├── inventories/
│   ├── inventory.ini
│   └── group_vars/
│       ├── all.yml
│       ├── web.yml
│       ├── ubuntu_web.yml
│       └── rhel_web.yml
├── playbooks/
│   ├── module1_ping.yml
│   ├── module2_service.yml
│   ├── module3_webserver.yml
│   ├── module4_variables.yml
│   ├── module5_template_deploy.yml
│   ├── module6_role_apply.yml
│   ├── module9_final_usecase.yml
│   └── ec2_exact_count.yml
├── roles/
│   └── web_config/
├── templates/
│   ├── index.html.j2
│   └── motd.j2
└── bonus/
    ├── netbox-source-of-truth.md
    └── creating-ec2-with-ansible.md
```

---

## Inventory Groups

The lab inventory includes both Ubuntu-based and Rocky/RHEL-based targets.

```text
ubuntu_web
  container1
  container2
  container3

rhel_web
  rhel1
  rhel2
  rhel3

web
  ubuntu_web
  rhel_web

linux
  web
```

Use these groups depending on what you want to target:

| Group        | Purpose                                 |
| ------------ | --------------------------------------- |
| `ubuntu_web` | Ubuntu-based web containers             |
| `rhel_web`   | Rocky/RHEL-based web containers         |
| `web`        | All web servers across both OS families |
| `linux`      | All Linux hosts in the lab              |

---

## Group Variables

OS-specific package and service names live in group variables.

```text
inventories/group_vars/ubuntu_web.yml
inventories/group_vars/rhel_web.yml
```

Ubuntu/Debian uses:

```yaml
package_name: apache2
service_name: apache2
```

Rocky/RHEL uses:

```yaml
package_name: httpd
service_name: httpd
```

The playbooks use variables instead of hardcoded package names:

```yaml
name: "{{ package_name }}"
```

```yaml
name: "{{ service_name }}"
```

This allows the same playbook to work across different Linux distributions.

---

## Common Commands

Run against all hosts:

```bash
ansible all -m ping
```

Run against Ubuntu hosts only:

```bash
ansible ubuntu_web -m ping
```

Run against Rocky/RHEL hosts only:

```bash
ansible rhel_web -m ping
```

Run against all web hosts:

```bash
ansible web -m ping
```

Run a playbook against all web hosts:

```bash
ansible-playbook playbooks/module3_webserver.yml --limit web
```

Run a playbook against only Rocky/RHEL hosts:

```bash
ansible-playbook playbooks/module3_webserver.yml --limit rhel_web
```

---

## Bonus Labs

Bonus labs are optional. They are useful for instructor demos, advanced discussions, or extra practice if time allows.

### NetBox Source of Truth

NetBox shows how infrastructure data can be managed outside of static inventory.

Reference:

```text
bonus/netbox-source-of-truth.md
```

Main idea:

```text
NetBox tells Ansible what exists.
Ansible performs the automation.
AAP controls and runs the automation.
```

---

### EC2 Creation with Ansible

This bonus lesson shows that Ansible can create cloud infrastructure directly, such as AWS EC2 instances.

Lesson:

[Creating EC2 with Ansible](https://github.com/Ansible-workshop-ch/bootcamp/blob/main/lab/bonus/creating-ec2-with-ansible.md)

Playbook:

[ec2_exact_count.yml](https://github.com/Ansible-workshop-ch/bootcamp/blob/main/lab/playbooks/ec2_exact_count.yml)

The main teaching point is the difference between:

```yaml
count: 1
```

and:

```yaml
exact_count: 1
```

`count: 1` can create a new instance every time the playbook runs if used carelessly.

`exact_count: 1` is safer for repeatable automation because it tells Ansible:

```text
Make sure exactly one matching instance exists.
```

This bonus lab uses:

```yaml
hosts: localhost
connection: local
```

That means Ansible is not configuring a remote host yet. It is running locally and calling the AWS API to create or manage infrastructure.

> Warning: This lab can create real AWS resources. Use only in a safe AWS sandbox or instructor-controlled demo environment.

---

## Using This with AAP

The runnable code lives in this subdirectory:

```text
bootcamp/lab/
```

When creating the AAP project against this Git repo, make sure AAP runs from the lab directory or uses the correct `ansible.cfg`.

A job template should reference playbooks such as:

```text
playbooks/module6_role_apply.yml
```

or:

```text
playbooks/module9_final_usecase.yml
```

For the EC2 bonus demo, the job template can reference:

```text
playbooks/ec2_exact_count.yml
```

