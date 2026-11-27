# Shared Security Engine

Reusable GitHub Actions workflow used by the language-agnostic project starter template.

## Execution model

- Trivy and Semgrep run on GitHub-hosted Ubuntu.
- For public repositories, their SARIF files are uploaded to GitHub Code Scanning.
- For private repositories without GitHub Code Security, their SARIF files are uploaded as Actions artifacts.
- CodeQL runs on GitHub-hosted Ubuntu for public repositories.
- CodeQL CLI can run on the organization Mac self-hosted runner for private repositories only when a valid GitHub Code Security license is available and `CODEQL_PRIVATE_LICENSED=true` is set.
- A private repository without that license skips CodeQL. A self-hosted runner does not bypass the CodeQL license terms.

## Local Gitleaks protection

The project template contains `.pre-commit-config.yaml` for Gitleaks. Developers install `pre-commit` and `gitleaks` locally, then run `pre-commit install` once per clone.

## Branch protection

The reusable workflow exposes a final check named `security / Security gate`. It verifies the applicable scanner jobs completed. It does not fail only because a SARIF finding exists. Repository owners should require this status check in a branch ruleset. To block on specific findings, configure the relevant scanner thresholds or GitHub Code Scanning merge protection where licensed.

## Runner safety

The private CodeQL job targets a runner with labels `self-hosted`, `macOS`, `X64`, and `codeql`. Restrict the organization runner group to intended private repositories. Never allow untrusted public forks to execute on a personal Mac runner.
