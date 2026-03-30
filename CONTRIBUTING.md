# Contributing to awesome-skills

Thank you for your interest in contributing! This guide explains how to add skills to the repository.

## Ways to Contribute

- **New skills**: Add a skill for a purpose or language not yet covered
- **Improvements**: Enhance existing skills with better examples or clearer instructions
- **Corrections**: Fix errors or outdated information
- **Translations**: Translate skills into other languages

## Skill Requirements

### Required Files

Every skill must include:

- [ ] `SKILL.md` — Main skill documentation

### Optional Files

- [ ] `examples/` — Code examples demonstrating the skill
- [ ] `prompts/` — Reusable prompt templates
- [ ] `resources/` — Links to documentation, tools, or learning materials

### SKILL.md Structure

```markdown
# Skill Name

## Description
What this skill does and why it matters.

## When to Use
Specific scenarios where this skill is applicable.
List any prerequisites or context needed.

## How to Use
Step-by-step instructions for applying the skill.
Include any configuration or setup required.

## Examples
At least one practical example showing the skill in action.
Code examples should be complete and runnable.

## Best Practices
Optional: Tips and common pitfalls to avoid.

## Related Skills
Optional: Links to related skills in this repository.
```

### Quality Standards

- **Clear and concise**: Write for a technical audience but avoid jargon
- **Practical**: Include real examples that users can adapt
- **Cross-platform**: Where possible, note compatibility across different agent platforms
- **Attributed**: If adapting from another source, provide attribution

## Submission Process

1. **Fork** the repository
2. **Create** your skill in the appropriate directory:
   - Purpose-based: `skills/<category>/<skill-name>/`
   - Language-based: `languages/<language>/<framework>/<skill-name>/`
3. **Follow** the skill format above
4. **Test** your examples work as expected
5. **Submit** a pull request with:
   - Clear title describing the skill
   - Explanation of why this skill is useful
   - Any relevant context for reviewers

## Directory Naming

Use lowercase with hyphens for directory names:

```
✅ skills/web-development/api-design
✅ languages/python/fastapi/best-practices
❌ skills/Web Development/APIdesign
```

## Review Criteria

Pull requests are reviewed based on:

- [ ] Follows the skill format
- [ ] Clear, actionable description
- [ ] Includes practical examples
- [ ] No spelling/grammar errors
- [ ] No copyrighted material without permission
- [ ] Appropriate category placement

## Questions?

Open an issue for discussion before starting major contributions.
