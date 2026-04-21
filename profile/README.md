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

## Status

Proof-of-concept phase. The first Tier 1 packages are being bootstrapped. Star & watch to follow along.

- 📦 **Packages index** &mdash; _coming soon once the first package ships_
- 📰 **Releases** &mdash; _no releases yet; watch this org to be notified_
- 🤝 **Contributing** &mdash; forks welcome. A repo-level `CONTRIBUTING.md` with DCO sign-off policy and the contract-test workflow will land with the first package template.

## License

All packages are MIT-licensed unless noted otherwise in their repository.
