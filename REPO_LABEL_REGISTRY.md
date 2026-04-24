# Repo Label Registry

Standardized classification for every PlausiDen-namespace repo. Per the
2026-04-24 directive: unless a repo's README/description explicitly declares
it's a specific-use tool or standalone project, it is treated and labeled by
what it looks designed for.

Each repo's `README.md` should carry an HTML-comment header at the top:

```html
<!-- repo-label: <category> -->
<!-- repo-class: <one-line description of role> -->
<!-- repo-consumes: <comma-separated repo names; or "nothing" if root> -->
<!-- repo-consumed-by: <comma-separated; or "leaf" if nothing depends on it> -->
```

## Label categories

| Category | Meaning |
|---|---|
| `meta-doctrine` | Governs how other repos are built. Sits above the rest. |
| `infrastructure` | Generic substrate consumed by products (engines, libraries, CLI tools that aren't end-user-facing). |
| `product` | End-user-facing application or service. |
| `tool` | Self-contained CLI / utility (small scope, may overlap product). |
| `reference` | Docs, examples, or a fork-with-no-original-code. |
| `data` | Datasets, training corpora, or other primarily-data repos. |
| `archive` | Historical / superseded; kept for reference but not actively maintained. |

## Registry

Status as of 2026-04-24. Update as repos are touched.

| Repo | Label | Class | Header applied? |
|---|---|---|---|
| **PlausiDen-Meta** | meta-doctrine | ecosystem-priority-and-governance-gate | ✅ |
| **PlausiDen-AVP-Doctrine** | meta-doctrine | validation-protocol-and-agent-standing-orders | ✅ |
| **PlausiDen-Canon** | infrastructure | canonical-invariant-substrate | ✅ |
| **PlausiDen-Audits** | infrastructure | enforcement-engine-and-audit-catalog | ✅ |
| **PlausiDen-Tests** | infrastructure | testing-doctrine-and-harness | ✅ |
| **PlausiDen-Obs** | infrastructure | observability-doctrine-and-substrate | ✅ |
| **PlausiDen-Harvest** | infrastructure | upstream-harvest-protocol-and-tooling | ✅ |
| PlausiDen-Engine | infrastructure | core-rust-substrate-for-plausiden-suite | ⏳ |
| PlausiDen-AI | product | plausiden-ai-app-bundle | ⏳ |
| PlausiDen-AI-dist | archive | distribution-artifacts | ⏳ |
| PlausiDen-Sentinel | product | system-detection-and-monitoring-daemon | ⏳ |
| PlausiDen-Atrium | product | plausiden-os-control-surface | ⏳ |
| PlausiDen-Browser-Ext | product | browser-extension-for-the-suite | ⏳ |
| PlausiDen-Desktop | product | desktop-application-shell | ⏳ |
| PlausiDen-Android | product | android-application | ⏳ |
| PlausiDen-OS-for-Mobile | product | mobile-os-distribution | ⏳ |
| PlausiDen-MCP | infrastructure | model-context-protocol-server | ⏳ |
| PlausiDen-Crawler | infrastructure | web-crawler-substrate | ⏳ |
| PlausiDen-Firewall | infrastructure | network-firewall-substrate | ⏳ |
| PlausiDen-AppGuard | product | application-permission-guard | ⏳ |
| PlausiDen-Tidy | tool | filesystem-cleanup-utility | ⏳ |
| PlausiDen-Purge | tool | secure-deletion-utility | ⏳ |
| PlausiDen-Inject | tool | input-injection-utility | ⏳ |
| PlausiDen-Suite | product | suite-installer-and-orchestrator | ⏳ |
| PlausiDen-Shard | infrastructure | data-sharding-substrate | ⏳ |
| PlausiDen-Swarm | infrastructure | mesh-swarm-substrate | ⏳ |
| PlausiDen-USB | tool | usb-device-management-utility | ⏳ |
| PlausiDen-Grants | reference | grants-application-tracking | ⏳ |
| PlausiDen-Training-Data | data | training-corpora-for-plausiden-ai | ⏳ |
| PlausiDen-Product-AppReclaim | product | app-reclaim-product | ⏳ |
| PlausiDen-Product-BorderCloak | product | border-crossing-privacy-product | ⏳ |
| PlausiDen-Product-ClassActionScout | product | class-action-discovery-product | ⏳ |
| PlausiDen-Product-ComplianceKit | product | compliance-toolkit-product | ⏳ |
| PlausiDen-Product-DataBrokerSlayer | product | data-broker-removal-product | ⏳ |
| PlausiDen-Product-DeadSwitch | product | dead-mans-switch-product | ⏳ |
| PlausiDen-Product-DisputeBot | product | dispute-automation-product | ⏳ |
| PlausiDen-Product-Ideas | reference | product-ideas-and-roadmap-notes | ⏳ |
| PlausiDen-Product-LawCopilot | product | legal-copilot-product | ⏳ |
| PlausiDen-Product-LFIOracle | product | lfi-oracle-product | ⏳ |
| PlausiDen-Product-MedicalCopilot | product | medical-copilot-product | ⏳ |
| PlausiDen-Product-MoveOut | product | move-out-companion-product | ⏳ |
| PlausiDen-Product-Paranoid | product | paranoid-mode-product | ⏳ |
| PlausiDen-Product-RevengeCleaner | product | online-cleanup-product | ⏳ |
| PlausiDen-Product-ReviewReal | product | review-authenticity-product | ⏳ |
| PlausiDen-Product-ScamShield | product | scam-shield-product | ⏳ |
| PlausiDen-Product-Scrapey | product | scraping-product | ⏳ |
| PlausiDen-Product-Scrubs | product | data-scrub-product | ⏳ |
| PlausiDen-Product-Senti | product | sentiment-analysis-product | ⏳ |
| PlausiDen-Product-SilentPro | product | silent-pro-product | ⏳ |
| PlausiDen-Product-SwarmVault | product | swarm-vault-product | ⏳ |
| PlausiDen-Product-Ticketly | product | ticketing-product | ⏳ |
| PlausiDen-Product-VoiceVault | product | voice-vault-product | ⏳ |

## How to apply a header to a not-yet-applied repo

1. Open the repo's `README.md`.
2. Insert the four `<!-- repo-* -->` lines at the very top, above the `# Title`.
3. Set values per this registry. Update the registry's "Header applied?" column to ✅.
4. Commit with message: `repo-label: standardize header per PlausiDen-Meta/REPO_LABEL_REGISTRY.md`.

## Discrepancy resolution

If a repo's README/description **explicitly** states it is a specific-use
tool or standalone project, the explicit statement wins over this registry.
Update this registry to reflect the explicit classification with a footnote.

If the explicit statement disagrees with what the repo *looks designed for*,
the repo author's stated intent is authoritative — but flag it in this
registry so the divergence is visible.
