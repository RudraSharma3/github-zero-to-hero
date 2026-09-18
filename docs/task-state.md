# Active Task State

This document captures the current work in progress, goals, acceptance criteria, and task handoff notes.

---

## 1. Goal

Establish the architectural and governance foundation for **GitHub Mastery for Developers (`github-zero-to-hero`)**, implement **Level 0 (Foundations & GitHub Mental Model)**, root `README.md` curriculum map, dual-audience journey, and hands-on Break/Fix lab engine.

---

## 2. Acceptance Criteria

- [x] Analyze existing repository baseline, structure, and AI-OS orchestration integration.
- [x] Update `AGENTS.md` with dual-audience scope (Learner & Maintainer) and development commands.
- [x] Update `docs/architecture.md` with durable system architecture, component breakdown, and security boundaries.
- [x] Update `docs/conventions.md` with 19-part lesson structure, 3 depth levels, and 9-part lab framework.
- [x] Update `docs/graph.md` with curriculum dependency graph, blast radius matrix, and pedagogical flow.
- [x] Author root `README.md` in UTF-8 with 16-level visual progression tracker, curriculum map, prerequisites, and learner journey.
- [x] Author `CONTRIBUTING.md` with curriculum authoring standards and PR workflows.
- [x] Record ADR 001 in `docs/decisions/001-curriculum-progression-and-pedagogy.md`.
- [x] Author Level 0 syllabus (`00-foundations/README.md`).
- [x] Author Lesson 01 (`00-foundations/01-git-vs-github-mental-model.md`) with 19-part specification.
- [x] Author Lesson 02 (`00-foundations/02-developer-environment-ssh-auth.md`) with 19-part specification.
- [x] Author Hands-on Lab 0 (`labs/00-environment-and-auth-lab.md`) with 9-part framework and deliberate Break/Fix failure injection.
- [x] Author Level 0 Challenges (`challenges/00-foundations-challenges.md`) with 5-tier difficulty system.
- [x] Author Level 0 Assessment (`assessments/00-foundations-assessment.md`) with diagnostics, scenarios, and mastery rubric.

---

## 3. Implementation Plan

- [x] **Phase 1: Codebase Analysis & Architecture Mapping** — Synchronize `AGENTS.md`, `docs/architecture.md`, `docs/conventions.md`, `docs/graph.md`.
- [x] **Phase 2: Root Platform Architecture & Governance** — Author root `README.md`, `CONTRIBUTING.md`, ADR 001.
- [x] **Phase 3: Level 0 Foundation Module** — Implement `00-foundations/` syllabus, Lesson 01, Lesson 02, Lab 0 Break/Fix, Challenges (Tiers 1–5), and Assessment.
- [x] **Phase 4: Level 1 — Git Fundamentals (Plumbing & Porcelain)** — Three-tree architecture, DAG graph, `hash-object`, `cat-file`, commit objects, tree objects, Lab 1 Break/Fix, Challenges (Tiers 1–5), Assessment.
- [x] **Phase 5: Level 2 — GitHub Repository Mastery & Hygiene** — Repository architecture, `.gitignore` mechanics, `.gitattributes`, repository templates, issue/PR templates, licensing, CODEOWNERS, Lab 2 Break/Fix, Challenges (Tiers 1–5), Assessment.
- [x] **Phase 6: Level 3 — Branching & Professional Git Workflows** — Trunk-Based vs GitHub Flow, Fast-Forward vs Merge Commits vs Squash vs Rebase, `git rebase -i`, 3-way `zdiff3` conflict surgery, `git rerere`, Lab 3 Break/Fix, Challenges (Tiers 1–5), Assessment.
- [x] **Phase 7: Level 4 — Issues, Projects & Collaboration** — GitHub Projects v2, Custom Fields, Automation Workflows, Milestones, Roadmaps, Issue Triage, Lab 4 Break/Fix, Challenges (Tiers 1–5), Assessment.
- [ ] **Phase 8: Level 5 — Pull Requests & Code Review** — PR Lifecycle, Reviewer Workflows, Suggested Changes, Merge Queue, CODEOWNERS integration, Lab 5.
- [ ] **Phase 9: Verification & Meta-Infrastructure** — Configure `.github/` workflows for markdown linting, link verification, and pre-commit checks.

---

## 4. Current Status & Decisions

- **Levels 0, 1, 2, 3 & 4 Completed**: Foundations, object database, plumbing mechanics, three-tree architecture, repository architecture, `.gitignore` mechanics, `.gitattributes` normalization, YAML issue forms, branching strategies, merge DAGs, interactive rebasing, `zdiff3` conflict surgery, `git rerere`, GitHub Issues, tasklists, sub-issues, label taxonomy, sprint milestones, Projects v2, custom fields, and automated workflows are fully authored and tested.
- **Next Milestone**: Level 5 (Pull Requests & Code Review).

---

## 5. Handoff Notes

- **Completed**: `04-issues-projects/` (Syllabus, Lesson 1, Lesson 2, Lesson 3, Lesson 4), `labs/04-issues-projects-and-automation-lab.md`, `challenges/04-issues-projects-challenges.md`, `assessments/04-issues-projects-assessment.md`.
- **Next Action**: Review Level 4 materials or proceed to authoring Level 5: Pull Requests & Code Review.


