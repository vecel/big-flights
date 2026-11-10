# Data Sources
This document provides research and overview of data sources intended to use in project.

## Summary table

| Data Source | Description | Request | Response |
| :--- | :--- | :--- | :--- |
| **OpenSky - REST API** | Fetches live flight data and vector states (e.g., flight ID, position, velocity) for planes in a specified bounding box. | HTTP request to `/states/all`. | JSON containing a `time` field and a `states` array of vector states. |
| **OpenSky - Historical Data** | Provides historical data regarding flight vector states. | Trino request to `https://trino.opensky-network.org`. | Trino JSON document with state vector data batches and a `nextUri` for subsequent batches. |
| **Open-Meteo - Weather API** | Fetches live and forecast weather data globally using various high-resolution models (e.g., ICON, ECMWF). | HTTP GET request to `/v1/forecast`. | JSON containing hourly/daily variable arrays and time vectors (CSV/XLSX also supported). |
| **Open-Meteo - Historical Weather** | Provides historical weather and climate reanalysis data globally, dating back to 1940. | HTTP GET request to `/v1/archive`. | JSON containing historical hourly/daily weather data variables. |
| **OpenAQ - REST API** | Fetches global air quality data (PM2.5, PM10, O3, NO2, etc.) from reference-grade and low-cost sensors worldwide. | HTTP GET request to endpoints like `/v3/measurements`. | JSON containing location metadata, parameters, and time-series measurement data. |
## Notes & limitations

**OpenSky - REST API**
* Requires an account for 4,000 daily credits via OAuth2.
* Tokens expire every 30 minutes and must be refreshed.
* Data has a 5-second time resolution and costs 4 credits per call, which can be reduced by using a smaller bounding box.
* Usage requires an academic citation.
* State vector data structure: https://openskynetwork.github.io/opensky-api/rest.html#all-state-vectors.
* Token refreshment automatization: https://openskynetwork.github.io/opensky-api/rest.html#authentication.
* Credits pricing: https://openskynetwork.github.io/opensky-api/rest.html#api-credits.
* Citation: https://openskynetwork.github.io/opensky-api/.

**OpenSky - Historical Data**
* Requires a verified university-affiliated research account.
* Strict performance guidelines apply to avoid a permanent ban. 
* Filtering by partition column is mandatory.
* Bulk downloads are strictly prohibited.
* Queries can be executed via Python and streamlined to NiFi.
* Abstract Trino response: https://trino.io/docs/current/develop/client-protocol.
* State vectors structure (differs slightly from API): https://openskynetwork.github.io/opensky-api/trino.html#state-vectors.
* Very important performance guidelines: https://openskynetwork.github.io/opensky-api/trino.html#tools-performance-considerations.

**Open-Meteo - Weather API**
- Free for non-commercial use up to 10,000 API calls per day.
- No API key or sign-up is required for the free tier.
- Minute and hourly rate limits apply: maximum 600 calls/minute and 5,000 calls/hour.
- Rate limits are tracked by IP address. Usage on shared platforms (e.g., Vercel, Google Sheets) may result in unexpected throttling.
- Attribution under the CC-BY 4.0 license is strictly required https://open-meteo.com/en/licence.
- Standard endpoint structure: https://open-meteo.com/en/docs.
- Terms of use and pricing limits: https://open-meteo.com/en/terms.

**Open-Meteo - Historical Weather**
- Shares the same daily, hourly, and minutely API limits as the Weather API.
- Reanalysis data (like ERA5) spans back to 1940, typically available in 0.1° to 0.25° global resolution.
- Live data from recent days is continuously appended, usually with a 2-5 day delay depending on the dataset.
- Large historical queries can be aggregated to daily levels directly in the API to save bandwidth.
- Historical endpoint structure: https://open-meteo.com/en/docs/historical-weather-api.

**OpenAQ - REST API**
- Requires registering for a user API key.
- Strict rate limits apply for general use: 60 requests per minute and 2,000 requests per hour.
- Exceeding API limits repeatedly can lead to a temporary or permanent ban.
- The API responds with HTTP 429 "Too many requests" if limits are breached, and provides headers (`x-ratelimit-remaining`, `x-ratelimit-reset`) to help manage quotas programmatically.
- Usage requires adherence to the OpenAQ Terms of Use, including clear source attribution.
- Official documentation: https://docs.openaq.org/.
- Rate limits specifics: https://docs.openaq.org/using-the-api/rate-limits.