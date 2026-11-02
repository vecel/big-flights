# Data Sources
This document provides research and overview of data sources intended to use in project.

## Summary table

| Data Source | Description | Request | Response |
| :--- | :--- | :--- | :--- |
| **OpenSky - REST API** | Fetches live flight data and vector states (e.g., flight ID, position, velocity) for planes in a specified bounding box. | HTTP request to `/states/all`. | JSON containing a `time` field and a `states` array of vector states. |
| **OpenSky - Historical Data** | Provides historical data regarding flight vector states. | Trino request to `https://trino.opensky-network.org`. | Trino JSON document with state vector data batches and a `nextUri` for subsequent batches. |

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