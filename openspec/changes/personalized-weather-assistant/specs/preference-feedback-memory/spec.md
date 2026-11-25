## ADDED Requirements

### Requirement: Segregated storage of profile, trip history, and feedback
The system SHALL maintain three separate data stores:
1. `UserProfile` storing `userId`, `explicitPreferences` (`rainTolerance`, `sunProtectionPriority`, `airQualityAlertSensitivity`, `thermalPreference`), `confirmedPreferenceVersion`, `createdAt`, and `updatedAt`.
2. Persisted trip records storing the user's itinerary input, `EnvironmentalSnapshot` at decision time, and `TripRecommendation` for reproducibility and auditability.
3. `FeedbackRecord` storing `itineraryId`, `segmentId`, `submittedAt`, `perceivedWeatherMatch`, `perceivedAirQualityMatch`, `recommendationUsefulness`, and `freeText`.

#### Scenario: Reproducing a historical recommendation
- **WHEN** an evaluation or audit inspects a past `TripRecommendation`
- **THEN** the exact decision can be reproduced from the persisted `ItinerarySegment`, `EnvironmentalSnapshot`, `UserProfile` version, and rule engine version

### Requirement: Confirmation-gated preference updates
Aggregated `FeedbackRecord` entries MAY generate a `PreferenceUpdateCandidate` (`preferenceKey`, `proposedValue`, `reason`, `evidenceSummary`, `status: pending-confirmation`), but the system MUST NOT overwrite `UserProfile` automatically without explicit user confirmation.

#### Scenario: User accepts or rejects a proposed preference adjustment
- **WHEN** the system presents a `PreferenceUpdateCandidate` with status `pending-confirmation`
- **THEN** accepting transitions status to `accepted` and increments `confirmedPreferenceVersion` in `UserProfile`, whereas rejecting transitions status to `rejected` and leaves `UserProfile` unchanged
