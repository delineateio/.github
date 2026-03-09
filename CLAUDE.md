# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Purpose

This is the delineate.io organization `.github` repository. It provides:
- **Default community health files** applied automatically to all org repos (CODE_OF_CONDUCT.md, CONTRIBUTING.md, SECURITY.md, LICENSE, PULL_REQUEST_TEMPLATE.md, issue templates)
- **Shared/reusable GitHub Actions workflows** (e.g., `reusable_markdown_lint.yml`)
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
pre-commit run gitleaks --all-files
```

### Markdown Linting

Markdown rules are configured in `.markdownlint.json`. Disabled: MD004, MD013, MD033, MD041.

## Workflow Architecture

All workflows use **StepSecurity runner hardening** (`step-security/harden-runner`) with egress policy auditing. Actions are pinned to full commit SHAs for security.

### Key Workflows

| Workflow | Trigger | Purpose |
|---|---|---|
| `dco.yml` | PR | Validates DCO sign-off on all commits |
| `dependency-review.yml` | PR | Blocks PRs with vulnerable dependencies |
| `tfsec_scan.yml` | PR + daily | Terraform security scanning → SARIF upload |
| `scorecard.yml` | master push + weekly | OpenSSF security posture analysis |
| `label_sync.yml` | `labels.yml` push to master | Syncs canonical labels org-wide via `LABEL_SYNC_TOKEN` |
| `release_please.yml` | master push | Automated semantic versioning and releases |
| `auto_approve_bots.yml` | PR | Auto-approves dependabot, pre-commit-ci, jf-delineate PRs |
| `stale.yml` | daily | Closes stale issues (60d) and PRs (30d) |
| `welcome.yml` | issue/PR open | Welcomes first-time contributors |
| `reusable_markdown_lint.yml` | `workflow_call` | Reusable markdown lint — called from other org repos |

### Secrets Required

- `LABEL_SYNC_TOKEN` — Cross-org GitHub token for label synchronization

## Conventions

- **Indentation**: 2 spaces universally (enforced by `.editorconfig`)
- **Line endings**: LF
- **Label definitions**: Canonical set maintained in `.github/labels.yml` (24 labels across type/status/priority/meta)
- **Dependabot**: Configured for weekly GitHub Actions updates, max 5 open PRs

## Pushes

All pushes should be rebased before besing pushed to ensure they are signed
