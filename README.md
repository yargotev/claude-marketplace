# Claude Commands for Exito Store

Welcome! This repository contains custom Claude Desktop commands designed to streamline and enhance the development workflow for the FastStore/VTEX e-commerce platform at Exito.

## Available Commands

Here are the commands you can use to supercharge your Pull Request reviews.

### `/tech-review` - Advanced PR Review for FastStore/VTEX

**Purpose**: Provides a comprehensive technical review of Pull Requests, intelligently adapting its strategy to the size and complexity of the changes. It's optimized for the FastStore/VTEX platform and can integrate with Azure DevOps for business context.

**Quick Start**:
```bash
# Basic usage for a PR
/tech-review 1695

# For a PR with related User Story context from Azure DevOps
/tech-review 1695 https://dev.azure.com/grupo-exito/GCIT-Agile/_workitems/edit/544232
```

**Key Features**:
- ✅ **Adaptive Strategy**: Automatically detects PR size (Small, Medium, Large, Very Large) and adjusts its review depth.
- ✅ **FastStore/VTEX Compliance**: Checks for adherence to platform-specific standards, including UI components, styling, and multi-store compatibility.
- ✅ **Business Context Validation**: Integrates with Azure DevOps to ensure the implementation aligns with user story requirements and acceptance criteria.
- ✅ **Risk Assessment**: Identifies and categorizes issues by severity (Critical, Important, Minor) to prioritize fixes.
- ✅ **Large PR Handling**: Employs advanced techniques like domain grouping, prioritized file review, and pattern-based analysis for large and complex PRs.
- ✅ **Actionable Recommendations**: Provides clear next steps, testing strategies, and post-deployment monitoring plans.

### `/code-quality-review` - Code Quality & Performance Review

**Purpose**: Performs an in-depth analysis of code quality, performance, and adherence to clean code principles. This command is ideal for a deep-dive into React, Next.js, and general web development best practices.

**Quick Start**:
```bash
# Basic usage for a PR
/code-quality-review 1695

# For a PR with multiple related User Stories
/code-quality-review 1695 <AZURE_DEVOPS_URL_1> <AZURE_DEVOPS_URL_2>
```

**Key Features**:
- ✅ **Performance Deep-Dive**: Analyzes React hooks, Next.js data fetching (`getStaticProps`, `getServerSideProps`), image optimization, and general web performance patterns.
- ✅ **Design Pattern Analysis**: Identifies the use of design patterns (Hooks, Provider, Factory, etc.) and suggests opportunities for architectural improvements.
- ✅ **Clean Code Evaluation**: Checks for adherence to principles like KISS (Keep It Simple, Stupid), DRY (Don't Repeat Yourself), and identifies common code smells.
- ✅ **TypeScript & Type Safety**: Reviews TypeScript usage, flagging `any` types, type assertions, and other potential safety issues.
- ✅ **Comprehensive Testing Review**: Assesses test coverage, quality, and strategy for unit, integration, and end-to-end tests.
- ✅ **Security & Accessibility**: Performs a quick scan for common security vulnerabilities (like XSS) and accessibility (a11y) issues.

---

## PR Size Handling Strategy

The `/tech-review` command intelligently adapts to PR size:

| Size | Lines Changed | Strategy | Review Time |
|------|---------------|----------|-------------|
| 🟢 Small | < 200 | Complete detailed review | 15-30 min |
| 🟡 Medium | 200-500 | Detailed review with focus | 30-60 min |
| 🟠 Large | 500-1000 | Prioritized + sampling | 1-2 hours |
| 🔴 Very Large | > 1000 | Risk-based + split recommendation | 2-4 hours |

---

## Development Workflow

### When to Use These Commands

**Use `/tech-review` or `/code-quality-review` when**:
- ✅ PR has passed all CI/CD checks (tests, lint, build).
- ✅ PR description is complete and clear.
- ✅ User Stories are linked (if applicable).
- ✅ The author has already self-reviewed the changes.

**Don't use them if**:
- ❌ CI/CD checks are failing.
- ❌ The PR is still in a draft state.
- ❌ There are merge conflicts.

### Recommended Workflow

1.  Author creates a PR.
2.  Automated checks run and pass ✅.
3.  Reviewer runs `/tech-review <PR_NUMBER> [HU_URL]` for a holistic review or `/code-quality-review <PR_NUMBER> [HU_URL]` for a deep code quality check.
4.  The AI provides a comprehensive analysis.
5.  The reviewer validates the AI's findings and provides feedback on GitHub.
6.  The author addresses the feedback.
7.  Repeat as needed, then merge!

---

## Prerequisites

1.  **GitHub CLI**: Required for extracting PR data.
    ```bash
    brew install gh
    gh auth login
    ```
2.  **Claude Desktop**: The environment where these commands run.
    - Install from: https://claude.ai/download
3.  **Azure DevOps Access** (Optional): For integrating User Story context.

---

## Configuration

### Claude Desktop

Add the commands to your `claude_desktop_config.json`:

```json
{
  "commands": {
    "tech-review": {
      "path": "/path/to/your/project/tech-review-plugin/commands/tech-review.md",
      "description": "Technical PR review with size-adaptive strategy for FastStore/VTEX."
    },
    "code-quality-review": {
      "path": "/path/to/your/project/tech-review-plugin/commands/code-quality-review.md",
      "description": "In-depth code quality and performance review for React/Next.js."
    }
  }
}
```

### Environment Variables

For Azure DevOps integration:

```bash
export AZURE_DEVOPS_ORG_URL="https://dev.azure.com/grupo-exito"
export AZURE_DEVOPS_PAT="your-personal-access-token"
```

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