# Memory vs No-Memory Comparison

| Metric | Memory-enabled | No-memory | Delta |
| --- | ---: | ---: | ---: |
| Evidence hit rate | 72.7% | 18.2% | +54.5% |
| Passed cases | 8/11 | 2/11 | +6 |
| Avg retrieval latency (ms) | 1348.8 | 0.0 | +1348.8 |
| Avg token reduction | 22.4% | 81.8% | -59.4% |

## Interpretation

Short-term cases can pass without durable memory because their evidence is still in the current thread. Cross-session, episodic and semantic cases should fail in the no-memory baseline and recover when memory retrieval is enabled.

A no-memory baseline may show near-100 percent token reduction simply because it retrieves nothing. Treat token reduction as useful only together with evidence hit rate; dropping all context is cheap but incorrect.