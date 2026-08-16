# GitHub Actions – Python CI Example

A simple practical example demonstrating how to use **GitHub Actions** to automatically install dependencies and run Python unit tests whenever code is pushed to the `main` branch or a Pull Request is created against `main`.

This repository is intended as a hands-on example for understanding the basics of **CI (Continuous Integration) using GitHub Actions**.

## 📌 Project Overview

The project contains a simple Python module with basic mathematical operations:

* Addition
* Subtraction

Unit tests are written using **pytest**, and GitHub Actions automatically executes these tests as part of the CI pipeline.

The workflow demonstrates a typical CI process:

```text
Developer
   │
   │ Push / Pull Request
   ▼
GitHub Repository
   │
   ▼
GitHub Actions
   │
   ├── Checkout source code
   │
   ├── Set up Python
   │
   ├── Install dependencies
   │
   └── Run pytest
   │
   ▼
CI Result
   ├── ✅ Tests Passed
   └── ❌ Tests Failed
```

GitHub Actions workflows are YAML files stored inside the `.github/workflows` directory. A workflow is triggered by repository events and executes one or more jobs on a runner.

---

## 🛠️ Technologies Used

* **Python**
* **pytest** – Unit testing framework
* **GitHub Actions** – CI automation
* **GitHub**
* **pip** – Python package management

---

## 📁 Project Structure

```text
appgithubactions/
│
├── .github/
│   └── workflows/
│       └── python-app.yml
│
├── src/
│   ├── __init__.py
│   └── math_operations.py
│
├── tests/
│   ├── __init__.py
│   └── test_operation.py
│
├── .gitignore
├── requirements.txt
├── README.md
└── final+github+action.pdf
```

### Directory Description

| Directory/File       | Description                                  |
| -------------------- | -------------------------------------------- |
| `.github/workflows/` | Contains GitHub Actions workflow definitions |
| `python-app.yml`     | Defines the Python CI pipeline               |
| `src/`               | Contains application/source code             |
| `math_operations.py` | Contains `add()` and `sub()` functions       |
| `tests/`             | Contains unit tests                          |
| `test_operation.py`  | Tests addition and subtraction               |
| `requirements.txt`   | Python dependencies                          |
| `README.md`          | Project documentation                        |

---

## 🧮 Application Code

The application contains two simple functions in `src/math_operations.py`:

```python
def add(a, b):
    return a + b

def sub(a, b):
    return a - b
```

The purpose of these functions is intentionally simple so that the focus remains on understanding the **GitHub Actions CI pipeline** rather than application complexity.

---

## 🧪 Unit Tests

The project uses `pytest` for automated testing.

Example tests include:

```python
from src.math_operations import add, sub

def test_add():
    assert add(2, 3) == 5
    assert add(-1, 1) == 0

def test_sub():
    assert sub(5, 3) == 2
    assert sub(4, 3) == 1
    assert sub(3, 3) == 0
    assert sub(2, 3) == -1
```

The tests verify that the mathematical operations return the expected results.

---

## ⚙️ GitHub Actions Workflow

The workflow is located at:

```text
.github/workflows/python-app.yml
```

The workflow is configured to run when:

* Code is pushed to the `main` branch
* A Pull Request targets the `main` branch

The repository's current workflow uses `ubuntu-latest` as the GitHub-hosted runner and installs the dependencies before executing `pytest`.

### CI Workflow

A recommended version of the workflow is:

```yaml
name: Python CI

on:
  push:
    branches: ["main"]

  pull_request:
    branches: ["main"]

jobs:
  test:
    runs-on: ubuntu-latest

    steps:

      # Step 1: Checkout source code
      - name: Checkout code
        uses: actions/checkout@v4

      # Step 2: Set up Python
      - name: Set up Python
        uses: actions/setup-python@v5
        with:
          python-version: "3.8"

      # Step 3: Install dependencies
      - name: Install dependencies
        run: |
          python -m pip install --upgrade pip
          pip install -r requirements.txt

      # Step 4: Run tests
      - name: Run tests
        run: pytest
```

### Workflow Explanation

#### 1. `name`

```yaml
name: Python CI
```

Defines the name of the workflow displayed in the **Actions** tab.

#### 2. `on`

```yaml
on:
  push:
    branches: ["main"]

  pull_request:
    branches: ["main"]
```

Defines when the workflow should execute.

In this example, the workflow runs for:

```text
Push to main
      │
      ▼
GitHub Actions
```

or:

```text
Pull Request → main
      │
      ▼
GitHub Actions
```

#### 3. `jobs`

```yaml
jobs:
  test:
```

Defines the jobs that GitHub Actions should execute.

This example contains one job called `test`.

#### 4. `runs-on`

```yaml
runs-on: ubuntu-latest
```

Specifies that the job should run on a GitHub-hosted Ubuntu runner.

#### 5. Checkout Code

```yaml
- name: Checkout code
  uses: actions/checkout@v4
```

This downloads/checks out the repository code onto the runner so subsequent steps can access it.

#### 6. Set Up Python

```yaml
- name: Set up Python
  uses: actions/setup-python@v5
  with:
    python-version: "3.8"
```

Installs/configures the required Python version on the runner.

> **Note:** The current repository workflow uses `actions/checkout@v2` in this step. That is not the correct action for configuring Python; `actions/setup-python` should be used instead.

#### 7. Install Dependencies

```yaml
- name: Install dependencies
  run: |
    python -m pip install --upgrade pip
    pip install -r requirements.txt
```

Installs the Python dependencies defined in `requirements.txt`.

The current repository specifies:

```text
pandas
pytest
```

#### 8. Run Tests

```yaml
- name: Run tests
  run: pytest
```

Executes all pytest tests.

If all tests pass:

```text
✅ CI Pipeline Passed
```

If any test fails:

```text
❌ CI Pipeline Failed
```

---

## ▶️ Running the Project Locally

### 1. Clone the repository

```bash
git clone https://github.com/Rishib-tech/appgithubactions.git
cd appgithubactions
```

### 2. Create a virtual environment

Windows:

```bash
python -m venv venv
venv\Scripts\activate
```

Linux/macOS:

```bash
python3 -m venv venv
source venv/bin/activate
```

### 3. Install dependencies

```bash
python -m pip install --upgrade pip
pip install -r requirements.txt
```

### 4. Run the tests

```bash
pytest
```

Expected result:

```text
============================= test session starts =============================
...
collected tests

============================== test result ================================
```

---

## 🚀 How GitHub Actions Works in This Example

Once the project is pushed to GitHub, the CI process becomes automatic.

For example:

```text
1. Developer changes Python code
              │
              ▼
2. git add / git commit
              │
              ▼
3. git push
              │
              ▼
4. GitHub receives push
              │
              ▼
5. GitHub Actions detects event
              │
              ▼
6. Ubuntu runner starts
              │
              ▼
7. Repository is checked out
              │
              ▼
8. Python environment is configured
              │
              ▼
9. Dependencies are installed
              │
              ▼
10. pytest executes
              │
              ▼
11. CI result
       ┌──────┴──────┐
       ▼             ▼
   ✅ PASS         ❌ FAIL
```

This is the fundamental idea behind **Continuous Integration** with GitHub Actions. GitHub Actions can automate build, test, package, release, and deployment activities directly from a repository.

---

## 🔍 What You Can Learn From This Project

This small project demonstrates several important GitHub Actions concepts:

### GitHub Actions

Learn how to create and configure a workflow using YAML.

### Workflow Triggers

Understand how events such as:

```yaml
push:
pull_request:
```

can trigger automated workflows.

### Jobs

Understand how a workflow is divided into jobs.

### Runners

Understand how GitHub-hosted environments such as:

```yaml
ubuntu-latest
```

are used to execute workflows.

### Actions

Understand how reusable actions such as:

```yaml
actions/checkout
actions/setup-python
```

are used inside workflows.

### Dependency Installation

Learn how CI environments install project dependencies before testing.

### Automated Testing

Learn how pytest can be integrated into a CI pipeline.

### CI Quality Gate

The pipeline provides an automated validation step before code is considered ready to merge.

---

## 🎯 Key Takeaway

The main purpose of this repository is to demonstrate the basic **CI lifecycle using GitHub Actions**:

```text
Code
  ↓
Commit
  ↓
Push / Pull Request
  ↓
GitHub Actions
  ↓
Checkout
  ↓
Setup Environment
  ↓
Install Dependencies
  ↓
Run Tests
  ↓
Pass / Fail
```

This pattern can be extended for real-world projects by adding:

* Code linting
* Code formatting checks
* Unit tests
* Integration tests
* Security scanning
* Docker image building
* Artifact generation
* Deployment
* Environment-specific deployments
* Continuous deployment (CD)

---

## 👨‍💻 Author

**Rishi Bhardwaj**

GitHub: [Rishib-tech](https://github.com/Rishib-tech)

Repository: [appgithubactions](https://github.com/Rishib-tech/appgithubactions)

---

## 📄 License

This project is intended primarily as a learning and demonstration project for GitHub Actions and Python CI.
