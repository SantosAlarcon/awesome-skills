# awesome-skills

A community-driven collection of AI agent skills, organized by purpose and programming language.

## What are Skills?

Skills are reusable knowledge packages for AI agents — they contain prompts, best practices, code patterns, and resources that help agents perform specific tasks better. Skills can target:

- **General capabilities** (debugging, testing, security)
- **Purpose-specific domains** (web development, data science)
- **Language-specific stacks** (Python, TypeScript, Go)

## Repository Structure

```
awesome-skills/
├── skills/           # Skills organized by purpose/domain
│   ├── web-development/
│   ├── data-science/
│   └── testing/
├── languages/       # Skills organized by programming language
│   ├── python/
│   ├── typescript/
│   └── go/
└── .github/         # GitHub workflows and templates
```

### Skills by Purpose

| Category | Description |
|----------|-------------|
| `web-development/` | Frontend, backend, and API design skills |
| `data-science/` | ML engineering, data analysis, visualization |
| `testing/` | Unit, integration, and E2E testing strategies |

### Skills by Language

| Language | Frameworks & Tools |
|----------|-------------------|
| `python/` | FastAPI, Django, Pandas, Testing |
| `typescript/` | React, Next.js, Node.js, Testing |
| `go/` | APIs, Microservices, Testing |

## Skill Format

Each skill follows a standard structure:

```
skill-name/
├── SKILL.md          # Required: description, usage, examples
├── examples/         # Optional: code examples
├── prompts/         # Optional: reusable prompt templates
└── resources/       # Optional: links to relevant docs
```

### Required: SKILL.md

```markdown
# Skill Name

## Description
Brief description of what this skill does.

## When to Use
When to apply this skill.

## How to Use
Step-by-step instructions.

## Examples
Code or prompt examples.
```

## Contributing

We welcome contributions from the community! See [CONTRIBUTING.md](CONTRIBUTING.md) for:

- How to submit new skills
- Skill format requirements
- Review process
- Coding standards

## Quick Start

1. Browse skills by [purpose](skills/) or [language](languages/)
2. Read the skill's `SKILL.md` for usage instructions
3. Copy relevant prompts or patterns into your agent's context
4. Contribute your own skills to help others

## License

Apache License 2.0 — see [LICENSE](LICENSE) for details.
