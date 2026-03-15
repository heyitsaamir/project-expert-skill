# project-expert

A [Claude Code skill](https://docs.anthropic.com/en/docs/claude-code/skills) that creates lightweight "project expert" agents — so you can reference any codebase from any other project.

## Why?

Have you ever worked on one project and wished you were able to reference it with ease from a different project? Current methods with Claude are a bit clunky — you either copy-paste context between sessions, maintain heavy documentation, or just hope Claude figures it out. None of that scales.

This skill fixes that. Run it once on a project, and it generates a lightweight agent that lives at `~/.claude/agents/`. From that point on, **any** Claude Code session can invoke that expert to answer questions about the codebase — architecture, patterns, where things live, how things work. The expert explores the code on-demand using Glob/Grep/Read, so there's no upfront analysis and answers are always fresh.

## Install

```bash
# Clone the repo
git clone https://github.com/AamirJawworsky/project-expert-skill.git ~/.claude/skills/project-expert

# Or symlink if you cloned elsewhere
ln -s /path/to/project-expert-skill ~/.claude/skills/project-expert
```

## Usage

From any Claude Code session:

```
create a project expert for /path/to/my-project
```

Or if you're already in the project directory:

```
create a project expert for this project
```

Claude will:
1. Quick-scan the project (README, package.json, pyproject.toml, etc.)
2. Show you what it found — name, description, tech stack — and let you confirm or correct
3. Generate `~/.claude/agents/<project-name>-expert.md`

That's it. The expert is now available from every Claude Code session.

## What you get

Each generated expert agent:
- **Explores on-demand** — uses Glob/Grep/Read to answer questions from actual source code
- **Always current** — no stale pre-analysis, reads the code live
- **Cites file paths** — every answer references specific files and line numbers
- **Read-only by default** — safe to use; opt into write access if you want the expert to make changes too

## Example

```
# In project A:
> create a project expert for ~/code/my-api

# Later, in project B:
> how does my-api handle authentication?
# Claude routes to the my-api-expert agent, which explores the codebase and answers
```

## How it works

The skill creates a markdown agent file at `~/.claude/agents/` with:
- Frontmatter that tells Claude when and how to invoke it
- The project path, description, and tech stack
- Instructions to explore the codebase on-demand rather than relying on cached knowledge

No database, no indexing, no background processes. Just a markdown file that teaches Claude how to be an expert on your project.
