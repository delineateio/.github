# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Purpose

This is the delineate.io organization `.github` repository. It provides:

- **Default community health files** applied automatically to all org repos (CODE_OF_CONDUCT.md, CONTRIBUTING.md, SECURITY.md, LICENSE, PULL_REQUEST_TEMPLATE.md, issue templates)
- **Shared/reusable GitHub Actions workflows** (e.g., `reusable_markdown_lint.yml`, `reusable_secret_scan.yml`)
- **Organization-wide automation** (label sync, release management, security scanning, bot auto-approval)

## Common Commands

### Pre-commit

```bash
# Install hooks
pre-commit install

# Run all hooks against all files
pre-commit run --all-files

# Run a specific hook
pre-commit run markdownlint --all-files
```

### Markdown Linting

Rules configured in `.markdownlint.json`. Disabled: MD004, MD013, MD033, MD041.

All markdown files should be linted and fixed

## Workflow Architecture

### Key Workflows

| Workflow | Trigger | Purpose |
|---|---|---|
| `dco.yml` | PR | Validates DCO sign-off on all commits |
| `secret_scan.yml` | PR + master push | GitGuardian secret scanning |
| `tfsec_scan.yml` | PR + daily | Terraform security scanning → SARIF upload |
| `scorecard.yml` | master push + weekly | OpenSSF security posture analysis |
| `label_sync.yml` | `labels.yml` push to master | Syncs canonical labels org-wide |
| `release_please.yml` | master push | Automated semantic versioning and releases |
| `auto_approve_bots.yml` | PR | Auto-approves dependabot, pre-commit-ci, jf-delineate PRs |
| `stale.yml` | daily | Closes stale issues (60d) and PRs (30d) |
| `welcome.yml` | issue/PR open | Welcomes first-time contributors |
| `reusable_markdown_lint.yml` | `workflow_call` | Reusable markdown lint — called from other org repos |
| `reusable_secret_scan.yml` | `workflow_call` | Reusable GitGuardian scan — called from other org repos |

### Required Secrets

- `GITGUARDIAN_API_KEY` — For secret scanning via ggshield
- `LABEL_SYNC_TOKEN` — Cross-org GitHub token for label synchronization

## Conventions

- **Indentation**: 2 spaces universally (enforced by `.editorconfig`)
- **Line endings**: LF
- **Label definitions**: Canonical set maintained in `.github/labels.yml`
- **Dependabot**: Configured for weekly GitHub Actions updates, max 5 open PRs

## Commits and Pushes

All commits should be signed before being pushed.  All pushes should be rebased before being pushed
