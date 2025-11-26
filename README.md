# Agentic Sermon Review

An AI-powered tool for analyzing teaching scripts and sermon transcripts using specialized AI agents.

## Table of Contents

- [Overview](#overview)
- [Why Use Local Agents vs. Chat Apps?](#why-use-local-agents-vs-chat-apps)
- [Available Agents](#available-agents)
- [Available Commands](#available-commands)
  - [review-transcript](#review-transcript)
- [Setup](#setup)
  - [Installing Claude Code](#installing-claude-code)
  - [Authentication Options](#authentication-options)
- [Getting Started](#getting-started)
  - [Using in This Project](#using-in-this-project)
  - [Copying to Other Projects](#copying-to-other-projects)
  - [Copying to Global Configuration](#copying-to-global-configuration)
  - [Configuration Locations](#configuration-locations)
  - [Setting Up Docker MCP Toolkit (Optional)](#setting-up-docker-mcp-toolkit-optional)
  - [Official Documentation](#official-documentation)

## Overview

This project provides a comprehensive framework for reviewing and improving teaching content through multiple AI-powered analysis agents. Each agent specializes in a different aspect of communication analysis, providing detailed feedback on effectiveness, clarity, engagement, and theological accuracy.

<img width="801" height="499" alt="agentic-sermon-review" src="https://github.com/user-attachments/assets/4f0a6262-f54d-4284-b4f6-183fefe41ec2" />

## Why Use Local Agents vs. Chat Apps?

When analyzing long transcripts, uploading them to chat applications like ChatGPT or Google AI Studio presents significant limitations:

**Context Window Constraints**: Chat apps use a single shared context window for the entire conversation. A 10,000-word transcript might consume 80%+ of the available context, leaving minimal room for detailed analysis, multiple perspectives, or iterative improvements.

**Agentic Sermon Review solves this through specialized Claude Code agents:**

- **Independent context windows** - Each agent runs with its own dedicated context window, not competing for space
- **Parallel processing** - Multiple agents analyze the same transcript simultaneously without context dilution
- **Specialized depth** - Each agent can deeply analyze one dimension (rhetoric, grammar, humor, theology) with full context awareness
- **Comprehensive coverage** - Run 10+ specialized analyses on the same transcript, each with complete access to the full content

**Example**: For a 15,000-word sermon transcript:
- *Chat app approach*: Transcript uses ~80% of context → limited analysis depth
- *Local agents approach*: Each of 12 agents gets a fresh context window → comprehensive multi-dimensional analysis

**Learn more**: [Claude Code Agents Documentation](https://code.claude.com/docs/en/agents.md)
>>>>>>> df3c207 (Readme updates)

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

### Setting Up Docker MCP Toolkit (Optional)

The `/review-transcript` command uses Obsidian MCP server by default to save analysis reports directly to your Obsidian vault. This requires Docker MCP Toolkit, which provides 200+ pre-built, containerized MCP servers with one-click deployment.

**Prerequisites:**
- [Docker Desktop 4.40 or later](https://www.docker.com/products/docker-desktop/)
- MCP Toolkit enabled in Docker Desktop
- [Obsidian](https://obsidian.md/) installed with a vault created (for default output)

**Step 1: Connect Claude Code to Docker MCP Toolkit**

Choose one of the following methods:

**Option 1: One-Click Connection (Recommended)**

1. Open Docker Desktop
2. Navigate to **MCP Toolkit** in the sidebar
3. Click the **Clients** tab
4. Find "Claude Code" in the list
5. Click **Connect**

Docker Desktop automatically configures the MCP Gateway connection.

**Option 2: Manual Command Line Setup**

If you prefer command-line setup or need project-specific configuration:

```bash
# Navigate to your project folder
cd /path/to/your/project

# Connect Claude Code to Docker MCP Toolkit
docker mcp client connect claude-code
```

You'll see output confirming the connection:

```
=== Project-wide MCP Configurations ===
● claude-code: connected
    MCP_DOCKER: Docker MCP Catalog (gateway server) (stdio)
```

**What's happening under the hood?**

The connection creates a `.mcp.json` file in your project:

```json
{
  "mcpServers": {
    "MCP_DOCKER": {
      "command": "docker",
      "args": ["mcp", "gateway", "run"],
      "type": "stdio"
    }
  }
}
```

**Step 2: Restart Claude Code**

```bash
# Exit Claude Code if running, then restart
claude code
```

On first run, you'll see a prompt about the new MCP server. Choose **"Use this and all future MCP servers in this project"** to automatically use MCP servers enabled in Docker Desktop.

**Step 3: Verify the Connection**

Inside Claude Code, type `/mcp` to see available MCP servers. You should see the Docker MCP Gateway listed.

**Step 4: Configure Obsidian MCP Server**

In Docker Desktop → MCP Toolkit → Catalog:

1. Search for "Obsidian"
2. Click **+ Add**
3. Go to the **Configuration** tab
4. Configure your Obsidian vault path:
   - `vault_path`: `/path/to/your/obsidian/vault`
5. Click **Save**
6. Click **Start Server**

**Step 5: Add Additional MCP Servers (Optional)**

The review-transcript command can leverage additional research tools:

In Docker Desktop → MCP Toolkit → Catalog:

- **Wikipedia** - For fact-checking and research
- **YouTube** - For transcript analysis
- **Web Search** - For web search capabilities

For each server: Click **+ Add** → Configure if needed → **Start Server**

**Verify Setup**

In Claude Code, type `/mcp` again. You should now see your enabled MCP servers listed.

**Alternative: Use Local Output**

If you prefer not to set up Docker MCP Toolkit, you can use the `--no-mcp` flag with the `/review-transcript` command. This saves reports to a local `transcript-analysis/` directory instead:

```bash
/review-transcript sermon.md --no-mcp
```

### Official Documentation

- [Setup Guide](https://code.claude.com/docs/en/setup.md) - Complete installation and configuration
- [Authentication & IAM](https://code.claude.com/docs/en/iam.md) - Subscription vs. API access setup
- [Quickstart Guide](https://code.claude.com/docs/en/quickstart.md) - Getting started with Claude Code
- [MCP Documentation](https://code.claude.com/docs/en/mcp.md) - Model Context Protocol integration guide
- [Docker MCP Toolkit Guide](https://www.docker.com/blog/add-mcp-servers-to-claude-code-with-mcp-toolkit/) - Official Docker guide for MCP setup


