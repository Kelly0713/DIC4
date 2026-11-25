## ADDED Requirements

### Requirement: Official multi-source data retrieval
For each normalized `ItinerarySegment`, the system SHALL retrieve weather forecasts from Taiwan's Central Weather Administration (CWA) Open Data (temperature, relative humidity, precipitation probability, UV index, and weather description) and air quality data from Taiwan's Ministry of Environment (MoENV) Open Data (AQI, major pollutant, and pollutant values).

#### Scenario: Retrieving aligned weather and air quality metadata
- **WHEN** a normalized segment is queried against CWA and MoENV sources
- **THEN** the system records `sourceType`, `sourceName`, `spatialReference` (`township`, `station`, or `air-quality-area`), `observedOrForecastTime`, `publishedAt`, and `fetchedAt` in `EnvironmentalSnapshot`

### Requirement: Temporal selection rules for air quality sources
The system SHALL select air quality sources strictly based on the segment's time window relative to official data availability:
- Current or in-progress segments SHALL use the nearest available MoENV real-time monitoring station.
- Future segments within the official next-day forecast horizon SHALL use the MoENV `AQF_P_01` air-quality-area forecast.
- Future segments beyond the official air quality forecast horizon SHALL still receive CWA weather guidance while displaying "尚無官方預報" (no official forecast) for air quality without generating an air-quality risk judgment.

#### Scenario: Current segment uses real-time monitoring station
- **WHEN** an itinerary segment is currently in progress
- **THEN** the system binds the nearest MoENV station measurement, displaying station name, distance, `publishedAt`, and `majorPollutant`

#### Scenario: Multi-day future segment exceeds AQF_P_01 horizon
- **WHEN** an itinerary segment falls beyond the MoENV `AQF_P_01` next-day forecast horizon
- **THEN** the system still evaluates CWA weather rules, marks air quality as `unavailable` ("尚無官方預報"), and suppresses air-quality risk conclusions

### Requirement: Strict data quality and non-fabrication policy
When official environmental data is missing, stale, spatially coarse, or temporally misaligned, the system SHALL populate `qualityFlags` (`missing`, `stale`, `spatially-coarse`, `unavailable`) and MUST NOT fabricate, extrapolate, or allow the LLM to fill in missing values or present real-time observations as future forecasts.

#### Scenario: Stale air quality data relative to updated weather forecast
- **WHEN** CWA weather forecast is current but MoENV station data is stale (e.g., six hours old)
- **THEN** the system flags the snapshot with `stale`, surfaces the limitation in output, and restricts air-quality conclusions accordingly
