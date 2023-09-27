# GitHub Actions Workflow Readme

This repository contains a GitHub Actions workflow named "My First GitHub Actions" that automates tasks related to a Python project. The workflow is triggered by the "push" event, meaning it runs automatically whenever changes are pushed to the repository.

## Workflow Details

### Workflow Trigger

The workflow is triggered by the "push" event, which means it will be executed whenever there are new commits pushed to the repository.

```yaml
on: [push] # On every commit, this job will run
```

### Jobs and Runner Environment

The workflow defines a job named "build," which is configured to run on a self-hosted runner. This runner is set up to use an EC2 instance, providing flexibility in the execution environment.

```yaml
jobs:
  build:
    runs-on: self-hosted # Self-hosted runner, configured to use an EC2 instance.
```

### Python Version Matrix

The "build" job employs a strategy matrix to run the job with different Python versions. Specifically, it runs the job for Python 3.8 and Python 3.9, resulting in two separate jobs.

```yaml
strategy:
  matrix:
    python-version: [3.8, 3.9] # There would be 2 jobs for 3.8 and 3.9 versions
```

### Job Steps

The "build" job consists of multiple steps, each with a specific purpose:

#### Step 1: Checkout Code

This step checks out the code from the repository, making it available for subsequent actions.

```yaml
- uses: actions/checkout@v3 # Step 1: Checkout the code from the repository.
```

#### Step 2: Set up Python Environment

This step sets up the Python environment using the specified Python version from the matrix.

```yaml
- name: Set up Python # Step 2: Set up the Python environment.
  uses: actions/setup-python@v2
  with:
    python-version: ${{ matrix.python-version }} # Use the Python version from the matrix.
```

#### Step 3: Install Python Dependencies

In this step, the workflow upgrades pip to the latest version and installs the pytest package, which is a common Python testing framework.

```yaml
- name: Install dependencies # Step 3: Install Python dependencies.
  run: |
    python -m pip install --upgrade pip # Upgrade pip to the latest version.
    pip install pytest # Install the pytest package.
```

#### Step 4: Run Tests

The final step changes the working directory to the "src" folder and runs the pytest command on the "addition.py" file to execute the project's tests.

```yaml
- name: Run tests # Step 4: Run tests.
  run: |
    cd src # Change the working directory to the 'src' folder.
    python -m pytest addition.py # Run pytest on the 'addition.py' file.
```

This GitHub Actions workflow streamlines the testing process for Python code, allowing you to easily test your project with different Python versions and ensuring code quality with pytest.