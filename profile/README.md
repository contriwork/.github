<div align="center">

![ContriWork](../assets/contriwork-banner-github.png)

### One API surface, three languages.

**The contribution that works!**

</div>

---

## What is ContriWork?

Every ContriWork library ships on three registries at the same version, on the same day, passing the same cross-language contract tests.

| Registry | Namespace        | Install                                  |
|----------|------------------|------------------------------------------|
| PyPI     | `contriwork-*`   | `pip install contriwork-<name>`          |
| NuGet    | `Contriwork.*`   | `dotnet add package Contriwork.<Name>`   |
| npm      | `@contriwork/*`  | `npm install @contriwork/<name>`         |

## Principles

- **Contract-first.** Every package has a language-agnostic `CONTRACT.md`; Python, C#, and TypeScript implementations are validated against the same fixture set.
- **All-or-nothing releases.** A version is published to PyPI, NuGet, and npm together or not at all. No single-language drift.
- **Battle-tested before "done".** Every package is integrated into a real consumer and replaces internal duplicated code before it is declared stable.
- **Supply-chain conscious.** OIDC trusted publishing (no long-lived tokens), CycloneDX SBOM per release, signed commits, and `Trivy` + `CodeQL` + `Semgrep` in CI.

## Runtime Baseline

One current LTS per language — short-lived LTS matrices are explicitly avoided.

| Language | Target               | Support window |
|----------|----------------------|----------------|
| Python   | **3.13**             | until Oct 2029 |
| .NET     | **10 (LTS)**         | until Nov 2028 |
| Node.js  | **24 (Active LTS)**  | until Apr 2027 |

Container base images are SHA-pinned (`python:3.13-slim-trixie`, `mcr.microsoft.com/dotnet/runtime:10.0-jammy-chiseled-extra`, `node:24-alpine` / distroless), non-root, read-only filesystem, `cap-drop=ALL`, with `HEALTHCHECK` and Trivy HIGH/CRITICAL = 0 as a release gate.

## Quality & Security Standards

Every ContriWork package is held to the same engineering bar, regardless of language. Standards are enforced in CI and tested against a shared fixture set.

### Performance & Resilience

- **Latency budget per package.** Hot-path operations have a documented P95 target; regressions fail CI.
- **No crash-loops.** Recoverable errors surface as typed failures; unrecoverable errors fail fast with a clear exit code.
- **Backpressure-aware.** I/O adapters implement timeouts, retry-with-jitter, and circuit breakers. No unbounded queues.
- **N+1 prevention.** Batched APIs are preferred; query-level instrumentation is part of the adapter contract.
- **GC-friendly.** No per-request allocations on hot paths; object pooling where the port contract allows it.
- **Partial-outage tolerance.** Optional adapters degrade gracefully instead of taking the consumer down.

### Security by Default

- **No secrets in code.** `gitleaks` runs pre-commit and in CI. Runtime secrets are injected (Vault / Conjur / cloud KMS), never baked into images or env files committed to git.
- **Transport security.** mTLS is the default for service-to-service; TLS 1.2+ minimum for egress.
- **AuthN/AuthZ.** Packages that expose HTTP surfaces ship with JWT or API-key middleware adapters; OWASP API Top 10 mitigations are part of `CONTRACT.md`.
- **Input validation.** Every public entry point validates at the boundary (Pydantic / FluentValidation / Zod). Injection, SSRF, and deserialization vectors are treated as default threats.
- **Supply chain.** Trusted Publishers (OIDC) for PyPI, NuGet, npm; npm builds ship `--provenance`. Lockfiles are required and verified in CI (`--frozen-lockfile`). `postinstall` scripts are whitelisted.
- **Container hardening.** Non-root, read-only root FS, `no-new-privileges`, `cap-drop=ALL`, minimal base images, dependency pinning.
- **Scanning.** `Trivy` + `Grype` on images, `CodeQL` + `Semgrep` + `Bandit` / `Security Code Scan` / `eslint-plugin-security` on source, `pip-audit` / `dotnet list --vulnerable` / `pnpm audit` on dependencies. HIGH/CRITICAL = release blocker.

### Penetration Testing Posture

Every package is developed with an adversarial mindset and validated against the three standard attacker viewpoints before it ships:

- **Black-box** — no source access; fuzz public APIs, HTTP surfaces, and input validators for unexpected states and crashes.
- **Gray-box** — partial knowledge; review authZ boundaries, trust assumptions, and adapter handoffs for privilege-escalation and confused-deputy vectors.
- **White-box** — full source review; threat-model the port, adapters, and config schema; cover deserialization, SSRF, path-traversal, timing side-channels, and supply-chain vectors.

Findings, including any zero-day-class issues uncovered during development, are fixed before the affected version is tagged. Each release documents:

- what attack classes the package is hardened against (in `SECURITY.md`);
- the scan tooling that produced green results (versions + configs, not just names);
- a coordinated-disclosure channel for anyone reporting a new issue.

If an issue cannot be fixed in the release window, the package is held back — we do not publish known-exploitable versions to PyPI, NuGet, or npm.

### Observability

- Structured logs + metrics + traces, correlated by a request/operation ID that propagates across languages.
- RFC 7807 `ProblemDetails`-shaped error responses for HTTP surfaces; machine-readable error codes across all three language variants.
- OpenTelemetry-compatible exporters are optional adapters — zero-config by default, opt-in when the consumer wants depth.

### Engineering Discipline

- **SOLID by default.** Small, single-responsibility ports; adapters are composable and substitutable.
- **Happy-path-only code is rejected.** Every error branch is considered and either handled or surfaced with a typed contract.
- **Root-cause fixes over symptom patches.** Each bug fix ships with a regression test and a note on adjacent impact.
- **No over-engineering.** The simplest design that satisfies the contract wins; speculative abstractions are removed in review.
- **Readable names, consistent style.** `ruff` + `mypy` + `dotnet format` + `tsc --strict` + `eslint` are CI-enforced.

## Status

Proof-of-concept phase. The first Tier 1 packages are being bootstrapped. Star & watch to follow along.

- 📦 **Packages index** &mdash; _coming soon once the first package ships_
- 📰 **Releases** &mdash; _no releases yet; watch this org to be notified_
- 🤝 **Contributing** &mdash; forks welcome. A repo-level `CONTRIBUTING.md` with DCO sign-off policy and the contract-test workflow will land with the first package template.

## License

All packages are MIT-licensed unless noted otherwise in their repository.
