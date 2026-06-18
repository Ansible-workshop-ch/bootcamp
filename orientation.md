# Pre-Training Orientation

> Light orientation only. Read or skim before Day 1 so the first morning is not spent on setup confusion. This is **not** full training.

## 1. What is Ansible?

Ansible is an automation tool that runs tasks across one or many systems. It needs **no agent** on the managed Linux hosts — it connects over SSH, reads an **inventory** of target machines, and runs **modules** to make changes or collect information.

## 2. What is AAP?

Ansible Automation Platform (AAP) is the enterprise platform that runs, schedules, and controls Ansible automation. Instead of every engineer running playbooks from a laptop, AAP pulls code from Git and runs it through **job templates** against shared **inventories**, with logged output. Target version for this class: **AAP 2.6 sandbox**.

## 3. How the Git repo is structured

```text
charter-ansible-training/
  ansible.cfg            # points Ansible at the inventory + roles
  inventories/           # who we manage
  group_vars/            # variables shared by a group of hosts
  host_vars/             # variables for one specific host
  playbooks/             # one playbook per module
  roles/                 # reusable automation (web_config)
  templates/             # Jinja2 templates
  docs/                  # the module you are reading now
```

## 4. How to clone the repo

```bash
git clone <training-repo-url> charter-ansible-training
cd charter-ansible-training
```

## 5. What a basic playbook looks like

```yaml
---
- name: Basic web server setup
  hosts: web
  become: true
  tasks:
    - name: Install httpd
      ansible.builtin.package:
        name: httpd
        state: present
```

## 6. What an AAP job template looks like

A job template ties together: a **project** (the Git repo), an **inventory** (the targets), a **credential** (how to log in), and a **playbook** (what to run). You press **Launch**, AAP runs the playbook, and you read the **output**.

## 7. How to confirm access before class

Before Day 1, confirm you can:

- [ ] Open a Linux terminal and run `ssh`
- [ ] Reach the VPN / pass MFA (if required)
- [ ] `git clone` the training repo
- [ ] Log into the **AAP 2.6 sandbox** web UI
- [ ] Log into the assigned **lab host**

If any box is unchecked, raise it before Day 1 — not during it.

## Prerequisites recap

Basic Linux CLI, basic SSH, basic Git (cloning). **No Ansible experience required.**
