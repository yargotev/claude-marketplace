# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a Claude Code plugin that provides comprehensive PR review commands for FastStore/VTEX e-commerce development at Exito. The plugin is distributed via marketplace and uses an agent-based architecture to perform multi-dimensional code analysis.

## Installation & Setup

To test the plugin after making changes:

1. **Install the plugin** (if not already installed):
   ```bash
   /plugin marketplace add yargotev/claude-exito-plugin
   /plugin install exito@yargotev-marketplace
   ```

2. **Setup required dependencies**:
   ```bash
   /setup
   ```
   This installs GitHub CLI (`gh`) and Azure CLI (`az`) via Homebrew if missing.

## Architecture

### Plugin Structure

The plugin uses a **multi-agent orchestration pattern**:

```
exito-plugin/
├── commands/          # User-facing slash commands
│   ├── review.md      # Main orchestrator command
│   ├── review-perf.md # Performance-focused review
│   ├── review-sec.md  # Security-focused review
│   └── setup.md       # Dependency installer
├── agents/            # Specialized sub-agents
│   ├── 1-context-gatherer.md      # PR metadata & diff collection
│   ├── 2-business-validator.md    # Azure DevOps integration
│   ├── 3-performance-analyzer.md  # React/Next.js performance
│   ├── 4-architecture-reviewer.md # Design patterns & structure
│   ├── 5-clean-code-auditor.md   # Code quality & style
│   ├── 6-security-scanner.md     # Security vulnerabilities
│   ├── 7-testing-assessor.md     # Test coverage & quality
│   └── 8-accessibility-checker.md # WCAG compliance
└── scripts/
    └── install-deps.sh            # Homebrew installer script
```

### How Commands Work

1. **User invokes a command**: `/review <PR_NUMBER> [AZURE_DEVOPS_URL]`
2. **Orchestrator** ([commands/review.md](exito-plugin/commands/review.md)) delegates to agents sequentially and in parallel
3. **Context Gatherer** ([agents/1-context-gatherer.md](exito-plugin/agents/1-context-gatherer.md)) runs first to collect PR data using GitHub CLI
4. **Specialized agents** run in parallel, each analyzing a specific dimension (performance, security, architecture, etc.)
5. **Orchestrator synthesizes** all agent reports into a single comprehensive review document

### Adaptive Strategy by PR Size

The context-gatherer agent classifies PRs and adapts the review strategy:

- **Small** (< 200 lines): Full detailed review of all changes
- **Medium** (200-500 lines): Detailed review with focused analysis
- **Large** (500-1000 lines): Prioritized review with strategic sampling
- **Very Large** (> 1000 lines): Risk-based review with split recommendations

For Large/Very Large PRs, the context-gatherer focuses diffs on critical patterns (hooks, state management, data fetching) rather than collecting everything.

## Key Technologies & Integrations

- **GitHub CLI (`gh`)**: All agents use `gh pr view`, `gh pr diff`, and related commands to extract PR data
- **Azure DevOps MCP Tools**: Business-validator agent uses MCP tools (`mcp_microsoft_azu_wit_get_work_item`, etc.) to fetch User Story details
- **Claude Agent SDK**: All agents are defined as markdown files with YAML frontmatter specifying tools, model, and parameters

## Common Development Tasks

### Testing Commands Locally

After modifying a command or agent:

1. Restart Claude Desktop to reload plugin changes
2. Test the command on a real PR:
   ```bash
   /review <PR_NUMBER>
   ```

### Adding a New Agent

1. Create a new file in `exito-plugin/agents/` with naming convention `<number>-<name>.md`
2. Follow the YAML frontmatter pattern:
   ```yaml
   ---
   name: agent-name
   description: "What this agent does"
   tools: Bash(gh:*)
   model: claude-sonnet-4-5-20250929
   ---
   ```
3. Update the orchestrator in [commands/review.md](exito-plugin/commands/review.md) to invoke your new agent
4. Document the agent's output format in its markdown file

### Adding a New Command

1. Create a new file in `exito-plugin/commands/` like `review-<focus>.md`
2. Add YAML frontmatter:
   ```yaml
   ---
   description: "Brief description of command"
   argument-hint: "<ARG1> [ARG2]..."
   ---
   ```
3. Define the command logic using markdown and agent invocations
4. Update [README.md](README.md) to document the new command

### Modifying Agent Behavior

Each agent's instructions are defined in its markdown file. To change what an agent analyzes:

1. Edit the relevant agent file in `exito-plugin/agents/`
2. Update the "Focus Areas" or "Steps" sections
3. Adjust the expected output format in the "Output" section
4. Test with a sample PR to verify changes

## Environment Variables

For Azure DevOps integration (optional):

```bash
export AZURE_DEVOPS_ORG_URL="https://dev.azure.com/grupo-exito"
export AZURE_DEVOPS_PAT="your-personal-access-token"
```

These are only required if using the business-validator agent with User Story URLs.

## Agent-Specific Notes

### Context Gatherer
- Always runs first; provides structured context to all other agents
- Uses `gh pr view --json` to fetch metadata
- Classifies PR size to determine review strategy
- For large PRs, uses targeted `gh pr diff` with grep patterns instead of full diff

### Business Validator
- Only invoked when Azure DevOps URLs are provided as arguments
- Requires Azure CLI authentication (`az login --allow-no-subscriptions`)
- Uses MCP tools to fetch Work Items and validate against acceptance criteria

### Performance Analyzer
- Focuses on React hooks (useEffect, useMemo, useCallback)
- Checks Next.js patterns (getStaticProps, next/dynamic, next/image)
- Identifies bundle size issues and runtime bottlenecks

### Security Scanner
- Looks for XSS risks (dangerouslySetInnerHTML, innerHTML)
- Scans for hardcoded secrets and sensitive data
- Reviews dependency changes in package.json

## Important Constraints

- All agents use `model: claude-sonnet-4-5-20250929`
- Agents can only use tools specified in their frontmatter `tools:` field
- The plugin assumes macOS environment (uses Homebrew for dependency installation)
- GitHub CLI must be authenticated before using any review commands

## Plugin Distribution

The plugin is distributed via the marketplace `yargotev/claude-exito-plugin` and installed as `exito@yargotev-marketplace`. Changes to the plugin require:

1. Committing changes to the repository
2. Users pulling the latest version via `/plugin update exito@yargotev-marketplace`
