# Brazil's Federal Fiscal Reports, Reproduced from Open Data

[Português](README.md) | **English**

This repository shows, with open code, that it is possible to **reproduce official figures from Brazil's federal public accounts using public data alone** — with no access to internal government systems.

This is not a theoretical proposal: the figures below match what the Federal Government officially publishes, at the unit in which those figures are released (thousands of Brazilian reais, R$ thousand).

> **Experimental / pilot project.** This is an exploratory proof of concept in fiscal transparency. The goal is to demonstrate the feasibility of the approach, not to replace the official statements — the authoritative source remains what the Federal Government publishes in the *Diário Oficial da União* (the Federal Official Gazette). What is demonstrated here is the **computational reproducibility** of the published figures: the normative interpretation of each criterion is that of the Treasury's *Manual de Demonstrativos Fiscais* (MDF) and remains open to discussion.

---

## Background for readers outside Brazil

Brazil's Fiscal Responsibility Law (*Lei de Responsabilidade Fiscal*, LC 101/2000) and the Federal Constitution require every level of government to publish two recurring fiscal statements:

| Report | Frequency | Legal basis |
|---|---|---|
| **RREO** — Summary Report on Budget Execution | every two months | Constitution art. 165 §3; LRF arts. 52–53 |
| **RGF** — Fiscal Management Report | every four months | LRF arts. 54–55 |

Their content and layout are not left to the producer. They are fixed, annex by annex and line by line, by the MDF, a normative manual issued by the National Treasury. The RGF also carries the ceilings that make the framework operative — personnel expenditure, consolidated net debt, guarantees, credit operations — all expressed as ratios of Net Current Revenue (RCL).

So the calculation rule is a legal norm, and the published figure is the result of applying that norm to budget execution data. Between the norm and the figure sits an implementation. That implementation is what this repository makes visible.

---

## 📄 Read the reports

Nothing to install, no programming required. Each link opens a page in the browser with the criteria explained, the calculation, and the check against the figure published in the Official Gazette.

| Statement | What it measures |
|---|---|
| [**RGPS** — RREO Annex 4](https://tesouro.github.io/relatorios-fiscais-lrf/rreo_a04_rgps.html) | Deficit of the general social security regime (INSS) |
| [**RCL** — RREO Annex 3](https://tesouro.github.io/relatorios-fiscais-lrf/rreo_a03_rcl.html) | Net Current Revenue, the yardstick for the fiscal ceilings |
| [**Health expenditure** — RREO Annex 12](https://tesouro.github.io/relatorios-fiscais-lrf/rreo_a12_saude.html) | Whether the Union met the constitutional minimum for health |
| [**Personnel expenditure** — RGF Annex 1](https://tesouro.github.io/relatorios-fiscais-lrf/rgf_a01_pessoal.html) | Consolidated payroll spending (accrued/*liquidada* portion) |
| [**Personnel by expenditure element** — RREO Table 2](https://tesouro.github.io/relatorios-fiscais-lrf/rreo_t02_pessoal.html) | Payroll broken down by type of expenditure |

The rest of this document explains how those figures were obtained and why it matters.

---

## Why this is different

In the search we conducted, we did not identify a government initiative — in Brazil or abroad — that publishes **together** the public source data, the reconstruction code and an explicit reconciliation, sufficient to reproduce its own official fiscal statements. If one exists, we would like to hear about it.

There are neighbouring initiatives with different purposes. It is worth setting out how each of them differs, because each is instructive in its own right.

### Rules as Code: Catala and OpenFisca

[**Catala**](https://catala-lang.org) (Inria, France) is the closest *conceptual* relative to this project's approach. It is a domain-specific language built for socio-fiscal legislative literate programming: each paragraph of statutory text is annotated with the code that formalises it, and the compiler either extracts the executable program (*tangling*) or produces a lawyer-readable document (*weaving*). It was designed together with legal scholars, embeds *default logic* — the general-rule-with-exceptions structure that pervades statutory drafting — as a first-class language feature, and the correctness of its core compilation steps has been formally proved using the F\* proof assistant. It grew out of technology-transfer work with the DGFiP on the French income tax computation. Formalising French family benefits in Catala surfaced a bug in the official implementation.

Catala is a far more rigorous instrument than what we use here. Quarto is not a verified compiler and R is not a legal DSL — the *Manual de Demonstrativos Fiscais* has the same general-rule-with-exceptions structure, but we handle it with ordinary conditional logic, and we make no claim of formal equivalence. The premise, however, is shared, and worth stating plainly: **the faithfulness between rule and code should be readable by the domain expert, not taken on trust from the implementer.**

Where we differ is in the object. Catala formalises *substantive law* — who is entitled to what — in order to compute an outcome for a given case. This project operationalises *public finance rules of a procedural nature*: the MDF's rules of aggregation and reporting, applied to budget execution that has already occurred, in order to reconstruct an aggregate statement that has already been published.

[**OpenFisca**](https://openfisca.org), also French in origin and now used across several continents, sits on the same axis: it encodes *legislation* in order to *simulate* policy, not to reproduce published statements from actual execution.

### Reproducible Analytical Pipelines (United Kingdom)

The closest *operational* relative. Developed in 2017 by the Department for Culture, Media and Sport with the Government Digital Service, and since promoted across the Government Statistical Service, RAP was a response to a problem we recognise exactly: official statistics produced through slow, error-prone manual processes, with heavy reliance on spreadsheets and proprietary tools. Its core principles — minimising manual steps, open-source tools, version control as an audit trail, embedded documentation and quality assurance, peer review — are closely aligned with the standard this repository was built to meet.

The difference in scope is worth being precise about, because it is narrow. RAP addresses how a producer builds *its own* pipeline reproducibly, and several UK statistical outputs do publish that pipeline's code. What is added here is the second half — what we will call, throughout this document, **independent reconstruction**:

- **Reproducibility (RAP):** the same data, the same code and the same pipeline yield the same result.
- **Independent reconstruction (here):** public execution data, plus the normative criteria, plus an implementation written from scratch, yield the result the government published.

The second does not depend on reproducing the producer's internal pipeline at all. It is closer to an independent computational audit than to the replication of an analysis.

One feature of the Brazilian arrangement sharpens this. The data sources — Siga Brasil, maintained by the Federal Senate, and SOF, in the Ministry of Planning — are institutionally distinct from the body that publishes the statement, the National Treasury in the Ministry of Finance. The reconstruction therefore crosses an institutional boundary, and the reconciliation is in effect a consistency test between what one institution discloses and what another publishes.

The UK experience is also instructive about the limits of technology on its own. The Office for Statistics Regulation's 2021 review is largely about barriers to adoption — tools, training, time, capability, organisational support — rather than about the approach itself.

### Reproducibility in official statistics (France)

INSEE has invested in the same direction from the statistical side: [utilitR](https://www.utilitr.org), a collaborative R documentation written by and for public statisticians, and the [SSP Cloud](https://www.sspcloud.fr), a containerised open platform designed to make statistical work replayable and shareable rather than merely documented.

### Data access packages (Brazil)

Packages in the Brazilian R ecosystem — `tesouror`, `siconfir`, `RREORGFdataR` — give convenient access to the SICONFI APIs and deliver the statement **already consolidated, at annex line level, as the reporting entity declared it**. They are excellent for comparing entities, but they start from the result: the rule that turned budget execution into that figure stays out of reach. Open-data portals do the reverse — they publish the execution without the logic that converts it into a statement.

### The gap we address

The slice of ground between the two: starting from public execution data and arriving, with open and auditable code, at exactly the figure the Treasury published.

**And what is here is a sample.** An internal pipeline already exists, in Microsoft Fabric, generating every RREO and RGF annex with this same approach — each notebook documented and reconciled against the Official Gazette. The five statements below are the ones today's public data allows us to reproduce; the rest is already built, waiting only for public data at the necessary granularity.

---

## 1. Starting point: the RGPS case

On 30 January 2026, *Jornal Nacional* — Brazil's main evening television news programme — reported that the **General Social Security Regime (RGPS)**, the pension system for private-sector workers managed by the INSS, ran a 2025 deficit of **more than R$ 320 billion**.

We reproduced that figure using only two public sources, with no access to any internal government system:

- **Siga Brasil** (Federal Senate) — collected revenue
- **SOF** (Federal Budget Secretariat) — committed expenditure

| Item | Calculated (R$ thousand) | Official (R$ thousand) | Difference |
|---|---:|---:|---:|
| Total revenue | 709,399,406 | 709,399,406 | 0.00% |
| Total expenditure | 1,030,366,445 | 1,030,366,445 | 0.00% |
| **Social security balance** | **−320,967,039** | **−320,967,039** | **0.00%** |

The difference is measured after rounding to R$ thousand, the unit in which the Official Gazette publishes. Before rounding, sub-thousand fractions remain — a few hundred reais against figures in the hundreds of billions — the floating-point noise of the source databases.

The result matches what the National Treasury published officially: the same figure that reached the evening news. Anyone, with no internal access whatsoever, can arrive at the same result — it is a matter of knowing where to find the data and how to apply the right criteria.

**Where it lives in the repository:**

- Executable code: [`standalones/rreo_a04_rgps.qmd`](standalones/rreo_a04_rgps.qmd) — a Quarto document holding narrative and calculation in one file (`quarto render`)
- Read directly in the browser: [rendered HTML](https://tesouro.github.io/relatorios-fiscais-lrf/rreo_a04_rgps.html)
- Input data: `dados/rgps/receita_siga.xlsx` and `dados/rgps/despesa_sof.xlsx`
- Official statement for reconciliation: [RREO — December 2025](https://www.tesourotransparente.gov.br/publicacoes/relatorio-resumido-da-execucao-orcamentaria-rreo/2025/12), Annex 4

**Why it matters:** citizens, journalists, members of parliament — anyone — can audit this account independently of the government. Confidence in the figure is strengthened because it can be verified from outside.

---

## 2. From a single case to five official statements

If the RGPS could be reproduced, what else can be, from public data?

We tested and validated **four further statements required by the Fiscal Responsibility Law**, converging with the figures published in the Official Gazette (financial year 2025) at the unit in which they are released — with one residual rounding difference in Table 2, of the order of R$ 4 thousand against R$ 431.7 billion, documented in the file itself.

| Statement | What it measures | Read online | Official source |
|---|---|---|---|
| **RGPS** (RREO Annex 4) | INSS pension deficit | [open](https://tesouro.github.io/relatorios-fiscais-lrf/rreo_a04_rgps.html) | RREO Dec/2025 |
| **RCL** (RREO Annex 3) | Net Current Revenue — the yardstick for fiscal limits | [open](https://tesouro.github.io/relatorios-fiscais-lrf/rreo_a03_rcl.html) | RREO Dec/2025 |
| **Health expenditure** (RREO Annex 12) | Whether the constitutional minimum for health was met | [open](https://tesouro.github.io/relatorios-fiscais-lrf/rreo_a12_saude.html) | RREO Dec/2025 |
| **Personnel expenditure** (RGF Annex 1, consolidated total) | Payroll spending across the three branches — measured here on the **accrued (*liquidada*) portion only**, excluding unprocessed carry-over commitments ([why](https://tesouro.github.io/relatorios-fiscais-lrf/rgf_a01_pessoal.html#nota-metodológica)) | [open](https://tesouro.github.io/relatorios-fiscais-lrf/rgf_a01_pessoal.html) | RGF 2025 — Consolidated |
| **Personnel by expenditure element** (RREO Table 2) | Payroll broken down by type of expenditure | [open](https://tesouro.github.io/relatorios-fiscais-lrf/rreo_t02_pessoal.html) | RREO Dec/2025 |

Official sources: [RREO December 2025](https://www.tesourotransparente.gov.br/publicacoes/relatorio-resumido-da-execucao-orcamentaria-rreo/2025/12) · [RGF 2025 Consolidated](https://www.tesourotransparente.gov.br/publicacoes/relatorio-de-gestao-fiscal-rgf/2025/31)

Each statement is a self-contained Quarto document in `standalones/`: the narrative of the normative criteria and the code that implements them live in the same file, end to end, with no need to navigate elsewhere in the repository.

### Known limitations

Each limitation below is already documented in the corresponding file. They are gathered here for quick reading — and because a project that states where public data runs into internal accounting is more useful than one that only shows what tied out.

- **Personnel expenditure (RGF Annex 1) — partial scope.** Only *accrued* (*liquidada*) expenditure is measured. Unprocessed carry-over commitments (*Restos a Pagar Não Processados*), which form part of personnel expenditure for the purposes of the LRF ceiling, are not included; because of that exclusion the base measured here is narrower than *Despesa Total com Pessoal* (DTP) as defined for the ceiling, and we therefore present neither the DTP/RCL ratio nor the compliance assessment under LRF arts. 19 and 20. The comparison is of values, against the accrued portion published in the Official Gazette. The carry-over data is available in the public base: the extension is immediate from the same pipeline.
- **Personnel by element (Table 2) — agency hierarchy in the Ministry of Defence.** The official calculation separates civilian from military staff by the top-level agency of the executing spending unit. For 45 of the Union's 49 top-level agencies this makes no difference, because top-level and superior agency coincide. In the Ministry of Defence they do not: the MD is the top-level agency, while the Navy, Army and Air Force Commands are superior agencies. The public base exposes the superior agency, and it is from this asymmetry — confined to the defence perimeter — that a ~0.06% difference arises in two items on the civilian/military boundary. The grand total matches.
- **Health expenditure (Annex 12) — single-dimension criterion.** The calculation rests on use identifier `6`, a flag assigned by SIAFI itself to the expenditure. It is the criterion of the official statement and it is traceable, but public data does not currently offer a second independent dimension to verify the flag record by record.
- **Exhaustiveness of classifications — not automatically tested.** The rules reproduce the official values, but the repository does not yet demonstrate, in an automated way, that the classification covers 100% of the base and that the criteria are mutually exclusive. Matching the official figure is strong evidence of computational correctness; it is not, by itself, proof that each classificatory premise is the only possible reading of the MDF. Auditing the rules — not merely the numbers — is the next step, and it is the kind of discussion this repository exists to enable.
- **Period and unit.** Financial year 2025, closed. The official values used for reconciliation are recorded in the body of each document; comparison is made in R$ thousand, the unit in which the Official Gazette publishes.

**A finding of the project: a single expenditure base covers every statement tested.**

- `dados/despesa_unificada.csv.gz` — one expenditure base (Siga Brasil), with no monthly breakdown — feeds **health expenditure, personnel expenditure, personnel by element, and the RCL deductions**. For a closed financial year the annual accumulated balance is sufficient; there is no need to organise data into monthly folders.
- `dados/rcl_receita.csv.gz` — the **RCL** starts from current revenue, so it needs its own revenue base. Its only deduction measured on the expenditure side (constitutional transfers) comes from the **same unified base** as the other three, which now also includes Programme and cancelled carry-over commitments.
- The **RGPS** (case 1 above) was the first statement tested, still with separate sources: revenue from Siga Brasil and expenditure from the SOF open data portal. Later findings showed it can also be generated from the unified base — we kept the original extractions (`dados/rgps/`) as a record of the path taken and in acknowledgement of SOF, an important open data source.

In short: there is no need to build new data infrastructure for each report — a few well-constructed public bases already cover several statements at once.

**Two lessons that carry beyond this project:**

- **Publishing data is not the same as guaranteeing reproducibility.** Siga Brasil and SOF already exist and are already public — what was missing was not the data, but the calculation logic: the criteria, the filters, the exceptions. All of it is in this repository, documented and auditable.
- **Open code makes every criterion inspectable.** Anyone can follow how a figure was calculated, line by line, from raw data to published value.

**Where it lives:** the [`standalones/`](standalones/) folder — one Quarto document per statement.

---

## 3. The full engine: what is already built

The five statements above are what today's public data allows us to reproduce. The internal pipeline goes considerably further.

There are **17 R notebooks**, running on Microsoft Fabric over SparkR and Delta Lake, covering the full set of RREO and RGF annexes currently prepared by the National Treasury:

- **RREO** — Annexes 1, 2, 3, 4, 6, 7, 8, 9 and 12, and Tables 1, 2 and 3
- **RGF** — Annexes 1 to 5

Each notebook follows the same approach as this repository: the MDF criterion written in plain Portuguese, the code that executes it immediately below, and the reconciliation against the figure published in the Official Gazette.

And not only the calculation. On the same base run the **Power BI (Direct Lake)** semantic model, **cross-validation between RREO and RGF**, the construction of **historical series**, and data sharing with other Treasury units — all from a single source, with no re-keying into spreadsheets.

**Today's constraint is not the code, it is the granularity of public data.** The annexes absent from this repository depend on attributes available only in Tesouro Gerencial, the internal reporting system — among others: general ledger account balances, current-account detail by entity, accounting event status, and funding source by ledger account. As data at that granularity becomes publicly available, the corresponding calculation logic is already built and tested.

That pipeline runs on Tesouro Gerencial extractions, which are not public — which is why it is not part of this repository, whose scope is deliberately what anyone can reproduce with open data. This section serves architectural context only: because it depends on non-public data, it is not verifiable by anyone without Tesouro Gerencial access, and nothing asserted here is a premise of the five statements above.

---

## Repository structure

```
relatorios-fiscais-lrf/
├── standalones/                   # one Quarto document per statement
│   ├── rreo_a04_rgps.qmd          # RGPS — the evening-news case
│   ├── rreo_a03_rcl.qmd           # RCL — every LRF deduction explained
│   ├── rreo_a12_saude.qmd         # Health expenditure (constitutional minimum)
│   ├── rgf_a01_pessoal.qmd        # Personnel expenditure — rules one by one
│   └── rreo_t02_pessoal.qmd       # Personnel by element — civilian vs. military
├── dados/                         # public data, versioned alongside the code
│   ├── despesa_unificada.csv.gz   # unified 2025 expenditure base
│   ├── rcl_receita.csv.gz         # 2025 revenue (RCL)
│   └── rgps/                      # RGPS-specific bases (Siga Brasil + SOF)
├── docs/                          # rendered HTML, published on GitHub Pages
├── LICENSE
├── README.md                      # Portuguese version
└── README.en.md                   # this document
```

---

## How to reproduce

Requires R (≥ 4.1) and [Quarto](https://quarto.org).

```r
install.packages(c("dplyr", "stringr", "tidyr", "readxl", "knitr"))
```

**One statement:**
```sh
quarto render standalones/rreo_a04_rgps.qmd
```

**All at once:**
```sh
quarto render standalones/
```

Or, in RStudio, open any `.qmd` and click **Render**.

The documents locate the `dados/` folder on their own, so they work without configuring paths or calling `setwd()`. No credentials, internal access or government system is required.

**To read without running anything:** the links at the [top of this document](#-read-the-reports) open the rendered version of each statement — narrative, code, result and the line-by-line reconciliation against the Official Gazette, on a single page. The same files are in [`docs/`](docs/) if you prefer to download them.

---

## Methodology

The project follows the principles of **literate programming** (Knuth, 1984): each statement describes the normative criteria (the Treasury's *Manual de Demonstrativos Fiscais*) in natural language and implements them in verifiable R code — the methodology is explicit, not just the final result.

The executable **Quarto** documents (`.qmd`) are the fullest expression of that idea: they hold the explanation of each criterion and the code that executes it in the same file, written so that an accountant with no programming experience can follow every step. All five statements have a `.qmd` version: RGPS (`rreo_a04_rgps.qmd`), RCL (`rreo_a03_rcl.qmd`, with every LRF deduction explained), health expenditure (`rreo_a12_saude.qmd`), personnel expenditure (`rgf_a01_pessoal.qmd`, with the inclusion and exclusion rules opened one by one) and personnel by element (`rreo_t02_pessoal.qmd`, with the civilian/military split explained). Each file is self-sufficient: it reads the data, applies the criteria, presents the result and reconciles it against the figure published in the Official Gazette.

**A note on language.** The five statements and their rendered pages are written in Portuguese, and deliberately so. Each one quotes the criteria of the *Manual de Demonstrativos Fiscais*, which is normative in Portuguese; translating those passages would insert a layer of interpretation at exactly the point where the project promises fidelity to the norm. This document is the conceptual entry point for readers who do not read Portuguese; the code itself is legible without it, and the reconciliation tables are numeric.

---

## Attribution and licence

Developed at the *Coordenação-Geral de Contabilidade da União* (CCONT) · *Gerência de Informações Contábeis e Fiscais da União* (GEINF) · Secretaria do Tesouro Nacional, Brazil.

Code under the [MIT licence](LICENSE). Public data, freely usable.

Comments, corrections and reuse are welcome — please open an issue.

---

## References

- Merigoux, D., Chataing, N. & Protzenko, J. (2021). *Catala: A Programming Language for the Law.* Proc. ACM Program. Lang. 5, ICFP. <https://arxiv.org/abs/2103.03198>
- Catala project — <https://catala-lang.org> · reference book: <https://book.catala-lang.org>
- Inria, *Catala translates law into code for more reliable administration* — <https://www.inria.fr/en/catala-software-dgfip-cnaf>
- OpenFisca — <https://openfisca.org>
- UK Government Analysis Function, *Reproducible Analytical Pipelines* — <https://analysisfunction.civilservice.gov.uk>
- Office for Statistics Regulation (2021), *Reproducible Analytical Pipelines: Overcoming barriers to adoption* — <https://osr.statisticsauthority.gov.uk>
- utilitR — <https://www.utilitr.org> · SSP Cloud — <https://www.sspcloud.fr>
- Knuth, D. E. (1984). *Literate Programming.* The Computer Journal, 27(2), 97–111.
- Brazil, Lei Complementar nº 101/2000 (LRF) and *Manual de Demonstrativos Fiscais* (STN)
