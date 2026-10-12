# AGENTS.md

## Project: Xmap-vibe

Interactive map of spots for extreme and action sports (BMX, skateboard, stunt scooter, snowboard, inline skates, etc.) with photo uploads and community ratings.

---

## Developer & Architectural Context

- **Developer Profile**: Senior Java developer. Do not explain basic Java, Spring, or SQL fundamentals.
- **Backend (Strict Engineering, TDD & Seam Testing)**:
  - Language & Framework: Java 21+, Spring Boot 3+, Spring Security.
  - Identity & Access Management: Keycloak (OAuth2 / OIDC, JWT Resource Server).
  - Persistence & Data: PostgreSQL with spatial capabilities (PostGIS if needed), Flyway or Liquibase migrations.
  - Object Storage: S3-compatible API (AWS S3 / Cloudflare R2 in prod, MinIO via Docker Compose locally).
  - Infrastructure: Docker, Docker Compose, CI/CD pipelines.
  - Testing Philosophy: Test-Driven Development (`tdd`). Test at the highest stable seam (`@SpringBootTest`, `MockMvc` or Service layer) with **Testcontainers** for PostgreSQL and MinIO. Never mock internal Spring services, repositories, or entities.
- **Frontend (Rapid Iteration / "Vibe Coding")**:
  - React + TypeScript (Vite), Tailwind CSS or UI kit of choice.
  - Interactive Map: Leaflet / MapLibre GL / Mapbox.
  - *Engineering Mode*: Vibe-coding is permitted and encouraged for the frontend UI components to achieve rapid visual iteration and slick UX without unnecessary testing ceremony on simple UI views.

---

## Agent Skills Configuration

### Issue tracker

Issues and specs for this repo live as local markdown files in `.scratch/`. See `docs/agents/issue-tracker.md`.

### Triage labels

Canonical triage roles: `needs-triage`, `needs-info`, `ready-for-agent`, `ready-for-human`, `wontfix`. See `docs/agents/triage-labels.md`.

### Domain docs

Single-context repository (`GLOSSARY.md` + `docs/adr/` at the repo root). See `docs/agents/domain.md`.

---

## Available Skills (in `.agents/skills/`)

Whenever relevant, activate and adhere to the procedures defined in `.agents/skills/<skill-name>/SKILL.md`:

1. **`grill-with-docs` / `grilling`**:
   - **When to use**: Before designing new features, models, or APIs.
   - **Procedure**: Conduct structured interview rounds. Map the *design tree*, identify the question *frontier*, propose concrete recommended answers, and wait for confirmation. Concurrently update `GLOSSARY.md` and propose ADRs in `docs/adr/`.

2. **`domain-modeling`**:
   - **When to use**: When discussing domain concepts (Spots, Disciplines, Media, Reviews).
   - **Procedure**: Enforce ubiquitous language from `GLOSSARY.md`. Challenge ambiguous terms. Record architectural decisions in `docs/adr/` sparingly (only if hard to reverse, surprising, and a real trade-off).

3. **`to-spec`**:
   - **When to use**: After an interview/design session to capture requirements.
   - **Procedure**: Synthesize context into `.scratch/<feature-slug>/spec.md` (Problem, Solution, User Stories, Implementation Decisions, Testing Decisions, Out of Scope).

4. **`to-tickets`**:
   - **When to use**: After a spec is approved.
   - **Procedure**: Break the spec down into atomic implementation tickets in `.scratch/<feature-slug>/issues/01-<slug>.md`.

5. **`tdd` & `implement-spec`**:
   - **When to use**: When writing or modifying backend code.
   - **Procedure**: Red -> Green -> Refactor. Write external behavioral tests at the seam before production code.

6. **`diagnosing-bugs`**:
   - **When to use**: When fixing bugs or failing tests.
   - **Procedure**: Never guess. Reproduce with an automated test first, trace execution, form a hypothesis, fix, and verify.

7. **`code-review`**:
   - **When to use**: Before finalizing changes or submitting a PR.
   - **Procedure**: Review against domain alignment, clean Spring architecture, seam testing, and performance.
