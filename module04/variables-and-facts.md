<p align="left">
  <a href="https://github.com/Ansible-workshop-ch/bootcamp/blob/main/module03/playbook-basics.md" target="_blank">
    <img src="/images/backred1.png" alt="Ansible Training" style="width:25px;" />
  </a>
</p>

<p align="right">
  <a href="https://github.com/Ansible-workshop-ch/bootcamp/blob/main/module05/conditions-loops-handlers-templates.md" target="_blank">
    <img src="/images/nexticon.webp" alt="Ansible Training" style="width:25px;" />
  </a>
</p>

Module 4: Variables, Facts, group_vars, and host_vars

Run all lab commands from bootcamp/lab/.

cd bootcamp/lab

Day 2 - Core Skills

Definition

Learning objectives

By the end of this module, you should be able to:

Explain why Ansible variables are useful.

Use group_vars for values shared by a group.

Use host_vars for values assigned to one host.

Read system information from Ansible facts.

Explain which value wins when the same variable is defined more than once.

Variables

A variable is a named value used by a playbook.

Instead of hardcoding a package name in every task:

name: httpd

Define the value once:

package_name: httpd

Then reference it in the playbook:

name: "{{ package_name }}"

Variables make playbooks easier to reuse across different hosts, operating systems, and environments.

Common variable sources

Source

Purpose

Example

Play variables

Values defined inside a play

vars:

Inventory variables

Values attached to a host or group in inventory

server1 web_port=8080

group_vars

Values shared by an inventory group

group_vars/rhel_web.yml

host_vars

Values used by one host

host_vars/server1.yml

Facts

Information collected from a managed host

ansible_facts['os_family']

Extra variables

Values supplied when a playbook is launched

-e "web_message=Hello"

group_vars and host_vars

Ansible matches variable files to inventory names.

inventory group: rhel_web
variable file:   group_vars/rhel_web.yml

inventory host:  server1
variable file:   host_vars/server1.yml

Use:

group_vars/all.yml for values used by every host.

group_vars/<group_name>.yml for values shared by one group.

host_vars/<host_name>.yml for values used by one host.

Facts

Facts are system details gathered from managed hosts when a play starts.

Common facts include:

Hostname

Operating system family

IP addresses

CPU count

Memory

Network interfaces

Examples:

ansible_facts['hostname']
ansible_facts['os_family']
ansible_facts['default_ipv4']['address']

Fact gathering is enabled by default unless the play contains:

gather_facts: false

Variable precedence

The same variable can be defined in more than one place. The more specific or higher-priority value wins.

For this module, remember this simplified order from lower to higher priority:

group_vars/all.yml
        |
        v
group_vars/<group>.yml
        |
        v
host_vars/<host>.yml
        |
        v
extra variables (-e or AAP survey)

Example:

# group_vars/web.yml
web_message: "Hello web servers"

# host_vars/server1.yml
web_message: "Hello server1"

Result:

server1 -> Hello server1
server2 -> Hello web servers

The host-specific value overrides the group value for server1.

Hands-On Walkthrough

1. Review the repository structure

inventories/
|-- inventory.ini
|-- group_vars/
|   |-- all.yml
|   |-- web.yml
|   |-- rhel_web.yml
|   `-- ubuntu_web.yml
`-- host_vars/
    `-- server1.yml

playbooks/
`-- module4_variables.yml

2. Define the inventory

Create or review inventories/inventory.ini:

[web]
server1
server2

[rhel_web]
server1

[ubuntu_web]
server2

The hosts belong to the following groups:

Host

Groups

server1

web, rhel_web

server2

web, ubuntu_web

3. Define variables for every host

Create inventories/group_vars/all.yml:

company: Charter
environment_name: Training

These variables are available to every host in the inventory.

4. Define variables for all web servers

Create inventories/group_vars/web.yml:

web_message: "Hello from {{ company }} {{ environment_name }}"

This value is available to hosts in the web group.

5. Define operating-system-specific values

Create inventories/group_vars/rhel_web.yml:

package_name: httpd
service_name: httpd

Create inventories/group_vars/ubuntu_web.yml:

package_name: apache2
service_name: apache2

The same playbook can now use the correct package and service names for each group.

6. Add one host-specific override

Create inventories/host_vars/server1.yml:

web_message: "Hello specifically from server1"

This value overrides group_vars/web.yml only for server1.

7. Create the playbook

Create playbooks/module4_variables.yml:

---
- name: Demonstrate variables and facts
  hosts: web
  gather_facts: true

  tasks:
    - name: Display resolved variables
      ansible.builtin.debug:
        msg:
          - "Host: {{ inventory_hostname }}"
          - "Package: {{ package_name }}"
          - "Service: {{ service_name }}"
          - "Message: {{ web_message }}"

    - name: Display selected facts
      ansible.builtin.debug:
        msg:
          - "Hostname: {{ ansible_facts['hostname'] }}"
          - "OS family: {{ ansible_facts['os_family'] }}"

The debug module displays information without changing the managed host. It is useful for checking variables, facts, and precedence.

8. Validate and run the playbook

Check the syntax:

ansible-playbook -i inventories/inventory.ini playbooks/module4_variables.yml --syntax-check

Run the playbook:

ansible-playbook -i inventories/inventory.ini playbooks/module4_variables.yml

Expected variable behavior:

Host

Package

Service

Message source

server1

httpd

httpd

host_vars/server1.yml

server2

apache2

apache2

group_vars/web.yml

9. Test an extra variable

Run the playbook with a runtime value:

ansible-playbook -i inventories/inventory.ini playbooks/module4_variables.yml \
  -e "web_message=Runtime message"

The extra variable overrides the values from both group_vars and host_vars.

AAP surveys also pass extra variables, so they follow the same behavior.

Quiz

What is the purpose of group_vars?

A. Store values for inventory groups

B. Store playbook output

C. Replace the inventory

D. Store only encrypted passwords

Which file applies variables to every host?

A. group_vars/all.yml

B. host_vars/all.yml

C. inventory/all.yml

D. playbooks/all.yml

What are Ansible facts?

A. Details collected from managed hosts

B. Git branch information

C. AAP job templates

D. Static values that must always be typed manually

Which source has the highest priority in this module's simplified precedence order?

A. group_vars/all.yml

B. Group variables

C. Host variables

D. Extra variables passed with -e or an AAP survey

Which module is commonly used to display variable and fact values?

A. ansible.builtin.copy

B. ansible.builtin.debug

C. ansible.builtin.package

D. ansible.builtin.service

<details>
<summary>Instructor answer key</summary>

A - Store values for inventory groups

A - group_vars/all.yml

A - Details collected from managed hosts

D - Extra variables passed with -e or an AAP survey

B - ansible.builtin.debug

</details>

Hands-On Lab

Goal

Make one playbook behave differently for different hosts without duplicating its tasks.

Tasks

Confirm that the inventory contains the web, rhel_web, and ubuntu_web groups.

Create or review these variable files:

inventories/group_vars/all.yml
inventories/group_vars/web.yml
inventories/group_vars/rhel_web.yml
inventories/group_vars/ubuntu_web.yml

Create inventories/host_vars/server1.yml and override web_message for server1.

Run the playbook and compare the values returned for each host.

Add this task to the playbook:

- name: Display host memory
  ansible.builtin.debug:
    msg: "Memory in MB: {{ ansible_facts['memtotal_mb'] }}"

Run the playbook again:

ansible-playbook -i inventories/inventory.ini playbooks/module4_variables.yml

Override the message at runtime:

ansible-playbook -i inventories/inventory.ini playbooks/module4_variables.yml \
  -e "web_message=Message from extra vars"

Explain why the runtime message appears on both hosts.

Verification commands

List the inventory structure:

ansible-inventory -i inventories/inventory.ini --graph

Display all resolved variables for server1:

ansible-inventory -i inventories/inventory.ini --host server1

Display gathered facts from the web hosts:

ansible web -i inventories/inventory.ini -m ansible.builtin.setup

Success check

I can explain the difference between group_vars and host_vars.

I can identify which variable value wins and why.

I can display facts with ansible.builtin.debug.

I can change playbook behavior without rewriting the tasks.

I can use an extra variable to override lower-priority values.

Key lesson

Keep the automation in the playbook and keep changing data in variable files.

<p align="left">
  <a href="https://github.com/Ansible-workshop-ch/bootcamp/blob/main/module03/playbook-basics.md" target="_blank">
    <img src="/images/backred1.png" alt="Ansible Training" style="width:25px;" />
  </a>
</p>

<p align="right">
  <a href="https://github.com/Ansible-workshop-ch/bootcamp/blob/main/module05/conditions-loops-handlers-templates.md" target="_blank">
    <img src="/images/nexticon.webp" alt="Ansible Training" style="width:25px;" />
  </a>
</p>