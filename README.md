# Universal Project Starter

A minimal, language-agnostic GitHub starter template for web apps, APIs, Python, Unity, scripts, and other projects.

## Included

- A generic `.gitignore`.
- A shared GitHub Actions security workflow.
- A local Gitleaks pre-commit hook configuration.

## Automatic GitHub security checks

The shared security workflow runs automatically when:
- A pull request targets a branch.
- Changes are pushed to `main`.
- You manually start the workflow from the Actions tab.

It runs **Trivy + Semgrep + CodeQL**. Weekly scheduled scans are intentionally disabled in this starter to avoid consuming Actions minutes on inactive private repositories.

The shared engine is maintained in [faithfatih07/security-workflows](https://github.com/faithfatih07/security-workflows), on the `security-engine` branch. Updating that central workflow updates future runs in all repositories that call it.

## Catch secrets before committing (local Mac setup)

The `.pre-commit-config.yaml` file configures Gitleaks to detect API keys, tokens, and passwords before a Git commit is created.

On your Mac, open Terminal in the project folder and run this **once for this project**:

```bash
brew install pre-commit && pre-commit install
```

After that, Gitleaks checks staged changes whenever you commit. If Homebrew or pre-commit is already installed, the command simply reuses it. This local hook is an extra safety net; the GitHub security workflow still scans pull requests.

## Protect the main branch

Template files are copied into new repositories, but GitHub repository settings such as branch rulesets are **not** copied.

After creating a repository from this template, open **Settings → Rulesets → New branch ruleset** and target `main`. Enable:
- Require a pull request before merging.
- Require status checks to pass before merging; select the Trivy, Semgrep, and CodeQL checks after the first workflow run.

Do not require an approval from another person if you are the only contributor; requiring the PR itself and successful checks is enough.

## Important

A successful scan means the tools ran; it does not prove the project is vulnerability-free. Findings are reported; the branch ruleset is what prevents merging until the required checks pass.
