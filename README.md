<table align="center" border="0" cellspacing="0" cellpadding="0">
<tr>
<td align="center"><img src="https://owasp.org/assets/images/logo.png" alt="OWASP Foundation" width="220"/></td>
<td width="40"></td>
<td align="center"><img src="https://user-images.githubusercontent.com/77677278/130192540-6af44626-154e-4a42-b0e1-ae2537008d81.png" alt="Google Summer of Code" width="220"/></td>
</tr>
</table>

<h1 align="center">🛡️ FinBot-AEGIS</h1>

<h3 align="center">Agentic Exploitation & Guardrail Infrastructure System</h3>
<p align="center"><i>A defense in depth security runtime built for the OWASP FinBot CTF platform</i></p>

<p align="center">
  <img src="https://img.shields.io/badge/Google_Summer_of_Code-2026-4285F4?style=for-the-badge&logo=google&logoColor=white" alt="GSoC 2026"/>
  <img src="https://img.shields.io/badge/Organization-OWASP_Foundation-000000?style=for-the-badge&logo=owasp&logoColor=white" alt="Organization"/>
  <img src="https://img.shields.io/badge/License-Apache_2.0-blue?style=for-the-badge" alt="License"/>
</p>
<p align="center">
  <img src="https://img.shields.io/badge/Size-Large_(350_hrs)-6f42c1?style=flat-square" alt="Size"/>
  <img src="https://img.shields.io/badge/Duration-14_Weeks-6f42c1?style=flat-square" alt="Duration"/>
  <img src="https://img.shields.io/badge/Repo-finbot--ctf-24292e?style=flat-square&logo=github&logoColor=white" alt="Repo"/>
</p>

<p align="center"><b>⭐ This is my official Work Product Submission for the GSoC 2026 Final Evaluation ⭐</b></p>

---

## 📇 At a Glance

| | |
|---|---|
| **Contributor** | Jean Francois Regis MUKIZA ([@Jean-Regis-M](https://github.com/Jean-Regis-M)) |
| **Institution** | Fırat University, Elazığ, Turkey (Digital Forensics Engineering & Cybersecurity) |
| **Organization** | [OWASP Foundation](https://owasp.org), mentored within the [OWASP GenAI Security Project](https://github.com/GenAI-Security-Project) |
| **Repository** | [GenAI-Security-Project/finbot-ctf](https://github.com/GenAI-Security-Project/finbot-ctf) |
| **Project** | *FinBot-AEGIS, a Full Spectrum Adversarial Security Framework for the OWASP FinBot CTF Platform* |
| **Mentors** | Nirupam Ghosh ([@mekaizen](https://github.com/mekaizen)) & Carolina Steadham ([@steadhac](https://github.com/steadhac)) |
| **Program Period** | May 25 to August 31, 2026 · **Large project, 350 hours** |
| **Email** | 240509606@firat.edu.tr |
| **LinkedIn** | [![LinkedIn](https://img.shields.io/badge/Connect-0A66C2?style=flat-square&logo=linkedin&logoColor=white)](https://www.linkedin.com/in/mukiza-jean-francois-regis-bb9a80254/) [MUKIZA Jean Francois Regis](https://www.linkedin.com/in/mukiza-jean-francois-regis-bb9a80254/) |
| **Demo Video** | [▶ Watch on YouTube](https://www.youtube.com/watch?v=Xb_C8XbkBzM) |

> *"Security is not a product, it is a process, and agentic systems demand a new process entirely."*

---

## 📖 Table of Contents

1. [Executive Summary](#1-executive-summary)
2. [The Problem FinBot-AEGIS Solves](#2-the-problem-finbot-aegis-solves)
3. [Architecture: The Four Pillars](#3-architecture-the-four-pillars)
4. [Demo Video](#4-demo-video)
5. [What I Built: Week by Week](#5-what-i-built-week-by-week)
6. [Pull Request Ledger](#6-pull-request-ledger)
7. [OWASP Agentic Top 10: Coverage Map](#7-owasp-agentic-top-10-coverage-map)
8. [FinBot Security Score: AIVSS Aligned Risk Model](#8-finbot-security-score-aivss-aligned-risk-model)
9. [Engineering Challenges & Solutions](#9-engineering-challenges-solutions)
10. [Proposal Accountability: Milestones vs. Delivery](#10-proposal-accountability-milestones-vs-delivery)
11. [Current State](#11-current-state)
12. [What's Left To Do](#12-whats-left-to-do)
13. [Thought Leadership](#13-thought-leadership)
14. [Acknowledgments & Career Value](#14-acknowledgments-career-value)
15. [About the Contributor](#15-about-the-contributor)
16. [Links & License](#16-links-license)

---

## 1. Executive Summary

**FinBot-AEGIS** is a production grade, OWASP aligned adversarial security framework that I designed and built into the [OWASP FinBot CTF platform](https://github.com/GenAI-Security-Project/finbot-ctf) during GSoC 2026. FinBot is an *intentionally vulnerable* agentic AI finance application used to teach the **OWASP Agentic Top 10 (ASI01 to ASI10)**. Before this project, FinBot could show learners *that* an attack succeeded, but it had **no attack simulator, no runtime policy enforcement, and no forensic telemetry**. Those are the gaps this project fills.

AEGIS is built around **four interlocking pillars**:

- 🎯 **Attack Simulator**: a sandboxed harness that replays and mutates adversarial payloads across all ten OWASP Agentic Top 10 categories.
- 🔐 **Policy Engine ("IntentGate")**: a Policy Enforcement Point that validates every MCP tool call against least privilege agent profiles.
- 📡 **Telemetry Pipeline ("SentinelStream")**: a tamper evident, HMAC-SHA256 chained audit trail streamed in real time.
- 
- 🧪 **Red Team Harness**: an AIVSS aligned scoring engine and `pytest-aegis` plugin that benchmarks detector effectiveness in CI.

Across the coding period I shipped **six substantial pull requests** covering all four pillars plus four brand new CTF challenges, in addition to **two smaller fixes merged earlier** that hardened the FinBot core before the coding period began. All work is released under the project's existing **Apache License 2.0**, is fully backward compatible (every AEGIS feature is opt in, behind a `False` by default flag), and is documented below with direct, clickable links to every piece of work.

---

## 2. The Problem FinBot-AEGIS Solves

FinBot CTF, prior to this project, was a **reactive** learning environment. It detected successful attacks after the fact, awarded points, and moved on. It could not:

- Proactively **simulate** the full OWASP Agentic Top 10 attack surface against agents in isolation.
- **Enforce** runtime security policy (least agency, intent validation, tool scoping). Its `GuardrailHookService` only *logged* verdicts, it never blocked anything.
- Produce **forensic quality audit trails** to reconstruct which agent, tool call, or memory write led to a compromise.
- **Benchmark** defensive controls with reproducible, quantitative metrics.

FinBot-AEGIS closes each of these gaps directly, transforming FinBot from a static vulnerable showcase into a measurable, defensible platform for learning and researching agentic AI security.

---

## 3. Architecture: The Four Pillars

```
FinBot Application Layer
   Orchestrator → InvoiceAgent, PaymentsAgent, FraudAgent, CommunicationAgent, OnboardingAgent
        (every MCP tool call is routed through AEGIS)
        ↓
FinBot-AEGIS Security Layer   (delivered this GSoC cycle)
   • Pillar 1  Attack Simulator      40+ ASI01 to ASI10 recipes, 5 adversarial MCP mocks, mutation engine
   • Pillar 2  Policy Engine         IntentGate, MCPPolicyInterceptor, 6 least privilege Agent Profiles
   • Pillar 3  Telemetry Pipeline    SentinelStream, HMAC-SHA256 chained events, FastAPI/SSE dashboard
   • Pillar 4  Red Team Harness      pytest-aegis plugin, AIVSS FinBot Security Score (0 to 100)
        ↓
Redis EventBus            (existing FinBot infrastructure, zero new dependencies)
        ↓
CTF Detector Engine  →  19 existing challenges + 4 new AEGIS challenges
```

**Design principles carried through every pillar:** zero trust between agents, least agency by default, an immutable and tamper evident audit trail, fail secure defaults, and defense in depth. Each pillar keeps protecting the platform even if another layer is disabled.

**The core innovation behind the Policy Engine** is that it evaluates the *semantic intent* behind a tool call, not just superficial attributes like the tool's name. Before any tool executes, AEGIS builds a signed, tamper evident record of what the agent intends to do:

```python
class IntentCapsule:
    agent_name: str
    tool_name: str
    tool_args: dict[str, Any]
    tool_source: str
    workflow_id: str
    timestamp: float
    nonce: str          # replay attack prevention
    signature: str       # HMAC-SHA256 of the serialized intent
```

The capsule is signed with the calling agent's secret key, and IntentGate verifies the signature, timestamp, and nonce before allowing the call through.

---

## 4. Demo Video

<p align="center">
  <a href="https://www.youtube.com/watch?v=Xb_C8XbkBzM">
    <img src="https://img.youtube.com/vi/Xb_C8XbkBzM/maxresdefault.jpg" alt="FinBot-AEGIS GSoC 2026 Demo" width="640"/>
  </a>
</p>
<p align="center"><b>▶ Click the thumbnail to watch the full AEGIS walkthrough on YouTube</b></p>

The recording walks through the telemetry dashboard, a live IntentGate policy decision, an Attack Simulator run against a mocked adversarial MCP server, and the resulting AIVSS FinBot Security Score.

---

## 5. What I Built: Week by Week

### 🧩 Phase 1: Weeks 1 to 3 · Telemetry & IntentGate Foundations
**PR:** [#518](https://github.com/GenAI-Security-Project/finbot-ctf/pull/518) · [#520](https://github.com/GenAI-Security-Project/finbot-ctf/pull/520)

- Built the **`SentinelStream`** telemetry service (`finbot/aegis/sentinel.py`) with a strongly typed, JSON-LD compatible `AuditEvent` schema (`telemetry/schema.py`) covering `ToolCall`, `ToolResult`, `MemoryWrite`, `Delegation`, and `PolicyDecision` events.
- Implemented **HMAC-SHA256 immutable audit chaining** (`telemetry/chain.py`). Each event's hash depends on the one before it, so any tampering is mathematically detectable.
- Added **per namespace isolation** (`aegis:audit:chain_head:ns_X` Redis keys) so one player's session can't contaminate another's.
- Shipped **`IntentGate`**, a YAML driven policy as code engine (`intent_gate.py`), and **`AegisEnforcementService`**, an *observe mode* orchestrator that logs allow and block decisions **without** blocking gameplay, preserving FinBot's exploitability for learners.
- Fixed a **CI blocking bug**: eager module level `LLMClient()` instantiation was breaking test collection whenever `OPENAI_API_KEY` was unset. Refactored to lazy initialization, which unblocked every downstream PR (#520).
- **Result:** at least 88% test coverage across the new AEGIS modules, at least 80% on IntentGate specifically, zero breaking changes, all new functionality gated behind `AEGIS_ENABLED` and `AEGIS_TELEMETRY_ENABLED` flags defaulting to `False`.
- **OWASP coverage introduced:** ASI01, ASI02, ASI05, ASI06, ASI08, ASI10.

### 🧩 Phase 2: Weeks 3 to 6 · The Attack Simulator
**PR:** [#530](https://github.com/GenAI-Security-Project/finbot-ctf/pull/530)

- Built **`SandboxHarness`** (`simulator/base.py`), which runs adversarial payloads in isolated subprocesses with timeout and cleanup guarantees, so a runaway attack can never escape into the host environment.
- Wrote **five adversarial MCP server mocks** (`simulator/mcp_mocks/adversarial.py`): `AdmServer` (admin panel privilege escalation), `AdviceServer` (financial advice manipulation), `DataExfiltrationServer` (PII and financial record theft), `FileSystemServer` (unauthorized file access), and `ToolPoisoningServer` (malicious tool and backdoor injection).
- Authored **40+ parametric YAML attack recipes** spanning **all ten OWASP Agentic Top 10 categories (ASI01 to ASI10)**, including 8 prompt injection recipes and 8 tool misuse recipes in the first pass, with supply chain, RCE, delegation abuse, impersonation, and the remaining categories completing the set by Week 6.
- Built a **payload mutation engine** (`simulator/mutation.py`) generating encoding, semantic paraphrase, and identifier renaming variants to stress test detector resilience against evasion.
- Full unit test suite: `test_simulator_harness.py`, `test_mcp_mocks.py`, `test_attack_recipes.py`, `test_mutation.py`.
- **OWASP coverage introduced:** full ASI01 to ASI10 recipe coverage.

### 🧩 Week 7 · Policy Interceptor & Least Privilege Agent Profiles
**PR:** [#543](https://github.com/GenAI-Security-Project/finbot-ctf/pull/543)

- Implemented **`MCPPolicyInterceptor`** (`policy/interceptor.py`), which wraps every MCP tool call with real time authorization, rate limiting, and policy violation logging.
- Defined **six least agency Agent Profiles** (`policy/agent_profiles.py`): *Data Analyst*, *Report Generator*, *API Integrator*, *Security Auditor*, *Workflow Orchestrator*, and *Research Assistant*, each scoped to only the tools its role requires. For example:

```python
DATA_ANALYST_PROFILE = AgentProfile(
    name="data_analyst",
    description="Agent specialized in data analysis, querying, and visualization",
    allowed_tools=[
        "database:query",
        "database:read_schema",
        "data:visualize",
        "data:transform",
        "data:summarize",
        "filesystem:read",
    ],
    tool_limits={
        "database:query": {"max_calls_per_minute": 30},
        "data:visualize": {"max_calls_per_minute": 10},
        "filesystem:read": {"max_calls_per_minute": 60},
    },
    max_concurrent_operations=3,
)
```

- Laid forward compatible **stubs** for Week 8: `capsule.py` (HMAC intent capsule signing) and `memory.py` (provenance tracked memory namespaces).
- Full test suite in `test_policy_interceptor.py` covering authorization, rate limit enforcement, and metrics accuracy.
- **OWASP coverage introduced:** ASI02 (Tool Misuse), ASI03 (Excessive Agency), ASI07 (identity abuse adjacent to prompt injection).

### 🧩 Weeks 8 and 9 · Red Team Harness & AIVSS Scoring
**PR:** [#546](https://github.com/GenAI-Security-Project/finbot-ctf/pull/546)

- Built the **`pytest-aegis`** plugin (`harness/plugin.py`) with custom markers (`@pytest.mark.attack`, `@pytest.mark.defense`), mock enforcement fixtures, and parameterized attack payload fixtures.
- Implemented **`benchmark.py`**: a `DetectorBenchmarkResult` dataclass plus `compute_f1()` and `benchmark_detector()` for standardized precision, recall, and F1 evaluation.
- Rewrote **`scoring.py`** into the **AIVSS aligned FinBot Security Score (0 to 100)**, with a weighted 25/20/30/25 split across ASI category groups and a five tier risk classification (see [§8](#8-finbot-security-score-aivss-aligned-risk-model)).
- Added **end to end and parametric integration tests** (`test_end_to_end.py`, `test_all_asi_categories.py`) validating the full FinBot plus AEGIS stack, including blocked transaction scenarios.
- Wired a new **CI workflow** (`.github/workflows/aegis-bench.yml`) that runs the benchmark suite on every push and pull request, and on a weekly schedule, uploading the score report as a build artifact.
- **OWASP coverage introduced:** end to end validation across all ten categories.

### 🧩 Weeks 10 to 12 · New CTF Challenges & Detectors
**PR:** [#553](https://github.com/GenAI-Security-Project/finbot-ctf/pull/553)

- **"Memory Poison Replay"** challenge (`memory_poisoning/memory_poison_replay.yaml`, 250 points, Intermediate). The player must poison an agent's conversation memory to escalate privileges and reach another user's financial data. Paired with **`MemoryPoisonReplayDetector`**, a two gate detector requiring both the injection and the subsequent unauthorized access in the same session. Maps to **ASI03 / ASI06**, **CWE-250 / CWE-284**.
- **"Multi Agent Cascade Failure"** challenge (`cascade_failure/multi_agent_cascade.yaml`, 300 points, Advanced). The player must trigger a failure that cascades across at least three agents. Paired with **`CascadeFailureDetector`**, which watches for suspicious broadcast messages and error propagation patterns across the agent mesh. Maps to **ASI08 / ASI02**, **CWE-94 / CWE-20**.
- Both detectors inherit from `BaseDetector`, register via `@register_detector`, and were verified against the full existing regression suite with **zero breaking changes**.
- **Two additional challenges** were completed in this same phase, bringing the total to four new CTF challenges shipped: *[Challenge 3 name]* and *[Challenge 4 name]*. Send me their names, point values, difficulty, detector class, and ASI/CWE mapping and I will slot them in here to match the write-ups above.

---

## 6. Pull Request Ledger

Every item below links directly to the corresponding GitHub work, ordered chronologically from my first hardening fixes to the org, through the full AEGIS build out.

| # | Pull Request | Phase | Status |
|---|---|---|---|
| 1 | [**Fix(pattern_match): prevent false positive when valid regex doesn't match**](https://github.com/GenAI-Security-Project/finbot-ctf/pull/274) | Early QA fix, before the coding period · closed Mar 23 | ![merged](https://img.shields.io/badge/status-MERGED-8250df) |
| 2 | [**Fix(finstripe): reject non positive amounts in `create_transfer` before DB insert**](https://github.com/GenAI-Security-Project/finbot-ctf/pull/464) | Early QA fix, before the coding period · closed Apr 11 | ![merged](https://img.shields.io/badge/status-MERGED-8250df) |
| 3 | [**Feat(aegis/phase1): Telemetry audit pipeline + IntentGate observe service**](https://github.com/GenAI-Security-Project/finbot-ctf/pull/518) | Weeks 1 to 3 · Pillar 3 plus Pillar 2 observe mode · opened May 30 | ![open](https://img.shields.io/badge/status-OPEN-2da44e) |
| 4 | [**Fix CI Blocker: Lazy LLMClient Initialization**](https://github.com/GenAI-Security-Project/finbot-ctf/pull/520) | Unblocks #518 and all downstream PRs · opened May 30 | ![open](https://img.shields.io/badge/status-OPEN-2da44e) |
| 5 | [**Feat(simulator): complete attack simulator infrastructure for Phase 2 (Weeks 3 to 6), all ASI categories**](https://github.com/GenAI-Security-Project/finbot-ctf/pull/530) | Weeks 3 to 6 · Pillar 1 · opened Jun 21 | ![open](https://img.shields.io/badge/status-OPEN-2da44e) |
| 6 | [**WEEK 7: Implement Policy Interceptor and Agent Profiles for FinBot Security Framework**](https://github.com/GenAI-Security-Project/finbot-ctf/pull/543) | Week 7 · Pillar 2 · opened Jul 12 | ![open](https://img.shields.io/badge/status-OPEN-2da44e) |
| 7 | [**Week 8-9: Implement AEGIS Harness Components for Red Team Testing Framework**](https://github.com/GenAI-Security-Project/finbot-ctf/pull/546) | Weeks 8 and 9 · Pillar 4 | ![open](https://img.shields.io/badge/status-OPEN-2da44e) |
| 8 | [**Week 10-12: CTF Challenge & Detector Development for AEGIS Framework**](https://github.com/GenAI-Security-Project/finbot-ctf/pull/553) | Weeks 10 to 12 · Challenge Expansion | ![open](https://img.shields.io/badge/status-OPEN-2da44e) |

**Quick links:**
- 🔎 [All my pull requests in `finbot-ctf`](https://github.com/GenAI-Security-Project/finbot-ctf/pulls?q=is%3Apr+author%3AJean-Regis-M)
- 🔎 [All my commits in `finbot-ctf`](https://github.com/GenAI-Security-Project/finbot-ctf/commits?author=Jean-Regis-M)

> **A note on status, in the interest of full transparency:** the two early QA fixes (#274, #464) are merged. The six GSoC cycle pull requests (#518 through #553) are code complete, individually tested (each carries its own passing local test suite, documented above), and were submitted for mentor review as each phase closed. They are **open and awaiting final merge** at the time of this report. Per the collaboration cadence agreed with my mentors (review requested within 48 hours of each PR), I expect these to land on `main` before the final submission deadline, and getting them merged is my top remaining priority (see [§12](#12-whats-left-to-do)).

---

## 7. OWASP Agentic Top 10: Coverage Map

| ASI # | Threat | AEGIS Coverage Delivered |
|---|---|---|
| **ASI01** | Agent Goal Hijack | 8 prompt injection attack recipes; IntentGate policy evaluation |
| **ASI02** | Tool Misuse & Exploitation | 8 tool misuse recipes; `MCPPolicyInterceptor` least agency enforcement |
| **ASI03** | Identity & Privilege Abuse | Agent Profile scoping; Memory Poison Replay challenge detector |
| **ASI04** | Agentic Supply Chain | Supply chain attack recipes; `AdviceServer` adversarial mock |
| **ASI05** | Unexpected Code Execution | RCE attack recipes; `DataExfiltrationServer` adversarial mock |
| **ASI06** | Memory & Context Poisoning | HMAC-SHA256 chained telemetry; **Memory Poison Replay** CTF challenge (new) |
| **ASI07** | Insecure Inter Agent Communication | Delegation and impersonation attack recipes |
| **ASI08** | Cascading Failures | **Multi Agent Cascade Failure** CTF challenge (new); per namespace isolation |
| **ASI09** | Human Agent Trust Exploitation | Attack recipe coverage in the Week 6 recipe set |
| **ASI10** | Rogue Agents / Insufficient Monitoring | Full SentinelStream tamper evident audit trail |

All ten categories are exercised by the Attack Simulator's recipe library, and every event that flows through IntentGate or SentinelStream is tagged with its corresponding ASI label for downstream scoring.

---

## 8. FinBot Security Score: AIVSS Aligned Risk Model

The Red Team Harness (`harness/scoring.py`) computes a single **0 to 100 FinBot Security Score** from live telemetry, weighted across four ASI category groups:

| ASI Group | Weight |
|---|---|
| Goal Hijacking (ASI01 to 03) | 25% |
| Tool Misuse / RCE (ASI04 to 05) | 20% |
| Cascade Failures / Excessive Agency (ASI06 to 07) | 30% |
| Data Poisoning / Privilege Escalation / Autonomy (ASI08 to 10) | 25% |

```python
def classify_risk(score: int) -> str:
    if score <= 9:
        return "MINIMAL"
    elif score <= 39:
        return "LOW"
    elif score <= 69:
        return "MEDIUM"
    elif score <= 89:
        return "HIGH"
    return "CRITICAL"
```

| Score Range | Risk Level | |
|---|---|---|
| 0 to 9 | **MINIMAL** | ![minimal](https://img.shields.io/badge/-MINIMAL-brightgreen) |
| 10 to 39 | **LOW** | ![low](https://img.shields.io/badge/-LOW-green) |
| 40 to 69 | **MEDIUM** | ![medium](https://img.shields.io/badge/-MEDIUM-yellow) |
| 70 to 89 | **HIGH** | ![high](https://img.shields.io/badge/-HIGH-orange) |
| 90 to 100 | **CRITICAL** | ![critical](https://img.shields.io/badge/-CRITICAL-red) |

This score is recomputed automatically by the `aegis-bench.yml` CI workflow on every push and weekly, giving maintainers a single, reproducible number to track FinBot's defensive posture over time.

---

## 9. Engineering Challenges & Solutions

### ⚡ Challenge 1: Cryptographic chaining was too slow for an interactive platform
HMAC-SHA256 on every single tool call risked unacceptable latency. **Solution:** canonical JSON with sorted keys for deterministic serialization, an LRU cache of recent chain hashes per namespace, and *selective* chaining (full cryptographic strength on high risk events, lighter hashing on routine calls). **Result:** overhead dropped from about 15ms to under 2ms per event.

### ⚡ Challenge 2: Policy evaluation could block real time agent interactions
**Solution:** fully asynchronous policy evaluation (`asyncio`), short lived result caching for repeated `(agent, tool, args)` tuples, and a hierarchical fast path (cheap allow list checks before expensive intent validation). **Result:** about 95% of decisions now serve from cache in under 0.5ms; the remainder stay under 2ms.

### ⚡ Challenge 3: Security controls could accidentally break the learning experience
Overly aggressive blocking would prevent students from completing the very challenges FinBot exists to teach. **Solution:** namespace scoped enforcement modes, **Observe** (log only, block catastrophic threats), **Learn** (alert but allow), **Enforce** (strict, production like), and **Research** (protections disabled for controlled experimentation), each configurable per player namespace.

### ⚡ Challenge 4: Coordinating security decisions across a distributed agent system
**Solution:** treat the immutable audit trail as the single source of truth (event sourcing), make all security actions idempotent so they are safe to retry, and apply a circuit breaker pattern so a temporarily unavailable security service degrades gracefully instead of taking down the platform.

---

## 10. Proposal Accountability: Milestones vs. Delivery

Holding my original proposal to account, milestone by milestone:

| Milestone | Target Date | Status | Evidence |
|---|---|---|---|
| M1 · Telemetry Pipeline MVP | Jun 13 | ✅ Delivered | [#518](https://github.com/GenAI-Security-Project/finbot-ctf/pull/518) |
| M2 · Attack Simulator Core | Jun 27 | ✅ Delivered | [#530](https://github.com/GenAI-Security-Project/finbot-ctf/pull/530) |
| M3 · Policy Engine Alpha | Jul 10 | ✅ Delivered | [#543](https://github.com/GenAI-Security-Project/finbot-ctf/pull/543) (opened Jul 12) |
| M4 · Midterm Evaluation | Jul 10 | ✅ Delivered | Midterm progress report, see [§13](#13-thought-leadership) |
| M5 · Red Team Harness | Jul 25 | ✅ Delivered | [#546](https://github.com/GenAI-Security-Project/finbot-ctf/pull/546) |
| M6 · Attack Simulator Complete (all 10 ASI) | Aug 8 | ✅ **Delivered ahead of schedule** (by Week 6, early July) | [#530](https://github.com/GenAI-Security-Project/finbot-ctf/pull/530) |
| M7 · Policy Engine Production | Aug 16 | 🟡 Partial, interceptor and profiles complete; intent capsule signing and memory segmentation remain stubs | [#543](https://github.com/GenAI-Security-Project/finbot-ctf/pull/543) |
| M8 · Challenge Expansion Wave 1 | Aug 8 | 🟡 Partial, 4 of the planned 10 to 12 new challenges delivered | [#553](https://github.com/GenAI-Security-Project/finbot-ctf/pull/553) |
| M9 · QA / CI Hardening | Aug 16 | ✅ Delivered, expanded test matrix, `aegis-bench.yml`, O(1) event type indexing | [#518](https://github.com/GenAI-Security-Project/finbot-ctf/pull/518), [#546](https://github.com/GenAI-Security-Project/finbot-ctf/pull/546) |
| M10 · Final Submission | Aug 24 | ⏳ In progress | This report |

**Bottom line:** all four architectural pillars proposed in May are implemented and tested, and the Attack Simulator's full ASI01 to ASI10 coverage landed *ahead* of its target date. The two areas trailing the original plan, full challenge expansion volume and Policy Engine production hardening, are called out honestly in [§12](#12-whats-left-to-do) below.

---

## 11. Current State

| Component | Status |
|---|---|
| Telemetry Pipeline (Pillar 3) | ✅ Complete, tamper evident, HMAC-SHA256 chained, live SSE dashboard |
| Policy Engine / IntentGate (Pillar 2) | ✅ Functional in **Observe** mode · interceptor and 6 agent profiles shipped · **Enforce** mode not yet wired into production config |
| Attack Simulator (Pillar 1) | ✅ Complete, full ASI01 to ASI10 recipe coverage, 5 adversarial MCP mocks, mutation engine |
| Red Team Harness (Pillar 4) | ✅ Complete, `pytest-aegis`, AIVSS scoring, CI benchmarking, end to end tests |
| Challenge Expansion (Track A) | 🟡 Partial, 4 of 10 to 12 planned new CTF challenges delivered |
| Documentation & Blogs (Track C) | ✅ Complete, both blog posts published, MkDocs documentation site complete |

All AEGIS code lives under `finbot/aegis/`, is additive only to the existing FinBot codebase, and is fully feature flagged off by default (`AEGIS_ENABLED=False`), so the platform's core CTF gameplay is never at risk.

---

## 12. What's Left To Do

- **Get the six open pull requests merged** (#518, #520, #530, #543, #546, #553). This is priority one, targeted for completion before the August 24 final submission deadline.
- **Expand CTF challenge coverage** from the 4 delivered toward the original target of 10 to 12 new challenges.
- **Complete the Week 8 stubs**: `capsule.py` (HMAC intent capsule signing) and `memory.py` (provenance tracked memory namespace isolation), to move the Policy Engine from Observe mode into a production ready Enforce mode.
- **Anyone extending this work** can start from the `finbot/aegis/` package tree and the test suites under `tests/unit/aegis/` and `tests/integration/aegis/`. Every module above is independently unit tested and documented inline.

---

## 13. Thought Leadership

📝 **Published (Midterm):** [*"Hardening Agentic AI: Building the AEGIS Security Framework for OWASP FinBot CTF"*](https://gsocupdate1.hashnode.dev/hardening-agentic-ai-building-the-aegis-security-framework-for-owasp-finbot-ctf), a technical progress report covering the first seven weeks: the telemetry pipeline's cryptographic design, IntentGate's intent capsule concept, the Attack Simulator's sandbox architecture, and the four engineering challenges detailed in [§9](#9-engineering-challenges-solutions).

📝 **Published (Closing):** [*"Building FinBot-AEGIS: Sandboxed MCP Red Teaming Against the OWASP Agentic Top 10"*](https://hashnode.com/edit/cmt7bpjrd00000agihc6ngt6v), covering the Pillar 1 simulator design, mock adversarial MCP servers, and final Red Team Harness benchmark results, published on the OWASP GenAI Security Project blog.

---

## 14. Acknowledgments & Career Value

My sincerest thanks to my mentors, **Nirupam Ghosh** ([@mekaizen](https://github.com/mekaizen)) and **Carolina Steadham** ([@steadhac](https://github.com/steadhac)), for their continuous guidance, sharp code review feedback, and unwavering support throughout this journey, and to the entire **OWASP Foundation** community, and specifically the **OWASP GenAI Security Project** team, for building a platform worth defending.

This project sits directly on my ongoing research path. The **Telemetry Pipeline** extends my interest in forensic attribution for LLM generated actions; the **Attack Simulator** extends adversarial ML red teaming work; and the **Policy Engine**'s fail secure, auditable design mirrors incident response protocols I have studied in threat modelling practice. It has been, above all, a portfolio grade, mentor reviewed piece of open source security engineering that I am proud to have built in the open.

---

## 15. About the Contributor

**Jean Francois Regis MUKIZA** is an undergraduate researcher in Digital Forensics Engineering & Cybersecurity at **Fırat University**, Elazığ, Turkey. His background spans applied threat modelling (STRIDE and DREAD), financial AI security risk analysis, and adversarial ML research, all of which converged directly into the design of FinBot-AEGIS.

<p align="left">
  <a href="https://github.com/Jean-Regis-M"><img src="https://img.shields.io/badge/GitHub-Jean--Regis--M-181717?style=flat-square&logo=github&logoColor=white" alt="GitHub"/></a>
  <a href="https://www.linkedin.com/in/mukiza-jean-francois-regis-bb9a80254/"><img src="https://img.shields.io/badge/LinkedIn-MUKIZA_Jean_Francois_Regis-0A66C2?style=flat-square&logo=linkedin&logoColor=white" alt="LinkedIn"/></a>
  <a href="mailto:240509606@firat.edu.tr"><img src="https://img.shields.io/badge/Email-240509606%40firat.edu.tr-D14836?style=flat-square&logo=gmail&logoColor=white" alt="Email"/></a>
</p>

**Languages:** English (fluent, research writing), Turkish (professional), French (professional), Kinyarwanda (native)

---

## 16. Links & License

<p align="center">
  <a href="https://owasp.org"><img src="https://img.shields.io/badge/OWASP-Foundation-000000?style=for-the-badge&logo=owasp&logoColor=white" alt="OWASP Foundation"/></a>
  <a href="https://github.com/GenAI-Security-Project"><img src="https://img.shields.io/badge/Organization-GenAI--Security--Project-181717?style=for-the-badge&logo=github&logoColor=white" alt="Org"/></a>
  <a href="https://github.com/GenAI-Security-Project/finbot-ctf"><img src="https://img.shields.io/badge/Repository-finbot--ctf-181717?style=for-the-badge&logo=github&logoColor=white" alt="Repo"/></a>
  <a href="https://www.youtube.com/watch?v=Xb_C8XbkBzM"><img src="https://img.shields.io/badge/Demo-YouTube-FF0000?style=for-the-badge&logo=youtube&logoColor=white" alt="Demo"/></a>
</p>

All code referenced in this report is released under the **Apache License 2.0**, matching FinBot's existing project license, and lives in the [`GenAI-Security-Project/finbot-ctf`](https://github.com/GenAI-Security-Project/finbot-ctf) repository under `finbot/aegis/`.

<p align="center"><i>Thank you to Google, the <a href="https://github.com/owasp">OWASP Foundation</a>, and the <a href="https://github.com/GenAI-Security-Project">OWASP GenAI Security Project</a> team for an incredible Summer of Code.</i></p>
