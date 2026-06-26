# Pre-Training Orientation

> Light orientation only. Read or skim before Day 1 so the first morning is not spent on setup confusion. This is **not** full training.

---

## 1. What is Ansible?

Ansible is an automation tool that runs tasks across one or many systems.

It does **not require an agent** on managed Linux hosts. Instead, Ansible usually connects over SSH, reads an **inventory** of target machines, and runs **modules** to make changes or collect information.

In simple terms:

```text
Inventory tells Ansible where to run.
Modules tell Ansible what to do.
Playbooks make the automation reusable.
```

---

## 2. What is AAP?

Ansible Automation Platform, or **AAP**, is the enterprise platform for running, scheduling, controlling, and auditing Ansible automation.

Instead of every engineer running playbooks from a laptop, AAP can:

* Pull automation code from Git
* Use shared inventories
* Use stored credentials
* Run job templates
* Show job output
* Track who launched what
* Provide a controlled automation workflow

Target version for this class:

```text
AAP 2.6 sandbox
```

---

## 3. How the Git repo is structured

The training repo contains both the course content and the hands-on lab files.

High-level structure:

```text
bootcamp/
  README.md
  orientation.md

  module01/
  module02/
  module03/
  module04/
  module05/
  module06/
  module07/
  module08/
  module09/

  lab/
    README.md
    ansible.cfg
    inventories/
      inventory.ini
      group_vars/
        all.yml
        web.yml
        ubuntu_web.yml
        rhel_web.yml
    playbooks/
      module1_ping.yml
      module2_service.yml
      module3_webserver.yml
      module4_variables.yml
      module5_template_deploy.yml
      module6_role_apply.yml
      module9_final_usecase.yml
      ec2_exact_count.yml
    roles/
      web_config/
    templates/
      index.html.j2
      motd.j2
    bonus/
      netbox-source-of-truth.md
      creating-ec2-with-ansible.md
```

The most important lab folder is:

```text
bootcamp/lab/
```

Most hands-on commands should be run from there:

```bash
cd bootcamp/lab
```

---

## 4. Inventory structure

The lab inventory is located here:

```text
lab/inventories/inventory.ini
```

This inventory includes multiple groups.

Example structure:

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

```

Why this matters:

* `ubuntu_web` targets Ubuntu-based containers
* `rhel_web` targets Rocky/RHEL-based containers
* `web` targets all web servers
* `linux` targets all Linux hosts

This lets us teach a real-world Ansible pattern:

```text
Same playbook.
Different operating systems.
Different package and service names.
Group variables make the playbook reusable.
```

---

## 5. Variables in this lab

Inventory variables live under:

```text
lab/inventories/group_vars/
```

Important files:

```text
all.yml          # variables for all hosts
web.yml          # variables shared by all web hosts
ubuntu_web.yml   # Ubuntu-specific variables
rhel_web.yml     # Rocky/RHEL-specific variables
```

Example:

```yaml
# inventories/group_vars/ubuntu_web.yml
---
package_name: apache2
service_name: apache2
```

```yaml
# inventories/group_vars/rhel_web.yml
---
package_name: httpd
service_name: httpd
```

This allows one playbook to work across different Linux distributions.

Ubuntu/Debian uses:

```text
Package: apache2
Service: apache2
```

Rocky/RHEL uses:

```text
Package: httpd
Service: httpd
```

---

## 6. How to clone the repo

```bash
git clone <training-repo-url> bootcamp
cd bootcamp
```

For lab commands:

```bash
cd lab
```

---

## 7. What a basic playbook looks like

A playbook is a YAML file that describes automation steps.

Example:

```yaml
---
- name: Basic web server setup
  hosts: web
  become: true

  tasks:
    - name: Install the web package
      ansible.builtin.package:
        name: "{{ package_name }}"
        state: present

    - name: Start the web service
      ansible.builtin.service:
        name: "{{ service_name }}"
        state: started
```

This playbook does not hardcode `apache2` or `httpd`.

Instead, it uses variables:

```text
package_name
service_name
```

The correct values come from the inventory group variables.

That is what makes the playbook reusable across Ubuntu and Rocky/RHEL hosts.

---

## 8. What an AAP job template looks like

An AAP job template ties together:

* **Project** - the Git repo
* **Inventory** - the target hosts
* **Credential** - how AAP logs into the hosts
* **Playbook** - what automation to run
* **Limit** - optional targeting filter
* **Variables** - optional runtime values

Basic flow:

```text
Git repo -> AAP Project -> Inventory -> Job Template -> Launch -> Output
```

You press **Launch**, AAP runs the playbook, and you review the job output.

---

## 9. What NetBox is in this course

NetBox is introduced as a **bonus source-of-truth topic**.

NetBox does not run automation.

NetBox stores infrastructure information such as:

* Sites
* Devices
* Virtual machines
* IP addresses
* Tags
* Roles
* Tenants
* Locations

In simple terms:

```text
NetBox tells Ansible what exists.
Ansible performs the automation.
AAP controls and runs the automation.
```

For the main beginner labs, we start with static inventory.

NetBox is introduced later as an enterprise pattern for dynamic inventory.

Reference:

[NetBox Source of Truth Bonus](https://github.com/Ansible-workshop-ch/bootcamp/blob/main/lab/bonus/netbox-source-of-truth.md)

---

## 10. What EC2 creation means in this course

EC2 creation is introduced as a **bonus cloud automation topic**.

Most of the main labs use machines that already exist:

```text
Inventory -> Existing hosts -> Ansible playbook -> Configuration
```

The EC2 bonus lab shows a different pattern:

```text
Ansible -> AWS API -> Create EC2 instance -> Configure the instance
```

This teaches that Ansible can do more than configure existing servers. It can also create infrastructure directly by calling cloud APIs.

The EC2 bonus playbook uses:

```yaml
hosts: localhost
connection: local
```

That means Ansible is running locally and calling AWS. It is not connecting to a remote managed host yet because the EC2 instance does not exist at the start of the playbook.

The key lesson is the difference between:

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

References:

* [Creating EC2 with Ansible](https://github.com/Ansible-workshop-ch/bootcamp/blob/main/lab/bonus/creating-ec2-with-ansible.md)
* [EC2 exact_count Playbook](https://github.com/Ansible-workshop-ch/bootcamp/blob/main/lab/playbooks/ec2_exact_count.yml)

> Warning: The EC2 bonus lab can create real AWS resources. Use only in a safe AWS sandbox or instructor-controlled demo environment.

---

## 11. How to confirm access before class

Before Day 1, confirm you can:

* [ ] Open a Linux terminal
* [ ] Run `ssh`
* [ ] Run `git`
* [ ] Clone the training repo
* [ ] Change into the lab directory
* [ ] Log into the AAP sandbox web UI
* [ ] Reach the assigned lab host or local lab environment
* [ ] Run basic Ansible commands if Ansible is already installed

Recommended quick checks:

```bash
ssh -V
git --version
ansible --version
```

If the local Podman lab is being used, also confirm:

```bash
podman ps
```

If the EC2 bonus lab will be demonstrated, the instructor should also confirm AWS access:

```bash
aws sts get-caller-identity
aws configure get region
```

If any item is not working, raise it before Day 1.

---

## 12. Prerequisites recap

You do **not** need previous Ansible experience.

You should be comfortable with basic:

* Linux CLI
* SSH
* Git clone
* Reading simple YAML
* Running commands from a terminal

The EC2 bonus lab is optional. It requires AWS access only if the instructor decides to run it live.

The course starts from the foundation and builds up step by step.
