# Lab code

The runnable Ansible for every CLI lab in this bootcamp. **Run all commands from this directory.**

```bash
cd bootcamp/lab

# 1. Edit your targets
$EDITOR inventories/inventory.ini      # replace the sample 10.0.0.x hosts

# 2. Verify connectivity
ansible all -m ping

# 3. Run a module's playbook
ansible-playbook playbooks/module3_webserver.yml
```

`ansible.cfg` here points at `./inventories/inventory.ini` and `./roles`, so no `-i` flag is needed as long as you are in this directory.

## Layout

```text
lab/
├── ansible.cfg
├── inventories/inventory.ini     # EDIT: your lab hosts
├── group_vars/   all.yml, web.yml
├── host_vars/    server1.yml      # precedence demo: overrides web.yml
├── playbooks/    module{1..6,9}_*.yml
├── roles/web_config/             # the reusable role (Modules 6 & 9)
└── templates/    index.html.j2, motd.j2
```

## Using this with AAP (Day 3)

The runnable code lives in this **subdirectory**, not the repo root. When you create the AAP **project** against this Git repo, set the playbook directory (or `ANSIBLE_CONFIG`) so AAP looks under `bootcamp/lab/`. Then a **job template** references a playbook such as `playbooks/module6_role_apply.yml`.

> If your AAP setup expects code at the repo root, the alternative is to move `lab/` into its own dedicated automation repo and keep this `bootcamp/` folder docs-only. Both patterns work — pick one before Day 3.
