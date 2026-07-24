# Security Policy

## Reporting a vulnerability

**Please do not report security vulnerabilities through public GitHub issues,
discussions, or pull requests.**

If you believe you have found a security vulnerability in an IURIX skill or in
an IURIX service reachable through one of these skills, report it privately:

- **Email:** security@iurix.ai
- Alternatively, use GitHub's **[Private vulnerability reporting](https://docs.github.com/en/code-security/security-advisories/guidance-on-reporting-and-writing-information-about-vulnerabilities/privately-reporting-a-security-vulnerability)**
  (the "Report a vulnerability" button in this repository's *Security* tab).

Please include:

- a description of the issue and its impact,
- steps to reproduce (proof-of-concept if available),
- the affected skill / endpoint and, if relevant, the version.

We aim to acknowledge reports within **3 business days** and will keep you
informed about the progress toward a fix.

## Scope

This repository contains **client-side integration instructions** (skills) — it
holds no credentials and no server code. Reports about the hosted IURIX services
(`api.iurix.ai`, `mcp.iurix.ai`) are welcome through the same private channel.

## API keys

Skills in this repository never contain secrets. Each user supplies **their own**
IURIX API key at runtime. If you believe a key has been exposed, contact us so we
can rotate it — keys are per-customer and individually revocable.
