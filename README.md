# Shared Security Engine

Reusable GitHub Actions workflow used by repositories created from the project starter template.

## Workflow

Call this workflow from another repository:

```yaml
jobs:
  security:
    uses: faithfatih07/security-workflows/.github/workflows/reusable-security.yml@security-engine
```

The reusable workflow runs:

- **Trivy** — filesystem vulnerability, misconfiguration, and secret checks.
- **Semgrep** — static security analysis with the `p/security-audit` ruleset.
- **CodeQL** — detects supported languages in the calling repository and runs `security-extended` analysis.

The workflow scans the **calling repository**, not this repository.

## Important

- Findings are reported to GitHub Code Scanning when supported by the repository and plan.
- A successful run means the scanners executed; it does not prove that the repository is vulnerability-free.
- The workflow reports findings but does not itself prevent merging. Its final check is named `security / Security gate`; configure a branch ruleset to require this check. To block based on actual findings, also enable GitHub's **Require code scanning results** rule and choose tools and severity thresholds.
- CodeQL is skipped when the repository has no supported source language.
- Trivy dependency scanning is strongest when supported dependency manifests or lockfiles are committed.
