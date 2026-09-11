# Calibration Ledger

Running record of actioned positions and notable no-trade calls, for scoring against final resolution. Keep exact ticker, date, option, price, thesis, and thesis killer. Score win or loss after settlement and note the calibration lesson.

## Open positions

### Chicago low, Sep 10, 2026 — user test position, 1 dollar — PENDING
Recorded before settlement at about 8:45 PM CDT, close 1 AM CDT (06:00 UTC Sep 11).
- Ticker KXLOWTCHI-26SEP10-B64.5, "64 to 65", YES, about 0.18, 1 dollar.
- Resolves YES if the Chicago daily minimum settles at 64 or 65 F. Resolving station Midway KMDW per the source spec.

My pre settlement estimate
- Probability this exact bracket hits about 35 percent. Priced at 0.18, so positive expected value by my read. This is the edge, 35 over 18.
- Do not confuse with P(low reaches 65 or below), which is about 62 percent and a wider event.
- Bracket probabilities, 66 to 67 about 55 percent, 64 to 65 about 35 percent, 62 to 63 or lower about 7 percent, 68 to 69 about 3 percent.
- Confidence on the strike is low, about 6 out of 10. High confidence the low comes tonight and lands mid 60s, low confidence on the exact bracket.

Evidence
- Morning low was 68 F at dawn, but a front is dropping temps this evening and the daily low will be set late tonight, not this morning. Market agreed, pricing 68 to 69 at about 0.01.
- Midway hourly trace from the Iowa State mesonet feed (NWS blocked my fetch), 75 at 16:53, 75 at 17:53, 73 at 18:53, 71 at 19:53. Falling about 2 F per hour with 3 plus hours to midnight CDT.
- I land the low near 65 to 66. Market has 66 to 67 at 0.77 and 64 to 65 at 0.18, so I read 66 to 67 as rich and 64 to 65 as cheap.

Thesis killer
- Cooling stalls after 10 PM, which it usually does, and the low holds at 66 or 67. Also the resolving 1 minute min group can print about a degree below these hourly reads, which helps the lower bracket, but the reverse timing risk dominates.

### Miami hourly 10 PM, Sep 10, 2026 — user test position, 1 dollar — PENDING
Recorded before settlement at about 9:30 PM ET, close 10 PM ET (02:00 UTC Sep 11).
- Ticker KXTEMPMIAH-26SEP1022-T81.99, "82 or above", YES, about 0.58, 1 dollar.
- Resolves YES if the Kalshi Weather Index Miami blend at 10 PM EDT is 82.00 F or higher.

My pre settlement estimate
- Central probability of YES about 55 percent. Plausible range 45 to 62 percent. Confidence moderate.
- Resolving source is the five station Miami blend, members KMIA1M, KFLL1M, KFXE1M, KOPF1M, KPMP1M.
- At 01:16 UTC the blend read 82.04 F and had held flat there for 90 minutes. Four members at 28.0 C (82.4 F), Fort Lauderdale Executive at 27.0 C (80.6 F).
- The feeds report in whole degrees Celsius, so the blend only moves when a station crosses a Celsius step. It sits 0.04 above the 82.00 line.

Thesis killer
- Any one of the four 28 C stations ticks down to 27 C before 10 PM. That alone drops the blend to 81.68 and the contract resolves NO. Estimated likelihood about 40 percent given the strong persistence but ongoing evening cooling.

Note. Roughly fair value at 0.58, a near coin flip on a threshold. Fine as a 1 dollar test. Score after settlement.

### San Diego high, Sep 9, 2026 — two tickets, one thesis — EXPECTED LOSS, pending settlement
Status as of 2026-09-10 02:00 ET: still active, not yet settled. Close 4 a.m. ET, settlement expected midafternoon ET. Expected outcome LOSS on both, about 85 to 90 percent.

Tickets
- KXHIGHTSAN-26SEP09-B92.5, "92° to 93°", YES, about 0.01, 5 dollars, roughly 500 contracts.
- KXHIGHTSAN-26SEP09-T93, "94° or above", NO, about 0.01, 1 dollar, roughly 100 contracts.
- Same directional bet. Both win if the settled high is 93, both lose if it is 94.

The decisive number
- KSAN METAR 6-hour maximum temperature group (the 1xxxx remark) reads 34.4C on the 17:51Z and 23:51Z reports. 34.4C converts to 93.9F, which rounds to 94F.
- Every hourly temperature reading was 33.9C or lower, which is 93F. So the peak that decides this landed between the hourly obs and appears only in the max group. The margin was half a degree Celsius, 33.9 versus 34.4, and the rounding of 93.9 up to 94.
- The NWS final CLI (issued 00:31Z) reports the daily max as 94F at 10:10 AM, consistent with the 34.4C max group. The market held 94-or-above at 99 percent throughout.

Why the portal shows 93 but it likely settles 94
- The weather.com/kalshi hourly grid displays the hourly temperature readings (33.9C = 93F max), not the daily maximum. The between-hour peak is not shown on the grid.
- The settlement is the daily maximum. The ASOS daily max is 34.4C = 93.9F = 94F rounded. So the grid can honestly read 93 while the contract settles 94.
- Residual uncertainty, the 10 to 15 percent: if Kalshi were to settle literally on the highest value shown in that hourly grid (93) rather than the daily max group, the tickets would win. Considered unlikely given the max group, the NWS report, and the market, but the rules do allow Kalshi discretion.

What I got wrong, and the churn
- I first called the high locked at 93 from the observation feed (topped 93.2F) and the hourly grid (93). Both undercounted the true peak.
- I then flipped to loss on the NWS 94, then wobbled back toward 93 when shown the portal grid. I should have anchored from the start on the METAR 6-hour max group, which is 34.4C and rounds to 94. That single value settles the question.

Rules to carry forward
- The settling daily max is the ASOS daily maximum, captured in the METAR 6-hour max group (1xxxx) and 24-hour group (4xxxx), and in the NWS CLI. It is NOT the max of the hourly temperature readings and NOT the max of the portal hourly grid. Read the max group before calling any near-threshold high.
- Watch for the Celsius rounding knife edge. A max group of 34.4C is 93.9F and rounds to 94, even though the nearby whole-degree hourly reads 93. Half a degree Celsius decides a bucket.
- A confident market at 95 percent or more on a same-day observed value is very hard to beat. When the proxy disagrees, the market is usually reading the max group or finer data. Weight it heavily.
- Thin does not mean stale or wrong. The San Diego 94 book was thin and correct.
- Third instance in one day (Sep 9, 2026) of the market being right and my airport read misleading, after Austin and San Francisco.

## Scoreboard
- Chicago low 64-65 YES, Sep 10 2026: PENDING (my 35 vs price 18)
- Miami hourly 82 or above YES, Sep 10 2026: PENDING
- San Diego 92-93 YES, Sep 9 2026: EXPECTED LOSS (pending)
- San Diego 94-or-above NO, Sep 9 2026: EXPECTED LOSS (pending)

## Scan log

### 2026-09-10, midnight ET window scan (scheduled run, fired 17:10 ET)
Task. Review open markets closing within 60 minutes of midnight ET, focus economics and climate/weather.

Window. Close between 03:00 and 05:00 UTC on Sep 11 (11 p.m. ET Sep 10 to 1 a.m. ET Sep 11). Unix 1789095600 to 1789102800.

Universe. Roughly 40 open markets, all closing 05:00 UTC (1 a.m. ET). All weather. Economics count in the window was zero, which is expected, since data release markets settle at release times, not at midnight.

Composition.
- KXRAIN daily rain, 8 cities. Already YES near 0.99 at NYC, Miami, Newark, Boston. Near NO at Trenton 0.03, Philadelphia 0.03, Washington DC 0.06, Atlanta 0.04.
- KXLOWT daily minimum temperature brackets for Trenton, Louisville (KSDF, zero volume, untradeable), Philadelphia, New York, Miami.

Verdict. Zero actionable. No STRONGER, no SMALL, no new position.

Reasons.
1. Priced out. Every decided market sits at 0.99 or 0.01. The 95 cent rule blocks buying the near certain YES side, and the cheap side is a bet against a near settled fact for about 1 cent of edge.
2. Live reversal on the tradeable low temperature brackets. At 5 p.m. ET the morning minimum is set but the Sep 10 daily window stays open until local midnight. Evening cooling can only push the minimum lower, which would drop it into a lower bracket. This is the Austin bucket, Closing regime with a live mechanism, so the strongest sizing is forbidden. Examples, Trenton 70 to 71 at 0.92, Philadelphia 75 or above at 0.90 bid and 0.96 ask (ask also breaks the 95 cent rule).
3. Not confirmable yet. The scan fired about 7 hours before close. No final CLI has issued for these Eastern cities, and free feeds are coarser than the 1 minute ASOS data that settles the value. Nothing here reads as a Settled contract.

Timing note. To catch these weather markets in a confirmable Settled state, run the scan after the local day closes and the final CLI issues, roughly 04:00 to 06:00 UTC for Eastern cities, not at 5 p.m. ET.
