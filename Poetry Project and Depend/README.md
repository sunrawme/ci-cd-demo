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

<img width="777" height="159" alt="image" src="https://github.com/user-attachments/assets/1b03409e-010c-4c65-b80e-e7be9f24c9dd" />


<img width="956" height="127" alt="image" src="https://github.com/user-attachments/assets/3d1e6f86-fbb1-4939-b946-bf8f6bd33be9" />

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

<img width="611" height="357" alt="image" src="https://github.com/user-attachments/assets/5bc10bbe-490d-45d8-876a-da18d1b6f1f7" />

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

<img width="642" height="245" alt="image" src="https://github.com/user-attachments/assets/f79a539b-2890-466d-8b10-1e6cea69bc03" />

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

<img width="653" height="479" alt="image" src="https://github.com/user-attachments/assets/dc797d9b-12ff-445c-a61f-bfbb6d4da325" />

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

<img width="670" height="285" alt="image" src="https://github.com/user-attachments/assets/481d85fa-c90d-44f0-afdb-7c602f09f294" />

### Check Poetry Environment

```bash
poetry env info
```

Poetry should display information about the Python version, virtual environment, environment path, executable, and validity.

<img width="704" height="318" alt="image" src="https://github.com/user-attachments/assets/d743f9dc-5ca7-4b26-b761-7c19f85242af" />

### Check Virtual Environment Path

```bash
poetry env info --path
```

<img width="826" height="85" alt="image" src="https://github.com/user-attachments/assets/61a1f3b1-314e-4df0-9ffe-4b1676ed221e" />

### Install Project Dependencies

```bash
poetry install
```

This creates or uses the Poetry virtual environment and installs the project dependencies.

<img width="1017" height="119" alt="image" src="https://github.com/user-attachments/assets/4fc02645-b5f3-4c3d-a614-f0d6880bf3a9" />

### Add a Python Dependency

For demonstration, add the `requests` package:

```bash
poetry add requests
```

Poetry resolves and installs the package and updates the project dependency information and lock file.

<img width="598" height="319" alt="image" src="https://github.com/user-attachments/assets/e06bc061-a9dc-473e-affd-c90762838821" />

### Verify Installed Dependencies

```bash
poetry show
```

The installed packages should be displayed.

<img width="821" height="164" alt="image" src="https://github.com/user-attachments/assets/0fc1a544-4b33-4a44-8957-65d61163b4ba" />

### Verify a Specific Dependency

```bash
poetry show requests
```

<img width="823" height="268" alt="image" src="https://github.com/user-attachments/assets/412b3738-38d5-4b43-a232-1769a5d06c8f" />

### Verify Updated `pyproject.toml`

```bash
cat pyproject.toml
```

The dependency section should contain `requests`.

<img width="784" height="310" alt="image" src="https://github.com/user-attachments/assets/dbe69a28-2e9a-4e13-91c8-f3977e002119" />

### Verify `poetry.lock`

```bash
ls -l poetry.lock
```
<img width="749" height="119" alt="image" src="https://github.com/user-attachments/assets/5bab560d-ee50-4552-8f91-f0b8bde4b925" />

Optionally inspect the beginning of the file:

```bash
head -30 poetry.lock
```

<img width="1342" height="1013" alt="image" src="https://github.com/user-attachments/assets/25b17102-6c81-4aab-83e3-d419a04bb8f2" />

## Step 3: Application Deployment / Execution

### Run Python Using Poetry

```bash
poetry run python --version
```

This confirms that Python is being executed from the Poetry-managed environment.

<img width="1100" height="124" alt="image" src="https://github.com/user-attachments/assets/78ced5ac-5f8e-4fb1-82b0-e41d44ae46a4" />

### Verify Dependency Inside the Poetry Environment

```bash
poetry run python -c "import requests; print(requests.__version__)"
```

<img width="1135" height="140" alt="image" src="https://github.com/user-attachments/assets/d7269a54-c6b6-4ef9-aa02-55ff05cae2ca" />

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

<img width="774" height="220" alt="image" src="https://github.com/user-attachments/assets/18c1a82b-4f85-43af-a55c-f7d41c7d470f" />

### Run the Python Application

```bash
poetry run python app.py
```

Expected result:

```text
Status Code: 200
```

The exact status code may vary depending on the external endpoint response.

<img width="1031" height="144" alt="image" src="https://github.com/user-attachments/assets/44d17516-8d39-4de2-a122-fff3cbd9dfc7" />

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

<img width="1067" height="86" alt="image" src="https://github.com/user-attachments/assets/d75f3df2-5916-4faf-a8f1-ac5d83cdd532" />

### Remove a Dependency

```bash
poetry remove requests
```

This removes the dependency and updates the project dependency information and lock file.

<img width="618" height="249" alt="image" src="https://github.com/user-attachments/assets/6543e3f7-c4ed-4ca4-8f69-278817ffa7d2" />

### Verify Dependency Removal

```bash
poetry show
```

Or:

```bash
poetry show requests
```

<img width="938" height="103" alt="image" src="https://github.com/user-attachments/assets/95f70d45-253c-466a-9dc0-047053f540d1" />

### Validate the Project

```bash
poetry check
```

This checks the Poetry project configuration.

<img width="705" height="278" alt="image" src="https://github.com/user-attachments/assets/275672a1-8a15-468d-bb7d-c7985e5fe553" />

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
<img width="869" height="112" alt="image" src="https://github.com/user-attachments/assets/514d0d14-ec53-413d-8d37-384c05690c21" />


<img width="1038" height="225" alt="image" src="https://github.com/user-attachments/assets/f6e987a7-a2e2-4d9a-9b80-eb47cdcc6c3c" />

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

<img width="758" height="401" alt="image" src="https://github.com/user-attachments/assets/560e7ecf-dad5-4a22-867d-6b75b5abd1bb" />

### Issue 3: Python Syntax Error

Inspect the Python file:

```bash
cat app.py
```

Correct the code and run:

```bash
poetry run python app.py
```

<img width="899" height="229" alt="image" src="https://github.com/user-attachments/assets/875844d2-3287-47f1-ba03-f2e0b3bf9dc1" />

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
## Contact Information

| Name | Email |
|------|-------|
| Sandeep Rawat  | sandeep.rawat.snaatak@mygurukulam.com |

## References

| **Reference** | **Description** |
|---|---|
| Poetry documentation | Official documentation for Poetry installation and usage |
| Python documentation | Python language and runtime reference |
| PyPI | Python package repository used for package distribution |
