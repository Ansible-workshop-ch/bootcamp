# Bonus Lab: NetBox as an Ansible Source of Truth

## Goal

Understand how NetBox can provide infrastructure data to Ansible and AAP.

## Key idea

Static inventory is good for learning.

NetBox is useful when inventory becomes too large or too dynamic to manage by hand.

NetBox tells Ansible what exists.
Ansible performs the automation.

## Demo objects

Site:

- Charter Lab

Cluster:

- Charter Podman Lab

Virtual machines:

- container1
- container2
- container3

Tags:

- linux
- web
- training

## Teaching flow

1. Show static inventory in \`inventories/inventory.ini\`.
2. Explain why static inventory can drift in large environments.
3. Open NetBox and show the same hosts as structured data.
4. Explain that AAP can sync inventory from NetBox.
5. Explain that NetBox is the source of truth, not the automation engine.

## Course message

Use static inventory first.
Use AAP inventory second.
Introduce NetBox dynamic inventory as an enterprise pattern.
