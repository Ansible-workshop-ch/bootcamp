# Puppet to Ansible Migration Training

## 24-Hour Instructor-Led Enablement

---

# Overview

This course is designed for organizations migrating from Puppet to Ansible where students have:

* Zero Ansible experience
* Zero YAML experience
* Existing infrastructure or systems administration knowledge
* Existing Puppet operational experience

The course is beginner-friendly while still aligned to enterprise operational practices.

---

# Course Goals

By the end of this course, students will be able to:

* Understand Ansible architecture
* Understand YAML fundamentals
* Install and configure Ansible
* Build inventories and playbooks
* Automate Linux administration tasks
* Replace common Puppet workflows with Ansible
* Create reusable automation using roles
* Secure secrets using Ansible Vault
* Troubleshoot playbook failures
* Use Git for automation workflows
* Follow enterprise Ansible best practices

---

# Course Format

| Item           | Details                     |
| -------------- | --------------------------- |
| Total Duration | 24 Hours                    |
| Days           | 3 Days                      |
| Daily Duration | 8 Hours                     |
| Delivery       | Instructor-Led              |
| Style          | Hands-On                    |
| Audience       | Puppet Users New to Ansible |

---

# Day 0 - Pre-Requisites & Environment Preparation

---

# Student Requirements

## Workstation Requirements

Students should have:

* Windows, macOS, or Linux workstation
* VSCode installed
* Stable internet connection
* SSH client
* Browser access

---

# Lab Environment Requirements

## Recommended Lab Architecture

### Control Node

1 Linux VM per student with:

* Ansible installed
* Python 3 installed
* Git installed
* SSH configured

### Managed Nodes

2-3 Linux VMs per student with:

* SSH enabled
* Sudo access
* Connectivity to control node

---

# Optional Enterprise Components

Optional additions depending on customer scope:

* GitHub or GitLab repository
* Ansible Automation Platform
* Vault integration
* CI/CD integration
* Windows hosts
* Network devices

---

# Accounts & Access

Students should have:

* SSH credentials
* Sudo permissions
* Git repository access
* Lab guide access

---

# Software Requirements

| Tool     | Purpose            |
| -------- | ------------------ |
| Ansible  | Automation Engine  |
| Python 3 | Ansible Dependency |
| Git      | Version Control    |
| VSCode   | YAML Editing       |
| SSH      | Remote Access      |

---

# Day 1 (8 Hours)

# Foundations, YAML, and First Automation

---

# Module 1 - Introduction to Ansible

## Duration

1 Hour

---

## Definition

Topics Covered:

* What is Ansible
* Why organizations migrate from Puppet to Ansible
* Agentless automation
* Push vs Pull architecture
* Infrastructure as Code
* Idempotency
* Control node vs managed nodes

---

## Puppet vs Ansible Comparison

| Puppet           | Ansible        |
| ---------------- | -------------- |
| Agent-based      | Agentless      |
| Pull model       | Push model     |
| Puppet manifests | YAML playbooks |
| Puppet Master    | Control Node   |
| DSL language     | YAML           |

---

## Hands-On Walkthrough

Instructor demonstrates:

* Ansible architecture
* Control node communication
* Running first automation commands
* Inventory overview

---

## Hands-On Lab

Students perform:

```bash
ansible all -m ping
```

```bash
ansible all -m setup
```

Tasks:

* Validate connectivity
* Gather system facts
* Explore output

---

## Quiz

### Question 1

What communication method does Ansible commonly use?

A. FTP
B. SSH
C. SMTP
D. SNMP

Answer: B

---

### Question 2

Does Ansible require agents installed on Linux hosts?

A. Yes
B. No

Answer: B

---

### Question 3

What system runs the Ansible commands?

A. Puppet Master
B. Control Node
C. Worker Node
D. Relay Server

Answer: B

---

# Module 2 - YAML Fundamentals

## Duration

1.5 Hours

---

## Definition

Topics Covered:

* What YAML is
* YAML syntax
* Indentation rules
* Lists
* Dictionaries
* Variables
* Common formatting mistakes

---

## Hands-On Walkthrough

Instructor demonstrates:

* Reading YAML
* Writing YAML
* Fixing indentation issues
* Understanding lists and key/value pairs

---

## Hands-On Lab

Students create YAML examples:

```yaml
users:
  - ray
  - admin
```

```yaml
package: nginx
service: httpd
```

---

## Quiz

### Question 1

Is YAML indentation important?

A. Yes
B. No

Answer: A

---

### Question 2

What symbol defines a list item?

A. *
B. -
C. &
D. #

Answer: B

---

### Question 3

Which syntax is valid YAML?

A.

```yaml
name=ray
```

B.

```yaml
name: ray
```

Answer: B

---

# Module 3 - Installing Ansible & Building Inventory

## Duration

2 Hours

---

## Definition

Topics Covered:

* Installing Ansible
* Inventory files
* Host groups
* SSH authentication
* Static inventory structure

---

## Hands-On Walkthrough

Instructor demonstrates:

* Installing Ansible
* Creating inventory file
* Testing SSH access
* Organizing hosts into groups

---

## Hands-On Lab

Students build:

```ini
[web]
server1
server2
```

Tasks:

* Create inventory.ini
* Test host communication
* Validate inventory structure

---

## Quiz

### Question 1

What file commonly stores managed hosts?

A. inventory.ini
B. package.json
C. Dockerfile
D. manifest.pp

Answer: A

---

### Question 2

What protocol does Ansible commonly use?

A. HTTP
B. SSH
C. FTP
D. LDAP

Answer: B

---

### Question 3

What command validates connectivity?

A. ansible ping
B. ansible all -m ping
C. ansible-connect
D. ssh-check

Answer: B

---

# Module 4 - Ad Hoc Commands

## Duration

1 Hour

---

## Definition

Topics Covered:

* Ad hoc automation
* Modules
* Shell vs command
* One-line administration tasks
* Gathering information remotely

---

## Hands-On Walkthrough

Instructor demonstrates:

* Running commands remotely
* Installing packages
* Checking uptime
* Managing services

---

## Hands-On Lab

Students execute:

```bash
ansible all -m command -a "uptime"
```

```bash
ansible all -m yum -a "name=httpd state=present" -b
```

---

## Quiz

### Question 1

What does -m represent?

A. Module
B. Machine
C. Memory
D. Manifest

Answer: A

---

### Question 2

What module runs Linux commands?

A. shell
B. network
C. cloud
D. sshd

Answer: A

---

### Question 3

What does -b commonly represent?

A. Build
B. Become
C. Backup
D. Bash

Answer: B

---

# Module 5 - First Playbook

## Duration

2.5 Hours

---

## Definition

Topics Covered:

* Playbook structure
* Tasks
* Modules
* State management
* Idempotency
* YAML in playbooks

---

## Hands-On Walkthrough

Instructor builds first playbook:

```yaml
- hosts: web
  become: true
  tasks:
    - name: Install nginx
      yum:
        name: nginx
        state: present
```

---

## Hands-On Lab

Students:

* Create playbook
* Install package
* Start service
* Validate service status

---

## Quiz

### Question 1

What file extension is commonly used for playbooks?

A. .pp
B. .yaml
C. .json
D. .py

Answer: B

---

### Question 2

What does idempotent mean?

A. Unsafe to rerun
B. Safe to rerun repeatedly
C. Requires reboot
D. Deletes configurations

Answer: B

---

### Question 3

What section contains automation actions?

A. tasks
B. vars
C. hosts
D. inventory

Answer: A

---

# Day 2 (8 Hours)

# Variables, Templates, Roles, and Migration Concepts

---

# Module 6 - Variables & Facts

## Duration

2 Hours

---

## Definition

Topics Covered:

* Variables
* Facts
* Registered variables
* Host vars
* Group vars
* Dynamic values

---

## Hands-On Walkthrough

Instructor demonstrates:

* Creating variables
* Using gathered facts
* Dynamic configuration
* Conditional logic

---

## Hands-On Lab

Students:

* Create variables
* Print hostnames
* Use ansible_facts
* Register command output

---

## Quiz

### Question 1

What are gathered system details called in Ansible?

A. Metadata
B. Facts
C. Tokens
D. Records

Answer: B

---

### Question 2

Where can variables be stored?

A. group_vars
B. host_vars
C. playbooks
D. All of the above

Answer: D

---

### Question 3

What keyword stores task output?

A. save
B. register
C. export
D. capture

Answer: B

---

# Module 7 - Templates & Jinja2

## Duration

2 Hours

---

## Definition

Topics Covered:

* Dynamic configuration files
* Jinja2 basics
* Variables inside templates
* Template deployment
* Service restarts

---

## Hands-On Walkthrough

Instructor demonstrates:

* Creating templates
* Deploying configuration files
* Using variables dynamically

---

## Hands-On Lab

Students:

* Build nginx template
* Deploy config file
* Restart services automatically

---

## Quiz

### Question 1

What templating engine does Ansible use?

A. Helm
B. Jinja2
C. PythonScript
D. Puppet DSL

Answer: B

---

### Question 2

What module deploys templates?

A. template
B. copy
C. config
D. push

Answer: A

---

### Question 3

What syntax references variables in Jinja2?

A. << variable >>
B. {{ variable }}
C. (( variable ))
D. [[ variable ]]

Answer: B

---

# Module 8 - Roles

## Duration

2 Hours

---

## Definition

Topics Covered:

* Reusable automation
* Standard role structure
* Defaults
* Tasks
* Templates
* Files
* Handlers

---

## Hands-On Walkthrough

Instructor demonstrates:

* Creating role structure
* Converting playbook into reusable role
* Organizing enterprise automation

---

## Hands-On Lab

Students:

* Build apache role
* Add templates
* Reuse role across multiple hosts

---

## Quiz

### Question 1

Why are roles useful?

A. Reusability
B. Standardization
C. Organization
D. All of the above

Answer: D

---

### Question 2

What directory stores templates in a role?

A. vars
B. handlers
C. templates
D. inventory

Answer: C

---

### Question 3

What file commonly starts a role's tasks?

A. tasks/main.yml
B. role.yaml
C. inventory.ini
D. defaults.ini

Answer: A

---

# Module 9 - Puppet to Ansible Migration Workshop

## Duration

2 Hours

---

## Definition

Topics Covered:

* Puppet manifests vs Ansible playbooks
* Puppet classes vs Ansible roles
* Migration strategies
* Common translation patterns
* Enterprise operational differences

---

## Hands-On Walkthrough

Instructor demonstrates:

* Converting Puppet examples into Ansible
* Migrating package installs
* Migrating service management
* Migrating users and permissions

---

## Hands-On Lab

Students migrate:

* Package installation
* User creation
* Service configuration
* Config deployment

---

## Quiz

### Question 1

What Ansible concept most closely resembles Puppet classes?

A. Roles
B. Templates
C. Facts
D. Variables

Answer: A

---

### Question 2

What language does Ansible primarily use?

A. Ruby DSL
B. YAML
C. XML
D. JSON

Answer: B

---

### Question 3

Is Ansible push or pull based by default?

A. Pull
B. Push

Answer: B

---

# Day 3 (8 Hours)

# Enterprise Automation, Security, and Troubleshooting

---

# Module 10 - Ansible Vault

## Duration

1.5 Hours

---

## Definition

Topics Covered:

* Secret management
* Encrypting passwords
* Protecting sensitive variables
* Vault workflows
* Enterprise secret handling

---

## Hands-On Walkthrough

Instructor demonstrates:

* Creating encrypted vault files
* Encrypting variables
* Running vault-enabled playbooks

---

## Hands-On Lab

Students:

* Encrypt passwords
* Create vault file
* Use encrypted secrets in playbooks

---

## Quiz

### Question 1

What feature secures sensitive data in Ansible?

A. SSH
B. Vault
C. Inventory
D. Handlers

Answer: B

---

### Question 2

Can Ansible Vault encrypt variables?

A. Yes
B. No

Answer: A

---

### Question 3

What command edits encrypted vault content?

A. ansible-vault edit
B. vault-edit
C. ansible edit
D. ansible-secret

Answer: A

---

# Module 11 - Error Handling & Troubleshooting

## Duration

2 Hours

---

## Definition

Topics Covered:

* Debugging playbooks
* Common failures
* YAML troubleshooting
* SSH troubleshooting
* Verbose logging
* Blocks and rescue

---

## Hands-On Walkthrough

Instructor intentionally breaks:

* YAML formatting
* SSH authentication
* Service configurations
* Variables

Students troubleshoot failures live.

---

## Hands-On Lab

Students troubleshoot:

* Broken playbooks
* YAML syntax errors
* Permissions issues
* Failed tasks
* Inventory mistakes

---

## Quiz

### Question 1

What option increases verbosity?

A. -v
B. -debug
C. -trace
D. -log

Answer: A

---

### Question 2

What is the most common YAML issue?

A. Networking
B. Indentation
C. DNS
D. SSH keys

Answer: B

---

### Question 3

What module helps display variables and output?

A. print
B. debug
C. shell
D. facts

Answer: B

---

# Module 12 - Git & Automation Workflow

## Duration

1.5 Hours

---

## Definition

Topics Covered:

* Git fundamentals
* Version control
* Tracking automation changes
* Collaboration workflows
* Enterprise automation repositories

---

## Hands-On Walkthrough

Instructor demonstrates:

* Git repository creation
* Committing changes
* Cloning repositories
* Managing automation versions

---

## Hands-On Lab

Students:

* Initialize Git repository
* Commit playbooks
* Push updates
* Track changes

---

## Quiz

### Question 1

Why use Git with Ansible?

A. Version control
B. Collaboration
C. Change tracking
D. All of the above

Answer: D

---

### Question 2

What command uploads local commits?

A. git upload
B. git push
C. git send
D. git deploy

Answer: B

---

### Question 3

What command creates a local repository?

A. git start
B. git init
C. git create
D. git new

Answer: B

---

# Module 13 - Ansible Best Practices

## Duration

1 Hour

---

## Definition

Topics Covered:

* Naming conventions
* Directory structures
* Reusable automation
* Security best practices
* Enterprise repository standards
* Documentation practices

---

## Hands-On Walkthrough

Instructor reviews:

* Enterprise repository examples
* Good vs bad practices
* Automation organization

---

## Hands-On Lab

Students:

* Clean existing repository
* Reorganize automation structure
* Improve readability

---

## Quiz

### Question 1

Why should automation be standardized?

A. Easier maintenance
B. Better collaboration
C. Improved troubleshooting
D. All of the above

Answer: D

---

### Question 2

What improves automation readability?

A. Good naming conventions
B. Random file names
C. Flat structures
D. Removing comments

Answer: A

---

### Question 3

Should secrets be hardcoded in playbooks?

A. Yes
B. No

Answer: B

---

# Module 14 - Final Capstone Project

## Duration

2 Hours

---

## Objective

Students build a complete automation workflow independently.

---

## Capstone Requirements

Students automate:

* Package installation
* User creation
* Service deployment
* Template deployment
* Variables
* Role usage
* Vault integration

---

## Final Hands-On

Students work independently with instructor guidance.

Instructor validates:

* Inventory structure
* YAML formatting
* Playbook execution
* Reusability
* Security handling

---

# Final Assessment

15-question assessment covering:

* YAML
* Inventories
* Playbooks
* Variables
* Templates
* Roles
* Vault
* Troubleshooting
* Puppet migration concepts

---

# Deliverables

Students receive:

* Slide deck
* Lab guide
* Example playbooks
* Example roles
* Migration examples
* Git repository
* Cheat sheets
* Troubleshooting guide

---

# Recommended Follow-Up Training

## Advanced Tracks

* Ansible Automation Platform
* Event-Driven Ansible
* Windows Automation
* Network Automation
* CI/CD Integration
* Ansible + Terraform
* Kubernetes/OpenShift Automation
* Enterprise Automation Governance
