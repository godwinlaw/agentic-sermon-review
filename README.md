# Agentic Sermon Review

An AI-powered tool for analyzing teaching scripts and sermon transcripts using specialized AI agents.

## Overview

This project provides a comprehensive framework for reviewing and improving teaching content through multiple AI-powered analysis agents. Each agent specializes in a different aspect of communication analysis, providing detailed feedback on effectiveness, clarity, engagement, and theological accuracy.

## Available Agents

1. **ah-counter** - Analyzes filler words and verbal tics in speech transcripts
2. **ethos-analyzer** - Evaluates credibility and authority elements
3. **fact-checker** - Verifies factual claims and statistics
4. **genz-advisor** - Reviews content for Gen Z audience appeal
5. **grammar-police** - Reviews grammar in spoken transcripts
6. **greg-stier** - Gospel clarity reviewer (ensures complete gospel message)
7. **humor-analyst** - Identifies opportunities for adding engagement-boosting humor
8. **logos-analyzer** - Evaluates logical reasoning and argumentative structure
9. **pathos-analyzer** - Analyzes emotional appeals and affective dimensions
10. **poetic-enhancer** - Transforms plain text into more poetic, evocative language
11. **prose-enhancer** - Analyzes and improves literary quality of written content
12. **speech-research-assistant** - Researches facts and evidence to strengthen arguments

## Available Commands

### review-transcript

Comprehensive workflow for reviewing sermon transcripts with multiple specialized agents.

**Prerequisites:**
- Default: Requires Docker MCP gateway with Obsidian MCP server and additional research tools (Wikipedia, YouTube, web search)
- With `--no-mcp` flag: No prerequisites, uses standard Claude Code tools only

**Usage:**
```bash
/review-transcript <transcript-file> [agents] [mode] [length] [--no-mcp]
/review-transcript help  # Show detailed usage guide
```

**Arguments:**

1. **transcript-file** (required) - Path to your transcript file
2. **agents** (optional, default: `all`) - Which agents to run:
   - `all` - All 10 agents
   - `rhetoric` - Logos, Ethos, Pathos
   - `content` - Facts, Grammar, Gospel
   - `style` - Prose, Poetic, Humor
   - `audience` - Gen Z, Pathos, Humor
   - Custom: comma-separated codes (e.g., `logos,ethos,pathos`)
3. **mode** (optional, default: `full`) - Output format:
   - `full` - Complete analysis with grades and recommendations
   - `analyze` - Findings only, no grades
   - `grade` - Quick grades with verdicts only
   - `recommend` - Actionable rewrites with examples
4. **length** (optional, default: `concise`) - Report length:
   - `none` - No executive summary, minimal reports
   - `concise` - 1-page summary, brief reports
   - `moderate` - 2-page summary, standard reports
   - `extensive` - 3-page summary, detailed reports

**Flags:**

- `--no-mcp` - Use local file output instead of Obsidian MCP server (saves to `transcript-analysis/` directory)

**Examples:**
```bash
# Use defaults (all agents, full analysis, concise summary, Obsidian output)
/review-transcript sermon.md

# Rhetorical analysis only with recommendations
/review-transcript sermon.md rhetoric recommend moderate

# Quick grades from all agents
/review-transcript sermon.md all grade none

# Custom agent selection with detailed recommendations
/review-transcript talk.md logos,ethos,facts recommend extensive

# Use local output instead of Obsidian
/review-transcript sermon.md --no-mcp

# Local output with custom options
/review-transcript sermon.md all full concise --no-mcp
```

**Output:**

- **Default:** Reports saved to your Obsidian vault in the `Transcript Analysis/` folder
- **With --no-mcp:** Reports saved to local `transcript-analysis/` directory

Each analysis includes:
- Individual agent reports in `reports/` subdirectory
- Executive summary (unless length is `none`)
- Grades, findings, and recommendations based on selected mode

## Setup

### Installing Claude Code

Choose one of the following installation methods:

**Homebrew (Recommended for macOS/Linux)**
```bash
brew install --cask claude-code
```

**Curl-based Installation**
```bash
curl -fsSL https://claude.ai/install.sh | bash
```

**NPM (Requires Node.js 18+)**
```bash
npm install -g @anthropic-ai/claude-code
```

**Verify Installation**
```bash
claude doctor
```

### Authentication Options

Claude Code requires authentication through one of these methods:

- **Claude Console API** - Create an account at [console.anthropic.com](https://console.anthropic.com) (requires active billing)
- **Claude Subscription** - Subscribe to Claude Pro or Max at [claude.ai](https://claude.ai)
- **Enterprise Options** - Amazon Bedrock, Google Vertex AI, or Microsoft Foundry (see [Enterprise Authentication Guide](https://code.claude.com/docs/en/iam.md))

### Setting Up Docker MCP Gateway (Optional)

The `/review-transcript` command uses Obsidian MCP server by default to save analysis reports directly to your Obsidian vault. This requires the Docker MCP gateway.

**Prerequisites:**
- [Docker Desktop](https://www.docker.com/products/docker-desktop/) installed and running
- [Obsidian](https://obsidian.md/) installed with a vault created

**Installation Steps:**

1. **Install Docker MCP Gateway**

   The Docker MCP gateway allows Claude Code to connect to MCP servers running in Docker containers.

   ```bash
   # Install the MCP Docker gateway globally
   npx -y @anaisbetts/mcp-docker install
   ```

2. **Configure Obsidian MCP Server**

   Add the Obsidian MCP server to your Claude Code configuration:

   ```bash
   # Add Obsidian MCP server
   claude mcp add obsidian

   # Configure the path to your Obsidian vault
   claude mcp config obsidian vault_path "/path/to/your/obsidian/vault"
   ```

   Replace `/path/to/your/obsidian/vault` with the actual path to your Obsidian vault directory.

3. **Add Additional MCP Servers (Optional)**

   The review-transcript command can leverage additional research tools:

   ```bash
   # Add Wikipedia for fact-checking and research
   claude mcp add wikipedia

   # Add YouTube for transcript analysis
   claude mcp add youtube

   # Add web search capabilities
   claude mcp add web-search
   ```

4. **Verify Setup**

   Start Claude Code and verify MCP servers are connected:

   ```bash
   claude
   ```

   In the Claude Code session, type `/mcp` to see available MCP servers and their status.

**Alternative: Use Local Output**

If you prefer not to set up the Docker MCP gateway, you can use the `--no-mcp` flag with the `/review-transcript` command. This will save reports to a local `transcript-analysis/` directory instead:

```bash
/review-transcript sermon.md --no-mcp
```

### Official Documentation

- [Setup Guide](https://code.claude.com/docs/en/setup.md) - Complete installation and configuration
- [Authentication & IAM](https://code.claude.com/docs/en/iam.md) - Subscription vs. API access setup
- [Quickstart Guide](https://code.claude.com/docs/en/quickstart.md) - Getting started with Claude Code
- [MCP Documentation](https://code.claude.com/docs/en/mcp.md) - Model Context Protocol integration guide

## Getting Started

### Using in This Project

1. Clone this repository and navigate to the project directory
2. Ensure Claude Code is installed and authenticated
3. Start Claude Code:
   ```bash
   claude
   ```
4. The agents and commands are automatically available when running Claude Code from this directory

### Copying to Other Projects

To use these agents and commands in a different project:

```bash
# Navigate to your target project
cd /path/to/your/project

# Create .claude directory structure
mkdir -p .claude/agents .claude/commands

# Copy agents and commands
cp /path/to/agentic-sermon-review/agents/*.md .claude/agents/
cp /path/to/agentic-sermon-review/commands/*.md .claude/commands/
```

### Copying to Global Configuration

To make these agents and commands available across all your projects:

```bash
# Copy agents to global configuration
cp /path/to/agentic-sermon-review/agents/*.md ~/.claude/agents/

# Copy commands to global configuration
cp /path/to/agentic-sermon-review/commands/*.md ~/.claude/commands/
```

**Note:** Project-specific configurations in `.claude/` take precedence over global configurations in `~/.claude/`

### Configuration Locations

- **Project-specific**: `.claude/agents/` and `.claude/commands/` in your project directory
- **Global**: `~/.claude/agents/` and `~/.claude/commands/` (available in all projects)

Agent configurations are located in the `agents/` directory. Custom commands for workflow automation are in the `commands/` directory.
