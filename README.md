# Project 02: Secure CI/CD Pipeline with Quality Gates

## About the Project

In this project, I worked on adding automated CI/CD and security checks to the Tabulae inventory management application.

The main goal was to understand how GitHub Actions can automatically test code, check frontend quality, validate Docker builds, and identify security issues whenever code is pushed to GitHub.

I also worked through pipeline failures and fixed them until the workflows were passing successfully.

## Tools Used

- **GitHub Actions** — Automating CI and security workflows
- **Python and Pytest** — Running backend tests
- **PostgreSQL** — Providing the database required by backend tests
- **Node.js and npm** — Checking and building the frontend
- **ESLint** — Detecting frontend code-quality issues
- **Docker** — Validating Docker image builds
- **Gitleaks** — Scanning for exposed secrets
- **pip-audit** — Checking Python dependencies for known vulnerabilities
- **npm audit** — Checking frontend dependencies for known vulnerabilities
- **Git and GitHub** — Version control and workflow execution

## 1. Continuous Integration Pipeline

I created the workflow at `.github/workflows/ci.yml`.

The workflow contains three jobs.

### Backend Tests

The backend job sets up Python 3.12 and a PostgreSQL test database before running the automated tests.

During setup, I configured `PYTHONPATH` so Python could import the application correctly. I also added a test-only `SECRET_KEY` because the application requires it to start the tests.

### Frontend Checks

The frontend job uses Node.js 22 to install dependencies, run ESLint, and create a production build.

I fixed a frontend linting error involving an unused variable before the job passed.

### Docker Build Validation

The Docker job checks whether the configured application image can be built successfully. This helps catch Docker build problems before moving further in the delivery process.

## 2. Security Pipeline

I created a separate workflow at `.github/workflows/security.yml`.

It runs three security checks:

- **Secret scanning:** Gitleaks scans the repository history for accidentally committed secrets.
- **Python dependency scanning:** `pip-audit` checks the packages listed in `backend/requirements.txt`.
- **Frontend dependency scanning:** `npm audit --audit-level=high` checks frontend dependencies for high-severity and more severe reported vulnerabilities.

I fixed the dependency issues reported during the initial runs until the configured checks passed.

## 3. Troubleshooting

One of the most useful parts of this project was debugging failures directly from GitHub Actions logs.

### Backend import error

The tests initially failed because Python could not find the application module. I configured `PYTHONPATH` to point to the repository root.

### Missing environment variable

The backend required `SECRET_KEY` during testing. I added a test-only value to the workflow rather than relying on a local environment file.

### PostgreSQL hostname resolution

The tests used the hostname `db_test`, but the GitHub Actions runner could not resolve it. I added a hostname mapping to `127.0.0.1` so the tests could reach the PostgreSQL service through the runner's published port.

### Frontend and dependency failures

I corrected the frontend linting issue and addressed the dependency vulnerabilities reported by the audit tools.

After these fixes, the CI jobs and security checks completed successfully.

## 4. How to Check the Results

1. Open the project repository on GitHub.
2. Select the **Actions** tab.
3. Open the CI and Security Checks workflows.
4. Review the latest workflow runs and their individual jobs.
5. Confirm that the configured checks have passed.

## 5. What I Learned

Through this project, I gained practical experience with:

- Creating and maintaining GitHub Actions workflows.
- Running backend tests against PostgreSQL in CI.
- Automating frontend linting and production builds.
- Validating Docker builds in a pipeline.
- Scanning source history and dependencies for security issues.
- Reading CI logs and troubleshooting failed jobs.
- Keeping test configuration separate from production secrets.

This project helped me understand that a working application is only one part of delivery. Automated checks also help detect problems before changes progress further.

## Project Scope

This project focuses on CI, testing, Docker build validation, and automated security checks. It does not include an AWS deployment or an automated production rollback.
