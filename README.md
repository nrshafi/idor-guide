# IDOR Hunting — Zero to Advanced

A practical, opinionated field guide to finding **Insecure Direct Object References** in real bug bounty programs. From your first `?id=123` to chained GraphQL alias-batch extractions — with current 2024-2026 techniques, real disclosed reports, and the modern tooling stack.

**Live guide:** [https://nrshafi.github.io/idor-guide/](https://nrshafi.github.io/idor-guide/)
**One-page cheatsheet:** [https://nrshafi.github.io/idor-guide/cheatsheet.html](https://nrshafi.github.io/idor-guide/cheatsheet.html) &middot; *(print-optimized desk reference)*

---

## About

This is a single-file, fully self-contained HTML guide aligned to the **OWASP Top 10:2025** (where Broken Access Control remains #1) and the **OWASP API Security Top 10:2023** (where BOLA — the API-shaped name for IDOR — remains API1). It progresses from absolute fundamentals to advanced chaining and AI-assisted hunting, with HTTP examples, real disclosed HackerOne reports, methodology checklists, and a printable one-pager at the end.

The guide is written for authorized bug bounty work only — every technique applies exclusively to programs whose scope explicitly permits testing.

## Table of Contents

The guide is structured as 11 progressive sections grouped into four phases:

### Foundations
- **00 — Fundamentals** &nbsp; What IDOR actually is, horizontal vs vertical, the **IDOR/BOLA/BOPLA/BFLA** taxonomy, the "knowledge ≠ authorization" anti-pattern (with the RFC 9562 citation), a CWE map, OWASP 2025 placement, impact menu
- **01 — Recon & Targets** &nbsp; Program selection, **source-map (`.js.map`) reconstruction**, a copy-paste recon pass (gau/waymore/katana/jsluice/arjun), Swagger/GraphQL/Postman discovery, mobile APK extraction
- **02 — Identifying Candidates** &nbsp; Full modern ID-format guide with a **"predictable portion"** column — **MongoDB ObjectID deep-dive**, the **UUIDv1 sandwich attack**, UUIDv7/ULID/KSUID/nanoid/Snowflake/Stripe, **Hashids/Sqids are reversible**, JWT & ETag references, decode-before-you-dismiss

### Hunting
- **03 — Testing Methodology** &nbsp; Two-account loop, Match & Replace, the **three-identity test**, Autorize vs Auth Analyzer vs AuthMatrix (+ Caido plugins), method matrix, **blind-IDOR detection**, swap-and-revert verification
- **04 — Bypass Techniques** &nbsp; Verb tampering, path normalization, header injection, parameter pollution, type confusion, Unicode tricks, **send the ID in two places**, **response-code oracles**
- **05 — GraphQL IDOR** &nbsp; Introspection + **graphw00f/Clairvoyance recovery**, nested-resolver gaps, the **Relay `node()` back door**, **directive deception (fragment bypass)**, alias/array batching, mutations, **APQ vs safelisting**
### Advanced
- **06 — Advanced Techniques** &nbsp; Chaining (IDOR→ATO, +race, +mass-assignment, +cache deception), the **Kia portal** real-world chain, **second-order (stored) IDOR**, WebSocket IDOR, mobile, predicting server-generated IDs
- **07 — Tooling & Automation** &nbsp; Burp vs Caido (Automate/Workflows/Match & Replace + auth plugins), must-have extensions, a dedicated **GraphQL tool set**, **mobile tooling** (Frida 17.x note), expanded CLI stack, safe Python automation
- **08 — AI-Assisted Hunting (2026)** &nbsp; Where AI helps vs doesn't, LLM-augmented recon, AI proxies (Caido Shift, Burp AI, PentestGPT, Chatio), the multi-agent swarm pattern (`evilsocket/audit`), the **autonomous-agent reality (XBOW #1 on HackerOne US 2025)**, critical caveats

### Closeout
- **09 — Writing the Report** &nbsp; Structure, **CVSS 3.1 + 4.0 worked vectors** (VC/VI/VA + SC/SI/SA, the new AT metric), triager-pushback pre-emption, 2024-2026 bounty ranges
- **10 — Practice & Resources** &nbsp; Labs (PortSwigger, crAPI, VAmPI, DVGA), reading list (Hacking APIs, OWASP GraphQL cheat sheet, GraphQL Threat Matrix, RFC 9562), hunters to follow, wordlists, printable checklist

## What's New in This Version

This revision (July 2026) is a deep expansion of every section — the goal is to be the most complete, technically-precise IDOR resource for bug hunters online. Highlights:

- **A real taxonomy.** Section 00 now distinguishes **IDOR / BOLA (API1) / BOPLA (API3) / BFLA (API5)**, states the single anti-pattern behind all of them ("knowledge of an ID ≠ authorization"), backs it with the **RFC 9562 §6.9** citation you can quote at a triager, and includes a **CWE map** (639/566/285/284) plus the full OWASP Top 10:2025 lineup.
- **The definitive ID-format section.** A rewritten guide ordered by *predictable portion*, with technically-accurate deep-dives on **MongoDB ObjectID** prediction (4-byte timestamp + 5-byte per-process value + 3-byte counter — and why one known ID collapses the space), the **UUIDv1 sandwich attack**, and the fact that **Hashids/Sqids are reversible obfuscation, not encryption**. Plus UUIDv7/ULID/KSUID/nanoid/Snowflake/Stripe/JWT/ETag.
- **Source-map recon.** Section 01 adds `.js.map` reconstruction (`sourcemapper`, `unwebpack-sourcemap`) and a copy-paste recon pass (`gau`/`waymore`/`katana -kb-endpoints`/`jsluice`/`arjun`).
- **Sharper methodology.** The **three-identity test** (owner / other user / unauthenticated), an Autorize vs Auth Analyzer vs AuthMatrix comparison (with the Caido `autorize`/`authmatrix`/`authswap` plugins), **blind-IDOR** detection via side effects, and swap-and-revert verification.
- **Modern GraphQL.** The **Relay `node()` back door**, **directive deception** (fragment-based `@auth` bypass), `graphw00f` + `Clairvoyance` schema recovery, and the **APQ-vs-safelisting** distinction that many hunters get wrong.
- **New bypasses.** Send the ID in two places at once, and read 401/403/404/200 as an **oracle**.
- **CVSS 3.1 *and* 4.0.** Section 09 adds worked v4.0 vectors — the **VC/VI/VA + SC/SI/SA** split and the new **AT (Attack Requirements)** metric — plus a note that HackerOne added CVSS 4.0 support in March 2025.
- **Grounded AI section.** Corrects the multi-agent attribution (the swarm pattern is best studied via the open-source `evilsocket/audit`; **Project Glasswing was an industry research program, not a Cloudflare tool**), and adds the 2025-2026 autonomous-agent reality (**XBOW reached #1 on HackerOne's US leaderboard**; offense vs defender-side tools like ZeroPath).
- **OWASP 2025-aligned.** OWASP Top 10:2025 (RC1, Nov 2025) with SSRF folded into A01; OWASP API Security Top 10 confirmed still 2023.
- **Polished, self-contained reading experience.** Copy-to-clipboard on every code block, active-section sidebar tracking, a collapsible mobile nav, a reading-progress bar, and back-to-top — plus full SEO/social metadata (Open Graph, Twitter cards, JSON-LD), an inline-SVG favicon, and accessibility passes (skip link, ARIA, `prefers-reduced-motion`). All in dependency-free vanilla JS. **MIT-licensed**.

## Quick Start

### View the live version

- **Full guide:** [https://nrshafi.github.io/idor-guide/](https://nrshafi.github.io/idor-guide/)
- **One-page cheatsheet:** [https://nrshafi.github.io/idor-guide/cheatsheet.html](https://nrshafi.github.io/idor-guide/cheatsheet.html)

### View locally

```bash
git clone https://github.com/nrshafi/idor-guide.git
cd idor-guide
# Open in your browser:
open index.html        # macOS — main guide
open cheatsheet.html   # macOS — printable cheatsheet
xdg-open index.html    # Linux
start index.html       # Windows
```

The HTML is fully self-contained — inline CSS, a small inline-JS enhancement layer (copy buttons, active-section nav, reading progress, mobile menu), Google Fonts loaded via CDN, no build step, and no third-party libraries.

### Printing the cheatsheet

The cheatsheet ships with a dedicated print stylesheet that flips the dark screen view to a clean black-on-white A4-landscape layout. Open `cheatsheet.html` in any browser, click **Print / Save PDF**, or use your browser's print dialog. The layout is optimized to fit a single landscape A4 page.

## Sources & Acknowledgments

Built with material from:

- **OWASP Top 10:2025** (RC1, November 2025), **OWASP API Security Top 10:2023** (BOLA/BOPLA/BFLA), and the **OWASP GraphQL Cheat Sheet**
- **RFC 9562** (the UUID specification) and **MITRE CWE** (639/566/285/284) for the format and weakness references
- **FIRST CVSS v4.0** specification, and HackerOne's CVSS 4.0 support documentation
- **PortSwigger Web Security Academy** — the foundational access-control, JWT, and GraphQL labs
- **HackerOne Hacktivity** — disclosed reports including #291531, #1016122, #1064543, #1987489
- **ID-prediction research & tooling** — `guidtool`/`uuidtool` and the sandwich attack (UUIDv1), `andresriancho/mongo-objectid-predict` (MongoDB ObjectID), and the Hashids/Sqids maintainers' own "not encryption" guidance
- **GraphQL security research & tooling** — `graphw00f` + the GraphQL Threat Matrix, `Clairvoyance`, `graphql-cop`, `BatchQL`, and alias-batching writeups
- **`evilsocket/audit`** as the studyable open-source example of the multi-agent ("swarm") vulnerability-discovery pattern
- **Burp Suite**, **Caido**, **PentestGPT**, and **Chatio** documentation for the 2025-2026 tooling and AI landscape
- The broader bug bounty community's writeups (2024-2026), including Sam Curry et al.'s automotive-portal access-control research

## Authorization & Ethics

Every technique in this guide is for use only against systems whose owners have **explicitly** authorized testing — through a published bug bounty program, a vulnerability disclosure policy, or a written engagement scope.

Out-of-scope testing is unauthorized access. The same word that describes the bug class also describes what you'd be doing without authorization.

## License

Released under the [MIT License](LICENSE) — free to use, modify, and share, with the copyright notice preserved. The guide's techniques are intended for educational use in **authorized** bug bounty research only (see *Authorization & Ethics* above).

## Contributing

Improvements, corrections, additional disclosed-report references, and new tooling sections welcome via issues or PRs.

---

*IDOR.guide &middot; v.2026 &middot; OWASP 2025-aligned*
