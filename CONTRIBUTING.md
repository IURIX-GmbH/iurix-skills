# Contributing to IURIX Agent Skills

Thanks for your interest! This repository is maintained by IURIX GmbH and is the
official, **public** home of the platform-agnostic skills our customers use to
integrate IURIX services into their AI agents.

## ⚠️ This is a public repository

Everything you add here is world-readable — **file contents, commit messages,
pull-request titles/descriptions, and issues**. Before you push or open anything,
make sure it contains **no confidential information**, in particular:

- no internal issue numbers, project codenames, or references to private/internal
  repositories;
- no internal hostnames, server paths, deployment details, or infrastructure
  internals;
- no API keys, tokens, or customer data of any kind.

A CI check (`.github/workflows/public-hygiene.yml`) scans changed files, commit
messages, and PR text for common internal markers and will fail the build if it
finds any. It is a safety net, not a substitute for care.

## Reporting bugs & asking questions

Use GitHub **Issues** — templates are provided. Please **never** include
confidential or personal data in an issue (see the note above). For security
vulnerabilities, follow [`SECURITY.md`](SECURITY.md) instead (private reporting).

## Pull requests

1. Keep skills as clean, self-contained Markdown (`SKILL.md`) that any capable
   agent can read and execute.
2. **Do not rename or move existing skill files** (`rici/SKILL.md`,
   `rita/SKILL.md`, …) — customer deployments pull these paths for auto-updates.
   Breaking a path breaks live customers.
3. Verify documented endpoints, parameters, and examples against the current
   production behaviour before submitting.
4. Write commit messages and PR descriptions that make sense to an outside reader
   (no internal references — see above).

### Developer Certificate of Origin (DCO)

By contributing, you certify the [DCO](https://developercertificate.org/). Sign
off each commit:

```bash
git commit -s -m "your message"
```

This adds a `Signed-off-by: Your Name <you@example.com>` line.

## Licensing

Contributions are accepted under the [Apache License 2.0](LICENSE), the license
of this repository.
