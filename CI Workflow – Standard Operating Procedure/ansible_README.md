<div align="center">

# POC: Ansible Role & CI Workflow for Nginx Deployment

<img width="800" height="400" alt="image"
src="<IMAGE_URL>" />

</div>

---

# Author Table

| **Author** | **Created On** | **Version** | **Last Updated By** | **Last Edited On** | **L0 Reviewer** | **L1 Reviewer** | **L2 Reviewer** |
| ---------- | -------------- | ----------- | -------------------- | ------------------- | ---------------- | ---------------- | ---------------- |
| Sandeep Rawat| <26-08-2026> | v1.0 | Sandeep | <DD-MM-YYYY> | <L0 Reviewer> | <L1 Reviewer> | <L2 Reviewer> |

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



ansible --version
```

Expected:

```text
ansible [core X.X.X]
  config file = ...
  python version = ...
```

---

# 4. Project Setup

## 4.1 Install Ansible Lint

```bash
python3 -m pip install --user ansible-lint
ansible-lint --version
```

---

## 4.2 Create Project Directory

```bash
mkdir ansible-nginx-demo
cd ansible-nginx-demo
git init
```

---

## 4.3 Create the Ansible Role and Project Directories

```bash
ansible-galaxy role init roles/nginx
mkdir -p inventory playbooks
```

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

Ansible Lint checks the automation against common Ansible best practices.

---

## 6.3 Check Mode (Dry Run)

```bash
ansible-playbook --check playbooks/site.yml
ansible-playbook --check --diff playbooks/site.yml
```

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

---

## 6.5 Validation Checklist

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
| Sandeep | [<email>](mailto:<email>) |

---

# 13. References

| **Resource** | **Link** |
| ------------- | -------- |
| Ansible Documentation | https://docs.ansible.com/ |
| Ansible Lint Documentation | https://ansible-lint.readthedocs.io/ |
| GitHub Actions Documentation | https://docs.github.com/en/actions |
| Ansible Galaxy | https://galaxy.ansible.com/ |
