# Bonus - Context-length sweep (prefill cost)

Host `Linux-x86_64` · llama.cpp `b10488` ·
`threads=8` `ngl=0` · RAM 31.0 GB

| Prompt tokens | Prefill (tok/s) | TTFT contribution (ms) | vs linear scaling |
|:--|--:|--:|--:|
| 256 | 422.5 | 605.9 | 1.00x |
| 1024 | 399.1 | 2565.6 | 1.06x |
| 2048 | 388.0 | 5278.1 | 1.09x |
| 4096 | 352.7 | 11613.9 | 1.20x |
| 8192 | 328.1 | 24968.8 | 1.29x |

At 8192 tokens, prefill costs **24969 ms** --
1.29x what linear scaling from the smallest point would predict. That excess
is attention's O(N^2) term becoming visible, and every millisecond of it lands in TTFT
before the user sees a single token.

Either way, this is the number to remember when someone proposes stuffing more retrieved
context into a RAG prompt "because the context window allows it". Prefill is paid in full,
on every request, before the first token appears.

## Your finding (required -- replace this line)

_At what prompt length does prefill start to dominate your end-to-end latency? Did you see
the quadratic bend, or is your range still linear -- and what does that tell you about how
many retrieved chunks your RAG pipeline can afford?_
