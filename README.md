# Centraleyezer Rules

Signed detection rule packs for the Centraleyezer scanner suite:

- **Netzer** — network device hardening + CVE detection (Cisco IOS, IOS-XE, NX-OS, ASA)
- **Eyezer DAST** — web application scanning (Nuclei-format templates, curated)

This repository is the canonical source of truth for the rule content. Customers
do **not** consume rules from this repo directly — they consume signed releases
via the published feed:

- https://rules.centraleyezer.io/netzer/v1/feed.json
- https://rules.centraleyezer.io/dast/v1/feed.json

Scanners pull manifests from those URLs, then download tarballs + signatures
from GitHub Releases on this repository.

## Layout

    packs/                    Rule YAML, fixtures, overlays
    ├── cis/                  CIS Benchmark L1 rules
    ├── cis-l2/               CIS Benchmark L2 rules
    ├── psirt/                Cisco PSIRT advisories (CVE + hardening)
    ├── stig/                 DISA STIG content
    ├── nist/                 NIST 800-53 overlays
    ├── cmmc/                 CMMC L2 overlays
    └── pci-dss/              PCI-DSS v4.0 overlays

    feed/v1/                  Customer-facing manifests served by GitHub Pages
    ├── feed.json             Per-product feed (per-pack URLs + sigs)
    ├── pubkey.txt            Sandline signing public key (baked into scanners)
    └── changelogs/           Per-release notes

    .github/workflows/        CI: lint, fixture-eval, release packaging

## Operating

See the platform spec in the `centraleyezer-rule-platform` project for full
architecture, signing workflow, and release process.

## Contributing

Internal use only. Curation via the editor app at
`editor.netzer.centraleyezer.io` and `editor.eyezer.centraleyezer.io`.
