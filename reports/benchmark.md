# Lab 17 Benchmark Report

- Implementation: `student`
- Kind: `practice`
- Cases: **11**
- Passed: **8/11**
- Evidence hit rate: **72.7%**
- Average retrieval latency: **1348.8 ms**
- Average token reduction vs full source context: **22.4%**

| Case | Layer | Pass | Latency ms | Retrieved tokens | Token reduction | Missing / Error |
| --- | --- | --- | ---: | ---: | ---: | --- |
| E01 | short_term | PASS | 0.0 | 133 | 0.0% |  |
| E06 | semantic | PASS | 2576.0 | 148 | 67.8% |  |
| E09 | long_term | PASS | 2072.4 | 814 | 0.0% |  |
| E10 | short_term | PASS | 0.4 | 195 | 0.0% |  |
| E02 | long_term | PASS | 2506.3 | 1102 | 0.0% |  |
| E03 | long_term | PASS | 2300.8 | 1102 | 0.0% |  |
| E04 | episodic | FAIL | 224.5 | 121 | 45.2% | missing=ClientSession, concurrency=20, ASYNC-FIX-20 |
| E05 | episodic | FAIL | 207.8 | 121 | 45.2% | missing=connection churn, timeout threshold |
| E07 | mixed | PASS | 2474.7 | 485 | 14.2% |  |
| E11 | semantic | PASS | 406.2 | 146 | 74.2% |  |
| E08 | long_term | FAIL | 2067.3 | 1113 | 0.0% | missing=BLUEBIRD-42, TypeScript, NestJS |

## Evidence excerpts

### E01 - short_term

`<RECENT_TURNS> user: Ten du an ca nhan cua toi la ORCHID-27. Toi thich Python va khong thich Java. Khi giai thich code, hay dung vi du ngan. assistant: Da hieu: demo ca nhan ORCHID-27, uu tien Python, tranh Java, vi du ngan. user: Toi dang hoc async/await va hay nham coroutine voi Task. Neu sau nay gap chu de nay, hay giai thich bang timeline. assistant: Toi se uu tien timeline khi giai thich coroutine va Task. user: TODO: hoan thanh benchmark report truoc thu Sau luc 16:00. Day la open loop LAB-REPORT-1600. </RECENT_TURNS>`

### E06 - semantic

`EPISODE: {"id":"kb-payment-retry","entity":"Payment API Retry Policy","summary":"For POST /payments, every retryable request MUST send the same Idempotency-Key. Retry only HTTP 429 or transient 5xx errors, use exponential-backoff, and stop after max-3-retries. Marker: PAYMENT-RULE-3.","source":"internal-api-guideline-v3","updated_at":"2026-08-10T00:00:00Z"} metadata= EPISODE: For POST /payments, every retryable request MUST send the same Idempotency-Key. Retry only HTTP 429 or transient 5xx errors, use exponential-backoff, and stop after max-3-retries. Marker: PAYMENT-RULE-3. metadata=`

### E09 - long_term

`<USER_SUMMARY> Lan's project is LOTUS-88. They prioritize Java and Spring Boot for backend examples and do not use Python for the backend. </USER_SUMMARY>  <EPISODES> Episodes are source message or document excerpts shown in selection order.   - Created At: 2026-08-01 11:00:20     Source: message     Content: Lab Assistant (assistant): Da hieu: LOTUS-88, Java + Spring Boot cho backend examples.   - Created At: 2026-08-01 11:00:00     Source: message     Content: [user] {   "user_id": "lan-lab17",   "first_name": "Lan",   "last_name": "Tran",   "user_alias": "Lan Tran" }: Toi la Lan. Du an cua toi la LOTUS-88. Toi uu tien Java va Spring Boot, va khong dung Python trong vi du backend. </EPISOD`

### E10 - short_term

`<SESSION_SUMMARY> user: Constraint: REVIEW-DEADLINE-1600 - project review is Friday at 16:00 and must not be forgotten. | assistant: Acknowledged review constraint. | user: Filler turn 1 about UI spacing. | assistant: Filler answer 1. | user: Filler turn 2 about naming. | assistant: Filler answer 2. | user: Filler turn 3 about logging. | assistant: Filler answer 3. </SESSION_SUMMARY> <DURABLE_NOTES> - user: Constraint: REVIEW-DEADLINE-1600 - project review is Friday at 16:00 and must not be forgotten. - assistant: Acknowledged review constraint. </DURABLE_NOTES> <RECENT_TURNS> user: Filler turn 4 about tests. assistant: Filler answer 4. user: Filler turn 5 about docs. assistant: Filler answe`

### E02 - long_term

`<USER_SUMMARY> Minh Nguyen's personal project is named ORCHID-27. Minh has a task to complete a benchmark report before Friday at 16:00, referred to as OPEN LOOP LAB-REPORT-1600.  Minh Nguyen prefers Python and dislikes Java.  When explaining coroutine and Task, the AI will prioritize using a timeline. The AI will use short examples when explaining code. </USER_SUMMARY>  <EPISODES> Episodes are source message or document excerpts shown in selection order.   - Created At: 2026-08-01 09:00:00     Source: message     Content: [user] {   "user_id": "minh-lab17",   "first_name": "Minh",   "last_name": "Nguyen",   "user_alias": "Minh Nguyen" }: Ten du an ca nhan cua toi la ORCHID-27. Toi thich Pyt`

### E03 - long_term

`<USER_SUMMARY> Minh Nguyen's personal project is named ORCHID-27. Minh has a task to complete a benchmark report before Friday at 16:00, referred to as OPEN LOOP LAB-REPORT-1600.  Minh Nguyen prefers Python and dislikes Java.  When explaining coroutine and Task, the AI will prioritize using a timeline. The AI will use short examples when explaining code. </USER_SUMMARY>  <EPISODES> Episodes are source message or document excerpts shown in selection order.   - Created At: 2026-08-01 09:04:00     Source: message     Content: [user] {   "user_id": "minh-lab17",   "first_name": "Minh",   "last_name": "Nguyen",   "user_alias": "Minh Nguyen" }: TODO: hoan thanh benchmark report truoc thu Sau luc 1`

### E04 - episodic

`EPISODE: Hom nay toi debug async HTTP. Toi da thu tang timeout len 60s nhung van fail. metadata= EPISODE: Ten du an ca nhan cua toi la ORCHID-27. Toi thich Python va khong thich Java. Khi giai thich code, hay dung vi du ngan. metadata= EPISODE: Toi dang hoc async/await va hay nham coroutine voi Task. Neu sau nay gap chu de nay, hay giai thich bang timeline. metadata= EPISODE: TODO: hoan thanh benchmark report truoc thu Sau luc 16:00. Day la open loop LAB-REPORT-1600. metadata=`

### E05 - episodic

`EPISODE: Hom nay toi debug async HTTP. Toi da thu tang timeout len 60s nhung van fail. metadata= EPISODE: Ten du an ca nhan cua toi la ORCHID-27. Toi thich Python va khong thich Java. Khi giai thich code, hay dung vi du ngan. metadata= EPISODE: Toi dang hoc async/await va hay nham coroutine voi Task. Neu sau nay gap chu de nay, hay giai thich bang timeline. metadata= EPISODE: TODO: hoan thanh benchmark report truoc thu Sau luc 16:00. Day la open loop LAB-REPORT-1600. metadata=`

### E07 - mixed

`<LONG_TERM> <USER_SUMMARY> Minh Nguyen's personal project is named ORCHID-27. Minh has a task to complete a benchmark report before Friday at 16:00, referred to as OPEN LOOP LAB-REPORT-1600.  Minh Nguyen prefers Python and dislikes Java.  When explaining coroutine and Task, the AI will prioritize using a timeline. The AI will use short examples when explaining code. </USER_SUMMARY>  <EPISODES> Episodes are source message or document excerpts shown in selection order.   - Created At: 2026-08-01 09:00:00     Source: message     Content: [user] {   "user_id": "minh-lab17",   "first_name": "Minh",   "last_name": "Nguyen",   "user_alias": "Minh Nguyen" }: Ten du an ca nhan cua toi la ORCHID-27. T`

### E11 - semantic

`EPISODE: {"id":"kb-async-http","entity":"Async HTTP Incident Playbook","summary":"When async HTTP calls time out, inspect connection pooling, downstream saturation and concurrency before increasing timeout. Reuse a long-lived client session where possible. Marker: CONN-POOL-FIRST.","source":"incident-playbook-2026","updated_at":"2026-08-11T00:00:00Z"} metadata= EPISODE: When async HTTP calls time out, inspect connection pooling, downstream saturation and concurrency before increasing timeout. Reuse a long-lived client session where possible. Marker: CONN-POOL-FIRST. metadata=`

### E08 - long_term

`<USER_SUMMARY> Minh Nguyen's personal project is named ORCHID-27. Minh has a task to complete a benchmark report before Friday at 16:00, referred to as OPEN LOOP LAB-REPORT-1600.  Minh Nguyen prefers Python and dislikes Java.  When explaining coroutine and Task, the AI will prioritize using a timeline. The AI will use short examples when explaining code. </USER_SUMMARY>  <EPISODES> Episodes are source message or document excerpts shown in selection order.   - Created At: 2026-08-01 09:02:00     Source: message     Content: [user] {   "user_id": "minh-lab17",   "first_name": "Minh",   "last_name": "Nguyen",   "user_alias": "Minh Nguyen" }: Toi dang hoc async/await va hay nham coroutine voi Ta`
