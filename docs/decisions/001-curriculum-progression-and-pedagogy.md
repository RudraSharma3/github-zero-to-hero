# ADR 001: 16-Level Curriculum Progression & Break/Fix Pedagogy

## Status
Accepted

## Context
Standard Git and GitHub tutorials suffer from two major flaws:
1. **Passive Reading / Toy Examples**: Users execute copy-paste commands without encountering real production failure states (e.g. merge conflicts, detached HEAD, leaked credentials, broken CI actions). When failures occur in production, developers lack diagnostic muscle memory.
2. **Platform vs. VCS Confusion**: Tutorials frequently blur the boundary between Git as a local, content-addressable version control system and GitHub as an enterprise developer platform encompassing security, automation, governance, and CI/CD.

## Decision
1. **16-Level Modular Taxonomy**: Structure the curriculum into Levels 0 through 15, progressing strictly from fundamental mental models (`00-foundations`) through enterprise engineering capstones (`15-capstone`).
2. **Break/Fix Pedagogical Loop**: Every major level requires a mandatory deliberate failure scenario (`LEARN → DO → BREAK → FIX → EXPLAIN → APPLY`).
3. **Dual-Audience Architecture**: Design the repository simultaneously for **Learners** (interactive developer training) and **Maintainers** (automated drift detection, versioning, living repository CI).
4. **Standardized Lesson & Lab Schemas**: Enforce the 19-part lesson structure and 9-part lab framework across all curriculum modules.

## Consequences
### Positive
- High practical retention and diagnostic capability for learners.
- Modular, decoupled levels facilitate easy updates as GitHub features evolve.
- Clear structural governance for contributors and maintainers.

### Negative / Trade-offs
- Authoring effort per module is higher due to strict 19-part lesson and 9-part lab compliance.
- Requires continuous validation to ensure mock failure scenarios remain reproducible across multiple OS environments.
