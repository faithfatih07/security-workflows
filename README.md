# Shared GitHub Security Workflow

Reusable GitHub Actions workflow and language-agnostic project starter template.

## What's in this repository?

- `main` contains the reusable security engine at `.github/workflows/reusable-security.yml`.
- `project-template` contains a clean starter repository: a generic `.gitignore`, a README, and a short workflow that calls the shared engine.

The reusable engine runs:

- **Trivy** — filesystem vulnerability, misconfiguration and secret checks.
- **Semgrep** — static security analysis using `p/security-audit`.
- **CodeQL** — detects supported languages in the calling repository and runs `security-extended` analysis. It is skipped when no supported source language is detected.

## Use the project template

After the repository's default branch is set to `project-template` and the **Template repository** option is enabled, use **Use this template → Create a new repository**.

The template branch is intentionally separate from `main`: generated projects get only the starter files, while the shared security engine stays on `main`.

## Connect an existing repository

Create `.github/workflows/security.yml` in the repository to be scanned:

```yaml
name: Security Scans

on:
  pull_request:
    branches: ["**"]
  push:
    branches: [main]
  schedule:
    - cron: "17 3 * * 1"
  workflow_dispatch:

permissions:
  contents: read
  actions: read
  pull-requests: read
  security-events: write

jobs:
  security:
    uses: faithfatih07/security-workflows/.github/workflows/reusable-security.yml@main
```

## Important notes

- The workflow scans the **calling repository**, not this shared repository.
- Pull requests to any target branch trigger scans; pushes to `main`, the weekly schedule, and manual dispatch also trigger scans.
- Findings are uploaded to GitHub Code Scanning when supported by the repository and plan.
- A successful run means the scanners executed. It does not prove the repository is vulnerability-free.
- The workflow reports findings but does not currently enforce a merge block.
- CodeQL covers supported languages (C/C++, C#, Go, Java/Kotlin, JavaScript/TypeScript, Python, Ruby, Rust, Swift). It is skipped if none are detected.
- Trivy dependency vulnerability coverage is strongest when the repository includes supported dependency manifests or lockfiles.
- Keep this repository public so public repositories can call the shared workflow without extra cross-repository access configuration.
