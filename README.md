# Project Starter Template

A lightweight, language-agnostic GitHub starter repository.

## Included

- A generic `.gitignore` for common operating systems and development tools.
- A GitHub Actions workflow that calls the shared security scanners maintained in [faithfatih07/security-workflows](https://github.com/faithfatih07/security-workflows).

## Security checks

When you create a repository from this template, GitHub Actions automatically runs on:
- Pull requests targeting any branch.
- Pushes to `main`.
- A weekly schedule.
- Manual workflow dispatch.

The shared workflow runs:
- **Trivy** for filesystem vulnerabilities, misconfigurations, and exposed secrets.
- **Semgrep** for static security analysis.
- **CodeQL** for supported languages detected in the repository. CodeQL is skipped when the repository has no supported source language.

## Use this template

On GitHub, click **Use this template → Create a new repository**.

No language-specific starter code is included. Use this for web apps, Python, Unity, scripts, APIs, and other projects.

## Important

A successful scan means the tools ran; it does not prove the project is vulnerability-free. Findings are reported, but this workflow does not currently enforce a merge block.
