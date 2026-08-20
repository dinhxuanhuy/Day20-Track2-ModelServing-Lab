# Bonus - Batch-size sweep (chunked prefill)

Host `Linux-x86_64` · llama.cpp `b10488` ·
`threads=8` `ngl=0` · metric `pp512`

| -b (logical) | -ub (micro) | pp512 (tok/s) | vs best |
|:--|--:|--:|--:|
| 128 | 128 | 407.8 | 100% |
| 256 | 256 | 401.3 | 98% |
| 512 | 256 | 380.6 | 93% |
| 512 | 512 | 397.3 | 97% |
| 1024 | 512 | 394.1 | 97% |
| 2048 | 512 | 387.8 | 95% |

Best: `-b 128 -ub 128` at 407.8 tok/s
(1.07x the slowest point tested).

This sweep only measures the throughput half of the trade. The cost it hides is
TTFT for queued requests: a larger micro-batch holds the device longer per step,
so anything waiting behind it waits longer. To see both halves, re-run
`make load-50` with your best and worst settings via
`.venv/bin/python labs/02-serve/serve.py -- -b N -ub M` and compare P95.

## Your finding (required -- replace this line)

_Which setting would you run in production, and what would you need to measure to
be sure it does not hurt the P95 of a contended server?_
