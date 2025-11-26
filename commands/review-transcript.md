---
description: Comprehensive transcript analysis using multiple specialized agents
hints: ["Path to transcript file", "Agents: all, rhetoric, content, style, or specific codes", "Mode: full, analyze, grade, or recommend", "Length: none, concise, moderate, or extensive", "Add --no-mcp to use local output"]
---

I'll perform a comprehensive analysis of the transcript using specialized agents. Each will provide their expert perspective based on the selected mode.

## Prerequisites

**With MCP (default):**
- Docker MCP gateway configured and running
- Obsidian MCP server - For saving analysis reports to your vault
- Additional MCP tools - Wikipedia, YouTube transcripts, web search, and other research tools used by the agents

**With --no-mcp flag:**
- No prerequisites - uses standard Claude Code tools only
- Reports saved to local `transcript-analysis/` directory

## Output Modes

| Mode | Description |
|------|-------------|
| `full` | Complete analysis with grades, findings, and recommendations (default) |
| `analyze` | Analysis and findings only - no grades or recommendations |
| `grade` | Grades only with brief verdicts - minimal output |
| `recommend` | **Actionable recommendations with improved excerpt rewrites** |

## Length Settings

| Length | Description |
|--------|-------------|
| `none` | No executive summary - individual agent reports only |
| `concise` | 1-page executive summary with brief agent reports (default) |
| `moderate` | 2-page executive summary with standard agent reports |
| `extensive` | 3-page executive summary with detailed agent reports |

## Available Agents

| Code | Agent | Description |
|------|-------|-------------|
| `genz` | Gen Z Advisor | Evaluates language and cultural resonance for Gen Z audiences |
| `humor` | Humor Analyst | Identifies humor opportunities and engagement points |
| `logos` | Logos Analyzer | Examines logical reasoning and argumentative structure |
| `ethos` | Ethos Analyzer | Assesses credibility, authority, and trustworthiness |
| `gospel` | Gospel Clarity (Greg Stier) | Ensures gospel message clarity and completeness |
| `prose` | Prose Enhancer | Analyzes literary quality, rhythm, and word choice |
| `grammar` | Grammar Police | Reviews grammatical correctness for speech patterns |
| `facts` | Fact Checker | Verifies factual claims, statistics, and assertions |
| `poetic` | Poetic Enhancer | Evaluates poetic language and evocative expression |
| `pathos` | Pathos Analyzer | Examines emotional appeals and affective dimensions |

## Agent Presets

- **`all`** - Run all 10 agents (default)
- **`rhetoric`** - Rhetorical analysis: `logos`, `ethos`, `pathos`
- **`content`** - Content quality: `facts`, `grammar`, `gospel`
- **`style`** - Style analysis: `prose`, `poetic`, `humor`
- **`audience`** - Audience focus: `genz`, `pathos`, `humor`

## Usage Examples

```bash
/review-transcript help                                      # Show usage guide
/review-transcript sermon.md                                 # All agents, full mode, concise length (defaults)
/review-transcript sermon.md all full concise                # Explicit defaults
/review-transcript sermon.md rhetoric analyze moderate       # Analysis mode, 2-page summary
/review-transcript sermon.md all grade none                  # Quick grades only, no executive summary
/review-transcript sermon.md all recommend extensive         # Full recommendations, 3-page summary
/review-transcript talk.md logos,ethos recommend concise     # Specific agents with 1-page summary
/review-transcript sermon.md --no-mcp                        # Use local output instead of Obsidian
/review-transcript sermon.md all full concise --no-mcp       # Explicit with local output
```

---

## Execution Plan

**IMPORTANT - Execution Order**:
1. Parse arguments (Step 1)
2. Setup output location (Step 2)
3. **Read the transcript FIRST** (Step 3)
4. **Then** launch agents with the actual transcript content (Step 4)
5. Generate executive summary (Step 5)
6. Report completion (Step 6)

### Step 1: Parse Arguments

**Check for help request first:**
If `$1` is `help`, `--help`, or `-h`: Display the usage guide below and stop execution.

---

#### Help Output

```
REVIEW-TRANSCRIPT - Comprehensive transcript analysis using specialized agents

USAGE:
  /review-transcript <transcript> [agents] [mode] [length] [--no-mcp]
  /review-transcript help

ARGUMENTS:
  transcript    Path to transcript file (required)
  agents        Agent selection (default: all)
                - all: All 10 agents
                - rhetoric: logos, ethos, pathos
                - content: facts, grammar, gospel
                - style: prose, poetic, humor
                - audience: genz, pathos, humor
                - Custom: comma-separated codes (e.g., logos,ethos,pathos)

  mode          Output mode (default: full)
                - full: Complete analysis with grades and recommendations
                - analyze: Findings only, no grades
                - grade: Quick grades with verdicts only
                - recommend: Actionable rewrites with examples

  length        Report length (default: concise)
                - none: No executive summary, minimal reports
                - concise: 1-page summary, brief reports
                - moderate: 2-page summary, standard reports
                - extensive: 3-page summary, detailed reports

FLAGS:
  --no-mcp      Use local file output instead of Obsidian MCP server
                Reports saved to transcript-analysis/ directory

AVAILABLE AGENTS:
  Code    Agent Name              Description
  ─────────────────────────────────────────────────────────────────────────
  genz    Gen Z Advisor           Evaluates language and cultural
                                  resonance for Gen Z audiences

  humor   Humor Analyst           Identifies humor opportunities and
                                  engagement points

  logos   Logos Analyzer          Examines logical reasoning and
                                  argumentative structure

  ethos   Ethos Analyzer          Assesses credibility, authority, and
                                  trustworthiness

  gospel  Greg Stier              Ensures gospel message clarity and
                                  completeness using GOSPEL acrostic

  prose   Prose Enhancer          Analyzes literary quality, rhythm,
                                  and word choice

  grammar Grammar Police          Reviews grammatical correctness for
                                  speech patterns

  facts   Fact Checker            Verifies factual claims, statistics,
                                  and assertions

  poetic  Poetic Enhancer         Evaluates poetic language and
                                  evocative expression

  pathos  Pathos Analyzer         Examines emotional appeals and
                                  affective dimensions

EXAMPLES:
  /review-transcript sermon.md
  /review-transcript talk.md rhetoric recommend moderate
  /review-transcript speech.md logos,ethos,pathos grade none
  /review-transcript sermon.md --no-mcp

OUTPUT:
  Default: Obsidian vault at Transcript Analysis/<name>-<timestamp>/
  With --no-mcp: Local directory at transcript-analysis/<name>-<timestamp>/
```

---

**If not a help request, parse arguments:**

First, check for `--no-mcp` flag in any argument position and set `USE_MCP` accordingly:
- If any argument contains `--no-mcp`: set `USE_MCP=false`
- Otherwise: set `USE_MCP=true`

Then parse positional arguments (ignoring `--no-mcp`):
- **Transcript path**: First non-flag argument (required)
- **Agent selection**: Second non-flag argument (defaults to `all` if not provided)
- **Output mode**: Third non-flag argument (defaults to `full` if not provided)
- **Length setting**: Fourth non-flag argument (defaults to `concise` if not provided)

Parse the agent selection:
- If empty or `all`: use all 10 agents
- If a preset (`rhetoric`, `content`, `style`, `audience`): expand to preset agents
- Otherwise: parse as comma-separated agent codes

Parse the output mode:
- `full`: Complete analysis (default)
- `analyze`: Findings only, no grades or recommendations
- `grade`: Quick grades with verdicts only
- `recommend`: Actionable recommendations with improved excerpts

Parse the length setting:
- `none`: Skip executive summary entirely, minimal agent reports
- `concise`: 1-page executive summary, brief agent reports (default)
- `moderate`: 2-page executive summary, standard agent reports
- `extensive`: 3-page executive summary, detailed agent reports

### Step 2: Setup Output Location

Extract filename from transcript path:
```
TRANSCRIPT_NAME=$(basename "<transcript-path>" | sed 's/\.[^.]*$//')
TIMESTAMP=$(date +%Y%m%d-%H%M%S)
```

**If USE_MCP is true (default):**

Determine the Obsidian vault path for saving reports:
```
ANALYSIS_FOLDER="Transcript Analysis/${TRANSCRIPT_NAME}-${TIMESTAMP}"
```

All reports will be saved to the Obsidian vault using the `mcp__MCP_DOCKER__obsidian_append_content` tool:
- Individual reports: `${ANALYSIS_FOLDER}/reports/[agent-code]-report.md`
- Executive summary: `${ANALYSIS_FOLDER}/executive-summary.md`

**If USE_MCP is false (--no-mcp flag):**

Determine the local directory path for saving reports:
```
ANALYSIS_FOLDER="transcript-analysis/${TRANSCRIPT_NAME}-${TIMESTAMP}"
```

Create the output directory structure:
```
mkdir -p "${ANALYSIS_FOLDER}/reports"
```

All reports will be saved to local files using the Write tool:
- Individual reports: `${ANALYSIS_FOLDER}/reports/[agent-code]-report.md`
- Executive summary: `${ANALYSIS_FOLDER}/executive-summary.md`

### Step 3: Read Transcript

First, read the transcript file using the Read tool to get its full content:

**Read the file**: Use `Read` tool with the path from `$1`

**Store the content**: Save the complete transcript text to include in agent prompts

### Step 4: Launch Selected Agents in Parallel

Launch **only the selected agents** in parallel using a single message with multiple Task tool calls.

**CRITICAL**: Each agent prompt MUST include the complete transcript content that was read in Step 3. Do NOT use placeholder variables like `${FULL_TRANSCRIPT_CONTENT}`. Instead, embed the actual transcript text directly in the `<transcript>` section of each agent's prompt.

Each agent receives mode-specific instructions with length-appropriate depth:

**Length-based report guidelines:**
- `none`: Minimal reports - 3-5 bullet points, no prose
- `concise`: Brief reports - ONE PAGE maximum (default)
- `moderate`: Standard reports - UP TO TWO PAGES with more detail
- `extensive`: Detailed reports - UP TO THREE PAGES with comprehensive analysis

Include the length setting in each agent's prompt so they produce appropriately sized reports.

#### Mode: `full` (default)

```markdown
# [Agent Name] Report

**Grade: [X]/100**

## Summary
[2-3 sentences max]

## Key Findings
- [Finding 1]
- [Finding 2]
- [Finding 3]

## Top 3 Recommendations
1. [Recommendation]
2. [Recommendation]
3. [Recommendation]

## Verdict
[One sentence overall assessment]
```

#### Mode: `analyze`

```markdown
# [Agent Name] Analysis

## Summary
[2-3 sentences max]

## Key Findings
- [Finding 1]
- [Finding 2]
- [Finding 3]
- [Finding 4]
- [Finding 5]

## Detailed Analysis
[Paragraph with deeper analysis - no grades, no recommendations]
```

#### Mode: `grade`

```markdown
# [Agent Name] Grade

**Grade: [X]/100**

## Verdict
[One sentence overall assessment]

## Quick Notes
- [Brief note 1]
- [Brief note 2]
```

#### Mode: `recommend`

This mode provides **actionable recommendations with specific excerpt improvements**, organized by section:

```markdown
# [Agent Name] Recommendations

## Section Analysis

### [Section Name] (Lines X-Y)

**Assessment**: [1-2 sentences on what needs improvement from this agent's perspective]

**Original**:
> [Quote the exact original text that needs improvement]

**Improved**:
> [Provide the rewritten/improved version]

**Why this works**: [1-2 sentences explaining the improvement]

---

### [Section Name] (Lines X-Y)

**Assessment**: [What needs improvement]

**Original**:
> [Quote the exact original text]

**Improved**:
> [Provide the rewritten/improved version]

**Why this works**: [Explanation]

---

### [Section Name] (Lines X-Y)

**Assessment**: [What needs improvement]

**Original**:
> [Quote the exact original text]

**Improved**:
> [Provide the rewritten/improved version]

**Why this works**: [Explanation]

---

## Patterns to Address

[List 3-5 recurring patterns or issues this agent identified across the transcript]

- ❌ [Pattern to eliminate]: [Brief explanation]
- ❌ [Pattern to eliminate]: [Brief explanation]
- ✅ [Pattern to adopt]: [Brief explanation]
```

**Important for `recommend` mode**:
- Agents must quote actual text from the transcript and provide concrete rewrites, not abstract advice
- Organize recommendations by section with line numbers for easy reference
- Include "Why this works" for each improvement
- Identify patterns that appear across multiple sections

**Save report:**
- If USE_MCP is true: Use `mcp__MCP_DOCKER__obsidian_append_content` tool with filepath: `${ANALYSIS_FOLDER}/reports/[agent-code]-report.md`
- If USE_MCP is false: Use Write tool with filepath: `${ANALYSIS_FOLDER}/reports/[agent-code]-report.md`

### Step 5: Generate Executive Summary

After all agent reports complete, generate a mode-specific executive summary based on the length setting.

**If length is `none`**: Skip executive summary entirely and proceed to Step 6.

**Save executive summary:**
- If USE_MCP is true: Use `mcp__MCP_DOCKER__obsidian_append_content` tool with filepath: `${ANALYSIS_FOLDER}/executive-summary.md`
- If USE_MCP is false: Use Write tool with filepath: `${ANALYSIS_FOLDER}/executive-summary.md`

**Length-based summary guidelines:**
- `concise`: 1-page summary - essential information only
- `moderate`: 2-page summary - standard detail level
- `extensive`: 3-page summary - comprehensive with full details

**Scaling templates by length:**
- `concise`: Use only the essential sections (grades table, top 3-5 items, one-paragraph synthesis)
- `moderate`: Include most sections with moderate detail
- `extensive`: Include all sections with comprehensive detail (full templates below)

#### Mode: `full` (default)

```markdown
# Executive Summary

**Transcript**: [filename] | **Date**: [timestamp] | **Overall Grade: [X]/100**

## Grades at a Glance
| Agent | Grade |
|-------|-------|
| ... | ... |

## Top 5 Recommendations
1. [Recommendation] - *[Agent]*
2. [Recommendation] - *[Agent]*
3. [Recommendation] - *[Agent]*
4. [Recommendation] - *[Agent]*
5. [Recommendation] - *[Agent]*

## Bottom Line
[2-3 sentence synthesis]
```

#### Mode: `analyze`

```markdown
# Analysis Summary

**Transcript**: [filename] | **Date**: [timestamp]

## Key Findings by Agent
[Aggregate findings organized by theme or importance]

## Overall Assessment
[2-3 paragraph synthesis of all analyses]
```

#### Mode: `grade`

```markdown
# Grade Summary

**Transcript**: [filename] | **Overall Grade: [X]/100**

| Agent | Grade | Verdict |
|-------|-------|---------|
| ... | ... | ... |

**Bottom Line**: [One sentence]
```

#### Mode: `recommend`

```markdown
# Comprehensive Multi-Dimensional Analysis

**Transcript**: [filename] | **Date**: [timestamp]

---

## Executive Summary

[2-3 sentence overview of the transcript's quality and potential]

### Universal Strengths
- [Strength identified by multiple agents]
- [Strength identified by multiple agents]
- [Strength identified by multiple agents]

### Universal Weaknesses
- [Weakness identified by multiple agents]
- [Weakness identified by multiple agents]
- [Weakness identified by multiple agents]

---

## Critical Fixes Required

Issues all agents agree need immediate attention:

### 1. [Issue Title]

[Description of the critical issue]

**Examples**:
- Line X: "[original]" → "[fixed]"
- Line Y: "[original]" → "[fixed]"

**Impact**: [Why this must be fixed immediately]

---

## High-Priority Enhancements by Section

### [SECTION NAME] (Lines X-Y)

**Agent Assessments**:
- **Prose Analysis**: "[What prose agent found]"
- **Humor Analysis**: "[What humor agent found]"
- **Poetic Analysis**: "[What poetic agent found]"
- **Grammar Analysis**: "[What grammar agent found]"
[Include all relevant agents that commented on this section]

**Integrated Recommendation**:

> [Provide the fully rewritten section that incorporates all agent suggestions into a unified, polished version]

**Why this works**: [Brief explanation of how this integrates agent feedback - e.g., "Warmth (humor), clarity (prose), anticipation (poetic setup)"]

---

### [SECTION NAME] (Lines X-Y)

**Agent Assessments**:
- **[Agent 1]**: "[Assessment]"
- **[Agent 2]**: "[Assessment]"
- **[Agent 3]**: "[Assessment]"

**Integrated Recommendation**:

> [Fully rewritten section]

**Why this works**: [Explanation]

---

[Continue for 5-8 highest-priority sections]

---

## Strategic Enhancements by Category

### Humor Placement

The humor analysis identified [N] opportunities. Here are the highest-value additions:

1. [Location/section] - [Purpose/effect]
2. [Location/section] - [Purpose/effect]
3. [Location/section] - [Purpose/effect]

**Where NOT to add humor** (all agents agree):
- [Section to keep serious]
- [Section to keep serious]

---

### Rhetorical Devices to Deploy

1. **[Device Name]** (e.g., Anaphora, Antithesis, Parallelism)

   [Example of how to apply it]

2. **[Device Name]**

   [Example]

---

### Patterns to Eliminate

Issues flagged across multiple analyses:

- ❌ **[Pattern]**: [Examples and why to eliminate]
- ❌ **[Pattern]**: [Examples and why to eliminate]
- ✅ **Replace with**: [Better approach]

---

## Implementation Roadmap

### Phase 1: Critical Fixes ([estimated time])
- [Task 1]
- [Task 2]
- [Task 3]

### Phase 2: High-Impact Enhancements ([estimated time])
- [Task 1]
- [Task 2]
- [Task 3]

### Phase 3: Refinement ([estimated time])
- [Task 1]
- [Task 2]
- [Task 3]

**Total Time Investment**: [X-Y hours] for transformative improvement

---

## Conflicts & Trade-offs to Consider

### [Trade-off Title]
- [Option A]
- [Option B]
- **Recommendation**: [Suggested approach]

### [Trade-off Title]
- [Consideration]
- **Recommendation**: [Suggested approach]

---

## Success Metrics

After implementing these changes, the transcript should achieve:

- ✅ [Measurable outcome 1]
- ✅ [Measurable outcome 2]
- ✅ [Measurable outcome 3]
- ✅ [Measurable outcome 4]
- ✅ [Measurable outcome 5]

---

## Next Steps

1. [Immediate action]
2. [Priority decision needed]
3. [Recommended focus area]
4. [Testing/validation step]

[Closing statement about the content's potential]
```

**Important for `recommend` mode**:
- Synthesize overlapping agent suggestions into unified improvements in the "Enhancements by Section"
- Each section should show what multiple agents said, then provide ONE integrated rewrite
- Include line numbers for easy reference
- The Implementation Roadmap should give realistic time estimates
- Success Metrics should be specific and measurable

### Step 6: Report Completion

Provide:
- Output location path: `${ANALYSIS_FOLDER}`
- Overall composite grade (if applicable for mode)
- Top 3 priority recommendations (if length is not `none`)
- Summary of key findings

**If length is `none`**: Simply report that individual agent reports are available and provide the folder path.

**Output location:**
- If USE_MCP is true: Reports are accessible in your Obsidian vault under "Transcript Analysis" folder
- If USE_MCP is false: Reports are accessible in the local `transcript-analysis/` directory

---

Let's begin the analysis!
