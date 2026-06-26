# Ansible & AAP Bootcamp

A hands-on, **code-first** course for Charter teams — especially Puppet engineers — moving into **Ansible** and **Ansible Automation Platform (AAP 2.6)**.

The course is **CLI first, AAP second, Linux-focused**.

Every module follows the same shape:

```text
Definition → Diagram/Workflow → Hands-On Walkthrough → Quiz → Hands-On Lab
```

Architecture diagrams are written in Mermaid and render automatically on GitHub.

Put any AAP UI screenshots or supporting images in [`images/`](images/).

---

## How this folder is organized

```text
bootcamp/
├── README.md              ← you are here
├── orientation.md         ← read before Day 1
├── images/                ← shared screenshots and icons
├── lab/                   ← runnable Ansible code for CLI labs
│   ├── README.md
│   ├── ansible.cfg
│   ├── inventories/
│   │   ├── inventory.ini
│   │   └── group_vars/
│   │       ├── all.yml
│   │       ├── web.yml
│   │       ├── ubuntu_web.yml
│   │       └── rhel_web.yml
│   ├── playbooks/
│   │   ├── module1_ping.yml
│   │   ├── module2_service.yml
│   │   ├── module3_webserver.yml
│   │   ├── module4_variables.yml
│   │   ├── module5_template_deploy.yml
│   │   ├── module6_role_apply.yml
│   │   ├── module9_final_usecase.yml
│   │   └── ec2_exact_count.yml
│   ├── roles/
│   │   └── web_config/
│   ├── templates/
│   │   ├── index.html.j2
│   │   └── motd.j2
│   └── bonus/
│       ├── netbox-source-of-truth.md
│       └── creating-ec2-with-ansible.md
└── module01 … module09/   ← one folder per written module
```

> **All lab commands run from `lab/`.**
> Run `cd bootcamp/lab` before using `ansible` or `ansible-playbook`.

See [`lab/README.md`](lab/README.md) for runnable lab details.

---

## Course map

**Before Day 1:** [Orientation](orientation.md)

---

### Day 1 — Foundations

| Module | Topic                                                                            |
| ------ | -------------------------------------------------------------------------------- |
| 01     | [Introduction and Architecture](module01/introduction.md)                        |
| 02     | [Inventory, Ad Hoc Commands, Idempotency](module02/inventory-and-idempotency.md) |
| 03     | [Playbook Basics](module03/playbook-basics.md)                                   |

---

### Day 2 — Core Ansible Skills

| Module | Topic                                                                                     |
| ------ | ----------------------------------------------------------------------------------------- |
| 04     | [Variables, Facts, group_vars, host_vars](module04/variables-and-facts.md)                |
| 05     | [Conditions, Loops, Handlers, Templates](module05/conditions-loops-handlers-templates.md) |
| 06     | [Roles and Code-First Structure](module06/roles-and-code-first.md)                        |

---

### Day 3 — AAP and Applied Workflow

| Module | Topic                                                                                                       |
| ------ | ----------------------------------------------------------------------------------------------------------- |
| 07     | [AAP Workflow for Operators](module07/aap-workflow.md)                                                      |
| 08     | [AAP Inventories, Schedules, Surveys, Troubleshooting](module08/aap-inventories-surveys-troubleshooting.md) |
| 09     | [Final Charter-Style Use Case](module09/final-use-case.md)                                                  |

---

## Lab inventory model

The lab supports multiple Linux OS families.

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

This allows the course to teach a real-world Ansible pattern:

```text
Same playbook.
Different operating systems.
Different package and service names.
Group variables make the playbook reusable.
```

Ubuntu/Debian hosts use:

```text
Package: apache2
Service: apache2
```

Rocky/RHEL hosts use:

```text
Package: httpd
Service: httpd
```

The values live in:

```text
lab/inventories/group_vars/ubuntu_web.yml
lab/inventories/group_vars/rhel_web.yml
```

Playbooks should use variables instead of hardcoded package names:

```yaml
name: "{{ package_name }}"
```

```yaml
name: "{{ service_name }}"
```

---

## Playbook ↔ module reference

All playbooks are under [`lab/playbooks/`](lab/playbooks/).

| Playbook                      | Module |
| ----------------------------- | ------ |
| `module1_ping.yml`            | 01     |
| `module2_service.yml`         | 02     |
| `module3_webserver.yml`       | 03     |
| `module4_variables.yml`       | 04     |
| `module5_template_deploy.yml` | 05     |
| `module6_role_apply.yml`      | 06     |
| `module9_final_usecase.yml`   | 09     |

---

## Bonus labs

Bonus labs are optional. They are useful for instructor demos, advanced discussion, or extra practice if time allows.

---

### NetBox Source of Truth

NetBox is introduced as a source-of-truth concept.

Main idea:

```text
NetBox tells Ansible what exists.
Ansible performs the automation.
AAP controls and runs the automation.
```

Bonus lesson:

```text
lab/bonus/netbox-source-of-truth.md
```

Use this to explain how real environments can move beyond static inventory.

---

### EC2 Creation with Ansible

This bonus lesson shows that Ansible can create cloud infrastructure directly, such as AWS EC2 instances.

Lesson:

[Creating EC2 with Ansible](https://github.com/Ansible-workshop-ch/bootcamp/blob/main/lab/bonus/creating-ec2-with-ansible.md)

Playbook:

[ec2_exact_count.yml](https://github.com/Ansible-workshop-ch/bootcamp/blob/main/lab/playbooks/ec2_exact_count.yml)

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

This bonus playbook uses:

```yaml
hosts: localhost
connection: local
```

That means Ansible is running locally and calling the AWS API. It is not connecting to an existing managed host yet.

> Warning: This lab can create real AWS resources. Use only in a safe AWS sandbox or instructor-controlled demo environment.

---

## Scope

### Emphasized

|                            |                           |                 |                     |
| -------------------------- | ------------------------- | --------------- | ------------------- |
| CLI-first Ansible workflow | Inventory                 | Ad hoc commands | Playbooks           |
| Variables                  | Facts                     | `group_vars`    | Conditions          |
| Loops                      | Templates                 | Handlers        | Roles               |
| Git-based structure        | Multi-OS Linux automation | AAP projects    | AAP inventories     |
| AAP job templates          | AAP job output            | Troubleshooting | Code-first workflow |

---

### Kept light

* Windows / WinRM / PowerShell
* Deep Puppet-to-Ansible migration
* HashiCorp Vault / CyberArk
* AAP installation
* Execution environment creation

AWS and NetBox are included as **bonus topics**, not required beginner labs.

---


