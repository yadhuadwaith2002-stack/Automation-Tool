# Capabilities

Two core capabilities of an autonomous, AI-driven penetration testing workflow, based on the architecture of [Strix](https://github.com/usestrix/strix).

> **Authorized use only.** Run automated security testing only against systems you own or have explicit written permission to test (for example, a local lab or a deliberately vulnerable app).

---

## 1. Multi-Agent Orchestration ("Graph of Agents")

Instead of one monolithic scanner, testing is split across a team of specialized agents that work in parallel.

### How it works

- A **root agent** reads the scope, decides how to divide the work, and never touches the target itself.
- It spawns **subagents** for each job:
  - Reconnaissance (asset discovery, technology fingerprinting)
  - Vulnerability assessment (injection, access control, authentication, business logic)
  - Exploit validation (proof-of-concept development)
  - Reporting (findings and remediation advice)
- All agents share a **threat model**: one common description of the target, the attacker, and the trust boundaries. Agents correct it as they learn more, so their findings stay consistent.
- Agents can be spawned **dynamically** as results come in, for example to chain two findings into a larger attack path.
- A **coverage tracker** records what has been tested and what still needs follow-up before the scan finishes.

### Why it matters

| Benefit | Result |
| --- | --- |
| Parallel execution | Broad coverage in hours instead of days |
| Specialization | Each agent focuses on one class of weakness |
| Shared context | Less duplicate work and fewer conflicting conclusions |

---

## 2. Automated, Validated Security Testing in CI/CD

Security testing runs as part of the development pipeline and reports only what it can prove.

### How it works

- Runs **headless** in any CI system (GitHub Actions, GitLab CI, Jenkins, CircleCI).
- Supports different **scan depths**:

  | Mode | Typical duration | Best for |
  | --- | --- | --- |
  | Quick | Minutes | Every pull request |
  | Standard | 30-60 minutes | Weekly scans |
  | Deep | 1-4 hours | Pre-release audits |

- On pull requests, quick scans can be limited to **changed files only**.
- Findings are **validated with a working proof of concept**, which cuts down the false positives common in traditional scanners.
- The pipeline is **gated by exit code**: `0` means clean, `2` means vulnerabilities were found at or above a chosen severity, and `1` means an execution error.
- Results are written as a Markdown report, per-vulnerability files, JSON, and **SARIF** so they plug into code-scanning dashboards.

### Example (conceptual)

```bash
# Headless quick scan that fails the build on high-severity findings
strix -n --target ./app --scan-mode quick --fail-on high
```

### Why it matters

| Benefit | Result |
| --- | --- |
| Shift-left security | Vulnerabilities are caught before they reach production |
| Fewer false positives | Developers spend time on real issues |
| Repeatable | The same process runs on every change, with no manual steps |

---

## Summary

| # | Capability | Key outcome |
| --- | --- | --- |
| 1 | Multi-agent orchestration | Parallel, specialized, coordinated testing |
| 2 | Automated CI/CD security testing | Proven findings that can block insecure code |
