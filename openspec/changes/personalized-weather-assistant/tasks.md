## 1. OpenSpec & GitHub Repository Setup

- [x] 1.1 Initialize OpenSpec (`openspec init --tools antigravity`) in workspace
- [x] 1.2 Author `proposal.md`, `design.md`, and capability specs (`itinerary-normalization`, `environmental-data-alignment`, `hybrid-decision-engine`, `preference-feedback-memory`, `ieee-overleaf-draft`)
- [x] 1.3 Configure `.gitignore` and repository `README.md` for GitHub collaboration

## 2. IEEE Conference Overleaf Draft (`overleaf_draft/`)

- [x] 2.1 Configure `IEEEtran` two-column LaTeX template with `xeCJK` and `latexmkrc` (`$pdf_mode = 5;`) for Traditional Chinese compilation on Overleaf
- [x] 2.2 Transcribe all 12 proposal references into `references.bib` with DOIs and URLs
- [x] 2.3 Author `main.tex` covering Abstract, Introduction, Related Work & Research Gap, Proposed Hybrid Architecture (with TikZ diagram, data models, Algorithm 1 pseudocode), and Evaluation Design
- [x] 2.4 Package `overleaf_ieee_draft.zip` ready for direct Overleaf import

## 3. Itinerary Normalization & Data Alignment Implementation (Planned)

- [ ] 3.1 Implement Taiwan location, township, coordinate, and air-quality-area resolver for `ItinerarySegment`
- [ ] 3.2 Implement CWA weather forecast and MoENV real-time / `AQF_P_01` forecast data adapters (MCP tool interface)
- [ ] 3.3 Implement spatio-temporal alignment and `qualityFlags` (`missing`, `stale`, `spatially-coarse`, `unavailable`) validator

## 4. Hybrid Rule Engine & Bounded LLM Layer (Planned)

- [ ] 4.1 Implement versioned rule engine (`rain`, `uv`, `air-quality`, `thermal-comfort`, `data-quality`) outputting `ruleId` and `evidence`
- [ ] 4.2 Implement `aggregateItineraryDecisions` and `prioritizeCriticalProtectionBeforeComfort` ensuring health alerts cannot be suppressed
- [ ] 4.3 Implement bounded LLM modules for slot parsing confirmation, layered summary generation, and provenance-grounded Q&A

## 5. Profile, History & Feedback Workflow (Planned)

- [ ] 5.1 Implement segregated storage for `UserProfile`, decision snapshots, and `FeedbackRecord`
- [ ] 5.2 Implement `PreferenceUpdateCandidate` generation and explicit user confirmation flow

## 6. Evaluation & Benchmarking (Planned)

- [ ] 6.1 Implement `Baseline A` (raw official data) and `Baseline B` (fixed rules + templates) comparators
- [ ] 6.2 Execute 6 evaluation scenarios (multi-stop, rain cost trade-off, high UV, identical AQI / different major pollutants, station-to-forecast transition, stale/missing data) and collect user metrics
