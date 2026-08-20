# 02 - Serve: load test + saturation reading

Host `Linux-x86_64` · llama.cpp `b10488` ·
`--parallel 4` · `ctx=2048` · `threads=16` ·
`ngl=0`

| Users | Reqs | RPS | P50 (ms) | P95 (ms) | P99 (ms) | Eff. concurrency | Failures |
|:--|--:|--:|--:|--:|--:|--:|--:|
| 10 | 0 | 0.00 | 0 | 0 | 0 | 0.0 | 0.0% |
| 50 | 0 | 0.00 | 0 | 0 | 0 | 0.0 | 0.0% |

*Effective concurrency = RPS x average latency (Little's Law) -- how many requests were
really in flight, regardless of how many users locust simulated. It counts queued requests
too, so the occupancy/slot ratio can legitimately exceed 1.0; it is occupancy, not
utilisation. For true slot utilisation use the server's own gauges (`make metrics`).*

## What these two runs say

| Going from 10 to 50 users | |
|:--|--:|
| Offered load | 5x |
| Throughput actually delivered | **0.00x** (0% of linear) |
| P95 latency | **0.00x** |
| Effective concurrency at 50 users | 0.0 vs `--parallel 4` slots (occupancy/slot ratio 0.00) |

**Saturated.** Throughput stopped scaling (0.00x delivered for 5x offered, 0% of linear) even though effective concurrency (0.0) sits below 4 slots. Something other than decode-slot count is the limit -- look at memory bandwidth, or context/KV pressure.

P95 grew no faster than throughput (0.00x vs 0.00x), so this server still has headroom at 50 users.

> **Small sample.** Only 0 requests completed in the
> shorter run, so these percentiles are indicative rather than solid. Note also that
> locust averages only *completed* requests: when the run ends with requests still
> queued, effective concurrency is an **under**-estimate. Trust the throughput-scaling
> row over the concurrency row here, and run longer (`-t 3m`) if you want firmer numbers.

## Your reading (required -- replace this line)

_Where does your server saturate, and what is the evidence? Name the number that
convinced you. Then say what you would change first to raise goodput at your SLO --
and why that knob and not another._
