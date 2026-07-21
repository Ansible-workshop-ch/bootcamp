# Ansible & AAP Bootcamp

A hands-on, **code-first** course for Charter teams — especially Puppet engineers — moving into **Ansible** and **Ansible Automation Platform (AAP 2.6)**.

The course is **CLI first, Navigator second, AAP third, Linux-focused**.

Every module follows the same shape:

```text
Definition → Diagram/Workflow → Hands-On Walkthrough → Quiz → Hands-On Lab
```

Architecture diagrams are written in Mermaid and render automatically on GitHub.

Put any AAP UI screenshots or supporting images in [`images/`](images/).

---

> **⚠️ Inventory group name — read before Day 2 afternoon**
>
> Modules 5 through 9 all target `hosts: linux` and load `group_vars/linux.yml`. The lab inventory below does not yet have a `linux` group — only `ubuntu_web`, `rhel_web`, and `web`. Add one before Module 5, either by:
>
> ```ini
> [linux:children]
> ubuntu_web
> rhel_web
> ```
>
> and creating `group_vars/linux.yml`, or by renaming `web` → `linux` throughout. Without this, every playbook from Module 5 onward fails with "no hosts matched."

---

## How this folder is organized

```text
bootcamp/
├── README.md                    ← you are here
├── orientation.md               ← read before Day 1
├── ansible.cfg                  ← repo-root config, used when AAP syncs the project
├── images/                      ← shared screenshots and icons
├── lab/                         ← runnable Ansible code for CLI and Navigator labs
│   ├── README.md
│   ├── ansible.cfg
│   ├── ansible-navigator.yml    ← Navigator settings (Module 9 onward)
│   ├── artifacts/               ← saved Navigator playbook artifacts (gitignored)
│   ├── inventories/
│   │   ├── inventory.ini
│   │   └── group_vars/
│   │       ├── all.yml
│   │       ├── web.yml
│   │       ├── linux.yml        ← required from Module 5 onward, see warning above
│   │       ├── ubuntu_web.yml
│   │       └── rhel_web.yml
│   ├── playbooks/
│   │   ├── module1_ping.yml
│   │   ├── module2_service.yml
│   │   ├── module3_webserver.yml
│   │   ├── module4_variables.yml
│   │   ├── module5_template_deploy.yml
│   │   ├── module6_role_apply.yml
│   │   ├── module8_operator_workflow.yml
│   │   ├── module9_simulate_alert.yml
│   │   ├── module9_final_usecase.yml
│   │   └── ec2_exact_count.yml
│   ├── roles/
│   │   ├── web_config/              ← Module 6, built with `ansible-galaxy role init`
│   │   └── service_remediation/     ← Module 9, incident-remediation role
│   ├── templates/
│   │   ├── index.html.j2
│   │   └── motd.j2
│   └── bonus/
│       ├── netbox-source-of-truth.md
│       ├── creating-ec2-with-ansible.md
│       ├── conditions.yml
│       ├── loops.yml
│       ├── cleanup.yaml
│       └── podman_machine_provision.yaml
└── module01 … module09/          ← one folder per written module
```

> **All lab commands run from `lab/`.**
> Run `cd bootcamp/lab` before using `ansible`, `ansible-playbook`, or `ansible-navigator`.

See [`lab/README.md`](lab/README.md) for runnable lab details.

---

## Scope

|                            |                           |                 |                     |
| -------------------------- | ------------------------- | --------------- | ------------------- |
| CLI-first Ansible workflow | Inventory                 | Ad hoc commands | Playbooks           |
| Variables                  | Facts                     | `group_vars`    | Conditions          |
| Loops                      | Templates                 | Handlers        | Roles               |
| Git-based structure        | Multi-OS Linux automation | Ansible Navigator | Execution environments (local) |
| AAP projects               | AAP inventories            | AAP job templates | AAP surveys        |
| AAP schedules              | AAP job output             | Troubleshooting  | Code-first workflow |

---

## Course map

**Before Day 1:** [Orientation](orientation.md)

---

### Day 1 — Foundations

| Module | Topic                                                                            |
| ------ | ---------------------------------------------------------------------------------- |
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
| 09     | [Final Charter-Style Use Case with Ansible Navigator](module09/final-use-case.md)                          |

Module 9 switches the primary local command from `ansible-playbook` to `ansible-navigator` for the rest of the course, and introduces running the same automation through an AAP job template with a survey.

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

linux            ← required from Module 5 onward; see warning at top of this file
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

Note that `common_packages` in both files already includes the web package itself (`apache2` / `httpd`). This is intentional — see Module 5 for why.

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

| Playbook                          | Module |
| ---------------------------------- | ------ |
| `module1_ping.yml`                 | 01     |
| `module2_service.yml`              | 02     |
| `module3_webserver.yml`            | 03     |
| `module4_variables.yml`            | 04     |
| `module5_template_deploy.yml`      | 05     |
| `module6_role_apply.yml`           | 06     |
| `module8_operator_workflow.yml`    | 08     |
| `module9_simulate_alert.yml`       | 09 (creates the training failure) |
| `module9_final_usecase.yml`        | 09 (remediation, run via Navigator, then AAP) |

---

## Roles

| Role                   | Introduced in | Purpose                                                              |
| ----------------------- | -------------- | --------------------------------------------------------------------- |
| `web_config`            | Module 06      | Installs, configures, and starts the web server across OS families    |
| `service_remediation`   | Module 09      | Validates and remediates a stopped web service; drives the final use case |

Both roles follow the standard layout (`tasks/`, `handlers/`, `templates/`, `defaults/`, `vars/`, `meta/`, `README.md`) and were generated with:

```bash
ansible-galaxy role init <role_name> --init-path roles
```

---

## Ansible Navigator

Starting in Module 9, [`lab/ansible-navigator.yml`](lab/ansible-navigator.yml) configures:

* stdout mode for classroom-friendly output
* a local execution environment (with `--ee false` as a fallback if Podman/Docker isn't available)
* saved playbook artifacts under `lab/artifacts/` (not committed — see `.gitignore`)

Command mapping from the traditional CLI:

| Traditional command | Navigator command |
| --------------------- | -------------------- |
| `ansible-playbook`    | `ansible-navigator run` |
| `ansible-inventory`   | `ansible-navigator inventory` |
| `ansible-doc`         | `ansible-navigator doc` |
| —                     | `ansible-navigator config` |
| —                     | `ansible-navigator exec` |
| —                     | `ansible-navigator images` |
| —                     | `ansible-navigator replay` |
| —                     | `ansible-navigator settings` |

---

## Bonus labs

Bonus labs are optional. They are useful for instructor demos, advanced discussion, or extra practice if time allows.

### NetBox Source of Truth

Main idea:

```text
NetBox tells Ansible what exists.
Ansible performs the automation.
AAP controls and runs the automation.
```

Bonus lesson: [`lab/bonus/netbox-source-of-truth.md`](lab/bonus/netbox-source-of-truth.md)

---

### EC2 Creation with Ansible

Shows that Ansible can create cloud infrastructure directly, such as AWS EC2 instances.

Lesson: [Creating EC2 with Ansible](https://github.com/Ansible-workshop-ch/bootcamp/blob/main/lab/bonus/creating-ec2-with-ansible.md)

Playbook: [ec2_exact_count.yml](https://github.com/Ansible-workshop-ch/bootcamp/blob/main/lab/playbooks/ec2_exact_count.yml)

The key lesson is the difference between:

```yaml
count: 1
```

and:

```yaml
exact_count: 1
```

`count: 1` can create a new instance every time the playbook runs if used carelessly. `exact_count: 1` is safer for repeatable automation because it tells Ansible: *make sure exactly one matching instance exists.*

This bonus playbook uses:

```yaml
hosts: localhost
connection: local
```

That means Ansible is running locally and calling the AWS API — it is not connecting to an existing managed host yet.

> **Warning:** This lab can create real AWS resources. Use only in a safe AWS sandbox or instructor-controlled demo environment. Requires AWS credentials configured ahead of time (`aws configure`, environment variables, or SSO) — never hardcode keys in the playbook.

---

### Conditions warm-up

A standalone, simpler condition demo (OS-family install logic) that can run before Module 5 as a lighter warm-up.

Playbook: [`lab/bonus/conditions.yml`](lab/bonus/conditions.yml)

---

### Loops deep dive

Self-contained loop demo: creates demo users and files via loop, with a built-in cleanup switch and an optional package-loop toggle.

Playbook: [`lab/bonus/loops.yml`](lab/bonus/loops.yml)

```bash
# create the demo users/files
ansible-playbook playbooks/loops.yml

# clean them up
ansible-playbook playbooks/loops.yml -e "loop_user_state=absent"

# also install common_packages via loop
ansible-playbook playbooks/loops.yml -e "install_loop_packages=true"
```

---

### Lab cleanup

Removes `apache2`/`httpd` and the loop-demo text files created during the Day 2 labs. Useful to reset containers between cohorts or at the end of the bootcamp.

Playbook: [`lab/bonus/cleanup.yaml`](lab/bonus/cleanup.yaml)

---

### Podman machine provisioning

Two-play pattern: provision a local Podman container first, then configure it in a second play using `add_host` for in-memory inventory. An AWS-free alternative to the EC2 bonus lab — good for demonstrating "provision, then configure."

Playbook: [`lab/bonus/podman_machine_provision.yaml`](lab/bonus/podman_machine_provision.yaml)

Requires a pre-built Podman image (e.g. `ansible-target-rocky`) and an SSH keypair at `~/.ssh/ansible_lab_key` set up ahead of time.

---
