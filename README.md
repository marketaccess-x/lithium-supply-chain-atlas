# Lithium Supply Chain Atlas

Interactive visualization of the global lithium supply chain, rendered on an orthographic globe with four stackable policy exposure overlays.

Live at [lithium.marketaccess-x.com](https://lithium.marketaccess-x.com) (pending deployment).

---

## What this is

The atlas maps the global lithium supply chain across three tiers — mining, refining, and cell manufacturing — and shows how regulatory regimes distort, redirect, or block the flows between them.

Unlike conventional supply chain visualizations that render tonnage and stop there, this atlas is built to surface **structural regulatory exposure** that volume alone does not reveal. A flow can be legal at origin but blocked at destination. Physical diversification does not cleanse ownership taint under US credit rules. Nameplate capacity is frequently an order of magnitude larger than operational output. The atlas captures these realities through four overlays that can be toggled independently.

---

## The four overlays

**US PFE Regime.** Prohibited Foreign Entity rules under the One Big Beautiful Bill Act of July 2025 and IRS Notice 2026-15 of February 2026. Governs eligibility for Sections 45X, 45Y, and 48E tax credits (Section 30D was terminated September 30, 2025). The overlay distinguishes five flow states: qualifying, blocked, blocked-once-refined, ambiguous, and pending regulatory definition. A single blocked link disqualifies an entire battery value chain from US credit contribution.

**Host-State Resource Controls.** Export restrictions, state-partnership mandates, and resource-nationalism regimes imposed by producing jurisdictions. Covers Chile's National Lithium Strategy, Zimbabwe's escalating export bans (December 2022 ore ban → February 2026 concentrate ban → April 2026 quota system), and Namibia's 2023 controls. Zimbabwe's regime is visually flagged as actively tightening rather than stable.

**EU Critical Raw Materials Act.** The 2024 CRMA benchmarks and the 65% third-country ceiling that takes effect in 2030. Distinguishes strategic projects (expedited permitting), flows subject to the ceiling, and flows that illustrate CRMA vulnerability from the demand side — the Chile-to-Belgium corridor is rendered with a distinct marker after Umicore halved its 2025 cathode production.

**Chinese Ownership and Control.** A node-marker overlay — not a flow-color overlay. Tags every node majority-owned or controlled by a Chinese entity regardless of physical location. Tianqi Kwinana sits in Western Australia but is 51% Chinese-owned, which classifies it as a Specified Foreign Entity under OBBBA and blocks its output from US credit contribution. The same principle applies to BTR-style operations in Indonesia and Morocco, and to Chinese-controlled Zimbabwean mines.

---

## Data and methodology

The atlas is built on three datasets.

`data/nodes.json` contains 31 nodes across the three tiers: nine mining origins, ten refining operations (with plant-level detail for POSCO Gwangyang, Tianqi Kwinana, Albemarle Kemerton, AMG Bitterfeld, Nemaska Bécancour, and Naraha/Toyotsu where country-level shares below the top three are not publicly verifiable), and twelve cell manufacturing nodes. Each node carries a Chinese-ownership flag and a brief provenance note.

`data/flows.json` contains 31 bilateral flows with LCE-equivalent volumes, uncertainty bounds, source citations, and analytical context. Flows are tagged by tier transition (mining-to-refining, refining-to-cells, mining-to-cells-direct) and by primary policy state.

`data/policy_overlay.json` defines the four overlay regimes, their visual encoding rules, per-flow analytical tooltips, and per-node regulatory classifications.

Every volume estimate is anchored to a cited published figure or flagged as a bounded estimate. Where published data does not support country-level granularity — notably for refining shares below the top three producers — the atlas uses plant-level operational output rather than inventing percentages.

### Verification

The dataset went through an eight-query verification pass before release. Each query tested a specific volume estimate, ownership claim, or policy classification against primary sources. The pass surfaced material corrections to the initial build, including:

- A seven-fold overstatement of the Argentina-to-US lithium flow (48.8 kt LCE to 5.9 kt)
- Reclassification of Tianqi Kwinana from qualifying to blocked under OBBBA
- Correction of China's global refining share from 65% to 70% with redistribution implications
- Identification that POSCO Gwangyang produced under 10% of its nameplate capacity in 2024
- A shift of Rio Tinto-Arcadium classification from qualifying to ambiguous pending Treasury guidance on aggregate-ownership tests
- The net-importer context: China refined 70% of global chemicals in 2024 but imported 241 kt LCE of refined carbonate from Chile and Argentina

These and other findings are documented in the metadata sections of each dataset file.

### Primary sources

- US Geological Survey, Mineral Commodity Summaries 2025 and 2026 (lithium sheet)
- International Energy Agency, Global Critical Minerals Outlook 2025
- MIIT / CABIA National Lithium-Ion Battery Industry Operation Report, February 2025
- BloombergNEF 5th Global Lithium-Ion Battery Supply Chain Ranking, May 2025
- Cochilco and SUBREI lithium market reports 2024 and Q1 2025
- Australian Bureau of Statistics trade data (via Argus Media)
- Chinese General Administration of Customs (via Mysteel and SMM)
- IRS Notice 2026-15 and Notice 2025-42; OBBBA Public Law 119-21
- Regulation (EU) 2024/1252 on Critical Raw Materials
- Corporate filings and annual reports: Albemarle, SQM, Pilbara Minerals, IGO, Tianqi, POSCO, Rio Tinto, Arcadium, Ganfeng, Sigma Lithium, Sinomine, Zhejiang Huayou, Chengxin Lithium, Premier African Minerals, Umicore, CATL, LG Energy Solution, Samsung SDI, SK On, Panasonic, Northvolt, ACC, AMG Critical Materials

---

## Technical notes

The atlas is a single-file HTML application built on D3.js and TopoJSON, following the architecture of Market Access X's companion [rare earth supply chain atlas](https://github.com/marketaccess-x/rare-earth-supply-chain).

Rendering choices worth naming:

- **Orthographic globe** with drag-to-rotate, initial framing on the Pacific basin. Flat projections distort great-circle geometry in ways that misrepresent Atlantic and Pacific flows at scale.
- **Ring-stack nodes** where concentric rings encode per-tier presence. A country with mining, refining, and cells produces nested rings; a country with mining only produces a single ring.
- **Square-root volume scaling** on flow arc widths. Linear scaling compresses smaller flows to invisibility given the 650 kt intra-China versus 6 kt Korea-Korea spread.
- **Gradient treatment for blocked-once-refined flows.** Neutral grey at source fading to policy color at destination. The visual grammar says "legal at origin, blocked downstream" without text. This is the atlas's most distinctive rendering choice.
- **Self-loops render as pulsing rings around the node**, not as tiny orbital arcs. The China-to-China intra-flow is the single largest movement in the system and deserves visual weight proportional to its scale.
- **Dashed concentric ring** for the Chinese ownership node-marker overlay, with partial-ring variant for mixed operator sites like Hungary and Indonesia.

---

## Repository structure

```
lithium-supply-chain-atlas/
├── index.html              single-file interactive atlas
├── data/
│   ├── nodes.json          31 nodes across mining / refining / cell mfg tiers
│   ├── flows.json          31 bilateral flows with volume and policy tagging
│   └── policy_overlay.json four regulatory overlays with per-flow analytical notes
├── README.md               this file
└── LICENSE                 TBD
```

---

## About Market Access X

[Market Access X](https://marketaccess-x.com) publishes paid strategic analysis reports, interactive data visualizations, and free analytical articles on trade policy, geopolitics, and supply chain intelligence.

This atlas is free to use and share. Reports cited as context for specific overlays may be available for purchase on the site.

---

## Contact

[contact@marketaccess-x.com](mailto:contact@marketaccess-x.com)

---

*Built April 2026. Regulatory status as of April 22, 2026.*
