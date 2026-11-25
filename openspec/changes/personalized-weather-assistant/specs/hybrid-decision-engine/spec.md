## ADDED Requirements

### Requirement: Versioned deterministic rule engine with provenance
The system SHALL evaluate each `EnvironmentalSnapshot` and `ItinerarySegment` using a versioned rule engine covering five rule domains: rain preparation, UV protection, air quality protection, thermal comfort, and data quality. Every generated alert in `SegmentDecision` MUST attach `category`, `priority`, `recommendedAction`, `evidence` (`source fields`, `ruleId`, `profile factors`), and `limitations`.

#### Scenario: Identical AQI with different major pollutants
- **WHEN** two segments have identical AQI values where one is dominated by PM2.5 and the other by O3
- **THEN** the rule engine preserves `majorPollutant` in `EnvironmentalSnapshot` and generates pollutant-specific protective guidance traceable to distinct `ruleId` entries

#### Scenario: Critical data quality degradation
- **WHEN** `EnvironmentalSnapshot.qualityFlags` indicates missing, stale, or spatially unsuitable critical data
- **THEN** the rule engine invokes `createLimitedDecision` to downgrade or halt affected judgments and records explicit `limitations`

### Requirement: Separation of health protection and subjective comfort
Health protection alerts triggered by official air quality or UV thresholds MUST NOT be suppressed or deleted by user comfort preferences. User preferences (`UserProfile.explicitPreferences`) MAY adjust presentation priority and comfort-related thresholds (such as rain tolerance or thermal comfort) but SHALL keep objective environmental values invariant.

#### Scenario: User disables comfort alerts despite hazardous AQI or high UVI
- **WHEN** a user sets low sensitivity preferences or repeatedly reports feeling no discomfort during poor air quality
- **THEN** the system continues to emit official health protection alerts and places critical protection alerts ahead of comfort alerts during itinerary aggregation

### Requirement: Bounded LLM explanation and provenance-grounded Q&A
After `aggregateItineraryDecisions` produces `TripRecommendation`, the LLM SHALL only rewrite the structured record into layered natural-language summaries (Layer 1: primary action; Layer 2: concise rationale; Layer 3: expandable raw data, sources, valid periods, and limitations) and answer follow-up "why" questions strictly citing existing `SegmentDecision.evidence` and `limitations`. The LLM MUST NOT fetch unrecorded values, alter alert priorities, or fabricate conclusions for under-sourced periods.

#### Scenario: User asks why a specific recommendation was made
- **WHEN** the user asks a follow-up "why" question about a stop's recommendation
- **THEN** the LLM generates an explanation referencing only the persisted `SegmentDecision.evidence` (`ruleId`, source fields, profile factors) and `limitations` without altering the underlying decision
