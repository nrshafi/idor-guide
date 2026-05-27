# IDOR Hunting — Zero to Advanced

A practical, opinionated field guide to finding **Insecure Direct Object References** in real bug bounty programs. From your first `?id=123` to chained GraphQL alias-batch extractions — with current 2024-2026 techniques, real disclosed reports, and the modern tooling stack.

**Live guide:** [https://nrshafi.github.io/idor-guide/](https://nrshafi.github.io/idor-guide/)

---

## About

This is a single-file, fully self-contained HTML guide aligned to the **OWASP Top 10:2025** (where Broken Access Control remains #1) and the **OWASP API Security Top 10:2023** (where BOLA — the API-shaped name for IDOR — remains API1). It progresses from absolute fundamentals to advanced chaining and AI-assisted hunting, with HTTP examples, real disclosed HackerOne reports, methodology checklists, and a printable one-pager at the end.

The guide is written for authorized bug bounty work only — every technique applies exclusively to programs whose scope explicitly permits testing.

## Table of Contents

The guide is structured as 11 progressive sections grouped into four phases:

### Foundations
- **00 — Fundamentals** &nbsp; What IDOR actually is, horizontal vs vertical, OWASP placement, impact menu
- **01 — Recon & Targets** &nbsp; Program selection, JS bundle mining, Swagger/GraphQL discovery, mobile APK extraction
- **02 — Identifying Candidates** &nbsp; ID format guide (sequential, UUIDv1/v4, base64, hashes, slugs, snowflake), where IDs hide

### Hunting
- **03 — Testing Methodology** &nbsp; Two-account loop, Burp Match & Replace, Autorize/AuthMatrix workflow, method matrix
- **04 — Bypass Techniques** &nbsp; HTTP verb tampering, path normalization, header injection, parameter pollution, type confusion, Unicode tricks
- **05 — GraphQL IDOR** &nbsp; Introspection, nested-resolver gaps, alias batching for mass extraction & rate-limit bypass, mutations

### Advanced
- **06 — Advanced Techniques** &nbsp; Chaining (IDOR→ATO, IDOR+race, IDOR+mass-assignment, IDOR+cache deception), WebSocket IDOR, mobile, UUID prediction
- **07 — Tooling & Automation** &nbsp; Burp vs Caido, must-have extensions, CLI stack, safe Python automation
- **08 — AI-Assisted Hunting (2026)** &nbsp; Where AI helps vs doesn't, LLM-augmented recon, AI proxies (Caido Shift, Burp AI, PentestGPT, Chatio), the multi-agent Glasswing pattern, critical caveats

### Closeout
- **09 — Writing the Report** &nbsp; Structure, CVSS framing, triager-pushback pre-emption, 2024-2026 bounty ranges
- **10 — Practice & Resources** &nbsp; Labs (PortSwigger, crAPI, VAmPI, DVGA), reading list, hunters to follow, wordlists, printable checklist

## What's New in This Version

- **OWASP 2025-aligned.** Updated to reference OWASP Top 10:2025 (released as RC1 in November 2025), with the notable change that SSRF was rolled into A01 Broken Access Control. Confirms that OWASP API Security Top 10 is still the 2023 edition.
- **AI-Assisted IDOR Hunting (Section 08).** A full section on where modern AI tooling helps the 2026 hunter — and where it actively breaks the workflow. Covers Caido Shift & Assistant, Burp AI, PentestGPT Agentic v1.0, the Cloudflare Glasswing multi-agent pattern (and the open-source `evilsocket/audit` reimplementation), AI prompt templates with safety caps, and a human-in-the-loop workflow checklist.
- **Modern bypass catalog.** Unicode/whitespace path tricks, type confusion, parameter pollution variants, header-based bypasses, and version-downgrade endpoint hunting.
- **2024-2026 disclosed reports cited.** Including HackerOne #291531, #1016122, #1064543, #1987489, and the August 2024 IDOR exposing 500K+ US passport scans.

## Quick Start

### View the live version

[https://nrshafi.github.io/idor-guide/](https://nrshafi.github.io/idor-guide/)

### View locally

```bash
git clone https://github.com/nrshafi/idor-guide.git
cd idor-guide
# Open in your browser:
open index.html        # macOS
xdg-open index.html    # Linux
start index.html       # Windows
```

The HTML is fully self-contained — inline CSS, Google Fonts loaded via CDN, no build step, no dependencies.

## Sources & Acknowledgments

Built with material from:

- **OWASP Top 10:2025** (RC1, November 2025) and **OWASP API Security Top 10:2023**
- **PortSwigger Web Security Academy** — the foundational access-control labs
- **HackerOne Hacktivity** — disclosed reports including #291531, #1016122, #1064543, #1987489
- **GraphQL alias-batching research** — Lorikeet Security, 0xrafasec, the GraphQL.org security guide
- **Cloudflare Project Glasswing** and the `evilsocket/audit` open-source reimplementation for the multi-agent vulnerability discovery pattern
- **Caido**, **PentestGPT**, **ZeroPath**, and **Chatio** documentation for the 2025-2026 AI tooling landscape
- The broader bug bounty community's writeups (2024-2026)

## Authorization & Ethics

Every technique in this guide is for use only against systems whose owners have **explicitly** authorized testing — through a published bug bounty program, a vulnerability disclosure policy, or a written engagement scope.

Out-of-scope testing is unauthorized access. The same word that describes the bug class also describes what you'd be doing without authorization.

## License

This guide is offered for educational use in authorized bug bounty research. If you'd like to add a formal open-source license (MIT, CC-BY, etc.), open an issue or PR.

## Contributing

Improvements, corrections, additional disclosed-report references, and new tooling sections welcome via issues or PRs.

---

*IDOR.guide &middot; v.2026 &middot; OWASP 2025-aligned*
