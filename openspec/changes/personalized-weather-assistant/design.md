## Context

This repository originates from the IoT Final Project Proposal (`物聯網_Final_Project_Proposal.pdf`), which proposes a **Personalized Weather Assistant (PWA) AI Agent** for multi-stop itineraries across Taiwan. Existing government platforms—Taiwan's Central Weather Administration (CWA) and Ministry of Environment (MoENV)—expose accurate meteorological and air quality data separately, forcing users to manually aggregate forecasts, UV indices, precipitation probabilities, and pollutant indicators and interpret them against personal sensitivity and activity exposure.

The assignment requires two coordinated deliverables:
1. **OpenSpec on GitHub**: Full spec-driven architecture and behavioral contracts under `openspec/` ready for version-controlled GitHub collaboration.
2. **Overleaf Draft for IEEE Conference**: A compilable Traditional Chinese IEEE conference manuscript (`overleaf_draft/`) based on the proposal PDF.

## Goals / Non-Goals

**Goals:**
- Formalize the 11-module hybrid architecture, 5 core data models (`UserProfile`, `ItinerarySegment`, `EnvironmentalSnapshot`, `SegmentDecision`, `FeedbackRecord`, `PreferenceUpdateCandidate`), and spatio-temporal alignment rules from the proposal into testable OpenSpec specifications.
- Enforce strict separation between deterministic official health-protection rules and customizable subjective comfort preferences.
- Constrain LLM responsibilities strictly to natural-language slot parsing, structured decision summarization, and provenance-grounded Q&A (`SegmentDecision.evidence` and `limitations`).
- Provide a clean, Overleaf-ready IEEE conference LaTeX package (`IEEEtran` + `xeCJK`) including BibTeX citations (`[1]`–`[12]`), TikZ system architecture and flowchart diagrams, and `algorithm2e`/listing pseudocode.

**Non-Goals:**
- Implementing live production CWA/MoENV API clients or deploying a live web server in this milestone (this milestone establishes the OpenSpec contracts and IEEE conference Overleaf draft).
- Evaluating transit routes between stops; only user-specified stationary/activity itinerary segments are assessed.
- Automatically modifying `UserProfile` from implicit user feedback without explicit user confirmation.

## Decisions

1. **Hybrid Rule-Engine + Bounded LLM Architecture (instead of End-to-End Autonomous LLM)**
   - *Rationale*: As analyzed in `WXImpactBench` [6] and the proposal's core design trade-offs (Debate 1), LLMs excel at natural-language intent parsing and human-friendly explanations, whereas numerical threshold checks, official protective guidelines, and reproducibility require deterministic, versioned rules (`ruleId`).
   - *Alternative Considered*: Pure LLM tool-calling or fixed rule-plus-template (`Baseline B`). The hybrid approach preserves auditability while enabling multi-turn follow-up Q&A, and defines `Baseline A` (raw data) and `Baseline B` (fixed templates) as experimental baselines.

2. **Strict Separation of Health Protection and Subjective Comfort**
   - *Rationale*: Low subjective discomfort does not imply zero health risk (e.g., short-term low-level PM2.5 exposure [9] or high UVI [10]). User preferences may adjust presentation priority or comfort thresholds (rain tolerance, thermal preference) but MUST NOT suppress official safety alerts triggered by AQI or UVI thresholds.

3. **Confirmation-Gated Preference Updates via `PreferenceUpdateCandidate`**
   - *Rationale*: Single feedback events can confound clothing, transient exertion, and baseline tolerance. Aggregated `FeedbackRecord` entries generate a `PreferenceUpdateCandidate` (`pending-confirmation`), requiring explicit user acceptance before creating a new `confirmedPreferenceVersion` in `UserProfile`.

4. **MCP-Compatible Environmental Data Abstraction**
   - *Rationale*: Modeling CWA and MoENV retrieval behind structured tool interfaces (aligned with MCP-style tool calling [2]) decouples spatio-temporal alignment and quality checking from LLM orchestration while preserving metadata provenance (`sourceType`, `sourceName`, `publishedAt`, `fetchedAt`).

5. **IEEE Conference LaTeX Format with `xeCJK` (`IEEEtran`)**
   - *Rationale*: Overleaf natively supports `XeLaTeX` via `latexmkrc` (`$pdf_mode = 5;`), allowing `\documentclass[conference]{IEEEtran}` to typeset Traditional Chinese cleanly alongside IEEE two-column formatting, TikZ diagrams, and BibTeX references.

## Risks / Trade-offs

- **Overleaf Compiler Selection** → Traditional Chinese `xeCJK` requires XeLaTeX rather than pdfLaTeX. *Mitigation*: Include a `latexmkrc` file (`$pdf_mode = 5;`) and explicit instructions in `overleaf_draft/README.md` so Overleaf builds cleanly.
- **No Local LaTeX Toolchain Installed** → Local `pdflatex`/`xelatex` binaries are absent on the current workstation, so compilation cannot be executed locally. *Mitigation*: Keep LaTeX preamble and packages strictly standard in TeX Live / Overleaf (`IEEEtran`, `xeCJK`, `ctex` fallback notes, `tikz`, `algorithm2e`/`listings`, `booktabs`, `hyperref`) and document verification steps.
- **Spatial/Temporal Mismatch Across Official Sources** → CWA forecasts operate at township level while MoENV real-time data is station-based and `AQF_P_01` is air-quality-area-based. *Mitigation*: Require explicit `spatialReference` and `qualityFlags` propagation and prohibit interpolation or LLM fabrication when data is stale or out of forecast horizon.
