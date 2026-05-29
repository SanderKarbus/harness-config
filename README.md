# AI Engineering Ruleset

This project uses a shared AI specification (`claude.md` / `.cursorrules` / `AGENTS.md`) to keep AI-generated code consistent, maintainable, and production-safe.

The ruleset helps:

* reduce hallucinated implementations
* prevent destructive refactors
* keep architecture consistent
* improve multi-agent collaboration
* avoid unnecessary complexity

## Core Principles

* Follow existing project patterns and structure
* Prefer simple and predictable solutions
* Avoid unnecessary abstractions and dependencies
* Do not rewrite unrelated working code
* Make minimal, isolated changes whenever possible
* Validate external input and maintain type safety
* Never assume infrastructure that does not exist in the repository

## Changelog

Meaningful AI-generated changes should be logged in:

```txt id="pdj3b6"
ai_changelog.md
```

This acts as a historical record for maintainers and future AI agents.
