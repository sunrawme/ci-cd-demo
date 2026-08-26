<div align="center">

# POC: Ansible Role & CI Workflow for Nginx Deployment

>

</div>

---

# Author Table

| **Author** | **Created on** | **Version** | **Last updated by** | **Last edited on** |
|---|---|---|---|---|
| Sandeep Rawat | 24-08-2026 | Version 1 | Sandeep Rawat | 24-08-2026 |

---

# Table of Contents

1. [Introduction](#1-introduction)
2. [Objective](#2-objective)
3. [Prerequisites](#3-prerequisites)
4. [Project Setup](#4-project-setup)
5. [Implementation / Execution](#5-implementation--execution)
6. [Validation](#6-validation)
7. [Observations](#7-observations)
8. [Use Cases](#8-use-cases)
9. [Troubleshooting](#9-troubleshooting)
10. [Best Practices](#10-best-practices)
11. [Conclusion](#11-conclusion)
12. [Contact Information](#12-contact-information)
13. [References](#13-references)

---

# 1. Introduction

This Proof of Concept demonstrates **how to build a reusable Ansible Role, deploy and configure Nginx on an Ubuntu server, validate the automation, and integrate it with a CI workflow** using:

- Ansible & Ansible Galaxy
- Ansible Lint
- Nginx
- GitHub Actions (CI)
- Git / GitHub

The goal of this POC is to **establish a production-oriented, reusable, and CI-validated Ansible automation workflow** that installs and configures Nginx on a target Ubuntu server in an idempotent and testable manner.

---

# 2. Objective

The objective of this POC is to:

- Create a reusable Ansible Role for installing and managing Nginx
- Structure the project using Ansible best practices (roles, inventory, playbooks)
- Validate the automation using syntax checks, lint checks, and dry runs (check mode)
- Verify idempotency of the automation
- Integrate the role with a GitHub Actions CI pipeline for automated validation on push/PR
- Establish secure practices for handling secrets within the automation

---

# 3. Prerequisites

| **Prerequisite** | **Details** |
| ---------------- | ----------- |
| Operating System | Ubuntu / Linux (control node), Ubuntu (target server) |
| Required Software | `ansible`, `python3`, `git` |
| Required Tools | `ansible-lint` |
| Access | SSH access to the target Ubuntu server |
| Permissions | `sudo` / privilege escalation (`become`) on target server |
| Dependencies | `python3-pip` |

### Verify Prerequisites

```bash
sudo apt update
sudo apt install -y ansible git python3-pip
<img width="903" height="343" alt="image" src="https://github.com/user-attachments/assets/839a279f-20e2-4c47-9461-939e29bb1119" />

ansible --version
```

Expected:

```text
ansible [core X.X.X]
  config file = ...
  python version = ...
```
<img width="786" height="173" alt="image" src="https://github.com/user-attachments/assets/7f68a08a-b2d0-46ef-a4f4-f2941069b47b" />

<img width="956" height="532" alt="image" src="https://github.com/user-attachments/assets/f35e8ac7-bae8-4bd4-88d7-83fcfdd68f0d" />

---

# 4. Project Setup

## 4.1 Install Ansible Lint

```bash
python3 -m pip install --user ansible-lint
ansible-lint --version
```
<img width="736" height="156" alt="image" src="https://github.com/user-attachments/assets/88bd3172-8569-405a-8e6a-68c9ac6e274e" />

<img width="789" height="145" alt="image" src="https://github.com/user-attachments/assets/c17cdee9-2612-43ef-ad40-5a41067cf63f" />

---

## 4.2 Create Project Directory

```bash
mkdir ansible-nginx-demo
cd ansible-nginx-demo
git init
```
<img width="885" height="177" alt="image" src="https://github.com/user-attachments/assets/1e41f077-082b-44c1-8932-609ea06e575b" />

<img width="1413" height="81" alt="image" src="https://github.com/user-attachments/assets/58dc9c68-2744-4ddc-8aba-fa951d349d4a" />


---

## 4.3 Create the Ansible Role and Project Directories

```bash
ansible-galaxy role init roles/nginx
mkdir -p inventory playbooks
```

<img width="925" height="61" alt="image" src="https://github.com/user-attachments/assets/2a010240-64b1-4201-92ab-1c70adde19af" />

<img width="935" height="124" alt="image" src="https://github.com/user-attachments/assets/c27c9931-4560-4af9-8ca1-ca5733b70e46" />


### Resulting Project Structure

```text
ansible-nginx-demo/
├── .github/
│   └── workflows/
│       └── ansible-ci.yml
├── inventory/
│   └── hosts
├── playbooks/
│   └── site.yml
├── roles/
│   └── nginx/
│       ├── defaults/
│       │   └── main.yml
│       ├── files/
│       ├── handlers/
│       │   └── main.yml
│       ├── meta/
│       │   └── main.yml
│       ├── tasks/
│       │   └── main.yml
│       ├── templates/
│       │   └── index.html.j2
│       ├── vars/
│       │   └── main.yml
│       └── README.md
├── ansible.cfg
├── .gitignore
└── README.md
```

---

## 4.4 Configuration

Create `ansible.cfg`:

```text
[defaults]
inventory = inventory/hosts
roles_path = roles
host_key_checking = False
retry_files_enabled = False
```

Create `inventory/hosts`:

```text
[web]
web01 ansible_host=192.168.73.209 ansible_user=ubuntu
```

<img width="787" height="309" alt="image" src="https://github.com/user-attachments/assets/ce5cdf99-9304-4471-879a-677c3253049f" />


> Replace the IP address and username with the actual target server details.

Test connectivity:

```bash
ansible all -m ping
```

Expected:

```text
web01 | SUCCESS => {
    "changed": false,
    "ping": "pong"
}
```

<img width="675" height="265" alt="image" src="https://github.com/user-attachments/assets/179b9f70-9769-4bef-8279-ff56f89619b6" />


---

# 5. Implementation / Execution

## 5.1 Define Default Variables

File: `roles/nginx/defaults/main.yml`

```yaml
---
nginx_package_name: nginx
nginx_service_name: nginx
nginx_service_enabled: true
nginx_service_state: started
```

---

## 5.2 Define Tasks

File: `roles/nginx/tasks/main.yml`

```yaml
---
- name: Install Nginx
  ansible.builtin.apt:
    name: "{{ nginx_package_name }}"
    state: present
    update_cache: true

- name: Ensure Nginx is running
  ansible.builtin.service:
    name: "{{ nginx_service_name }}"
    state: "{{ nginx_service_state }}"
    enabled: "{{ nginx_service_enabled }}"

- name: Deploy demo web page
  ansible.builtin.template:
    src: index.html.j2
    dest: /var/www/html/index.html
    mode: "0644"
```

---

## 5.3 Define Handler

File: `roles/nginx/handlers/main.yml`

```yaml
---
- name: Restart Nginx
  ansible.builtin.service:
    name: nginx
    state: restarted
```

Handlers execute only when a task notifies them:

```yaml
notify: Restart Nginx
```

---

## 5.4 Create Template

File: `roles/nginx/templates/index.html.j2`

```html
<!DOCTYPE html>
<html>
<head>
    <title>Ansible Demo</title>
</head>
<body>
    <h1>Hello from Ansible</h1>
    <p>This server was configured using an Ansible role.</p>
</body>
</html>
```

---

## 5.5 Create the Playbook

File: `playbooks/site.yml`

```yaml
---
- name: Configure web servers
  hosts: web
  become: true

  roles:
    - nginx
```

The playbook:
1. Targets the `web` inventory group.
2. Enables privilege escalation using `become`.
3. Executes the `nginx` role.

---

## 5.6 Set Up CI Workflow

File: `.github/workflows/ansible-ci.yml`

```yaml
name: Ansible CI

on:
  push:
    branches:
      - main
      - develop
  pull_request:

jobs:
  ansible-check:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout repository
        uses: actions/checkout@v4

      - name: Set up Python
        uses: actions/setup-python@v5
        with:
          python-version: "3.x"

      - name: Install Ansible
        run: |
          python -m pip install --upgrade pip
          pip install ansible ansible-lint

      - name: Check Ansible version
        run: ansible --version

      - name: Run syntax check
        run: ansible-playbook --syntax-check playbooks/site.yml

      - name: Run Ansible Lint
        run: ansible-lint playbooks/site.yml
```

---

# 6. Validation

## 6.1 Syntax Check

```bash
ansible-playbook --syntax-check playbooks/site.yml
```

Expected:

```text
playbook: playbooks/site.yml
```

<img width="675" height="265" alt="image" src="https://github.com/user-attachments/assets/b555b02c-2603-45bb-9c7b-e005fa130c68" />



This validates the YAML/playbook structure without applying the configuration.

---

## 6.2 Ansible Lint

```bash
ansible-lint playbooks/site.yml
```

Expected:

```text
Passed: 0 failure(s), 0 warning(s) on X files.
```

<img width="1015" height="212" alt="image" src="https://github.com/user-attachments/assets/f9759e74-eee6-4ba6-97ef-34fb41dd91e9" />


Ansible Lint checks the automation against common Ansible best practices.

---

## 6.3 Check Mode (Dry Run)

```bash
ansible-playbook --check playbooks/site.yml
ansible-playbook --check --diff playbooks/site.yml
```

<img width="938" height="422" alt="image" src="https://github.com/user-attachments/assets/fc461129-b508-4e0d-8ed3-a2af1790848d" />


---

## 6.4 Idempotency Check

Run the playbook twice:

```bash
ansible-playbook playbooks/site.yml
ansible-playbook playbooks/site.yml
```

Expected on second run (play recap):

```text
changed=0
```

<img width="790" height="195" alt="image" src="https://github.com/user-attachments/assets/3b596853-3fe9-484e-9d48-464bcf1d4abc" />

<img width="509" height="142" alt="image" src="https://github.com/user-attachments/assets/ecfb3fd5-d644-4a87-a00e-d325fe09fb6d" />

---

## Idempotency
Ansible is designed to be idempotent:

```bash
ansible-playbook playbooks/site.yml
```
<img width="760" height="281" alt="image" src="https://github.com/user-attachments/assets/c06a2711-4651-461c-a87c-eac90c03586b" />

Then run it again:
```bash
ansible-playbook playbooks/site.yml
```
<img width="783" height="309" alt="image" src="https://github.com/user-attachments/assets/81092307-56f4-4671-9dfa-75e4b5a3584d" />

The second execution should normally make little or no change if the server is already in the desired state.
Check the play recap:
changed=0
The goal is:
Desired State
     |
     v
Ansible
     |
     v
Server
     |
     v
Run Again
     |
     v
No Unnecessary Changes

## Git Workflow:

```bash
git push -u origin feature/nginx-role
```
<img width="613" height="314" alt="image" src="https://github.com/user-attachments/assets/d3d363c3-74b5-4771-a642-da864b5b68e2" />

Create a Pull Request and wait for CI validation.

## CI Workflow
The project uses GitHub Actions to validate Ansible code.
File:
.github/workflows/ansible-ci.yml
Example:
name: Ansible CI

on:
  push:
    branches:
      - main
      - develop
  pull_request:

jobs:
  ansible-check:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout repository
        uses: actions/checkout@v4

      - name: Set up Python
        uses: actions/setup-python@v5
        with:
          python-version: "3.x"

      - name: Install Ansible
        run: |
          python -m pip install --upgrade pip
          pip install ansible ansible-lint

      - name: Check Ansible version
        run: ansible --version

      - name: Run syntax check
        run: ansible-playbook --syntax-check playbooks/site.yml

      - name: Run Ansible Lint
        run: ansible-lint playbooks/site.yml

## CI Pipeline Flow
Git Push / Pull Request
          |
          v
    Checkout Code
          |
          v
     Setup Python
          |
          v
    Install Ansible
          |
          v
    Syntax Validation
          |
          v
     Ansible Lint
          |
       +--+--+
       |     |
      FAIL   PASS
       |     |
       v     v
    Fix Code  Review
             |
             v
            Merge

## 6.5 Validation Checklist

## Security
Use protected CI/CD secrets, Ansible Vault, or an appropriate external secret-management solution.
```bash
Failed
```
<img width="975" height="349" alt="image" src="https://github.com/user-attachments/assets/008717a6-0e12-45de-ae00-53fae6dfc224" />

```bash
Fix
```
<img width="977" height="384" alt="image" src="https://github.com/user-attachments/assets/53c58ace-509d-4925-8505-bd815ad63d90" />



| **Validation** | **Expected Result** |
| -------------- | -------------------- |
| Connectivity (`ansible all -m ping`) | `pong` response, SUCCESS |
| Syntax check | Passes with no errors |
| Ansible Lint | 0 failures |
| Check mode (dry run) | Runs without applying changes |
| Playbook execution | Nginx installed, service started |
| Idempotency (2nd run) | `changed=0` |
| Nginx service status | `active` |
| HTTP response (`curl`) | Demo page served successfully |

---

# 7. Observations

## 7.1 Successfully Executed Components

| **Component / Module** | **Status** |
| ----------------------- | ---------- |
| Package installation (`apt`) | PASS |
| Service management (`service`) | PASS |
| Template deployment (`template`) | PASS |
| Handler (restart on change) | PASS |
| Syntax check | PASS |
| Ansible Lint | PASS |
| CI Pipeline (GitHub Actions) | PASS |

---

## 7.2 Failed Components

| **Component / Module** | **Issue** |
| ----------------------- | --------- |
| <Component, if any> | <Issue Description> |

---

## 7.3 Key Findings

- The role structure (`ansible-galaxy role init`) enforces a clean, reusable, and standardized layout.
- Idempotency was confirmed — the second playbook run reported `changed=0`.
- Ansible Lint caught style/best-practice issues before they reached CI, reducing pipeline failures.
- Integrating syntax check and lint into GitHub Actions ensures automation quality is verified on every push/PR, before manual code review.

---

# 8. Use Cases

| **Scenario** | **Commands / Actions** |
| ------------ | ------------------------ |
| Verify connectivity to target hosts | `ansible all -m ping` |
| Validate playbook syntax before running | `ansible-playbook --syntax-check playbooks/site.yml` |
| Check automation against best practices | `ansible-lint playbooks/site.yml` |
| Preview changes without applying them | `ansible-playbook --check --diff playbooks/site.yml` |
| Deploy/update Nginx on target servers | `ansible-playbook playbooks/site.yml` |
| Validate automation automatically on every PR | GitHub Actions workflow (`ansible-ci.yml`) |

---

# 9. Troubleshooting

| **Issue** | **Cause** | **Solution** |
| --------- | --------- | ------------- |
| `ansible: command not found` | Ansible not installed | `sudo apt update && sudo apt install -y ansible` |
| SSH connection failure | Wrong IP/user/key/port, firewall, or SSH service down | Test manually with `ssh ubuntu@<IP>`, then `ansible all -m ping -vvv` |
| Permission denied during task execution | Missing privilege escalation | Add `become: true` in playbook or run with `ansible-playbook -K playbooks/site.yml` |
| Role not found | Incorrect `roles_path` or role missing | Check `ansible-config dump \| grep roles_path` and verify `ls -la roles/` |
| Lint failure | Playbook/role violates a lint rule | Run `ansible-lint playbooks/site.yml`, review the reported rule and line number, fix, and re-run |

---

# 10. Best Practices

| **Best Practice** | **Description** |
| ------------------- | ------------------ |
| Use Roles for reusability | Keep tasks, handlers, variables, and templates organized and reusable across projects |
| Enforce idempotency | Design tasks so repeated runs make no unnecessary changes |
| Validate before applying | Always run syntax check, lint, and check mode before executing against real servers |
| Automate validation via CI | Run syntax check and lint automatically on every push/PR using GitHub Actions |
| Never commit secrets | Keep passwords, SSH keys, API tokens, and cloud credentials out of the repository — use Ansible Vault or a secret manager |
| Use handlers for service restarts | Trigger restarts only when configuration actually changes, via `notify` |

---

# 11. Conclusion

This Proof of Concept demonstrates **a reusable, idempotent, and CI-validated Ansible automation workflow for deploying Nginx** using **Ansible Roles, Ansible Lint, and GitHub Actions**.

The implementation validates:

- A clean, reusable role structure for Nginx installation and configuration
- Successful syntax validation and lint compliance
- Confirmed idempotency across repeated playbook executions
- End-to-end CI validation (syntax check + lint) triggered on push and pull request

Based on the validation results, **the workflow is suitable as a baseline pattern for production-oriented Ansible automation**. The recommended next step is to add **Molecule testing** to the CI pipeline, allowing the role to be deployed and verified in a temporary test environment rather than relying solely on syntax and lint checks.

---

# 12. Contact Information

| **Name** | **Email** |
| -------- | --------- |
| Sandeep Rawat| sandeep.rawat.snaatak@mygurukulam.com |

---

# 13. References

| **Resource** | **Link** |
| ------------- | -------- |
| Ansible Documentation | https://docs.ansible.com/ |
| Ansible Lint Documentation | https://ansible-lint.readthedocs.io/ |
| GitHub Actions Documentation | https://docs.github.com/en/actions |
| Ansible Galaxy | https://galaxy.ansible.com/ |
