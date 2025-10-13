# Claude Commands for Exito Store

Welcome! This repository contains custom Claude Desktop commands designed to streamline and enhance the development workflow for the FastStore/VTEX e-commerce platform at Exito.

## Installation

This project is distributed as a Claude Code plugin. Follow these steps to install it:

1.  **Add the Marketplace**:
    This command registers the repository as a source for plugins.

    ```shell
    /plugin marketplace add yargotev/claude-review-plugin
    ```

2.  **Install the Plugin**:
    Install the `review` plugin from the marketplace you just added.

    ```shell
    /plugin install exito@yargotev-marketplace
    ```

After installation, restart Claude Code to enable the new commands.

## Available Commands

The `review` plugin provides a suite of commands for comprehensive PR analysis:

### `/review` - Comprehensive PR Review

**Purpose**: Performs a holistic technical review of a Pull Request, adapting its strategy based on the PR's size and complexity. It can integrate with Azure DevOps to incorporate business context from user stories.

**Quick Start**:
```bash
# Basic usage for a PR
/review 1695

# For a PR with related User Story context from Azure DevOps
/review 1695 https://dev.azure.com/grupo-exito/GCIT-Agile/_workitems/edit/544232
```

### `/review-perf` - Performance-Focused Review

**Purpose**: Conducts a deep-dive analysis on performance aspects of the code, focusing on potential bottlenecks, inefficient patterns, and adherence to performance best practices.

**Quick Start**:
```bash
/review-perf 1695
```

### `/review-sec` - Security-Focused Review

**Purpose**: Scans the code for common security vulnerabilities, insecure patterns, and potential risks.

**Quick Start**:
```bash
/review-sec 1695
```

---

## PR Size Handling Strategy

The `/review` command intelligently adapts to PR size:

| Size | Lines Changed | Strategy | Review Time |
|------|---------------|----------|-------------|
| 🟢 Small | < 200 | Complete detailed review | 15-30 min |
| 🟡 Medium | 200-500 | Detailed review with focus | 30-60 min |
| 🟠 Large | 500-1000 | Prioritized + sampling | 1-2 hours |
| 🔴 Very Large | > 1000 | Risk-based + split recommendation | 2-4 hours |

---

## Development Workflow

### When to Use These Commands

**Use `/review`, `/review-perf`, or `/review-sec` when**:
- ✅ PR has passed all CI/CD checks (tests, lint, build).
- ✅ PR description is complete and clear.
- ✅ User Stories are linked (if applicable for `/review`).
- ✅ The author has already self-reviewed the changes.

**Don't use them if**:
- ❌ CI/CD checks are failing.
- ❌ The PR is still in a draft state.
- ❌ There are merge conflicts.

### Recommended Workflow

1.  Author creates a PR.
2.  Automated checks run and pass ✅.
3.  Reviewer runs `/review <PR_NUMBER> [HU_URL]` for a holistic review, or a specialized command like `/review-perf` or `/review-sec`.
4.  The AI provides a comprehensive analysis.
5.  The reviewer validates the AI's findings and provides feedback on GitHub.
6.  The author addresses the feedback.
7.  Repeat as needed, then merge!

---

## Prerequisites

1.  **Claude Desktop**: The environment where these commands run.
    - Install from: https://claude.ai/download

2.  **Required Tools (GitHub CLI & Azure CLI)**: The `review` plugin requires `gh` and `az` to function. After installing the plugin, run the following command inside Claude to automatically install these tools:
    ```bash
    /setup
    ```
    This command will check for the tools and install them using Homebrew if they are missing. It will also guide you to log in if necessary. If you prefer to install them manually, you can use `brew install gh` and `brew install azure-cli`.

---

## Environment Variables

For Azure DevOps integration with the `/review` command:

```bash
export AZURE_DEVOPS_ORG_URL="https://dev.azure.com/grupo-exito"
export AZURE_DEVOPS_PAT="your-personal-access-token"
```

> **Security note:** Treat your Personal Access Token (PAT) as a secret. Do not commit it to source control, paste it into public chats, or include it in logs. Store secrets in a secure secrets manager (CI/CD secrets, environment vault, etc.) and rotate them periodically.

---


## Contributing

To improve or add new commands, please see the `docs/` directory for more information on the project structure and development conventions.

---

## Changelog

### Version 2.0 (2025-10-04)
- ✨ Added `/code-quality-review` command for deep-dive analysis.
- ✨ Restructured README for clarity and user-friendliness.
- ✨ Enhanced `/tech-review` with intelligent PR size detection and adaptive strategies.
- 📚 Added comprehensive usage examples and workflow recommendations.

### Version 1.0 (2025-10-03)
- 🎉 Initial release of the `/tech-review` command.
- Basic PR review functionality with Azure DevOps integration.

---

**Maintainer**: Development Team
**Last Updated**: October 13, 2025
**License**: Internal Use Only