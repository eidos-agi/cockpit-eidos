---
id: TASK-50
title: 'Research: Helios Browser — Chromium fork with native agent control, roles & ABAC'
status: To Do
assignee: []
created_date: '2026-03-07 12:00'
labels:
  - research
  - architecture
  - helios
  - browser
  - eidos-core
dependencies: []
priority: high
---

## Description

<!-- SECTION:DESCRIPTION:BEGIN -->
From phone call with Vybhav (2026-03-07). Perplexity Comet gets a bunch of things right — AI-native browser with built-in agent capabilities, integrated search+browse — but we think we can get more right.

**Vision:** Build a Chromium-based browser (working name: Helios) that natively integrates AI agent control with security-first architecture. Takes the best of:

- **Chromium** — real browser, not headless, full web platform
- **Helios MCP** (eidos-agi/helios) — session isolation ("orbits"), audit logging, multi-session multiplexing, hub daemon
- **Fort AI security model** — ABAC permissions, roles, profiles, defense-in-depth
- **agent-browser (Vercel npm)** — fast CLI automation, session persistence, element snapshots

**What we'd do better than Comet:**
- **Roles & profiles** — different agent connections get different permission sets (not all-or-nothing)
- **ABAC** — per-agent, per-session permissions for domains, actions, and data each connection can touch
- **Audit lineage** — every agent action traced, logged, provable (Fort AI ch9-10 jobid_guid pattern)
- **Multi-agent isolation** — orbits baked into browser-level tab groups
- **Security-by-design** — browser enforces permissions at engine level, not via extension (can't be bypassed)
- **Eidos AGI integration** — controlled from agent mesh, not just one chat session

**Existing assets:**
- `eidos-agi/helios` — MCP server + hub daemon + Chrome extension (last commit 2026-03-01)
- DB schema for roles, devices, scopes already exists (not yet enforced)
- Personas config planned (TASK-7 in helios backlog)
- agent-browser v0.16.3 already used in slack tools + sable-cockpit
- Fort AI book chapters 1-2 written (security architecture foundation)

**GitHub issue:** https://github.com/eidos-agi/helios/issues/2
<!-- SECTION:DESCRIPTION:END -->

## Acceptance Criteria

<!-- SECTION:ACCEPTANCE:BEGIN -->
- [ ] Architecture decision: Chromium fork vs Electron vs CEF
- [ ] Draft ABAC permission model for agent connections
- [ ] Map existing Helios hub capabilities to browser-native equivalents
- [ ] Competitive analysis: Comet, Arc, agent-browser CLI
- [ ] Decision on whether this is a Fort AI chapter or separate book
- [ ] Written brief for Vybhav review
<!-- SECTION:ACCEPTANCE:END -->

## Research Questions

<!-- SECTION:NOTES:BEGIN -->
1. **Fork Chromium or build on Electron?** Comet is Electron-based. Full Chromium fork gives engine-level control but is much harder.
2. **ABAC model** — what attributes? Domain allowlists, action types (read/click/type/download), data sensitivity?
3. **Profile system** — how do agent profiles differ from user profiles? Can an agent inherit a user's session with restricted permissions?
4. **Helios hub** — does the daemon become the browser's built-in agent router, or stay external?
5. **agent-browser CLI** — integrate as a mode, or replace entirely?
6. **Book** — Fort AI chapter, or "The Helios Browser" as its own publication?
<!-- SECTION:NOTES:END -->
