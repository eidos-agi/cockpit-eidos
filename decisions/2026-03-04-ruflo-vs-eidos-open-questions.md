# Open Questions: Ruflo vs Eidos — Multi-Agent Strategy

**Date:** 2026-03-04
**Pilot:** Vybhav (VLR)
**Status:** Open — Awaiting Daniel's Review
**Trigger:** Review of https://github.com/ruvnet/ruflo

---

## Context

Ruflo is an enterprise AI orchestration platform that deploys **60+ specialized agents** in coordinated swarms with self-learning capabilities. It represents an alternative approach to building powerful AI systems.

This document captures open questions for Daniel about whether Eidos should incorporate Ruflo-inspired architecture, or stay focused on the current "single capable agent" vision.

---

## Ruflo: Key Capabilities Summary

| Area | What Ruflo Has |
|------|----------------|
| **Agents** | 60+ specialized agents (coding, testing, security, DevOps, etc.) |
| **Coordination** | Hierarchical (Queen), Mesh, Ring, Star patterns |
| **Communication** | Claims system, mailbox messaging, shared memory namespace |
| **Memory** | 3-scope (project/local/user), 8 types, HNSW vector search |
| **Learning** | SONA self-learning, EWC++ anti-forgetting, 9 RL algorithms |
| **Performance** | WASM kernels (352x faster), 30-50% token optimization |
| **Fault Tolerance** | Byzantine consensus, Raft, CRDTs, auto-recovery |
| **Multi-LLM** | Claude, GPT, Gemini, Ollama with failover |

---

## Eidos: Current State

### Three-Pillar Architecture
| Pillar | What It Is | Status |
|--------|-----------|--------|
| **Eidos Core** (brain) | PEFM memory, pods, orchestrator, memory compression | `eidos-v5` — in development |
| **Hancock** (auth) | Delegation certificates, approval dashboard, 2FA relay | `hancock` — Phase 1 complete |
| **Helios** (body) | Browser automation, identity-aware web control | `helios` — active |

### Current Capabilities
21 capabilities across 7 categories (screen, terminal, HTTP, communication, data, filesystem, financial)

---

## Critical Gaps: What Eidos Is Missing

| Area | Ruflo Has | Eidos Status | Impact |
|------|-----------|--------------|--------|
| **Multi-agent coordination** | 60+ specialized agents, swarm patterns | Single agent (Eidos) | Cannot parallelize complex tasks |
| **Agent memory system** | 3-scope memory, 8 types, vector search | PEFM (specs unclear) | Limited cross-session learning |
| **Reinforcement learning** | 9 RL algorithms, self-improving | None defined | Cannot learn from experience |
| **Fault tolerance** | Byzantine consensus, Raft, CRDTs | None | Single point of failure |
| **Performance optimization** | WASM kernels, 352x boost, token optimization | None | Higher latency, cost |
| **Multi-LLM support** | Claude, GPT, Gemini, Ollama failover | Claude-only | Vendor lock-in, no fallback |
| **Agent specialization** | 60+ pre-built agents | 1 general agent | Must build from scratch |

---

## Open Questions for Daniel

### Q1: Multi-Agent vs Single Agent Strategy

**Ruflo approach:** 60+ specialized agents in coordinated swarms

**Eidos current:** Single generalist agent (Eidos) with capability-based permissions

**Question:** Should Eidos evolve toward a multi-agent swarm, or stay focused on being a single capable agent?

**Options:**
| Option | Pros | Cons |
|--------|------|------|
| **Multi-agent swarm** | Parallelize complex tasks, specialize per domain, Ruflo-proven architecture | More complex to build and operate, harder to reason about |
| **Single agent + capabilities** | Simpler, clearer identity, easier to debug | Limited parallelization, single point of failure |
| **Hybrid** | Start single, add specialists as natural capability extensions | Architectural complexity grows over time |

**Daniel's input needed:** Which path aligns with the Eidos vision?

---

### Q2: Memory Architecture

**Ruflo approach:** 3-scope memory (project/local/user), 8 memory types (episodic, semantic, procedural, social, etc.), HNSW vector search with sub-ms retrieval

**Eidos current:** PEFM memory (details unclear), no vector search specified

**Question:** What should Eidos's memory architecture look like?

**Sub-questions:**
- Should Eidos adopt the 3-scope model (project/local/user)?
- What memory types are needed for Eidos's use cases?
- Is vector search a requirement, or can we start with simpler retrieval?
- How does PEFM fit into a multi-scope architecture?

**Daniel's input needed:** What are the memory requirements for Eidos to be effective?

---

### Q3: Learning and Self-Improvement

**Ruflo approach:** SONA self-learning (<0.05ms adaptation), EWC++ catastrophic forgetting prevention, 9 RL algorithms (PPO, A2C, DQN, Q-Learning, etc.)

**Eidos current:** No learning system defined

**Question:** Should Eidos have self-learning capabilities, or remain rule-based?

**Sub-questions:**
- Is self-learning required for Eidos to be a "full agent" vs "bot"?
- What should Eidos learn from? User feedback? Task outcomes? Execution traces?
- How do we prevent catastrophic forgetting as Eidos learns?
- Do we need reinforcement learning, or is supervised learning sufficient?

**Daniel's input needed:** Is learning a core requirement, or a future enhancement?

---

### Q4: Fault Tolerance and Reliability

**Ruflo approach:** Byzantine fault tolerance (f < n/3), Raft consensus for strong consistency, CRDTs for eventual consistency, automatic error recovery and task retry

**Eidos current:** No fault tolerance defined

**Question:** What level of fault tolerance does Eidos need for production?

**Sub-questions:**
- Is Eidos allowed to fail catastrophically, or must it degrade gracefully?
- Do we need consensus algorithms if we're single-agent?
- What error recovery mechanisms are needed?
- Should tasks be retried automatically, or require human approval?

**Daniel's input needed:** What's the reliability bar for Eidos in production?

---

### Q5: Performance Optimization

**Ruflo approach:** WASM kernels (Rust-based policy engine), Agent Booster (352x faster for simple transforms), 30-50% token reduction, Int8 quantization (~4x memory reduction), LoRA/MicroLoRA for lightweight adaptation

**Eidos current:** No performance optimization defined

**Question:** How important is performance optimization to Eidos's success?

**Sub-questions:**
- Is latency a critical concern for Eidos's use cases?
- Should we optimize for token cost, response speed, or both?
- Is WASM/Rust justified, or can we stay in TypeScript/Node?
- Do we need quantization, or is model selection more important?

**Daniel's input needed:** What are the performance requirements for Eidos?

---

### Q6: Multi-LLM Support

**Ruflo approach:** Claude, GPT, Gemini, Ollama with automatic failover and cost optimization

**Eidos current:** Claude-only (via Anthropic API)

**Question:** Should Eidos support multiple LLM providers, or stay Claude-focused?

**Sub-questions:**
- Is vendor lock-in a concern for Eidos?
- Do we need fallback providers for reliability?
- Should Eidos choose the best model per task, or use one consistently?
- Does multi-LLM support complicate the Hancock auth story?

**Daniel's input needed:** Is Claude-only acceptable, or do we need provider flexibility?

---

### Q7: Capability Taxonomy Expansion

**Ruflo approach:** 60+ specialized agents, each with domain-specific capabilities

**Eidos current:** 21 capabilities across 7 categories

**Question:** Should Eidos expand its capability taxonomy, or is 21 sufficient?

**Sub-questions:**
- Are there missing capabilities that Eidos needs to be effective?
- Should capabilities be organized differently?
- Do we need domain-specific capability groups (e.g., research, testing, security)?
- How do capabilities map to specialist agents in a multi-agent future?

**Daniel's input needed:** What capabilities is Eidos missing?

---

## Strategic Recommendations (Preliminary)

### Short Term (1-3 months)

1. **Complete Hancock** — Auth foundation is prerequisite for multi-agent delegation
2. **Clarify PEFM specs** — Define memory architecture before building more
3. **Add simple fault tolerance** — Task retry, graceful degradation
4. **Document capability gaps** — Identify what's missing from the 21 capabilities

### Medium Term (3-6 months)

1. **Prototype 2-3 specialist agents** — Coder, tester, researcher (if multi-agent path chosen)
2. **Add vector search** — For memory retrieval (if needed)
3. **Implement multi-LLM support** — Start with GPT fallback (if needed)
4. **Performance optimization** — Token compression, caching (if needed)

### Long Term (6-12 months)

1. **Reinforcement learning** — Self-improving agents (if learning is a requirement)
2. **Full fault tolerance** — Byzantine consensus, Raft (if reliability is critical)
3. **WASM performance layer** — Rust kernels for hot paths (if latency is critical)
4. **Expand to 30+ capabilities** — Cover more domains (as needed)

---

## Architectural Proposals (For Discussion)

### Proposal A: Single Agent + Capabilities (Current Path)

```
Eidos (single agent)
  └── Hancock (auth/delegation)
      └── 21 capabilities across 7 categories
```

**Trade-off:** Simple, clear, but limited parallelization.

### Proposal B: Multi-Agent Swarm (Ruflo-inspired)

```
Orchestrator (Queen agent)
  ├── Coder agent
  ├── Tester agent
  ├── Researcher agent
  ├── Security agent
  └── ... (expand to 20+ agents)
  └── Hancock (auth/delegation for all)
```

**Trade-off:** Powerful, parallel, but complex.

### Proposal C: Hybrid (Evolutionary)

```
Phase 1: Eidos (single agent) + 21 capabilities
Phase 2: Add specialist agents for high-frequency tasks
Phase 3: Evolve into swarm as natural extension of capability system
```

**Trade-off:** Best of both worlds, but architectural evolution required.

---

## Next Steps

1. **Daniel reviews this document** — Provides direction on open questions
2. **Decision record** — Capture architectural choices
3. **Update roadmap** — Reflect strategic direction
4. **Implement** — Build according to chosen path

---

## Appendix: Ruflo Resources

- Repository: https://github.com/ruvnet/ruflo
- Author: RuvNet
- License: MIT
- Tech Stack: Rust (WASM kernels), PostgreSQL, TypeScript/JavaScript
- Entry Point: `claude-flow v3.5.2`

---

**Waiting on:** Daniel's review and strategic direction.
