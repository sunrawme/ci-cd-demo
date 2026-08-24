# SOP – Python Poetry Project and Dependency Management

| **Author** | **Created on** | **Version** | **Last updated by** | **Last edited on** |
|---|---|---|---|---|
| Sandeep Rawat | 24-08-2026 | Version 1 | Sandeep Rawat | 24-08-2026 |

## Purpose

This SOP provides a standard procedure for creating and managing a Python project using Poetry on Ubuntu Linux.

Poetry is used to manage Python project dependencies, virtual environments, project configuration, dependency locking, and application execution. This document provides a repeatable workflow suitable for developers and DevOps engineers and includes verification points and screenshot placeholders for Pre/L0 review.

## Pre-requisites

Before starting the setup, ensure the following requirements are available:

- Ubuntu/Linux system
- Python 3 installed
- pip installed
- Internet connectivity
- Terminal access
- Basic Python knowledge
- Sufficient permissions to install software and create files

## System Requirements

| **Component** | **Minimum Recommendation** |
|---|---|
| Operating System | Ubuntu Linux |
| Python | Python 3 |
| RAM | 4 GB |
| Disk | 20 GB |
| Internet | Required |
| Terminal | Required |

## Dependencies

### Build Time Dependency

| **Name** | **Version** | **Description** |
|---|---|---|
| Python 3 | Installed version | Required to create and execute the Python project |
| pip3 | Installed version | Python package installer |
| Poetry | 2.x or installed version | Python project and dependency management tool |

### Run Time Dependency

| **Name** | **Version** | **Description** |
|---|---|---|
| Python | Project-supported version | Runtime for executing the application |
| requests | Installed project version | Third-party package used by the demonstration application |

### Other Dependency

| **Name** | **Version** | **Description** |
|---|---|---|
| Internet connectivity | N/A | Required for Poetry installation and package download |
| Ubuntu terminal | N/A | Used to execute setup and verification commands |

## Important Ports

The demonstration application does not expose a local server port.

| **Traffic** | **Port** | **Description** |
|---|---:|---|
| Outbound HTTPS | 443 | Used by the sample `requests` application to access `https://example.com` |
| Inbound | N/A | No inbound application port is required |

## Others

| **Requirement** | **Description** |
|---|---|
| Virtual Environment | Poetry-managed virtual environment |
| Project Configuration | `pyproject.toml` |
| Dependency Lock File | `poetry.lock` |
| Application File | `app.py` |
| Project Name | `poetry-demo` |

## Architecture

The project uses Poetry as the project and dependency management layer.

```text
                    Ubuntu Linux
                         |
                         v
                  Python 3 + pip
                         |
                         v
                      Poetry
                         |
          +--------------+--------------+
          |                             |
          v                             v
   pyproject.toml                 Poetry Environment
          |                             |
          |                             v
          |                        Installed Packages
          |                             |
          v                             v
     poetry.lock  ------------>    Python Application
                                        |
                                        v
                                  app.py / requests
                                        |
                                        v
                              External HTTPS Endpoint
```

**Screenshot – Architecture Diagram:** Add the architecture screenshot/diagram here.

## Dataflow Diagram

The dataflow for the demonstration application is:

```text
Developer
   |
   v
Poetry Project
   |
   +--> pyproject.toml
   |       |
   |       +--> Project configuration
   |       +--> Dependencies
   |
   +--> poetry.lock
   |       |
   |       +--> Locked/resolved dependency information
   |
   +--> Poetry Virtual Environment
           |
           +--> Python
           +--> requests
                   |
                   v
                 app.py
                   |
                   v
          HTTPS request to example.com
                   |
                   v
             HTTP response
                   |
                   v
          Status Code displayed
```

The developer creates the project with Poetry, defines dependencies through Poetry, and executes the application using the Poetry-managed environment. The sample `app.py` uses the `requests` package to make an HTTPS request and prints the returned HTTP status code.

**Screenshot – Dataflow Diagram:** Add the dataflow screenshot/diagram here.

# Step-by-step installation of Python Poetry Project

## Step 1: Installation of Software Dependencies

### Build Dependency

Verify Python:

```bash
python3 --version
```

Verify pip:

```bash
pip3 --version
```

**Screenshot 1 – Python Version:** Add screenshot showing `python3 --version` and its output.

**Screenshot 2 – pip Version:** Add screenshot showing `pip3 --version` and its output.

### Run Time Dependency

Poetry is installed using the official Poetry installer:

```bash
curl -sSL https://install.python-poetry.org | python3 -
```

If required, add Poetry to the PATH:

```bash
export PATH="$HOME/.local/bin:$PATH"
```

Verify Poetry:

```bash
poetry --version
```

**Screenshot 3 – Poetry Installation/Verification:** Add screenshot showing Poetry installation and `poetry --version`.

### Other Dependency

Internet connectivity is required to download Poetry and Python packages.

No third-party integration is required for the basic demonstration.

## Step 2: Build / Artifact Generation

### Create the Project

Move to the working directory:

```bash
cd ~
```

Create the Poetry project:

```bash
poetry new poetry-demo
```

Enter the project:

```bash
cd poetry-demo
```

**Screenshot 4 – Project Creation:** Add screenshot showing `poetry new poetry-demo` and `cd poetry-demo`.

### Verify Project Structure

If `tree` is installed:

```bash
tree
```

If it is not installed:

```bash
sudo apt install tree
```

Then:

```bash
tree
```

Expected structure:

```text
poetry-demo/
├── README.md
├── pyproject.toml
├── poetry_demo/
│   └── __init__.py
└── tests/
    └── __init__.py
```

**Screenshot 5 – Project Structure:** Add screenshot showing the complete project structure.

### Verify `pyproject.toml`

```bash
cat pyproject.toml
```

The file contains project configuration such as:

- Project name
- Version
- Python requirement
- Dependencies
- Build configuration

**Screenshot 6 – pyproject.toml:** Add screenshot showing the contents of `pyproject.toml`.

### Check Poetry Environment

```bash
poetry env info
```

Poetry should display information about the Python version, virtual environment, environment path, executable, and validity.

**Screenshot 7 – Poetry Environment:** Add screenshot showing `poetry env info`.

### Check Virtual Environment Path

```bash
poetry env info --path
```

**Screenshot 8 – Environment Path:** Add screenshot showing the Poetry virtual environment path.

### Install Project Dependencies

```bash
poetry install
```

This creates or uses the Poetry virtual environment and installs the project dependencies.

**Screenshot 9 – Poetry Install:** Add screenshot showing successful `poetry install`.

### Add a Python Dependency

For demonstration, add the `requests` package:

```bash
poetry add requests
```

Poetry resolves and installs the package and updates the project dependency information and lock file.

**Screenshot 10 – Add Dependency:** Add screenshot showing `poetry add requests`.

### Verify Installed Dependencies

```bash
poetry show
```

The installed packages should be displayed.

**Screenshot 11 – Installed Dependencies:** Add screenshot showing `poetry show`.

### Verify a Specific Dependency

```bash
poetry show requests
```

**Screenshot 12 – Requests Package:** Add screenshot showing `poetry show requests`.

### Verify Updated `pyproject.toml`

```bash
cat pyproject.toml
```

The dependency section should contain `requests`.

**Screenshot 13 – Updated pyproject.toml:** Add screenshot showing `requests` in the dependency configuration.

### Verify `poetry.lock`

```bash
ls -l poetry.lock
```

Optionally inspect the beginning of the file:

```bash
head -30 poetry.lock
```

**Screenshot 14 – poetry.lock:** Add screenshot showing the lock file and its contents.

## Step 3: Application Deployment / Execution

### Run Python Using Poetry

```bash
poetry run python --version
```

This confirms that Python is being executed from the Poetry-managed environment.

**Screenshot 15 – Poetry Python:** Add screenshot showing `poetry run python --version`.

### Verify Dependency Inside the Poetry Environment

```bash
poetry run python -c "import requests; print(requests.__version__)"
```

**Screenshot 16 – Requests Verification:** Add screenshot showing the installed `requests` version.

### Create the Python Application

Create the application:

```bash
nano app.py
```

Add:

```python
import requests

response = requests.get("https://example.com")

print("Status Code:", response.status_code)
```

Save the file and verify it:

```bash
cat app.py
```

**Screenshot 17 – Application File:** Add screenshot showing the complete `app.py` code.

### Run the Python Application

```bash
poetry run python app.py
```

Expected result:

```text
Status Code: 200
```

The exact status code may vary depending on the external endpoint response.

**Screenshot 18 – Application Execution:** Add screenshot showing successful execution of `app.py`.

### Run Commands in the Poetry Context

Check Python:

```bash
poetry run python --version
```

Check installed packages:

```bash
poetry run pip list
```

Run the application:

```bash
poetry run python app.py
```

**Screenshot 19 – Poetry Run Commands:** Add screenshot showing the `poetry run` commands and their output.

### Remove a Dependency

```bash
poetry remove requests
```

This removes the dependency and updates the project dependency information and lock file.

**Screenshot 20 – Remove Dependency:** Add screenshot showing successful `poetry remove requests`.

### Verify Dependency Removal

```bash
poetry show
```

Or:

```bash
poetry show requests
```

**Screenshot 21 – Dependency Removal Verification:** Add screenshot showing the verification result.

### Validate the Project

```bash
poetry check
```

This checks the Poetry project configuration.

**Screenshot 22 – Poetry Check:** Add screenshot showing successful project validation.

## Monitoring

Monitoring for this SOP focuses on validating the health and correctness of the development environment and application execution.

### Metrics

| **Parameter** | **Description** | **Priority** | **Threshold / Expected Result** |
|---|---|---|---|
| Python Availability | Verifies Python is installed and executable | High | `python3 --version` succeeds |
| Poetry Availability | Verifies Poetry is installed | High | `poetry --version` succeeds |
| Dependency Installation | Verifies dependencies can be installed | High | `poetry install` succeeds |
| Dependency Resolution | Verifies package resolution | High | `poetry add requests` succeeds |
| Environment Validity | Verifies Poetry environment | High | `poetry env info` reports valid environment |
| Application Execution | Verifies application runs | High | `poetry run python app.py` completes successfully |
| Project Validation | Verifies project configuration | High | `poetry check` succeeds |

### Health Check

| **Name** | **Type** | **Initial Delay** | **Period** | **Timeout** | **Success Threshold** | **Failure Threshold** |
|---|---|---:|---:|---:|---:|---:|
| Poetry Environment | Command Check | N/A | Manual | N/A | Command succeeds | Command fails |
| Python Application | Execution Check | N/A | Manual | N/A | Application executes | Application returns an error |

### Explanation of Parameters

| **Parameter** | **Description** |
|---|---|
| Python Availability | Confirms Python can be executed |
| Poetry Availability | Confirms Poetry can be executed |
| Dependency Installation | Confirms required packages can be resolved and installed |
| Environment Validity | Confirms the Poetry environment is available |
| Application Execution | Confirms the sample application runs |
| Project Validation | Confirms Poetry project configuration is valid |

## Logging

For this local demonstration, logs are primarily available through command output.

| **Log Type** | **Location / Source** | **Description** |
|---|---|---|
| Installation Output | Terminal | Poetry and package installation messages |
| Dependency Output | Terminal | Output from `poetry show`, `poetry install`, and related commands |
| Application Output | Terminal | Output from `poetry run python app.py` |
| Error Output | Terminal | Errors generated during commands or application execution |

## Disaster Recovery

The main project configuration and dependency definitions should be retained in source control.

Important files include:

```text
pyproject.toml
poetry.lock
app.py
poetry_demo/
tests/
```

If the local environment is lost, the project can be recreated by obtaining the project source and running:

```bash
poetry install
```

The Poetry environment can then be verified with:

```bash
poetry env info
```

## High Availability

High availability is not applicable to this local demonstration application because it is intended to demonstrate Python project and dependency management rather than a production service.

For a production Python application, HA would normally be implemented at the application and infrastructure layers by using multiple application instances, load balancing, health checks, redundant infrastructure, and appropriate deployment automation.

## Troubleshooting

### Issue 1: `poetry: command not found`

Check the PATH:

```bash
echo $PATH
```

Add Poetry to the PATH:

```bash
export PATH="$HOME/.local/bin:$PATH"
```

Verify:

```bash
poetry --version
```

**Screenshot – Troubleshooting:** Add screenshot showing the issue and the successful PATH/Poetry verification.

### Issue 2: `ModuleNotFoundError`

Example:

```text
ModuleNotFoundError: No module named 'requests'
```

Install the dependency:

```bash
poetry add requests
```

Run the application:

```bash
poetry run python app.py
```

**Screenshot – ModuleNotFoundError:** Add screenshot showing the error and resolution.

### Issue 3: Python Syntax Error

Inspect the Python file:

```bash
cat app.py
```

Correct the code and run:

```bash
poetry run python app.py
```

**Screenshot – Syntax Error:** Add screenshot showing the issue and corrected execution.

## FAQs

- **What is Poetry?**
  - Poetry is used to manage Python projects, dependencies, virtual environments, project configuration, and execution.

- **What is `pyproject.toml`?**
  - It contains the project's configuration and dependency information.

- **What is `poetry.lock`?**
  - It records the resolved dependency information used for reproducible installations.

- **Can Poetry manage a virtual environment?**
  - Yes. Poetry creates and manages a project-specific virtual environment.

- **How do I run the application using Poetry?**
  - Use `poetry run python app.py`.

- **How do I validate the project?**
  - Use `poetry check`.

## Common Poetry Commands

| **Command** | **Purpose** |
|---|---|
| `poetry --version` | Check Poetry version |
| `poetry new project-name` | Create a new project |
| `poetry init` | Initialize Poetry in an existing project |
| `poetry install` | Install project dependencies |
| `poetry add package` | Add a dependency |
| `poetry remove package` | Remove a dependency |
| `poetry show` | Display installed packages |
| `poetry update` | Update dependencies |
| `poetry env info` | Display environment information |
| `poetry env info --path` | Display environment path |
| `poetry run command` | Run a command inside the Poetry environment |
| `poetry check` | Validate project configuration |

## Recommended Demo for Pre/L0 Reviewer

Use the following workflow during the review:

```text
Verify Python
     ↓
Verify pip
     ↓
Verify Poetry
     ↓
Create Project
     ↓
Show Project Structure
     ↓
Show pyproject.toml
     ↓
Check Virtual Environment
     ↓
Install Dependencies
     ↓
Add requests
     ↓
Show poetry.lock
     ↓
Run Python using poetry run
     ↓
Verify requests
     ↓
Create Application
     ↓
Run Application
     ↓
Validate Project
```

### Live Demo Commands

```bash
python3 --version

pip3 --version

poetry --version

cd ~

poetry new poetry-demo

cd poetry-demo

tree

cat pyproject.toml

poetry env info

poetry env info --path

poetry install

poetry add requests

poetry show

poetry show requests

poetry run python --version

poetry run python -c "import requests; print(requests.__version__)"

nano app.py

cat app.py

poetry run python app.py

poetry check
```

## Expected Final Project Structure

```text
poetry-demo/
├── README.md
├── app.py
├── poetry.lock
├── pyproject.toml
├── poetry_demo/
│   └── __init__.py
└── tests/
    └── __init__.py
```

## Screenshot Checklist

Add your existing screenshots at the indicated locations in this document.

- [ ] Screenshot 1 – Python version
- [ ] Screenshot 2 – pip version
- [ ] Screenshot 3 – Poetry installation/version
- [ ] Screenshot 4 – Project creation
- [ ] Screenshot 5 – Project structure
- [ ] Screenshot 6 – `pyproject.toml`
- [ ] Screenshot 7 – Poetry environment information
- [ ] Screenshot 8 – Environment path
- [ ] Screenshot 9 – `poetry install`
- [ ] Screenshot 10 – `poetry add requests`
- [ ] Screenshot 11 – `poetry show`
- [ ] Screenshot 12 – `poetry show requests`
- [ ] Screenshot 13 – Updated `pyproject.toml`
- [ ] Screenshot 14 – `poetry.lock`
- [ ] Screenshot 15 – `poetry run python --version`
- [ ] Screenshot 16 – Requests verification
- [ ] Screenshot 17 – `app.py`
- [ ] Screenshot 18 – Application execution
- [ ] Screenshot 19 – Poetry context commands
- [ ] Screenshot 20 – Dependency removal
- [ ] Screenshot 21 – Dependency removal verification
- [ ] Screenshot 22 – `poetry check`
- [ ] [ ] Architecture diagram
- [ ] Dataflow diagram

## Conclusion

Poetry provides a standardized approach for managing Python projects, dependencies, virtual environments, project configuration, and application execution.

The two key project files are:

```text
pyproject.toml
       ↓
Project configuration + dependencies

poetry.lock
       ↓
Resolved/locked dependency information
```

The key application execution command is:

```bash
poetry run python app.py
```

The overall workflow is:

```text
Create
  ↓
Configure
  ↓
Install
  ↓
Add Dependencies
  ↓
Lock Dependencies
  ↓
Run
  ↓
Validate
  ↓
Test
  ↓
Deploy
```

## References

| **Reference** | **Description** |
|---|---|
| Poetry documentation | Official documentation for Poetry installation and usage |
| Python documentation | Python language and runtime reference |
| PyPI | Python package repository used for package distribution |
