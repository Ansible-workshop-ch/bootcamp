# Ansible & AAP Bootcamp

A hands-on, **code-first** course for Charter teams — especially Puppet engineers — moving into **Ansible** and **Ansible Automation Platform (AAP 2.6)**. CLI first, AAP second. Linux-focused.

Every module follows the same shape: **Definition → Diagram/Workflow → Hands-On Walkthrough → Quiz → Hands-On Lab.** Architecture diagrams are Mermaid and render automatically on GitHub. Put any AAP UI screenshots in [`images/`](images/).

---

## How this folder is organized

```text
bootcamp/
├── README.md           ← you are here
├── orientation.md      ← read before Day 1
├── images/             ← shared screenshots (AAP UI, etc.)
├── lab/                ← the runnable Ansible code (all CLI labs use this)
│   ├── ansible.cfg
│   ├── inventories/  group_vars/  host_vars/
│   ├── playbooks/   roles/   templates/
│   └── README.md
└── module01 … module09/   ← one folder per module (the written course)
```

> **All lab commands run from `lab/`.** `cd bootcamp/lab` then run `ansible …` / `ansible-playbook …`. See [`lab/README.md`](lab/README.md).

---

## Course map

**Before Day 1:** [Orientation](orientation.md)

### Day 1 — Foundations
| Module | Topic |
|--------|-------|
| 01 | [Introduction and Architecture](module01/introduction.md) |
| 02 | [Inventory, Ad Hoc Commands, Idempotency](module02/inventory-and-idempotency.md) |
| 03 | [Playbook Basics](module03/playbook-basics.md) |

### Day 2 — Core Ansible skills
| Module | Topic |
|--------|-------|
| 04 | [Variables, Facts, group_vars, host_vars](module04/variables-and-facts.md) |
| 05 | [Conditions, Loops, Handlers, Templates](module05/conditions-loops-handlers-templates.md) |
| 06 | [Roles and Code-First Structure](module06/roles-and-code-first.md) |

### Day 3 — AAP and applied workflow
| Module | Topic |
|--------|-------|
| 07 | [AAP Workflow for Operators](module07/aap-workflow.md) |
| 08 | [AAP Inventories, Schedules, Surveys, Troubleshooting](module08/aap-inventories-surveys-troubleshooting.md) |
| 09 | [Final Charter-Style Use Case](module09/final-use-case.md) |

---

## Playbook ↔ module reference (all under `lab/playbooks/`)

| Playbook | Module |
|----------|--------|
| `module1_ping.yml` | 01 |
| `module2_service.yml` | 02 |
| `module3_webserver.yml` | 03 |
| `module4_variables.yml` | 04 |
| `module5_template_deploy.yml` | 05 |
| `module6_role_apply.yml` | 06 |
| `module9_final_usecase.yml` | 09 |

## Scope

**Emphasized:** CLI, inventory, ad hoc commands, playbooks, variables, facts, `group_vars`/`host_vars`, conditions, loops, templates, handlers, roles, Git structure, AAP projects/inventories/job templates/output, troubleshooting, code-first workflow.

**Kept light:** Windows/WinRM/PowerShell, AWS, deep NetBox, HashiCorp Vault / CyberArk, AAP installation, execution environment creation, deep Puppet-to-Ansible comparison.
