## ADDED Requirements

### Requirement: Multi-stop Taiwan itinerary validation and normalization
The system SHALL validate every segment in a user-submitted itinerary and normalize valid segments into an `ItinerarySegment` containing `segmentId`, `itineraryId`, `placeName`, `latitude`, `longitude`, `township`, `airQualityArea`, `startTime`, `endTime`, `activityType`, and `outdoorExposureLevel`.

#### Scenario: Complete multi-stop Taiwan itinerary
- **WHEN** a user submits a multi-stop itinerary with valid Taiwan locations, start/end time windows, activity types, and outdoor exposure levels
- **THEN** the system produces normalized `ItinerarySegment` records mapped to their corresponding Taiwan township, geographic coordinates, and air quality area without computing inter-stop transit routes

#### Scenario: Missing or invalid segment fields
- **WHEN** any itinerary segment lacks a valid Taiwan location, valid time window, or activity type
- **THEN** the system rejects normalization for the incomplete input and returns a prompt requesting the missing or invalid fields

### Requirement: Natural-language supplement parsing with confirmation
When the user provides natural-language itinerary descriptions, the LLM parser SHALL extract candidate structured fields and MUST prompt the user to confirm uncertain fields before finalizing `ItinerarySegment` records.

#### Scenario: Ambiguous natural-language time or exposure level
- **WHEN** a user enters free-text input such as an unspecified outdoor duration or ambiguous district name
- **THEN** the system extracts candidate structured fields and requests explicit user confirmation on uncertain slots before proceeding to data retrieval
