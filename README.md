# Shared GitHub Security Workflow

Reusable GitHub Actions workflow that runs three security tools against the repository that calls it:

- **Trivy** — filesystem vulnerability, misconfiguration and secret checks.
- **Semgrep** — static code analysis using the `p/security-audit` ruleset.
- **CodeQL** — Python security analysis using `security-extended`.

## Add it to a repository

Create `.github/workflows/security.yml` in the repository that should be scanned:

```yaml
name: Security Scans

on:
  pull_request:
    branches: [main]
  push:
    branches: [main]
  schedule:
    - cron: "17 3 * * 1"
  workflow_dispatch:

permissions:
  contents: read
  security-events: write
  actions: read
  pull-requests: read

jobs:
  security:
    uses: faithfatih07/security-workflows/.github/workflows/security.yml@main
    with:
      python-version: "3.12"
```

Change the Python version if the project requires a different supported version.

## Important notes

- This workflow scans the **calling repository**; it does not scan this shared repository.
- The caller's workflow controls when scans run. The example runs on pull requests to `main`, pushes to `main`, weekly, and manually.
- Findings are uploaded to GitHub Code Scanning when the repository and plan support it.
- The workflow reports findings but does not currently block merges based on findings. Treat green job status as "the scanners ran successfully", not as proof that the repository is vulnerability-free.
- CodeQL is configured for Python projects. For a repository without Python source, remove or adapt the CodeQL job before using this workflow.
- Trivy dependency vulnerability coverage is best when the repository includes supported dependency manifests or lockfiles.
- Keep this repository public if public repositories need to call it without cross-repository access configuration.
