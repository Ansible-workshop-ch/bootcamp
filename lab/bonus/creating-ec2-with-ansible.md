# Bonus Lab: Creating EC2 Instances with Ansible

> This is a bonus lesson. It is not required for the main 3-day flow.
> The goal is to show that Ansible can create cloud infrastructure, not only configure existing servers.

---

## Goal

In the main labs, Ansible connects to existing hosts such as:

```text
ubuntu_web
rhel_web
web
linux
```

Those hosts already exist.

In this bonus lab, we introduce a different idea:

```text
Ansible can create the machine first.
Then Ansible can configure it.
```

For example, Ansible can create an AWS EC2 instance using the `amazon.aws` collection.

---

## Important Warning

This lab can create real AWS resources.

Before running it, confirm:

* You are using the correct AWS account
* You are using the correct AWS region
* You have permission to create EC2 instances
* Your AWS key pair exists in that region
* You understand that running cloud resources may cost money

For training, this should be treated as an instructor demo unless students have a safe AWS sandbox.

---

## Why This Lesson Matters

Most of the course focuses on managing existing machines:

```text
Inventory -> Hosts -> Playbook -> Configuration
```

But Ansible can also manage infrastructure:

```text
AWS API -> Create EC2 -> Get public IP -> Configure server
```

This shows students that Ansible can work in two ways:

| Use Case                    | Example                          |
| --------------------------- | -------------------------------- |
| Configuration management    | Install Apache on existing hosts |
| Infrastructure provisioning | Create an EC2 instance in AWS    |

---

## Diagram / Workflow

```mermaid
flowchart TD
    A[Ansible runs on localhost] --> B[Call AWS API]
    B --> C[Find default VPC]
    C --> D[Find subnet]
    D --> E[Find latest Ubuntu AMI]
    E --> F[Create or reuse security group]
    F --> G[Ensure EC2 instance exists]
    G --> H[Return instance ID and public IP]
```

### Diagram Explanation

This workflow is different from the earlier labs.

In the normal labs, Ansible connects to existing managed hosts such as `container1`, `container2`, `rhel1`, or `rhel2`.

In this EC2 lab, the server does not exist yet.

So the playbook runs on `localhost`.

That means Ansible is running from the control machine and using the AWS API to create infrastructure.

The playbook does the following:

1. Finds the default VPC in the AWS region.
2. Finds a subnet inside that VPC.
3. Finds the latest Ubuntu AMI.
4. Creates a security group for SSH and HTTP.
5. Ensures an EC2 instance exists.
6. Prints the instance ID and public IP.

In simple terms:

```text
localhost does not mean we are configuring localhost.
localhost means Ansible is using the local machine to call AWS.
```

---

## Key Concept: `count` vs `exact_count`

This is the most important part of the lesson.

When creating cloud resources, we must be careful. We do not want to accidentally create a new server every time the playbook runs.

---

### `count`

`count` means:

```text
Create this many instances for this request.
```

If used carelessly, it can create a new instance every time the playbook runs.

Example:

```yaml
count: 1
```

Teaching point:

```text
count: 1 can mean create one more instance.
```

That can be dangerous in a lab because every run may create another EC2 instance.

---

### `exact_count`

`exact_count` means:

```text
Make sure exactly this many matching instances exist.
```

Example:

```yaml
exact_count: 1
```

With filters, Ansible checks for matching instances first.

If one matching instance already exists, Ansible does not create another one.

If zero matching instances exist, Ansible creates one.

If more than one matching instance exists, Ansible can reduce the count back down.

Teaching point:

```text
exact_count: 1 is safer for repeatable labs.
```

---

## Simple Comparison

| Option           | Meaning                                     | Risk                                    |
| ---------------- | ------------------------------------------- | --------------------------------------- |
| `count: 1`       | Create one instance as part of this run     | May create another instance on each run |
| `exact_count: 1` | Ensure exactly one matching instance exists | Safer and more idempotent               |

---

## Why Filters Matter

`exact_count` needs filters so Ansible knows which instances count as matching.

Example filters:

```yaml
filters:
  "tag:Name": "{{ instance_name }}"
  "tag:ManagedBy": Ansible
  "tag:Lab": exact-count-test
  instance-state-name: running
```

These filters tell Ansible:

```text
Only count running instances with these specific tags.
```

That is what prevents Ansible from confusing this lab instance with other EC2 instances in the account.

---

## How This Relates to Our Existing Lab Hosts

In the main lab, we use groups like:

```text
ubuntu_web
rhel_web
web
linux
```

Those groups represent machines that already exist.

In this EC2 bonus lab, we use:

```yaml
hosts: localhost
connection: local
```

That is because the EC2 instance does not exist yet.

Ansible must first call AWS from the control machine.

After the instance exists, it could later be added to inventory and configured like any other host.

---

## Hands-On Walkthrough

Before running the playbook, confirm AWS access:

```bash
aws sts get-caller-identity
```

Confirm the AWS region:

```bash
aws configure get region
```

Confirm the key pair exists in the selected region:

```bash
aws ec2 describe-key-pairs --region us-east-2
```

If the key pair does not exist, the playbook will fail.

Example error:

```text
InvalidKeyPair.NotFound
```

That means the key name in the playbook does not exist in the selected AWS region.

---

## Run the Playbook

From the lab directory:

```bash
cd bootcamp/lab
```

Run:

```bash
ansible-playbook playbooks/bonus_ec2_exact_count.yml
```

Run it a second time:

```bash
ansible-playbook playbooks/bonus_ec2_exact_count.yml
```

Expected behavior:

```text
First run: creates the EC2 instance
Second run: should not create another matching EC2 instance
```

That is the idempotency lesson.

---

## Success Check

* [ ] You can explain why this playbook uses `hosts: localhost`.
* [ ] You can explain that Ansible is calling the AWS API.
* [ ] You can explain the risk of `count: 1`.
* [ ] You can explain why `exact_count: 1` is safer.
* [ ] You can explain why filters and tags matter.
* [ ] You can explain how this differs from configuring existing inventory hosts.

---

## Quiz

1. Why does this EC2 playbook use `hosts: localhost`?

   * A. Because Ansible is calling the AWS API from the control node
   * B. Because EC2 only runs on localhost
   * C. Because SSH is disabled
   * D. Because inventory is not supported

2. What is the risk of using `count: 1` carelessly?

   * A. It may create another instance each time the playbook runs
   * B. It deletes all instances
   * C. It disables SSH
   * D. It only works with Windows

3. Why is `exact_count: 1` useful?

   * A. It ensures exactly one matching instance exists
   * B. It always creates ten instances
   * C. It skips AWS authentication
   * D. It removes the need for tags

4. Why do filters matter with `exact_count`?

   * A. They tell Ansible which instances should be counted
   * B. They install Apache
   * C. They create SSH keys
   * D. They replace the AWS region

---

<details>
<summary>Instructor answer key</summary>

1. **A** — Because Ansible is calling the AWS API from the control node
2. **A** — It may create another instance each time the playbook runs
3. **A** — It ensures exactly one matching instance exists
4. **A** — They tell Ansible which instances should be counted

</details>
