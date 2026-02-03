# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Repository Overview

This is a Claude Code plugin marketplace repository containing **63 focused plugins** with **85 specialized agents**, **47 agent skills**, and **44 development tools** for software development automation and orchestration.

### Core Architecture

- **Plugin-based system**: Each plugin is isolated in `plugins/` with its own agents, commands, and skills
- **Marketplace catalog**: All plugins defined in `.claude-plugin/marketplace.json`
- **Progressive disclosure**: Skills use three-tier architecture (metadata → instructions → resources)
- **Hybrid model strategy**: 47 Haiku agents for fast execution, 97 Sonnet agents for complex reasoning

## Repository Structure

```
/
├── .claude-plugin/
│   └── marketplace.json          # Marketplace catalog (63 plugins)
├── plugins/                       # 63 plugin directories
│   ├── {plugin-name}/
│   │   ├── agents/               # Specialized AI agents (.md files)
│   │   ├── commands/             # Slash commands/tools (.md files)
│   │   └── skills/               # Agent skills (SKILL.md files)
├── docs/                          # Documentation
│   ├── plugins.md                # Complete plugin catalog
│   ├── agents.md                 # Agent reference
│   ├── agent-skills.md           # Skills guide
│   ├── usage.md                  # Usage guide
│   └── architecture.md           # Architecture patterns
└── README.md                      # Quick start guide
```

## File Conventions

### Agents (`plugins/{name}/agents/*.md`)
```yaml
---
name: agent-name                  # hyphen-case
description: What it does. Use PROACTIVELY when... # < 1024 chars
model: sonnet|haiku              # Model selection
---
# Agent system prompt content
```

### Commands (`plugins/{name}/commands/*.md`)
```yaml
---
name: command-name                # hyphen-case
description: What it does         # < 1024 chars
---
# Command implementation content
```

### Skills (`plugins/{name}/skills/{skill-name}/SKILL.md`)
```yaml
---
name: skill-name                  # hyphen-case
description: What it does. Use when... # < 1024 chars, must include "Use when"
---
# Skill content with progressive disclosure
```

### Marketplace Entry (`.claude-plugin/marketplace.json`)
```json
{
  "name": "plugin-name",
  "source": "./plugins/plugin-name",
  "description": "Plugin description",
  "version": "1.2.0",
  "category": "development|security|infrastructure|...",
  "commands": ["./commands/cmd.md"],
  "agents": ["./agents/agent.md"],
  "skills": ["./skills/skill-name"]
}
```

## Development Workflow

### Adding a New Plugin

1. **Create plugin directory**: `plugins/{plugin-name}/`
2. **Create subdirectories as needed**:
   - `agents/` for AI agents
   - `commands/` for slash commands
   - `skills/` for agent skills
3. **Add content files** with proper frontmatter
4. **Register in marketplace**: Add entry to `.claude-plugin/marketplace.json`
5. **Update documentation**: Add to appropriate docs file

### Adding Components to Existing Plugins

**Add an Agent:**
1. Create `plugins/{plugin}/agents/{name}.md`
2. Add frontmatter with name, description, model
3. Add to plugin's `agents` array in marketplace.json

**Add a Command:**
1. Create `plugins/{plugin}/commands/{name}.md`
2. Add frontmatter with name, description
3. Add to plugin's `commands` array in marketplace.json

**Add a Skill:**
1. Create `plugins/{plugin}/skills/{name}/SKILL.md`
2. Add frontmatter with name and "Use when" description
3. Optional: Add `assets/`, `references/`, `scripts/` subdirectories
4. Add to plugin's `skills` array in marketplace.json

### Naming Conventions

- **All names**: lowercase, hyphen-separated (kebab-case)
- **Plugins**: `plugin-name` (e.g., `python-development`)
- **Agents**: `agent-name` (e.g., `python-pro`, `backend-architect`)
- **Commands**: `command-name` (e.g., `test-generate`, `security-hardening`)
- **Skills**: `skill-name` (e.g., `async-python-patterns`)

### Version Management

- Semantic versioning: `MAJOR.MINOR.PATCH`
- Update version in marketplace.json when modifying plugins
- Maintain backward compatibility for MINOR/PATCH updates
- Document breaking changes for MAJOR updates

## Key Design Principles

### Single Responsibility
- Each plugin has one focused purpose
- Average 3.4 components per plugin
- Clear boundaries between plugins
- Composable for complex workflows

### Token Efficiency
- Granular plugins load only what's needed
- Skills use progressive disclosure
- No unnecessary resources in context
- Isolated dependencies

### Model Selection Strategy

**Use Haiku for:**
- Code generation from clear specs
- Test generation with patterns
- Documentation with templates
- Infrastructure operations
- Database query optimization
- SEO optimization tasks
- Deployment pipelines

**Use Sonnet for:**
- System architecture design
- Technology selection decisions
- Security audits and reviews
- Complex AI/ML pipelines
- Language-specific expertise
- Multi-agent orchestration
- Business-critical operations

### Agent Skills Architecture

Skills enable progressive disclosure:
1. **Metadata** (frontmatter): Always loaded, enables discovery
2. **Instructions**: Core guidance, loaded when skill activated
3. **Resources**: Examples/templates in subdirectories, loaded on demand

Subdirectory structure for skills:
- `assets/` - Templates, config files, examples
- `references/` - Best practices, patterns, guides
- `scripts/` - Helper scripts and automation

## Categories

The marketplace organizes plugins into 23 categories:

- development, documentation, workflows, testing, quality
- ai-ml, data, database, operations, performance
- infrastructure, security, languages, blockchain
- finance, payments, gaming, marketing, business

## Git Workflow

- Branch: `main` (primary branch)
- No build or test commands (markdown-based repository)
- All content is documentation and configuration
- Modified files staged with `git add`
- Commit with descriptive messages following repository style

## Important Notes

- **No code execution**: This is a documentation/configuration repository
- **File format**: All agents/commands/skills are markdown (.md)
- **JSON validation**: marketplace.json must be valid JSON
- **Frontmatter required**: YAML frontmatter in all .md files
- **Consistent formatting**: Follow existing file patterns
- **Clear descriptions**: Activation criteria in descriptions crucial for agent selection

## Quality Standards

### For All Content
- Clear, concise descriptions (< 1024 characters)
- Hyphen-case naming throughout
- Proper YAML frontmatter
- Focused scope (single responsibility)
- Complete documentation

### For Agent Descriptions
- Must include "Use PROACTIVELY when..." or "Use when..."
- Specify activation criteria clearly
- Indicate model preference (sonnet/haiku)

### For Skill Descriptions
- Must include "Use when..." phrase
- Describe activation triggers explicitly
- Keep metadata tier minimal for token efficiency

## Common Patterns

### Multi-Agent Workflows
Orchestrator plugins coordinate multiple agents:
1. Architecture/planning phase (Sonnet)
2. Implementation phase (Haiku/Sonnet)
3. Testing phase (Haiku)
4. Review phase (Sonnet)
5. Deployment phase (Haiku)

### Plugin Composition
Complex tasks combine multiple plugins:
- `backend-development` + `security-scanning` + `unit-testing`
- `kubernetes-operations` + `cloud-infrastructure` + `observability-monitoring`
- `llm-application-dev` + `python-development` + `deployment-strategies`

### Skill Activation
Skills load knowledge when agents need specialized expertise:
- Agent selected via natural language or command
- Skill activated based on "Use when" criteria
- Resources loaded progressively as needed
