---
title: "Advanced Prompt Engineering for Agentic Programming"
subtitle: ""
date: 2026-03-06
tags:
  - AI
category: ["AI", "LLM"]
mermaid: true
---

Agentic programming is an emerging approach to building software where developers leverages an AI agent than can plan, take actions, use tools, and self-correct within defined constraints. Instead of a one-shot prompt, the agent operates in a loop of planning, executing function calls, observing results, and refining its approach. Developers provide the goals, capabilities, guardrails, and memory structures that let the agent work through multi-step tasks autonomously or semi‑autonomously. The result is a shift from querying a model to collaborating with a programmable partner to adapt to complex real-world environments.

Below are a few tips and techniques on how I like engaging with my programming agent. 

***

## 1) Core Agent Loop

What it is  
A strict loop that separates cognition from actions, enforces validation after each step, and keeps edits minimal and auditable.

Example (meta‑prompt, drop‑in)

```text
You are a Python coding agent.

Workflow:
1) Think — restate goal, constraints, acceptance criteria.
2) Analyze — locate impacted modules; read contracts (pydantic, OpenAPI, DB); list unknowns.
3) Plan — propose minimal steps; files to touch; commands to run (ruff/black/mypy/pytest); tests to add/update.
4) Execute — perform exactly one step; run format → lint → type check → targeted tests.
5) Review — verify acceptance criteria; if unmet, refine or request specific context. Emit a JSON report.

Constraints:
- Minimal diffs; no public API changes unless explicitly allowed.
- Prefer snippet requests over guessing missing code.
- Never output secrets; avoid disallowed paths (.git, .venv, node_modules, dist).
- Hide internal reasoning from user output; keep it in logs only.
```

Important considerations

*   Log inputs, retrievals, diffs, commands, and results for auditability.
*   Provide small, focused tools (read ranges, write diffs, run commands).
*   Use targeted tests (paths/markers) to speed feedback loops.

***

## 2) Cognitive Scaffolding & Task Decomposition (Contracts‑First)

What it is  
A structure that prevents premature coding by reading contracts before code and splitting work into atomic, gated stages.

Example (service hardening)

```text
Think: Add timeout + retries to `payments.py` without altering public API.
Analyze: Uses `requests`; no timeouts; basic error handling; tests at `tests/services/test_payments.py`.
Plan: (1) Add TIMEOUT_SECONDS; (2) mount urllib3 Retry adapter; (3) add tests; (4) run validations.
Execute: Perform only step 1; run ruff/black/mypy/pytest.
Review: If green, proceed; else fix and rerun.
```

Important considerations

*   Retrieve first: `pyproject.toml`, `mypy.ini`, `ruff`, `pytest.ini`, OpenAPI, pydantic models, DB migrations, ADRs.
*   Persist plan, diffs, and command outputs for CI/policy checks.
*   Keep steps reversible and scoped to one concern each.

***

## 3) Planning Prompts (Subgoals + Validation Contracts)

What it is  
Generate a machine‑checkable plan with explicit steps, files, tests, and validations—so you can approve or auto‑gate.

Example (planning prompt)

```text
Before editing:
- Think: Restate acceptance criteria (timeout=3s; retries=3 with jitter; no API change).
- Plan: Ordered steps, files to modify, commands to run, expected outputs/signals.
- Tests: Which to add/update and why (positive, negative, boundary).
- Validation: Gates (ruff, black --check, mypy, pytest) and thresholds (coverage ≥ 85% on touched files).
Output: JSON plan following the agreed schema.
```

Important considerations

*   Plans should be small, explicit, and diff‑aware (list lines/regions).
*   Require acceptance criteria up front; reject vague objectives.
*   Plans are inputs to policy engines (e.g., no new deps without approval).

***

## 4) Stepwise Execution (One Step, Evidence Attached)

What it is  
Execute exactly one planned step, produce diffs and check results, and propose next actions based on evidence.

Example (execution prompt)

```text
Execute only Step 1 now.
Evidence:
- Diff in unified format with brief rationale per hunk.
- Commands: ruff, black --check, mypy, pytest (targeted).
- Summaries: pass/fail + key lines from outputs.
If any gate fails, propose the smallest fix or request the specific missing context.
Output: JSON result following the step schema.
```

Important considerations

*   Disable multi‑file freeform edits; require explicit file lists.
*   Keep diffs minimal; justify each hunk.
*   Always run format → lint → type → tests in that order for fast fail.

***

## 5) Test‑Driven Prompting & Self‑Review

What it is  
Agents must prove correctness by adding/updating tests and clearing gates before finalizing.

Example (finalization prompt)

```text
Before finalizing:
- Add/update tests: positive, negative, boundary (use Hypothesis if properties exist).
- Run: pytest -q; fix failures.
- Ensure coverage ≥ 85% on touched files; mutation score ≥ 60% if configured.
- Emit: checklist + JSON with pass/fail, coverage per touched file, and mutation summary.
```

Important considerations

*   Prefer pytest markers to scope runs; collect coverage by path for touched files.
*   Apply Hypothesis selectively for API/contract heavy functions.
*   Run mutation tests (mutmut/cosmic‑ray) only on critical modules or nightly.

***

## 6) Constraint‑Driven Prompting (Policies that Prevent Rework)

What it is  
Hard rules that avoid unsafe or non‑compliant changes; the agent must honor them or stop and ask.

Example (policy block)

```text
- HTTP clients: `httpx` or `requests` only; no new deps without approval.
- Complexity: aim for O(n) time / O(1) space unless justified.
- Do not change public pydantic models/dataclasses without spec + versioning plan.
- Respect repo config: pyproject.toml, mypy.ini, ruff, pytest.ini.
- If config or env source is missing, stop and request its location and format.
- Never touch .git/, .venv/, node_modules/, dist/.
- Mask secrets in all logs and outputs.
```

Important considerations

*   Encode policies as pre‑checkers over plans and diffs.
*   Require justification strings when exceptions are proposed.
*   Keep policies short and testable (boolean checks in CI).

***

## 7) Output Structuring (Machine‑Verifiable JSON)

What it is  
Standard JSON contracts for plans, step results, and final reports—so CI and dashboards can reason over agent outputs.

Examples (schemas)
Plan

```json
{
  "summary": "Add timeout+retries to payments client",
  "acceptance": ["no API change", "timeout=3s", "retries=3 with jitter"],
  "steps": [
    {"id": "1", "title": "Inject TIMEOUT_SECONDS", "files": ["app/services/payments.py"]},
    {"id": "2", "title": "Configure Retry adapter", "files": ["app/services/payments.py"]},
    {"id": "3", "title": "Add tests", "files": ["tests/services/..."]},
    {"id": "4", "title": "Run validations", "files": []}
  ],
  "validations": {"ruff": true, "black_check": true, "mypy": true, "pytest": true, "coverage_min": 0.85}
}
```

Important considerations

*   Keep fields stable; version your schemas.
*   Limit large artifacts (diffs/logs) via tailing or attachments.
*   Use per‑file coverage for touched paths, not global project coverage.

***

## 8) Few‑Shot Prompting (Teach the Form, Not the Thoughts)

What it is  
Tiny examples that imprint your workflow shape (not chain‑of‑thought) and your JSON output schemas.

Examples

```text
Example A
Think: Identify missing config.
Plan: 3 steps with explicit commands.
Execute: Do Step 1; run ruff/mypy/pytest; emit JSON result.

Example B
Think: Read OpenAPI first.
Plan: Reconcile handlers with spec; generate tests.
Execute: Modify handlers minimally; validate contracts and emit JSON.
```

Important considerations

*   Keep examples short and domain‑specific (use your real file paths).
*   Demonstrate the intended formatting and schema fields.
*   Refresh few‑shots when policies or schemas change.

***

## 9) Retrieval‑Augmented Prompting (Just‑in‑Time Repo Context)

What it is  
Fetch the repo’s source‑of‑truth artifacts so the agent reasons over your contracts, not assumptions.

Example (instruction)

```text
Retrieve first: pyproject.toml, mypy.ini, ruff config, pytest.ini, OpenAPI fragments, pydantic models, DB migrations, ADRs.
Read contracts first and treat them as authoritative. If code conflicts with contracts, propose a resolution plan (schema change + version bump + migrations + tests).
Use diff‑first retrieval (line ranges) and cache manifest files.
```

Important considerations

*   Hash and cache manifests for stability across steps.
*   Prefer line‑range snippets over whole files to control tokens.
*   If an artifact is missing or ambiguous, stop and request it with expected structure.

***

## Closing

Make agents predictable by engineering cognition (think -> analyze -> plan -> execute -> review), codifying guardrails (style, typing, architecture, security), and feeding precise repo context (contracts and targeted snippets). With these patterns.