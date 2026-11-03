# CI/CD Pipeline for Django Todo App

A DevOps project focused on automating the delivery workflow for a
Django-based Todo application using **GitHub Actions**.

The repository combines application source code, GitHub Actions
workflows, and Kubernetes Helm chart configuration. Its CI/CD task
focuses on cross-platform testing, Docker image publishing, manual
workflow execution, deployment environments, and repository protection.

## Project Goals

-   Run automated unit tests across multiple Python versions and
    operating systems.
-   Build and publish Docker images to Docker Hub.
-   Support manually triggered workflows with `workflow_dispatch`.
-   Select a matrix artifact for deployment.
-   Separate `development` and `staging` deployment environments.
-   Require manual approval before staging deployment.
-   Protect the `main` branch with pull request and CI status-check
    requirements.
-   Cancel outdated workflow runs for the same pull request using
    concurrency controls.

## Technologies

  Area                       Technology
  -------------------------- ------------------------
  CI/CD                      GitHub Actions
  Application                Python, Django
  Test matrix                Python 3.8, Python 3.9
  Operating systems          Ubuntu, Windows
  Containerization           Docker
  Container registry         Docker Hub
  Deployment controls        GitHub Environments
  Credentials                GitHub Secrets
  Kubernetes packaging       Helm
  Local Kubernetes cluster   kind, Kubernetes

## CI/CD Workflow

``` text
       Push / Pull Request / Manual Trigger
                      |
                      v
            +---------------------+
            |     Python CI       |
            |                     |
            | Matrix test jobs:   |
            | Python 3.8 / 3.9    |
            | Ubuntu / Windows    |
            +---------------------+
                      |
                      v
            +---------------------+
            |    Docker Build     |
            +---------------------+
                      |
                      v
            +---------------------+
            | Publish to Docker   |
            | Hub                 |
            +---------------------+
                      |
                      v
            +---------------------+
            | Manual Workflow     |
            | Dispatch            |
            | Select artifact     |
            +---------------------+
                      |
             +--------+--------+
             |                 |
             v                 v
      +-------------+   +----------------+
      | Development |   | Staging        |
      | Environment |   | Environment    |
      +-------------+   +----------------+
                                |
                                v
                       +----------------+
                       | Manual Approval|
                       +----------------+
                                |
                                v
                       Staging Deployment
```

*This diagram summarizes the CI/CD task requirements. Refer to the
workflow definitions in `.github/workflows/` for the exact job
dependencies, triggers, and deployment commands.*

## CI/CD Implementation

### 1. Matrix Testing

GitHub Actions matrix strategy runs the test suite across multiple
combinations:

  Operating System   Python versions
  ------------------ -----------------
  Ubuntu             3.8, 3.9
  Windows            3.8, 3.9

This helps detect compatibility issues across supported Python versions
and operating systems.

### 2. Docker Image Build and Publishing

The pipeline integrates Docker to package the application and publish
the resulting image to Docker Hub.

The process includes:

1.  Checking out the repository.
2.  Preparing the build environment.
3.  Building the Docker image.
4.  Authenticating to Docker Hub using GitHub Secrets.
5.  Pushing the image to the configured Docker Hub repository.

The Docker image repository name is configured through
`DockerImageName`.

Credentials must be stored as GitHub Secrets and must never be committed
to source control.

### 3. Manual Workflow Execution

The workflow supports manual execution through GitHub Actions
`workflow_dispatch`.

Manual input parameters allow the user to choose which matrix artifact
should be used for deployment. Example selections include:

-   `ubuntu-3.8`
-   `ubuntu-3.9`
-   `windows-3.8`
-   `windows-3.9`

The selected value must match an artifact produced by the matrix jobs.

### 4. GitHub Environments

The project uses separate GitHub Environments:

-   **development** --- development deployment environment.
-   **staging** --- staging environment for additional validation before
    release.

Environment-specific secrets are configured in GitHub rather than
hard-coded in workflow files.

The staging environment uses a manual approval gate. A deployment job
targeting staging must wait for approval before it can proceed.

### 5. Branch Protection and Pull Request Validation

The `main` branch is protected to ensure changes pass the required
review and validation process.

The repository configuration includes:

-   Pull requests required before merging into `main`.
-   The `Python CI` job configured as a required status check.
-   CI validation on pull requests.

These rules help prevent changes from being merged before the required
checks pass.

### 6. Workflow Concurrency

Concurrency controls prevent unnecessary parallel workflow executions
for the same pull request.

When a new run starts for the same concurrency group, the previous
in-progress run can be cancelled. This saves CI resources and
prioritizes validation of the latest changes.

## Repository Structure

``` text
CI-CD_todoapp/
├── .github/
│   └── workflows/          # GitHub Actions workflow definitions
├── helm-charts/
│   └── todoapp/             # Helm chart for the Todo application
├── src/                     # Django application source code
├── LICENSE
├── README.md
├── bootstrap.sh             # Kubernetes resource setup script
└── cluster.yml              # kind cluster configuration
```

## GitHub Configuration

Configure the repository settings and secrets referenced by the workflow
YAML files before running the complete pipeline.

### Repository Secrets

Typical credentials include:

-   Docker Hub username.
-   Docker Hub access token.

### GitHub Environments

Create the following environments:

-   `development`
-   `staging`

Add the environment-specific secrets referenced by the deployment jobs.
Configure required reviewers for the staging environment.

### Branch Protection

Configure branch protection for `main`:

-   Require pull requests before merging.
-   Require the `Python CI` status check.
-   Require configured checks to pass before merging.

> Secret names, environment variables, and workflow inputs must match
> the actual definitions in `.github/workflows/`.

## Running the Application Locally

The repository contains a Django Todo application.

### Prerequisites

-   Python 3.8 or later
-   pip

### 1. Clone the repository

``` bash
git clone https://github.com/VadymMelnyk/CI-CD_todoapp.git
cd CI-CD_todoapp
```

### 2. Install dependencies

``` bash
pip install -r requirements.txt
```

### 3. Apply database migrations

``` bash
python manage.py migrate
```

### 4. Start the development server

``` bash
python manage.py runserver
```

Open the application at `http://localhost:8000`.

*If the Django source or `requirements.txt` is located in a subdirectory
in your checkout, run these commands from the appropriate directory.*

## Key DevOps Skills Demonstrated

-   Designing CI/CD workflows with GitHub Actions.
-   Configuring matrix testing across operating systems and Python
    versions.
-   Automating test execution for pull requests.
-   Building and publishing Docker images.
-   Managing registry credentials with GitHub Secrets.
-   Configuring manual workflow triggers and input parameters.
-   Passing and selecting artifacts between workflow jobs.
-   Managing environment-specific deployment configuration.
-   Implementing manual approval gates for staging.
-   Configuring branch protection and required status checks.
-   Managing workflow concurrency and cancelling outdated runs.
-   Organizing Kubernetes application deployment configuration with
    Helm.

## Project Repository

[CI-CD_todoapp on GitHub](https://github.com/VadymMelnyk/CI-CD_todoapp)

## Author

**Vadym Melnyk**

[GitHub Profile](https://github.com/VadymMelnyk)
