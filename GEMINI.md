# Project Overview

This project contains the configuration and documentation for a custom Claude command, `/tech-review`, designed for comprehensive technical reviews of Pull Requests (PRs) on the FastStore/VTEX e-commerce platform.

The primary goal of this project is to provide a powerful, automated PR review process that adapts to the size and complexity of the changes, integrates with business context from Azure DevOps, and enforces development best practices.

**Main Technologies:**

*   **Claude:** The AI model that powers the `/tech-review` command.
*   **GitHub CLI (`gh`):** Used to extract PR data, diffs, and other information from GitHub.
*   **Azure DevOps:** Used to pull in business context from user stories.
*   **Markdown:** Used for defining the command's logic and for all documentation.

**Architecture:**

The project is structured around a single, powerful Claude command (`/tech-review`) defined in `commands/tech-review.md`. This command is broken down into several phases:

1.  **Data Collection & Size Assessment:** Gathers PR metadata and classifies the PR size to determine the review strategy.
2.  **Business Context Validation:** Integrates with Azure DevOps to validate the PR against user stories.
3.  **Technical Analysis:** Performs a deep dive into the code, checking for code quality, performance, security, and compliance with FastStore/VTEX standards.
4.  **Risk Assessment:** Categorizes issues by severity and provides a risk analysis, especially for large PRs.
5.  **Recommendations & Next Steps:** Provides actionable feedback, suggestions, and a plan for testing and monitoring.

The project also includes extensive documentation on how to use the command, best practices, and how to extend the system with new commands.

# Building and Running

This is not a traditional software project with a build process. The "running" of this project involves using the `/tech-review` command within the Claude environment.

**Prerequisites:**

*   **GitHub CLI:** `brew install gh`
*   **Claude Desktop:** Installed from `https://claude.ai/download`
*   **Azure DevOps Access (optional):** For user story integration.

**To use the `/tech-review` command:**

1.  **Configure Claude Desktop:** Add the command to your `claude_desktop_config.json`:
    ```json
    {
      "commands": {
        "tech-review": {
          "path": "/path/to/pr-review-ext/commands/tech-review.md",
          "description": "Technical PR review with size-adaptive strategy"
        }
      }
    }
    ```
2.  **Set Environment Variables (optional):** For Azure DevOps integration:
    ```bash
    export AZURE_DEVOPS_ORG_URL="https://dev.azure.com/grupo-exito"
    export AZURE_DEVOPS_PAT="your-personal-access-token"
    ```
3.  **Run the command:**
    ```bash
    # Basic usage
    /tech-review <PR_NUMBER>

    # With User Story context
    /tech-review <PR_NUMBER> <AZURE_DEVOPS_USER_STORY_URL>
    ```

# Development Conventions

*   **Command Definition:** Commands are defined in Markdown files in the `commands/` directory.
*   **Documentation:** All documentation is in Markdown in the `docs/` directory.
*   **Modularity:** The `/tech-review` command is broken down into logical phases, making it easier to understand and maintain.
*   **Extensibility:** The project is designed to be extended with new commands by adding new Markdown files to the `commands/` directory.
*   **Versioning:** The `README.md` includes a changelog for tracking versions of the command.
