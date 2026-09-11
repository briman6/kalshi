# Ground Truth Source Specification, Kalshi Temperature Markets

Purpose. Remove all ambiguity about exactly which reading resolves these contracts. Every evaluation must name the station, the source, the measurement, and the exact metric before any analysis. This document is the single source of truth for that.

## FIRST, identify the market type. They use DIFFERENT sources.

There are two distinct Kalshi temperature product families and they DO NOT share a data provider or a metric. Confirm which one you are looking at from the ticker and the rule text before anything else.

- DAILY high/low markets. Series like KXHIGH{CITY} and KXLOWT{CITY}. Question form, highest or lowest temperature today. Source is The Weather Company on the airport ASOS daily max/min. See the DAILY section below.
- HOURLY markets. Series like KXTEMP{CITY}H. Question form, temperature in {city} at a specific hour, for example KXTEMPMIAH, temperature in Miami at 3 AM EDT. Source is the Kalshi Weather Index, a calibrated multi-station average of 1-minute ASOS feeds. See the HOURLY section below. NWS climate reports are explicitly NOT authoritative for these.

Never apply the daily rule to an hourly market or the reverse.

## DAILY markets, the metric that resolves

For a HIGH market, resolution is the station's DAILY MAXIMUM temperature. For a LOW market, the DAILY MINIMUM. As reported by The Weather Company, which passes through the official NWS and FAA ASOS value. It is a whole number of degrees Fahrenheit, obtained by converting the ASOS value in Celsius and rounding to the nearest whole degree.

This daily max or min is NOT any of these:
- Not the hourly METAR temperature (the number in the METAR body, or the T-group).
- Not the highest or lowest cell in the weather.com/kalshi hourly grid.
- Not a forecast.

The daily max or min comes from the ASOS max/min temperature GROUPS, which are built from 1-minute data and capture peaks that fall between the hourly observations. Read the group, never the hourly.

### Where to read the daily number, in priority order

1. NWS CLI climate report, final issuance. api.weather.gov/products/types/CLI/locations/{CODE}. The MAXIMUM and MINIMUM lines under TODAY give the official whole-degree F value and the time. The final CLI issues after the day ends. This is authoritative and is what The Weather Company mirrors.
2. METAR remark max/min groups on the correct ICAO. 6-hour max is the 1xxxx group, 6-hour min is the 2xxxx group, 24-hour max and min is the 4xxxx group. Convert tenths of a degree C to F, round to whole. These carry the between-hour peak.
3. The weather.com/kalshi portal Final value. Convenient, but its hourly grid displays hourly readings and can show a number 1 to 2 F below the true daily max. Use it only to confirm Final status, not to read the peak.

### Celsius rounding knife edge

The ASOS records in tenths of a degree C. Convert then round. Example that cost a real trade. San Diego Sep 9, 2026 daily max group was 34.4C, which is 93.9F, which rounds to 94, while every hourly reading showed 33.9C which is 93F. Half a degree C decides the bucket. Always compute from the group in C, then round.

## HOURLY markets, the metric that resolves

Confirmed from the KXTEMPMIAH rules and the live Kalshi Weather Index endpoint, Sep 10, 2026.

- Source. The Kalshi Weather Index, delivered via Synoptic Data. It is a CALIBRATED MULTI-STATION AVERAGE, not a single sensor and not The Weather Company. The rules also name Synoptic and state NWS Climate Reports and Google Weather are references only.
- Metric. The index value at the exact stated hour and timezone, for example 3 AM EDT. Minute resolution, point in time, not a max or min over a window.
- Precision. Fahrenheit to the hundredth. Thresholds run to the hundredth, for example above 79.99.
- Method. The published value equals the average of the configured member stations' 1-minute readings. Verified example, five members read 84.2, 84.2, 84.2, 84.2, 86.0 F and the index printed 84.56, exactly their mean. A per-station calibration is baked into the config version.

### How to read the hourly index and its members

- Endpoint. GET https://external-api.kalshi.com/trade-api/v2/live_data/weather/{city}?detailed=true with a time window (last_sec, or from and to in unix ms). detailed=true returns each member station's reading plus the blended value v and the field t as the minute timestamp.
- The settled hourly market carries the resolving value in expiration_value. The bracket of yes and no results across strikes also pins it.

### Confirmed Miami hourly member stations (config miami-temperature-v1.0-cal-20260907)

Five 1-minute ASOS feeds, source hf_asos, index is their calibrated average:
- KMIA1M, Miami International
- KFLL1M, Fort Lauderdale-Hollywood International
- KFXE1M, Fort Lauderdale Executive
- KOPF1M, Opa-Locka Executive
- KPMP1M, Pompano Beach Airpark

Consequence. The Miami hourly index is NOT Miami Intl alone. Three of five members sit in Broward, so the index runs cooler than KMIA on hot afternoons. On Sep 9, 2026 KMIA peaked 32.2C while the index peaked 31.0C. Never resolve or model an hourly Miami market off KMIA alone. Each city's member set must be pulled from the endpoint separately, do not assume it mirrors Miami.

## Station table for DAILY markets, verified from CLI headers Sep 9-10, 2026

Read the station named in the CLI header. Do not guess from the city name.

| City | Contract CLI code | Resolving station | ICAO |
|---|---|---|---|
| New York | CLINYC | Central Park NY | KNYC |
| Miami | CLIMIA | Miami Intl | KMIA |
| Boston | CLIBOS | Boston Logan MA | KBOS |
| Philadelphia | CLIPHL | Philadelphia Intl PA | KPHL |
| Washington DC | CLIDCA | Washington National DC | KDCA |
| Atlanta | CLIATL | Atlanta Hartsfield | KATL |
| Newark | CLIEWR | Newark NJ | KEWR |
| Trenton | CLITTN | Trenton NJ | KTTN |
| Louisville | CLISDF | Louisville KY | KSDF |
| Chicago | CLIMDW | Chicago Midway | KMDW |
| Dallas | CLIDFW | Dallas Fort Worth | KDFW |
| Houston | CLIHOU | Houston Hobby | KHOU |
| Austin | CLIAUS | Austin Bergstrom | KAUS |
| San Antonio | CLISAT | San Antonio | KSAT |
| Oklahoma City | CLIOKC | Oklahoma City | KOKC |
| New Orleans | CLIMSY | New Orleans | KMSY |
| Minneapolis | CLIMSP | Twin Cities MN | KMSP |
| Phoenix | CLIPHX | Phoenix AZ | KPHX |
| Denver | CLIDEN | Denver CO | KDEN |
| Los Angeles | CLILAX | Los Angeles Intl | KLAX |
| Seattle | CLISEA | Seattle Tacoma | KSEA |
| San Francisco | CLISFO | San Francisco Intl | KSFO |
| San Diego | CLISAN | San Diego Intl (Lindbergh) | KSAN |

Note Austin and Houston. Austin resolves on Bergstrom KAUS, not Camp Mabry KATT. Houston resolves on Hobby KHOU, not Bush KIAH. These are the two easiest station errors. This table is for the DAILY markets. The hourly markets use a multi-station blend, confirm each city from the index endpoint.

## Empirical proof the daily metric matters, Sep 9, 2026

Comparing the hourly-reading max to the authoritative CLI or max-group max, the hourly reading understated the daily max in about 15 of 23 cities. Confirmed 1 F gaps at Philadelphia, Trenton, Austin, San Antonio, San Diego, and a 2 F gap at Los Angeles. Only cities where the peak happened to sit on an hourly observation matched. A method built on hourly readings is wrong more often than right for highs.

## Mandatory pre-evaluation checklist, every contract

1. Identify the market family first. Daily (KXHIGH/KXLOWT, Weather Company on ASOS max/min group) or hourly (KXTEMP..H, Kalshi Weather Index multi-station blend). Read the rule text, do not assume.
2. Name the exact source. Daily, the station from the CLI header. Hourly, the member-station set pulled from the weather index endpoint for that city.
3. State the exact metric. Daily maximum, daily minimum, or point-in-time hour and timezone.
4. Read the value from the correct source. Daily, the max/min group or final CLI, never the hourly reading or portal grid. Hourly, the index v from the endpoint, or the settled expiration_value.
5. Match precision. Daily rounds to whole F. Hourly runs to hundredths.
6. For daily, compute from Celsius then round. Confirm Final, not preliminary.
7. If any of these cannot be pinned exactly, mark UNVERIFIED and do not trade.
