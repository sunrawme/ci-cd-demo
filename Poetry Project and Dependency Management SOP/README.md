# SOP – Python Poetry Project and Dependency Management

| **Author** | **Created on** | **Version** | **Last updated by** | **Last edited on** |
|------------|----------------|-------------|---------------------|--------------------|
| Sandeep Rawat | 24-08-2026 | Version 1 | Sandeep Rawat | 24-08-2026 |

---

## Purpose

This document provides a Standard Operating Procedure (SOP) for creating and managing Python projects using Poetry.

Poetry is a dependency management and packaging tool for Python. It provides a standardized way to create Python projects, manage dependencies, maintain virtual environments, generate lock files, and execute Python applications.

This SOP demonstrates the complete workflow of:

- Installing Poetry
- Creating a Python project
- Understanding the Poetry project structure
- Managing virtual environments
- Installing dependencies
- Adding and removing dependencies
- Understanding `pyproject.toml`
- Understanding `poetry.lock`
- Running Python applications
- Validating the project
- Troubleshooting common Poetry issues

---

## Pre-requisites

Before starting the Poetry setup, ensure that the following requirements are available.

### System Requirements

| **Hardware Specifications** | **Minimum Recommendation** |
|-----------------------------|-----------------------------|
| Processor                   | Dual-core                   |
| RAM                         | 4 GB                        |
| Disk                        | 20 GB                       |
| OS                          | Ubuntu 22.04 or later       |
| Internet                    | Required                    |

### Software Requirements

| **Software** | **Version** | **Description** |
|--------------|-------------|-----------------|
| Python       | 3.x         | Python runtime |
| pip          | 3.x         | Python package installer |
| Poetry       | 2.x         | Python dependency and project management |
| curl         | Latest      | Used to download Poetry installer |
| tree         | Optional    | Used to display project structure |

### Knowledge Requirements

The user should have basic knowledge of:

- Linux commands
- Python
- Python packages
- Virtual environments
- Git basics
- Terminal usage

---

## Dependencies

### Build Time Dependency

| **Name** | **Version** | **Description** |
|----------|-------------|-----------------|
| Python   | 3.x         | Required to create and execute Python projects |
| Poetry   | 2.x         | Used for project creation and dependency management |

### Run Time Dependency

| **Name** | **Version** | **Description** |
|----------|-------------|-----------------|
| Python   | 3.x         | Executes the Python application |
| Requests | 2.x         | Example third-party Python dependency |

### Other Dependency

| **Name** | **Version** | **Description** |
|----------|-------------|-----------------|
| curl     | Latest      | Used to install Poetry |
| tree     | Latest      | Used to display project directory structure |
| Internet | N/A         | Required to download Poetry and Python packages |

---

## Important Ports

This SOP does not require any application-specific inbound or outbound ports.

| **Traffic Type** | **Port** | **Description** |
|------------------|----------|-----------------|
| Outbound         | 443      | HTTPS communication for downloading Poetry and Python packages |
| Inbound          | N/A      | No inbound port required |

---

## Others

| **Requirement** | **Description** |
|-----------------|-----------------|
| Internet Connectivity | Required for installing Poetry and Python dependencies |
| Terminal Access | Required to execute commands |
| User Permissions | `sudo` may be required for installing system packages |
| Python Version | Python 3.x should be available |
| PATH Configuration | Poetry binary should be available in the user's PATH |

---

# Architecture

The Poetry project follows a simple dependency-management architecture.

```text
                    Python Project
                          |
                          v
                   pyproject.toml
                          |
              +-----------+-----------+
              |                       |
              v                       v
       Project Configuration     Dependencies
              |                       |
              +-----------+-----------+
                          |
                          v
                    poetry.lock
                          |
                          v
                Poetry Virtual Environment
                          |
                          v
                    Python Application
