MASTER SPECIFICATION & QUALITY HARNESS

## Universal AI Engineering Ruleset (`claude.md` / `.cursorrules` / `AGENTS.md`)

This specification defines mandatory engineering, architectural, and behavioral rules for all AI coding agents operating within the project.

The goal is:

* deterministic multi-agent collaboration
* architectural consistency
* minimized hallucination/refactoring drift
* scalable long-term maintenance
* production-safe code generation

If project-specific conventions conflict with generic best practices, prioritize consistency with the existing codebase unless explicitly instructed otherwise.

---

# 0. PROJECT CONTEXT & STACK AUTODETECTION

Before generating, modifying, or refactoring code, perform a full repository context scan.

## Repository Detection

Detect project structure using:

* `pnpm-workspace.yaml`
* `turbo.json`
* `lerna.json`
* root `package.json`
* workspace definitions

Determine whether the project is:

* monorepo
* split frontend/backend
* standalone API
* standalone frontend
* microservice architecture
* hybrid application

---

## Frontend Stack Detection

Inspect:

* `package.json`
* framework config files
* build tooling

Detect:

* Next.js
* Nuxt
* Remix
* React
* Vue
* Svelte
* Angular

Also detect:

* TailwindCSS
* shadcn/ui
* Radix UI
* Material UI
* Zustand
* Redux
* TanStack Query
* Vite
* Webpack

---

## Backend Stack Detection

Inspect:

* `requirements.txt`
* `pyproject.toml`
* `go.mod`
* `Cargo.toml`
* `pom.xml`
* `build.gradle`
* `package.json`

Detect:

* Express
* NestJS
* FastAPI
* Django
* Spring Boot
* Go services
* Rust services

---

## Database & ORM Detection

Inspect:

* `schema.prisma`
* `drizzle.config.*`
* `typeorm`
* `sequelize`
* `alembic.ini`
* migration directories
* docker compose files
* environment variables

Detect:

* PostgreSQL
* MySQL
* MongoDB
* Redis
* SQLite

Detect ORM/query tooling:

* Prisma
* Drizzle
* TypeORM
* Sequelize
* SQLAlchemy

---

## Existing Pattern Priority

Always prioritize:

* existing architecture
* existing naming conventions
* existing folder structure
* existing dependency choices
* existing coding style

Consistency is more important than theoretical perfection.

Do not introduce new architectural patterns unless necessary.

---

# 1. MULTI-AGENT SYNCHRONIZATION RULES

## Context Anchoring

Read relevant:

* schemas
* types
* services
* interfaces
* configs
* related modules

before writing code.

Do not generate isolated assumptions.

---

## Zero Guesswork

If ambiguity affects:

* business logic
* security
* infrastructure
* database structure
* API contracts
* architectural direction

ask the user before proceeding.

Otherwise:

* follow existing project conventions
* infer behavior from surrounding code

---

## Deterministic Generation

Generated code must be:

* complete
* executable
* production-safe
* internally consistent

Forbidden:

* TODO placeholders
* pseudo-code
* incomplete implementations
* fake mock logic unless requested

---

## Interoperability

Write explicit, standard, portable code.

Avoid:

* framework magic
* hidden metaprogramming
* overly clever abstractions
* tool-specific lock-in patterns

---

## No Phantom Refactoring

Do not refactor unrelated working code.

Avoid:

* opportunistic rewrites
* style-only rewrites
* architectural reshuffling
* unnecessary renaming

unless explicitly requested.

---

## Incremental Modification Strategy

Prefer:

* minimal diffs
* isolated changes
* targeted fixes

over large rewrites.

Preserve existing behavior unless instructed otherwise.

---

# 2. ABSTRACTION & DRY STRATEGY

## Core Logic Centralization

Business-critical logic must exist in one authoritative location.

Examples:

* authorization rules
* pricing logic
* validation rules
* database schemas

---

## AHA Principle

Avoid Hasty Abstractions.

Only abstract when:

* logic repeats multiple times
* duplication creates real maintenance burden
* abstraction improves clarity

---

## Context Preservation

If abstraction reduces readability or AI comprehension:
prefer explicit duplication.

AI maintainability is part of maintainability.

---

## Module Isolation

Modules must minimize runtime coupling.

Changing module A should not silently break module B.

Use:

* DTOs
* interfaces
* explicit contracts

instead of hidden assumptions.

---

# 3. ARCHITECTURAL BOUNDARIES

## File Size

Prefer files under 300 lines.

Files exceeding 500 lines require architectural justification.

---

## Function Size

Functions should remain focused and readable.

Prefer:

* early returns
* guard clauses
* SRP

Avoid excessive extraction that harms readability.

---

## Nesting

Avoid deep nesting.

Prefer:

* guard clauses
* early exits
* extracted condition helpers

Maximum recommended nesting depth: 3 levels.

---

## Pure Logic Separation

Separate:

* business logic
* side effects
* I/O
* database access
* time-dependent behavior

whenever practical.

---

## Immutability

Prefer:

* `const`
* readonly data
* immutable transformations

Avoid mutating function inputs.

---

## Dependency Direction

High-level modules must not depend directly on low-level implementation details.

Use:

* dependency injection
* interfaces
* adapters

where appropriate.

---

# 4. TYPE SAFETY & VALIDATION

## Strict Typing

Prefer strict typing everywhere practical.

Avoid:

* `any`
* weak generic objects
* implicit typing

Use `unknown` at trust boundaries when validation is required.

---

## Explicit Contracts

All public functions, APIs, and services should define:

* input types
* output types
* failure behavior

explicitly.

---

## Boundary Validation

Validate all external input:

* API payloads
* forms
* webhooks
* query params
* environment variables

at entry boundaries.

---

## Schema Enforcement

Prefer schema validation libraries:

* Zod
* Pydantic
* Marshmallow
* Joi
* Valibot

where applicable.

---

## Null Safety

Handle:

* null
* undefined
* optional values

explicitly and safely.

---

# 5. ERROR HANDLING & OBSERVABILITY

## No Silent Failures

Empty `catch`/`except` blocks are forbidden.

All failures must:

* log context
* return explicit failure states
* or fail safely

---

## Targeted Exception Handling

Catch specific exceptions whenever possible.

Avoid broad global exception swallowing.

---

## Predictable Failure States

Unexpected failures must:

* fail safely
* preserve system integrity
* avoid hidden corruption

---

## Structured Logging

Prefer structured logs with:

* traceId
* requestId
* userId
* operation context

Use machine-readable formats where possible.

---

## Sensitive Data Protection

Never log:

* passwords
* tokens
* secrets
* personal identification data
* raw credentials

---

# 6. SECURITY & PRODUCTION SAFETY

## Injection Prevention

Use:

* parameterized queries
* ORM protections
* prepared statements

Never build raw SQL via string concatenation.

---

## XSS & Input Sanitization

Sanitize and escape user-generated content before rendering.

---

## Secrets Management

Never hardcode:

* API keys
* secrets
* credentials
* salts

Use environment configuration.

---

## Least Privilege

Grant only minimum required permissions.

---

## Dependency Discipline

Do not add dependencies unnecessarily.

Prefer:

* existing project utilities
* standard library solutions

before introducing new packages.

New dependencies must provide clear value.

---

# 7. TESTABILITY & MAINTAINABILITY

## Testable Architecture

Prefer decoupled logic enabling:

* unit tests
* mocks
* isolated validation

---

## Test Coverage Expectations

New logic should include:

1. happy path coverage
2. failure path coverage
3. edge case coverage

where practical.

---

## Regression Protection

Bug fixes should include tests reproducing the original issue whenever feasible.

---

## Intent-Driven Comments

Comments should explain:

* business reasoning
* architectural intent
* constraints

Do not explain obvious syntax.

---

## Self-Documenting Naming

Use explicit, descriptive naming.

Avoid vague identifiers like:

* data
* temp
* value
* handler2

---

# 8. PERFORMANCE AWARENESS

Avoid:

* unnecessary re-renders
* N+1 queries
* redundant allocations
* blocking I/O
* duplicated computation
* unnecessary API calls

Prefer scalable solutions over convenience abstractions.
Optimize only where complexity is justified.

---

# 9. CI/CD & VERSION CONTROL

## Lint & Formatting Compliance

Generated code must comply with:

* lint rules
* formatter rules
* type checks
* build validation

without requiring manual cleanup.

---

## Atomic Changes

Keep changes:

* isolated
* minimal
* logically scoped

One concern per commit whenever possible.

---

## Conventional Commits

Prefer commit messages like:

* `feat(auth): add MFA validation`
* `fix(api): prevent null session crash`
* `refactor(user): simplify token parsing`

---

## Backward Compatibility

Prefer non-breaking:

* migrations
* API changes
* schema evolution

Support rolling deployments where applicable.

---

# 10. LANGUAGE & COMMUNICATION RULES

## Codebase Language

All:

* code
* comments
* commits
* branch names
* documentation

must be written in English.

---

## User Communication

All interaction with the user must be in Estonian unless requested otherwise.

---

# 11. AI CHANGELOG PROTOCOL

## Changelog File

Maintain `ai_changelog.md` in the project root.

Create it if missing.

---

## Logging Requirement

Append entries after meaningful:

* features
* fixes
* refactors
* architectural changes

Minor formatting-only edits may be omitted.

---

## Required Entry Format

```markdown
### [YYYY-MM-DD HH:MM] - Change Title

* **Harness Used:** Cursor / Claude / GPT / Copilot
* **Files Modified:** `src/example.ts`

* **What Was Done:**
  * Added...
  * Refactored...
  * Fixed...

* **Why It Was Done:**
  Business or architectural justification.

* **Impact / Breaking Changes:**
  None
```

---

## Historical Integrity

`ai_changelog.md` is append-only.

Never:

* delete entries
* rewrite history
* compress old records

---

# 12. MULTI-STACK BOUNDARY ENFORCEMENT

Frontend and backend concerns must remain isolated.

Do not:

* leak backend architecture into frontend code
* place database access logic inside UI layers
* mix transport DTOs with database entities
* expose internal backend models directly to frontend consumers
* tightly couple frontend rendering logic to backend implementation details

Respect clear separation of concerns across application layers.

---

# 13. ARCHITECTURE ESCALATION RULE

Do not introduce:

* new architectural layers
* CQRS
* repository patterns
* event systems
* state managers
* caching layers
* service abstraction layers
* plugin systems
* microservice splits

unless:

* the project already uses them
* or the user explicitly requests them.

Prefer extending existing architecture over redesigning it.

---

# 14. PREFER BORING SOLUTIONS

Prefer:

* simple
* explicit
* maintainable
* predictable

solutions over clever abstractions or highly engineered implementations.

Readability and long-term maintainability are more important than engineering novelty.

Avoid abstraction layers that reduce clarity without solving a measurable problem.

---

# 15. INFRASTRUCTURE ASSUMPTION RESTRICTION

Do not assume the existence of:

* Redis
* Kafka
* RabbitMQ
* SQS
* background workers
* cron systems
* websocket infrastructure
* cloud services
* object storage
* CDN integrations
* environment variables
* third-party APIs

unless:

* explicitly present in the repository
* referenced in configuration
* or requested by the user.

Never invent infrastructure dependencies.

---

# 16. RUNTIME & DEPENDENCY AWARENESS

Before introducing:

* libraries
* frameworks
* middleware
* SDKs
* runtime services

verify whether:

* equivalent functionality already exists
* the standard library is sufficient
* the dependency aligns with the current stack

Avoid dependency sprawl.

---

# 17. AI MAINTAINABILITY RULE

Generated code must remain understandable for:

* future AI agents
* future maintainers
* partial-context editing sessions

Avoid:

* excessive indirection
* deeply nested abstractions
* dynamic metaprogramming
* hidden execution flow

Prioritize explicit execution paths and readable control flow.

---

# 18. REFACTOR SAFETY RULE

Before refactoring existing code:

1. verify the current behavior
2. identify dependent modules
3. preserve API compatibility where possible
4. avoid changing unrelated logic
5. prefer incremental restructuring over rewrites

Working code has priority over theoretical improvements.

---

# 19. STATE & SIDE-EFFECT AWARENESS

Avoid hidden state mutations.

Make:

* side effects
* async operations
* database writes
* cache writes
* external requests

explicit and traceable.

Avoid implicit shared mutable state whenever practical.

---

# 20. PRODUCTION STABILITY PRIORITY

When tradeoffs exist between:

* elegance
* abstraction
* performance
* delivery speed
* stability

prioritize:

1. correctness
2. stability
3. maintainability
4. consistency
5. optimization

Do not prematurely optimize at the expense of readability or reliability.

---

# 21. AI EXECUTION BOUNDARY

AI agents must not:

* fabricate completed integrations
* invent API responses
* assume infrastructure behavior
* create fake production data
* simulate unverified architecture

If uncertainty materially affects correctness:

* inspect the repository
* or ask the user.

Never present assumptions as confirmed implementation details.


see on vaja teha failiks ja pushida sellesse samasse reposse =) folder nimega agent.md
