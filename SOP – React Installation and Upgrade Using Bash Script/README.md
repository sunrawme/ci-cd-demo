<div align="center">

# React Installation and Upgrade Using Bash Script

<img width="700" height="350" alt="React Installation and Upgrade POC"
src="<IMAGE_URL>" />

</div>

---

# Author Table

| **Author** | **Created On** | **Version** | **Last Updated By** | **Last Edited On** | **L0 Reviewer** | **L1 Reviewer** | **L2 Reviewer** |
| ---------- | -------------- | ----------- | ------------------- | ------------------ | --------------- | --------------- | --------------- |
| Sandeep Rawat | 25-08-2026 | v1.0 | <Author Name> | 25-08-2026 | <L0 Reviewer> | <L1 Reviewer> | <L2 Reviewer> |

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

This Proof of Concept demonstrates a generic Bash-based approach for installing, managing, upgrading, and verifying multiple React major versions.

The implementation uses:

- Ubuntu/Linux
- Bash
- Node.js
- npm
- React
- React DOM

The goal of this POC is to provide a reusable and controlled interface for React installation and upgrade instead of manually executing multiple npm commands.

---

# 2. Objective

The objective of this POC is to:

- Check Node.js and npm prerequisites.
- Verify the Bash environment.
- Create a React project.
- Install a selected React major version.
- Support multiple approved React major versions.
- Upgrade React between supported major versions.
- Verify the installed React and React DOM versions.
- Reject unsupported React versions.
- Handle invalid commands and display usage information.

---

# 3. Prerequisites

| **Prerequisite** | **Details** |
| ---------------- | ----------- |
| Operating System | Ubuntu/Linux |
| Required Software | Node.js, npm |
| Required Tools | Bash |
| Access | Linux terminal / shell access |
| Permissions | Permission to create files and install npm packages |
| Dependencies | Internet connectivity, Node.js, npm |

### 3.1 Verify Operating System

```bash
cat /etc/os-release
```

Expected result:

```text
Ubuntu/Linux operating system information
```

<img width="938" height="349" alt="image" src="https://github.com/user-attachments/assets/b23dbe9c-8359-4d96-8e5a-6da43830c835" />


---

### 3.2 Verify Bash

```bash
bash --version
```

Expected result:

```text
GNU bash version information
```

<img width="914" height="209" alt="image" src="https://github.com/user-attachments/assets/49ce84ab-6949-4b92-b04c-3b1b693cf2be" />


---

### 3.3 Verify Node.js

React applications require Node.js.

```bash
node --version
```
<img width="969" height="125" alt="image" src="https://github.com/user-attachments/assets/50e63d6d-1281-4c01-bfc4-f57a35a0467f" />

---

### 3.4 Verify npm

```bash
npm --version
```

Expected result:

```text
Installed npm version
```

<img width="723" height="100" alt="image" src="https://github.com/user-attachments/assets/f333de69-04be-41a5-a722-c2d35369f43c" />


---

# 4. Project Setup

## 4.1 Create Working Directory

Create a directory for the React projects:

```bash
mkdir -p ~/react-projects
cd ~/react-projects
```

Verify the working directory:

```bash
pwd
```

Expected:

```text
/home/<user>/react-projects
```

<img width="888" height="186" alt="image" src="https://github.com/user-attachments/assets/0d93375a-ed2b-493e-ad10-a78b61a1ec43" />


---

## 4.2 Create the Bash Installation Script

Create the script:

```bash
nano install-react.sh
```

Add the following script:

```bash
#!/bin/bash

set -e

SUPPORTED_VERSIONS=("17" "18" "19")

show_usage() {
    echo
    echo "React Installation Script"
    echo
    echo "Usage:"
    echo "  $0 install <react-version>"
    echo "  $0 upgrade <react-version>"
    echo "  $0 verify"
    echo "  $0 help"
    echo
    echo "Supported React versions:"
    printf '  %s\n' "${SUPPORTED_VERSIONS[@]}"
    echo
}

check_prerequisites() {
    echo "Checking prerequisites..."

    if ! command -v node >/dev/null 2>&1; then
        echo "ERROR: Node.js is not installed."
        exit 1
    fi

    if ! command -v npm >/dev/null 2>&1; then
        echo "ERROR: npm is not installed."
        exit 1
    fi

    echo "Node.js version: $(node --version)"
    echo "npm version: $(npm --version)"
    echo
}

validate_version() {
    local version="$1"

    for supported in "${SUPPORTED_VERSIONS[@]}"; do
        if [[ "$version" == "$supported" ]]; then
            return 0
        fi
    done

    echo "ERROR: Unsupported React version: $version"
    echo "Supported versions: ${SUPPORTED_VERSIONS[*]}"
    exit 1
}

install_react() {
    local version="$1"

    validate_version "$version"
    check_prerequisites

    echo "Installing React $version..."

    npm install "react@^${version}.0.0" "react-dom@^${version}.0.0"

    echo
    echo "React $version installation completed."
}

upgrade_react() {
    local version="$1"

    validate_version "$version"
    check_prerequisites

    echo "Upgrading React to version $version..."

    npm install "react@^${version}.0.0" "react-dom@^${version}.0.0"

    echo
    echo "React upgrade completed."
}

verify_react() {
    check_prerequisites

    echo "Checking installed React version..."

    npm list react react-dom --depth=0
}

case "$1" in

    install)
        if [[ -z "$2" ]]; then
            echo "ERROR: React version is required."
            show_usage
            exit 1
        fi

        install_react "$2"
        ;;

    upgrade)
        if [[ -z "$2" ]]; then
            echo "ERROR: React version is required."
            show_usage
            exit 1
        fi

        upgrade_react "$2"
        ;;

    verify)
        verify_react
        ;;

    help|"")
        show_usage
        ;;

    *)
        echo "ERROR: Invalid command."
        show_usage
        exit 1
        ;;

esac
```

Save the file:

```text
Ctrl + O
Enter
Ctrl + X
```

<img width="1183" height="231" alt="image" src="https://github.com/user-attachments/assets/927ff377-84b5-400c-8646-338d15fc7efd" />


---

## 4.3 Make the Script Executable

```bash
chmod +x install-react.sh
```

Verify:

```bash
ls -l install-react.sh
```

Expected permissions should include `x`.

<img width="1183" height="231" alt="image" src="https://github.com/user-attachments/assets/bde2fad1-9475-44eb-b5d1-545985debfda" />



---

## 4.4 Check the Script Syntax

Before executing the script, check the Bash syntax:

```bash
bash -n install-react.sh
```

If there is no output, the syntax check passed.

Expected result:

```text
No output / no syntax errors
```

<img width="1328" height="67" alt="image" src="https://github.com/user-attachments/assets/a4b75b24-f280-4967-b2ba-e2f58607b7bf" />



---

# 5. Implementation / Execution

## 5.1 Display Script Help

Run:

```bash
./install-react.sh help
```

Expected result:

```text
React Installation Script

Usage:
  ./install-react.sh install <react-version>
  ./install-react.sh upgrade <react-version>
  ./install-react.sh verify
  ./install-react.sh help

Supported React versions:
  17
  18
  19
```
<img width="748" height="325" alt="image" src="https://github.com/user-attachments/assets/06f50462-8cab-420f-8baf-c6acca224b7e" />



---

## 5.2 Create a React Project

Create the project directory:

```bash
mkdir react-demo
cd react-demo
```

Initialize npm:

```bash
npm init -y
```

Verify:

```bash
ls -la
```

Expected:

```text
package.json
```

<img width="777" height="350" alt="image" src="https://github.com/user-attachments/assets/55eec8d0-db4e-4f2b-bf69-d2050d4d2dbc" />
>


---

## 5.3 Install React 18

Run the Bash script from the project directory.

If the script is located in the parent directory:

```bash
../install-react.sh install 18
```

Expected result:

```text
Checking prerequisites...
Node.js version: <version>
npm version: <version>

Installing React 18...
React 18 installation completed.
```
<img width="925" height="240" alt="image" src="https://github.com/user-attachments/assets/a2d5f8c8-2ed2-4cbb-a876-4ceca4f00d27" />



Verify:

```bash
npm list react react-dom --depth=0
```

Expected result should show React 18.x and React DOM 18.x.

<img width="913" height="197" alt="image" src="https://github.com/user-attachments/assets/3fa7fe10-497a-4560-9751-e798930f1cfd" />



---

## 5.4 Install React 19

The same script supports another React major version.

Run:

```bash
../install-react.sh install 19
```

Verify:

```bash
npm list react react-dom --depth=0
```

Expected:

```text
react@19.x
react-dom@19.x
```

<img width="826" height="254" alt="image" src="https://github.com/user-attachments/assets/631b1164-7e4a-4a35-b96a-cf953dc8d5b5" />



---

## 5.5 Upgrade React

Assume the project currently has React 18.

Check the current version:

```bash
npm list react react-dom --depth=0
```
<img width="865" height="148" alt="image" src="https://github.com/user-attachments/assets/7f55bda4-fd4f-4886-bce4-45bcb55181c4" />



Upgrade to React 19:

```bash
../install-react.sh upgrade 19
```

<img width="749" height="221" alt="image" src="https://github.com/user-attachments/assets/ecf5ad2a-0b57-4b84-a455-b46a6062b1a1" />



Verify:

```bash
npm list react react-dom --depth=0
```

Expected:

```text
react@19.x
react-dom@19.x
```

<img width="1333" height="72" alt="image" src="https://github.com/user-attachments/assets/7e7e5a81-05f8-4093-b716-0c06bcdf3330" />



---

## 5.6 Verify React Using the Script

The script also provides a verification command.

Run:

```bash
../install-react.sh verify
```

Expected result:

```text
Checking installed React version...
react@<version>
react-dom@<version>
```

<img width="1281" height="406" alt="image" src="https://github.com/user-attachments/assets/b6007735-ebc2-4847-bd0e-4b64c7fff47c" />



---

## 5.7 Test Invalid React Version

The script should reject unsupported versions.

Run:

```bash
../install-react.sh install 20
```

Expected:

```text
ERROR: Unsupported React version: 20
Supported versions: 17 18 19
```

The project should not install React 20 through this script because it is not included in the approved supported-version list.

<img width="1314" height="134" alt="image" src="https://github.com/user-attachments/assets/fd2cd4e0-c6ef-4385-89c1-b18aff4a17d4" />



---

## 5.8 Test Invalid Command

Run:

```bash
../install-react.sh test
```

Expected:

```text
ERROR: Invalid command.
```

The script should display the usage information.

<img width="537" height="229" alt="image" src="https://github.com/user-attachments/assets/78fa2670-74c8-40b9-a752-4864b4dd02be" />



---

# 6. Validation

## 6.1 Verify package.json

After React installation, check the project configuration:

```bash
cat package.json
```

Expected:

The `dependencies` section should contain React and React DOM.

Example:

```json
"dependencies": {
  "react": "...",
  "react-dom": "..."
}
```
<img width="927" height="378" alt="image" src="https://github.com/user-attachments/assets/47d95409-3078-448a-a53b-96686b3864a1" />


---

## 6.2 Verify Installed Packages

Run:

```bash
npm list --depth=0
```

Expected:

The installed top-level packages should be displayed.

<img width="1234" height="195" alt="image" src="https://github.com/user-attachments/assets/2f361bed-2ce8-4bfb-bed9-09129996ae8d" />


---

## 6.3 Validation Checklist

| **Validation** | **Expected Result** |
| -------------- | ------------------- |
| Operating system check | Linux/Ubuntu information is displayed |
| Bash check | Bash version is displayed |
| Node.js check | Node.js version is displayed |
| npm check | npm version is displayed |
| Script syntax | No syntax errors are reported |
| Script help | Usage and supported versions are displayed |
| React 18 installation | React 18 and React DOM 18 are installed |
| React 19 installation | React 19 and React DOM 19 are installed |
| React upgrade | Existing supported version is upgraded |
| Script verification | Installed React versions are displayed |
| Invalid version | Unsupported version is rejected |
| Invalid command | Usage information is displayed |
| package.json | React dependencies are present |
| Installed packages | Top-level npm packages are displayed |

---

## 6.4 Complete Installation Workflow

```text
Check Ubuntu
     |
     ↓
Check Bash
     |
     ↓
Check Node.js
     |
     ↓
Check npm
     |
     ↓
Create Bash Script
     |
     ↓
Validate Script
     |
     ↓
Create React Project
     |
     ↓
Select React Version
     |
     ↓
Install React
     |
     ↓
Verify Installation
     |
     ↓
Upgrade React
     |
     ↓
Verify Upgrade
```

---

# 7. Observations

## 7.1 Successfully Executed Components

| **Component / Module** | **Status** |
| ---------------------- | ---------- |
| Operating system verification | PASS |
| Bash verification | PASS |
| Node.js verification | PASS |
| npm verification | PASS |
| Bash script syntax validation | PASS |
| Script help | PASS |
| React project creation | PASS |
| React 18 installation | PASS |
| React 18 verification | PASS |
| React 19 installation | PASS |
| React upgrade | PASS |
| React verification | PASS |
| Invalid version handling | PASS |
| Invalid command handling | PASS |
| package.json verification | PASS |
| Installed package verification | PASS |

---

## 7.2 Failed Components

| **Component / Module** | **Issue** |
| ---------------------- | --------- |
| Unsupported React version | The script intentionally rejects versions not present in `SUPPORTED_VERSIONS`. |
| Invalid command | The script intentionally rejects commands other than `install`, `upgrade`, `verify`, and `help`. |

---

## 7.3 Key Findings

- The script validates Node.js and npm before performing React operations.
- React versions are controlled through the `SUPPORTED_VERSIONS` array.
- React and React DOM are installed together.
- The same script can be used for installation, upgrade, and verification.
- Unsupported React versions are rejected.
- Invalid commands display the usage information.
- The process provides a standardized interface for React version management.

---

# 8. Use Cases

| **Scenario** | **Commands / Actions** |
| ------------ | ---------------------- |
| Check prerequisites | `node --version`, `npm --version`, `bash --version` |
| Validate script | `bash -n install-react.sh` |
| Show supported versions | `./install-react.sh help` |
| Install React 18 | `../install-react.sh install 18` |
| Install React 19 | `../install-react.sh install 19` |
| Upgrade to React 19 | `../install-react.sh upgrade 19` |
| Verify React | `../install-react.sh verify` |
| Test unsupported version | `../install-react.sh install 20` |
| Test invalid command | `../install-react.sh test` |

---

# 9. Troubleshooting

| **Issue** | **Cause** | **Solution** |
| --------- | --------- | ------------ |
| Node.js is not installed | `node` command is unavailable | Install Node.js and rerun the script |
| npm is not installed | `npm` command is unavailable | Install npm and rerun the script |
| Unsupported React version | Version is not in `SUPPORTED_VERSIONS` | Use a supported version or update the approved list |
| Invalid command | Command is not recognized by the script | Run `./install-react.sh help` |
| Script permission denied | Script does not have execute permission | Run `chmod +x install-react.sh` |
| Bash syntax error | Script contains a syntax problem | Run `bash -n install-react.sh` and correct the reported issue |
| npm installation failure | Dependency installation could not complete | Check internet connectivity and npm/Node.js prerequisites |

---

# 10. Best Practices

| **Best Practice** | **Description** |
| ----------------- | --------------- |
| Validate prerequisites | Always verify Node.js and npm before installing React. |
| Validate script syntax | Run `bash -n install-react.sh` before execution. |
| Control supported versions | Maintain the approved versions in `SUPPORTED_VERSIONS`. |
| Verify after installation | Use `npm list react react-dom --depth=0` or the script's `verify` command. |
| Test invalid input | Validate unsupported versions and invalid commands before using the script in a live demonstration. |
| Maintain version compatibility | Validate new React versions against the project's Node.js version and application requirements before adding them. |

---

# 11. Conclusion

This Proof of Concept demonstrates React installation and upgrade using a reusable Bash script.

The implementation validates:

- Node.js and npm prerequisites.
- Bash script syntax.
- React project creation.
- Installation of supported React major versions.
- React upgrades.
- Installed React and React DOM versions.
- Invalid React version handling.
- Invalid command handling.

The reusable interface is:

```bash
./install-react.sh install <version>
./install-react.sh upgrade <version>
./install-react.sh verify
./install-react.sh help
```

The script provides a standardized method for React installation and upgrade rather than requiring users to manually execute multiple npm commands.

The supported versions can be extended by updating:

```bash
SUPPORTED_VERSIONS=("17" "18" "19")
```

Additional versions should be added only after validating their compatibility with the project's Node.js version and application requirements.

---

# 12. Contact Information

| **Name** | **Email** |
| -------- | --------- |
| Sandeep Rawat | sandeep.rawat.snaatak@mygurukulam.com |

---

# 13. References

| **Resource** | **Link** |
| ------------ | -------- |
| React Documentation | <REACT_DOCUMENTATION_URL> |
| Node.js Documentation | <NODEJS_DOCUMENTATION_URL> |
| npm Documentation | <NPM_DOCUMENTATION_URL> |
| Bash Documentation | <BASH_DOCUMENTATION_URL> |
| Project Repository | <PROJECT_REPOSITORY_URL> |

---

## Screenshot Replacement Guide

Replace each `<SCREENSHOT_...>` placeholder with the image URL or repository-relative image path after uploading your screenshots.

Suggested screenshot names:

```text
screenshots/
├── os-version.png
├── bash-version.png
├── node-version.png
├── npm-version.png
├── working-directory.png
├── script.png
├── script-permissions.png
├── syntax-validation.png
├── script-help.png
├── project-creation.png
├── react18-install.png
├── react18-verify.png
├── react19.png
├── current-version.png
├── react-upgrade.png
├── upgrade-verify.png
├── script-verify.png
├── invalid-version.png
├── invalid-command.png
├── package-json.png
└── installed-packages.png
```
