## Why

Users planning multi-stop outdoor activities in Taiwan currently have to consult fragmented official weather and air quality portals and manually reconcile forecasts, UV indices, precipitation probabilities, and pollutant levels against their personal comfort preferences and exposure levels. This project proposes the **Personalized Weather Assistant (PWA) AI Agent**—a hybrid rule-and-LLM architecture grounded in Central Weather Administration (CWA) and Ministry of Environment (MoENV) open data—and prepares an IEEE conference Overleaf LaTeX draft alongside version-controlled OpenSpec specifications for GitHub collaboration.

## What Changes

- Introduce multi-stop Taiwan itinerary parsing and spatial-temporal normalization (`ItinerarySegment`).
- Introduce official environmental data ingestion (CWA forecasts, MoENV real-time monitoring stations, and MoENV `AQF_P_01` air quality area forecasts) with strict temporal selection, freshness validation, and uncertainty flagging (`EnvironmentalSnapshot`).
- Introduce a deterministic, versioned rule decision engine that separates non-overridable official health protection from adjustable thermal/rain comfort preferences, paired with an LLM layer strictly bounded to structured field extraction, natural-language explanation, and provenance-grounded Q&A (`SegmentDecision`, `TripRecommendation`).
- Introduce explicit user preference management, reproducible decision snapshot logging, post-trip feedback collection, and confirmation-gated `PreferenceUpdateCandidate` workflows (`UserProfile`, `FeedbackRecord`).
- Produce an Overleaf-ready IEEE conference LaTeX draft (`IEEEtran` + `xeCJK`) converted from `物聯網_Final_Project_Proposal.pdf` with BibTeX references, TikZ architecture/flowchart diagrams, and pseudocode.

## Capabilities

### New Capabilities
- `itinerary-normalization`: Parse, validate, and normalize multi-stop Taiwan itineraries into townships, coordinates, air quality areas, time windows, activities, and outdoor exposure levels.
- `environmental-data-alignment`: Retrieve and spatio-temporally align CWA weather forecasts and MoENV real-time/next-day AQI data, enforcing data-quality flags (`missing`, `stale`, `spatially-coarse`, `unavailable`) without LLM fabrication.
- `hybrid-decision-engine`: Evaluate versioned domain rules (rain, UV, air quality, thermal comfort, data quality) with traceable `ruleId` evidence and bounded LLM natural-language explanation and Q&A.
- `preference-feedback-memory`: Manage explicit `UserProfile` preferences, persist decision snapshots, record `FeedbackRecord` surveys, and propose confirmation-gated `PreferenceUpdateCandidate` updates.
- `ieee-overleaf-draft`: Package a compilable Traditional Chinese IEEE conference Overleaf draft (`IEEEtran`, `xeCJK`, BibTeX, TikZ diagrams, and pseudocode) derived from the proposal PDF.

### Modified Capabilities
<!-- None: initial specification baseline for the repository. -->

## Impact

- **OpenSpec Artifacts**: `openspec/changes/personalized-weather-assistant/{proposal.md, design.md, tasks.md, specs/*/spec.md}`
- **Overleaf Package**: `overleaf_draft/` (`main.tex`, `references.bib`, `latexmkrc`, `README.md`) and `overleaf_ieee_draft.zip`
- **External Data Dependencies (Specified)**: Taiwan CWA Open Data API, Taiwan MoENV AQI Real-time & `AQF_P_01` Forecast APIs
- **Tooling & Workflows**: OpenSpec CLI (`1.2.0`), MCP tool-calling interface (planned for environmental data retrieval), Git/GitHub repository structure
