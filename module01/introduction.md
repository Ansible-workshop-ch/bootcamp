# Introduction to Ansible

## Overview

In this lecture, we will get introduced to Ansible and understand why it has become one of the most widely adopted IT automation tools in the DevOps and infrastructure world.

If you are a Systems Engineer, IT Administrator, DevOps Engineer, Cloud Engineer, or anyone working in IT operations, you are likely involved in performing many repetitive tasks on a daily basis.

These tasks may include:

- Creating and sizing virtual machines
- Provisioning servers
- Applying configurations
- Patching systems
- Performing migrations
- Deploying applications
- Conducting security or compliance audits
- Managing infrastructure changes

Most of these operations involve executing hundreds of commands across hundreds of servers while maintaining the correct order of operations, service dependencies, reboots, and validations.

---

# The Traditional Way

Traditionally, engineers automate these tasks using scripts.

Examples include:
- Bash Scripts
- Python Scripts
- PowerShell Scripts

However, scripting comes with several challenges:

- Requires coding knowledge
- Scripts become difficult to maintain
- Large scripts are hard to troubleshoot
- Time-consuming to develop
- Difficult to scale across environments

---

# Where Ansible Helps

## What is Ansible?

:contentReference[oaicite:0]{index=0} is a powerful IT automation tool designed to simplify infrastructure automation and configuration management.

Ansible allows engineers to automate tasks using simple YAML-based playbooks that are easy to read and write.

Unlike traditional scripting, Ansible focuses on:
- Simplicity
- Readability
- Agentless automation
- Fast deployment
- Scalability

---

# Why Ansible is Popular

## Key Benefits

- Easy to learn
- Minimal coding required
- Uses human-readable YAML syntax
- No agents required on target systems
- Works over SSH
- Supports cloud and on-prem environments
- Massive community support
- Large ecosystem of built-in modules

---

# Traditional Automation vs Ansible

## Traditional Scripting Approach

```text
Write Script
    ↓
Test Script
    ↓
Debug Script
    ↓
Maintain Script
    ↓
Execute on Servers
```
### Ansible Automation Approach
Write Playbook
    ↓
Run Playbook
    ↓
Automate Infrastructure

Example Use Case 1: Restarting Application Infrastructure

Imagine you have a production environment with:

Web Servers
Database Servers

You need to restart the entire application stack in a specific order.

Required Restart Sequence
Shutdown Phase