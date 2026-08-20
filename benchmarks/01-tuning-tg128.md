# 01 - Tune: thread-count sweep

Model `Qwen3.5-0.8B-Q4_K_M.gguf` · host `Linux-x86_64` · llama.cpp `b10488`
CPU: **16 physical · 16 logical** cores · `ngl=0` · metric `tg128`

| threads (-t) | tg128 (tok/s) | vs best |
|:--|--:|--:|
| 1 | 24.7 | 38% |
| 8 | 65.5 | 100% |
| 16 | 0.4 | 1% |
| 32 | 8.6 | 13% |

**Best**: `-t 8` at 65.5 tok/s
**Slowest tested**: `-t 16` at 0.4 tok/s (163.78x spread)
**Against the physical-core default** (`-t 16`, 0.4 tok/s): 163.78x

Use this in your run:

```bash
LAB_N_THREADS=8 make bench
```

## Your explanation (required -- replace this line)

_Where is the knee, and why there? If the peak sits at your physical core count
and drops above it, say what the extra threads are competing for. If your curve
does something else -- flat, or still climbing at 2x logical cores -- say that
instead and reason about why. A result that contradicts the expected shape is
worth more than one that matches it, as long as you explain it._
