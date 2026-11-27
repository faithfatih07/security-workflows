# Universal Project Starter

A minimal, language-agnostic GitHub starter template for web apps, APIs, Python, Unity, scripts, and other projects.

## Included

- A generic `.gitignore`.
- A GitHub Actions workflow that calls the shared security engine.
- A local Gitleaks pre-commit configuration.

## Automatic security checks

The shared workflow runs when:
- A pull request is opened or updated for any target branch.
- Changes are pushed to `main`.
- You manually start the workflow from the Actions tab.

Weekly scheduled scans are disabled to avoid running on inactive repositories.

### Public repositories

Trivy, Semgrep, and CodeQL run automatically. Their SARIF results are uploaded to GitHub Code Scanning.

### Private repositories without GitHub Code Security

Trivy and Semgrep run automatically on GitHub-hosted Ubuntu runners. Their SARIF reports are saved as Actions artifacts for 14 days.

CodeQL is skipped. **Using the CodeQL CLI on a self-hosted Mac does not remove GitHub's CodeQL licensing restrictions for private/closed-source code in automated CI/CD.** A self-hosted runner is not a license workaround.

### Private repositories with a valid GitHub Code Security license

The workflow can also run CodeQL CLI on the organization Mac runner. The organization or repository must have the Actions variable `CODEQL_PRIVATE_LICENSED=true`, and the runner must be online and available to that repository.

The runner must have these labels:
`self-hosted`, `macOS`, `X64`, `codeql`.

The runner's PATH must include the CodeQL CLI executable. The workflow checks `codeql version` before starting. Temporary CodeQL databases and SARIF files are removed at the end of each job.

## Local secret protection (macOS)

The `.pre-commit-config.yaml` file configures Gitleaks to check staged changes before each Git commit.

Install the tools and enable the hook **once per local project clone**:

```bash
brew install pre-commit gitleaks
pre-commit install
```

After that, Gitleaks runs automatically before commits. This is an extra local safety net; Trivy also checks for secrets after code is pushed.

## Protect the main branch

Template files are copied into new repositories, but GitHub repository settings such as branch rulesets and runner access policies are **not** copied.

After creating a repository from this template, open **Settings → Rulesets → New branch ruleset** and target `main`. Enable:
- **Require a pull request before merging**.
- **Require status checks to pass before merging**, then select `security / Security gate` after the first workflow run.

The Security gate verifies that the applicable scanners completed. It does not by itself fail solely because a scanner reported a finding. On private repositories without GitHub Code Security, Trivy and Semgrep SARIF reports are available as Actions artifacts rather than Code Scanning alerts.

For an organization Mac runner, restrict its runner group to the private repositories that should use it. Do not expose a personal self-hosted runner to public repositories or untrusted external contributors.

## Shared engine

The reusable security engine is maintained in [faithfatih07/security-workflows](https://github.com/faithfatih07/security-workflows), on the `security-engine` branch. New runs fetch the current engine from that branch, so engine updates are shared across repositories that call it.

The shared repository is public so private repositories in an organization can call it, provided the organization's Actions policy allows public reusable workflows.

## Important

A successful scan means the tools ran; it does not prove the project is vulnerability-free. CodeQL supports only specific languages; unsupported languages are not analyzed.
