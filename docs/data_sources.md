# Data Sources

### OpenSky - REST API
Fetches live data about flights. Provides vector states (e.g. flight id, position, velocity) for planes in specified region (bounding box). 

**Request:** HTTP request to endpoint `/states/all`.

**Response:** JSON with two fields: *time*, *states*. *States* is an array with each row representing vector state of particular plane. All states are associated with *time* field from top-level response. See: https://openskynetwork.github.io/opensky-api/rest.html#all-state-vectors.

**Notes and limitations:**
1. Requires an account to provide 4000 credits per day (instead of 400 for anonymous users). Supports OAuth2 authentication method only.
2. OAuth2 token expires every 30min and has to be refreshed. This can be done with HTTP request to OpenSky service (see: https://openskynetwork.github.io/opensky-api/rest.html#authentication).
3. Enpoint `/states/all` serves data with 5s time resolution. One call costs 4 credits, but this value can be reduced with smaller bounding box (see: https://openskynetwork.github.io/opensky-api/rest.html#api-credits). E.g. making request every 10s allows us to get live data for about 3h.
4. Using OpenSky API in project requires citation of: *Matthias Schäfer, Martin Strohmeier, Vincent Lenders, Ivan Martinovic and Matthias Wilhelm.
"Bringing Up OpenSky: A Large-scale ADS-B Sensor Network for Research".
In Proceedings of the 13th IEEE/ACM International Symposium on Information Processing in Sensor Networks (IPSN), pages 83-94, April 2014.* Adding link to official site https://opensky-network.org is welcome.

### OpenSky - Historical Data
Provides historical data about flight's vector states.

**Request:** Trino request to https://trino.opensky-network.org.

**Response:** Trino JSON document (see: https://trino.io/docs/current/develop/client-protocol.html#important-queryresults-attributes) with state vectors data batches (see: https://openskynetwork.github.io/opensky-api/trino.html#state-vectors) and *nextUri* for following batches.

**Notes and limitations:**
1. Requires an account and verification for application purpose. Our app has to be considered university-affiliated research to have access to historical data.
2. OpenSky's Trino calls must follow performance guidelines (see: https://openskynetwork.github.io/opensky-api/trino.html#tools-performance-considerations) or the app will be banned for ever.
3. Filtering by partition column MUST be used in particular.
4. Bulk downloads (e.g. week global data) should not be used.
5. Trino calls can be done from Python script and streamlined to Nifi.