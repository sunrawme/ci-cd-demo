<div align="center">

# CI Workflow – Standard Operating Procedure

<img width="700" height="350" alt="CI Workflow POC"
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

This Proof of Concept demonstrates a basic Continuous Integration (CI) workflow using:

- Git
- GitHub
- Jenkins
- Python
- Pytest

The CI pipeline automatically checks out source code, installs dependencies, runs automated tests, executes the application/build step, and reports the final pipeline result.

---

# 2. Objective

The objective of this POC is to demonstrate a basic CI workflow that automatically:

1. Checks out source code from GitHub.
2. Installs required dependencies.
3. Runs automated tests.
4. Runs the application/build step.
5. Reports the pipeline result as SUCCESS or FAILURE.

---

# 3. Prerequisites

| **Prerequisite** | **Details** |
| ---------------- | ----------- |
| Operating System | Linux/Ubuntu |
| Required Software | Git, Python 3, pip, Java |
| Required Tools | Jenkins, Pytest |
| Access | Linux terminal, GitHub repository, Jenkins |
| Permissions | Permission to create files, Git repository, and run Jenkins builds |
| Dependencies | Internet connectivity |

## 3.1 Verify Git

```bash
git --version
```
---

## 3.2 Verify Python

```bash
python3 --version
```
---

## 3.3 Verify pip

```bash
pip3 --version
```
---

## 3.4 Verify Java

```bash
java --version
```
---

## 3.5 Verify Jenkins

```bash
sudo systemctl status jenkins
```
Jenkins should show:

```text
Active: active (running)
```

---

# 4. Project Setup

## 4.1 CI Workflow

The overall CI workflow is:

```text
Developer
    |
    | git push
    ↓
GitHub Repository
    |
    ↓
Jenkins
    |
    ↓
Checkout Code
    |
    ↓
Install Dependencies
    |
    ↓
Run Tests
    |
    ↓
Build / Execute Application
    |
    ↓
SUCCESS / FAILURE
```
<img width="918" height="715" alt="image" src="https://github.com/user-attachments/assets/83d3e2ca-2f7e-4729-b422-76f07639dde7" />


---

## 4.2 Create Project Directory

Create the project:

```bash
mkdir -p ~/ci-demo
cd ~/ci-demo
```

Verify:

```bash
pwd
ls -la
```

<img width="780" height="206" alt="image" src="https://github.com/user-attachments/assets/283bc955-24e3-4f5c-8c3e-472b0868aa17" />


---

## 4.3 Initialize Git Repository

Run:

```bash
git init
```

Expected:

```text
Initialized empty Git repository
```

Check status:

```bash
git status
```

<img width="742" height="333" alt="image" src="https://github.com/user-attachments/assets/d9a343c5-399b-4629-b2ba-450ebe770afd" />


---

## 4.4 Create Python Application

Create the application:

```bash
nano app.py
```

<img width="624" height="209" alt="image" src="https://github.com/user-attachments/assets/429b5998-9e25-4eba-8266-da51549301f6" />


---

## 4.5 Create Test File

Create:

```bash
nano test_app.py
```

<img width="855" height="194" alt="image" src="https://github.com/user-attachments/assets/b51aa1c6-223f-4c15-875c-01949d63bb54" />


---

## 4.6 Install Pytest

Install pytest:

```bash
python3 -m pip install --user pytest
```

Verify:

```bash
python3 -m pytest --version
```

<img width="877" height="667" alt="image" src="https://github.com/user-attachments/assets/c090b6fe-9b29-4009-84f9-fee79de368da" />


---

# 5. Implementation / Execution

## 5.1 Run Tests Locally

Before configuring CI, verify that the code works locally.

Run:

```bash
python3 -m pytest
```

Expected:

```text
1 passed
```

<img width="972" height="226" alt="image" src="https://github.com/user-attachments/assets/a20861f2-2506-49c0-9619-6f0503bb2725" />


---

## 5.2 Run Application Locally

Run:

```bash
python3 app.py
```

Expected:

```text
30
```

<img width="858" height="130" alt="image" src="https://github.com/user-attachments/assets/6a350132-88fb-47e2-b6d1-54dffd8e5cd9" />


---

## 5.3 Create .gitignore

Create:

```bash
nano .gitignore
```

<img width="822" height="195" alt="image" src="https://github.com/user-attachments/assets/02fa5f83-811d-43da-8fa3-774ee53abb35" />


---

## 5.4 Check Git Status

Run:

```bash
git status
```

<img width="670" height="263" alt="image" src="https://github.com/user-attachments/assets/60948dee-952e-4012-bb60-82ac59821279" />


---

## 5.5 Add Files to Git

Run:

```bash
git add .
```

Verify:

```bash
git status
```

<img width="553" height="233" alt="image" src="https://github.com/user-attachments/assets/8b2c9b05-f833-4b48-b925-634060693a6f" />


---

## 5.6 Commit the Code

Run:

```bash
git commit -m "Add CI demo application"
```

Verify:

```bash
git log --oneline
```

<img width="1303" height="269" alt="image" src="https://github.com/user-attachments/assets/93ec7b90-1813-4a8c-8cdc-562a10f2e045" />


---

## 5.7 Connect GitHub Repository

Create an empty repository on GitHub.

Configure the remote:

```bash
git remote add origin <GITHUB_REPOSITORY_URL>
```

Verify:

```bash
git remote -v
```

Rename the branch:

```bash
git branch -M main
```

Push the code:

```bash
git push -u origin main
```

<img width="692" height="239" alt="image" src="https://github.com/user-attachments/assets/f2d629f0-494f-4c1e-838a-3b8bd78e9919" />


---

## 5.8 Verify GitHub Repository

Open the GitHub repository and verify that the following files are available:

```text
app.py
test_app.py
.gitignore
```

<img width="752" height="395" alt="image" src="https://github.com/user-attachments/assets/9dc8fae1-588e-41d6-ab4e-81db2d19564a" />


---

## 5.9 Create Jenkins Pipeline

Open Jenkins in the browser.

Go to:

```text
Jenkins
   ↓
New Item
```

Enter:

```text
ci-demo
```

Select:

```text
Pipeline
```

Click:

```text
OK
```

<img width="751" height="462" alt="image" src="https://github.com/user-attachments/assets/adc911fb-9206-46a4-b06c-9c38689b83dc" />


---

## 5.10 Configure Jenkins Pipeline

Open:

```text
ci-demo
    ↓
Configure
```

Scroll to the Pipeline section.

Select:

```text
Definition: Pipeline script
```

Add the following Pipeline:

```groovy
pipeline {
    agent any

    stages {

        stage('Checkout') {
            steps {
                git branch: 'main',
                    url: '<GITHUB_REPOSITORY_URL>'
            }
        }

        stage('Install Dependencies') {
            steps {
                sh 'python3 -m pip install --user pytest'
            }
        }

        stage('Run Tests') {
            steps {
                sh 'python3 -m pytest'
            }
        }

        stage('Build') {
            steps {
                sh 'python3 app.py'
            }
        }
    }

    post {
        success {
            echo 'CI Pipeline completed successfully.'
        }

        failure {
            echo 'CI Pipeline failed.'
        }
    }
}
```

Click:

```text
Apply
Save
```

**Screenshot:**

<img width="896" height="513" alt="image" src="https://github.com/user-attachments/assets/3ab9753a-c052-4bc3-8bea-0e49dd0c7cef" />


---

## 5.11 Run Jenkins Pipeline

Open:

```text
Jenkins
    ↓
ci-demo
```

Click:

```text
Build Now
```

Jenkins will start the CI pipeline.

**Screenshot:**

<img width="646" height="507" alt="image" src="https://github.com/user-attachments/assets/44031ae6-0a51-4087-9042-dc9f476ca9ed" />


---

## 5.12 Checkout Stage

The first stage is:

```text
Checkout
```

Jenkins downloads the source code from GitHub.

Pipeline command:

```groovy
git branch: 'main',
    url: '<GITHUB_REPOSITORY_URL>'
```

<img width="694" height="528" alt="image" src="https://github.com/user-attachments/assets/96c0698b-b285-4c05-b1a0-2f2e909d94c0" />


Expected Result:

```text
Source code is successfully checked out from GitHub.
```

---

## 5.13 Install Dependencies Stage

Jenkins executes:

```bash
python3 -m pip install --user pytest
```

This installs the dependency required for testing.

<img width="825" height="282" alt="image" src="https://github.com/user-attachments/assets/5cd52679-6cd9-427c-8def-11284a901798" />


---

## 5.14 Run Tests Stage

Jenkins executes:

```bash
python3 -m pytest
```

Expected:

```text
1 passed
```

<img width="847" height="188" alt="image" src="https://github.com/user-attachments/assets/5badb5ff-15e3-49ea-9379-0cad2192a63c" />


---

## 5.15 Build Stage

Jenkins executes:

```bash
python3 app.py
```

Expected:

```text
30
```

<img width="784" height="95" alt="image" src="https://github.com/user-attachments/assets/aaf9eb70-79a1-46da-89fc-f89da302e98b" />


---

## 5.16 Check Console Output

Open:

```text
ci-demo
    ↓
Build #
    ↓
Console Output
```

Verify all stages:

```text
Checkout
Install Dependencies
Run Tests
Build
```

At the end, Jenkins should display:

```text
Finished: SUCCESS
```

<img width="593" height="269" alt="image" src="https://github.com/user-attachments/assets/6c233e47-d27e-4bec-97b8-5aa05c842578" />


---

# 6. Validation

## 6.1 Test CI Failure

To verify that CI catches errors, intentionally modify the test.

Open:

```bash
nano test_app.py
```

Change:

```python
assert add(10, 20) == 30
```

to:

```python
assert add(10, 20) == 50
```

Save the file.

Run the test locally:

```bash
python3 -m pytest
```

Expected:

```text
FAILED
```

<img width="738" height="308" alt="image" src="https://github.com/user-attachments/assets/c2239f6c-cc7a-44e8-9b31-3833a0dedbbb" />


---

## 6.2 Commit the Failure Test

Run:

```bash
git add .
git commit -m "Test CI failure"
git push
```

<img width="978" height="267" alt="image" src="https://github.com/user-attachments/assets/75b5324a-cc32-4190-846a-a254cd2193d9" />


---

## 6.3 Run Jenkins Again

Open Jenkins:

```text
ci-demo
    ↓
Build Now
```

Jenkins will execute the pipeline again.

The `Run Tests` stage should fail.

Expected:

```text
FAILED
```

Final result:

```text
Finished: FAILURE
```

<img width="492" height="291" alt="image" src="https://github.com/user-attachments/assets/6191d1d4-2c1c-4d8b-a947-8ab55b38b6e2" />


---

## 6.4 Fix the Test

Change the test back:

```python
assert add(10, 20) == 30
```

Run:

```bash
python3 -m pytest
```

Expected:

```text
1 passed
```

Commit and push:

```bash
git add .
git commit -m "Fix CI test"
git push
```

<img width="692" height="319" alt="image" src="https://github.com/user-attachments/assets/bb265d13-808a-42be-9120-79d45fb031b5" />


---

## 6.5 Run Jenkins After Fix

Run:

```text
Jenkins
    ↓
ci-demo
    ↓
Build Now
```

Verify:

```text
Checkout       SUCCESS
Install        SUCCESS
Run Tests      SUCCESS
Build          SUCCESS
```

Final result:

```text
Finished: SUCCESS
```
<img width="736" height="358" alt="image" src="https://github.com/user-attachments/assets/7181bd41-93f9-4cbc-b77e-e0968b95c9c3" />


---

## 6.6 Validation Checklist

| **Validation** | **Expected Result** |
| -------------- | ------------------- |
| Git verification | Git version is displayed |
| Python verification | Python version is displayed |
| pip verification | pip version is displayed |
| Java verification | Java version is displayed |
| Jenkins verification | Jenkins service is active |
| Git repository initialization | Repository is initialized |
| Local pytest execution | Test passes |
| Local application execution | Application returns `30` |
| GitHub push | Code is available in GitHub |
| Jenkins Checkout | Source code is checked out |
| Install Dependencies | Pytest is installed |
| Run Tests | Tests pass |
| Build | Application executes successfully |
| Successful pipeline | `Finished: SUCCESS` |
| Failure test | Pipeline fails at Run Tests |
| Fixed test | Pipeline returns `Finished: SUCCESS` |

---

# 7. Observations

## 7.1 Successfully Executed Components

| **Component / Module** | **Status** |
| ---------------------- | ---------- |
| Git verification | PASS |
| Python verification | PASS |
| pip verification | PASS |
| Java verification | PASS |
| Jenkins service verification | PASS |
| Git repository initialization | PASS |
| Python application creation | PASS |
| Test file creation | PASS |
| Pytest installation | PASS |
| Local test execution | PASS |
| Local application execution | PASS |
| GitHub repository push | PASS |
| Jenkins pipeline creation | PASS |
| Checkout stage | PASS |
| Install Dependencies stage | PASS |
| Run Tests stage | PASS |
| Build stage | PASS |
| Successful CI pipeline | PASS |
| CI failure detection | PASS |
| CI recovery after test fix | PASS |

---

## 7.2 Failed Components

| **Component / Module** | **Issue** |
| ---------------------- | --------- |
| Run Tests stage during failure test | Intentionally failed because the expected result was changed from `30` to `50`. |
| Jenkins pipeline during failure test | Intentionally returned `Finished: FAILURE`. |

---

## 7.3 Key Findings

- Jenkins successfully checks out source code from GitHub.
- Required Python dependencies are installed during the pipeline.
- Automated Pytest tests are executed by Jenkins.
- The application/build step executes after successful tests.
- A failed test causes the Jenkins pipeline to fail.
- Correcting the failed test allows the pipeline to return to SUCCESS.
- The workflow provides a repeatable CI validation process.

---

# 8. Use Cases

| **Scenario** | **Commands / Actions** |
| ------------ | ---------------------- |
| Verify Git | `git --version` |
| Verify Python | `python3 --version` |
| Verify pip | `pip3 --version` |
| Verify Java | `java --version` |
| Verify Jenkins | `sudo systemctl status jenkins` |
| Run local tests | `python3 -m pytest` |
| Run application | `python3 app.py` |
| Push source code | `git push -u origin main` |
| Create Jenkins pipeline | Jenkins → New Item → Pipeline |
| Execute CI | Jenkins → ci-demo → Build Now |
| Review result | Jenkins → Build # → Console Output |
| Test failure handling | Modify test and run Jenkins again |
| Validate recovery | Fix test, push, and run Jenkins again |

---

# 9. Troubleshooting

| **Issue** | **Cause** | **Solution** |
| --------- | --------- | ------------ |
| Git command not found | Git is not installed | Install Git and verify with `git --version` |
| Python command not found | Python 3 is unavailable | Install/enable Python 3 and verify with `python3 --version` |
| Pytest command fails | Pytest is not installed | Run `python3 -m pip install --user pytest` |
| Jenkins service is inactive | Jenkins service is stopped | Check `sudo systemctl status jenkins` and start the service |
| GitHub checkout fails | Repository URL/branch configuration is incorrect | Verify repository URL and `main` branch configuration |
| Jenkins test stage fails | Python test is failing | Run `python3 -m pytest` locally and correct the test/code |
| Jenkins build fails | Application/build command failed | Review Console Output and run `python3 app.py` locally |
| Pipeline returns FAILURE | One or more pipeline stages failed | Review the failed stage in Jenkins Console Output |

---

# 10. Best Practices

| **Best Practice** | **Description** |
| ----------------- | --------------- |
| Test locally first | Run Pytest locally before configuring or running CI. |
| Keep tests in source control | Store application tests such as `test_app.py` with the project. |
| Validate dependencies | Install required dependencies before running automated tests. |
| Review Console Output | Use Jenkins Console Output to identify failed stages. |
| Test failure handling | Intentionally validate that a failing test causes a failed CI build. |
| Fix and rerun | Correct the failing test and confirm that the pipeline returns to SUCCESS. |
| Keep pipeline stages clear | Separate Checkout, Install Dependencies, Run Tests, and Build activities. |

---

# 11. Conclusion

This Proof of Concept demonstrates a basic Continuous Integration workflow using Git, GitHub, Jenkins, Python, and Pytest.

The implementation validates:

- Source code checkout from GitHub.
- Dependency installation.
- Automated test execution.
- Application/build execution.
- Jenkins SUCCESS reporting.
- Jenkins FAILURE reporting when tests fail.
- Recovery to SUCCESS after the test is corrected.

The CI workflow provides a repeatable and automated process for validating application code before it moves to the next stage of the software delivery lifecycle.

---

# 12. Contact Information

| **Name** | **Email** |
| -------- | --------- |
| Sandeep Rawat | sandeep.rawat.snaatak@mygurukulam.com |

---

# 13. References

| **Resource** | **Link** |
| ------------ | -------- |
| Git Documentation | <GIT_DOCUMENTATION_URL> |
| GitHub Repository | <GITHUB_REPOSITORY_URL> |
| Jenkins Documentation | <JENKINS_DOCUMENTATION_URL> |
| Python Documentation | <PYTHON_DOCUMENTATION_URL> |
| Pytest Documentation | <PYTEST_DOCUMENTATION_URL> |

---

## Screenshot Replacement Guide

Replace each `<SCREENSHOT_...>` placeholder with the corresponding screenshot path or repository-relative image path.

Suggested screenshot structure:

```text
screenshots/
├── git-version.png
├── python-version.png
├── pip-version.png
├── java-version.png
├── jenkins-status.png
├── ci-workflow.png
├── project-directory.png
├── git-init.png
├── app-py.png
├── test-app-py.png
├── pytest-install.png
├── local-test.png
├── app-execution.png
├── gitignore.png
├── git-status.png
├── git-add.png
├── git-commit.png
├── github-push.png
├── github-repository.png
├── jenkins-new-item.png
├── jenkins-pipeline-config.png
├── jenkins-build-now.png
├── checkout-stage.png
├── install-stage.png
├── run-tests-stage.png
├── build-stage.png
├── console-success.png
├── failure-local.png
├── failure-commit.png
├── jenkins-failure.png
├── fix-test.png
└── final-success.png
```
