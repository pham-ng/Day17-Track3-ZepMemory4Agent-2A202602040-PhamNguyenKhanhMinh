# Memory vs No-Memory Comparison

| Metric | Memory-enabled | No-memory | Delta |
| --- | ---: | ---: | ---: |
| Evidence hit rate | 63.6% | 100.0% | -36.4% |
| Passed cases | 7/11 | 2/2 | +5 |
| Avg retrieval latency (ms) | 1434.5 | 0.2 | +1434.3 |
| Avg token reduction | 26.8% | 0.0% | +26.8% |

## Interpretation

Short-term cases can pass without durable memory because their evidence is still in the current thread. Cross-session, episodic and semantic cases should fail in the no-memory baseline and recover when memory retrieval is enabled.

A no-memory baseline may show near-100 percent token reduction simply because it retrieves nothing. Treat token reduction as useful only together with evidence hit rate; dropping all context is cheap but incorrect.