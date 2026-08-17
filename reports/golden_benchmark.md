# Lab 17 Golden Set Report

- Implementation: `student`
- Kind: `golden`
- Cases: **20**
- Passed: **12/20**
- Evidence hit rate: **60.0%**
- Average retrieval latency: **1519.1 ms**
- Average token reduction vs full source context: **20.7%**
- Golden bonus: **0/10** (100% required)

| Case | Layer | Pass | Latency ms | Retrieved tokens | Token reduction | Missing / Error |
| --- | --- | --- | ---: | ---: | ---: | --- |
| G01 | short_term | PASS | 0.2 | 227 | 0.0% |  |
| G02 | short_term | PASS | 0.0 | 133 | 0.0% |  |
| G08 | long_term | PASS | 4356.3 | 843 | 0.0% |  |
| G09 | long_term | PASS | 2348.1 | 965 | 0.0% |  |
| G12 | semantic | PASS | 398.6 | 232 | 49.5% |  |
| G14 | semantic | PASS | 274.8 | 84 | 78.3% |  |
| G15 | semantic | FAIL | 312.6 | 84 | 81.7% | missing=BUDGET-10-4-3-3 |
| G19 | mixed | PASS | 2662.2 | 568 | 0.0% |  |
| G03 | long_term | PASS | 2272.6 | 965 | 0.0% |  |
| G04 | long_term | PASS | 2257.1 | 972 | 0.0% |  |
| G05 | long_term | PASS | 2353.6 | 963 | 0.0% |  |
| G10 | episodic | FAIL | 218.1 | 126 | 43.0% | missing=ClientSession, concurrency=20, ASYNC-FIX-20 |
| G11 | episodic | FAIL | 212.6 | 126 | 43.0% | missing=connection churn, timeout threshold, ASYNC-FIX-20 |
| G13 | semantic | PASS | 296.1 | 229 | 59.5% |  |
| G16 | mixed | PASS | 2553.4 | 581 | 0.0% |  |
| G18 | mixed | FAIL | 517.5 | 284 | 49.7% | missing=ClientSession |
| G20 | mixed | FAIL | 2716.3 | 713 | 0.0% | missing=ClientSession |
| G06 | long_term | FAIL | 2130.1 | 987 | 0.0% | missing=BLUEBIRD-42, TypeScript, NestJS |
| G07 | long_term | FAIL | 2122.5 | 965 | 0.0% | missing=BLUEBIRD-42, TypeScript, NestJS |
| G17 | mixed | FAIL | 2378.7 | 568 | 10.1% | missing=TypeScript, NestJS |

## Evidence excerpts

### G01 - short_term

`<SESSION_SUMMARY> user: Constraint HOLD-ALPHA-0900: standup is 09:00 sharp and must not be forgotten. | assistant: Noted standup constraint. | user: Constraint HOLD-BETA-STAGING: writes go to staging DB only. | assistant: Noted staging constraint. | user: Filler A about button padding. | assistant: Filler A. | user: Filler B about color tokens. | assistant: Filler B. | user: Filler C about copy tone. | assistant: Filler C. </SESSION_SUMMARY> <DURABLE_NOTES> - user: Constraint HOLD-ALPHA-0900: standup is 09:00 sharp and must not be forgotten. - assistant: Noted standup constraint. - user: Constraint HOLD-BETA-STAGING: writes go to staging DB only. - assistant: Noted staging constraint. </DURA`

### G02 - short_term

`<RECENT_TURNS> user: Ten du an ca nhan cua toi la ORCHID-27. Toi thich Python va khong thich Java. Khi giai thich code, hay dung vi du ngan. assistant: Da hieu: demo ca nhan ORCHID-27, uu tien Python, tranh Java, vi du ngan. user: Toi dang hoc async/await va hay nham coroutine voi Task. Neu sau nay gap chu de nay, hay giai thich bang timeline. assistant: Toi se uu tien timeline khi giai thich coroutine va Task. user: TODO: hoan thanh benchmark report truoc thu Sau luc 16:00. Day la open loop LAB-REPORT-1600. </RECENT_TURNS>`

### G08 - long_term

`<USER_SUMMARY> Lan's project is LOTUS-88. They prioritize Java and Spring Boot for backend examples and do not use Python for the backend. </USER_SUMMARY>  <EPISODES> Episodes are source message or document excerpts shown in selection order.   - Created At: 2026-08-01 11:00:00     Source: message     Content: [user] {   "user_id": "lan-lab17",   "first_name": "Lan",   "last_name": "Tran",   "user_alias": "Lan Tran" }: Toi la Lan. Du an cua toi la LOTUS-88. Toi uu tien Java va Spring Boot, va khong dung Python trong vi du backend.   - Created At: 2026-08-01 11:00:20     Source: message     Content: Lab Assistant (assistant): Da hieu: LOTUS-88, Java + Spring Boot cho backend examples. </EPISOD`

### G09 - long_term

`<USER_SUMMARY> Minh Nguyen's personal project is named ORCHID-27.  Minh Nguyen prefers Python and dislikes Java.  When explaining coroutine and Task, the AI will prioritize using a timeline. The AI will use short examples when explaining code. </USER_SUMMARY>  <EPISODES> Episodes are source message or document excerpts shown in selection order.   - Created At: 2026-08-01 09:00:00     Source: message     Content: [user] {   "user_id": "minh-lab17",   "first_name": "Minh",   "last_name": "Nguyen",   "user_alias": "Minh Nguyen" }: Ten du an ca nhan cua toi la ORCHID-27. Toi thich Python va khong thich Java. Khi giai thich code, hay dung vi du ngan.   - Created At: 2026-08-01 09:02:20     Source`

### G12 - semantic

`EPISODE: {"id":"kb-memory-privacy","entity":"Agent Memory Privacy Rule","summary":"Do not persist personal data without explicit opt-in. A deletion request must remove user-scoped memory and be verified across every store. Marker: DELETE-VERIFY-ALL.","source":"memory-governance-policy","updated_at":"2026-08-12T00:00:00Z"} metadata= EPISODE: {"id":"kb-payment-retry","entity":"Payment API Retry Policy","summary":"For POST /payments, every retryable request MUST send the same Idempotency-Key. Retry only HTTP 429 or transient 5xx errors, use exponential-backoff, and stop after max-3-retries. Marker: PAYMENT-RULE-3.","source":"internal-api-guideline-v3","updated_at":"2026-08-10T00:00:00Z"} metada`

### G14 - semantic

`EPISODE: {"id":"kb-memory-privacy","entity":"Agent Memory Privacy Rule","summary":"Do not persist personal data without explicit opt-in. A deletion request must remove user-scoped memory and be verified across every store. Marker: DELETE-VERIFY-ALL.","source":"memory-governance-policy","updated_at":"2026-08-12T00:00:00Z"} metadata=`

### G15 - semantic

`EPISODE: {"id":"kb-memory-privacy","entity":"Agent Memory Privacy Rule","summary":"Do not persist personal data without explicit opt-in. A deletion request must remove user-scoped memory and be verified across every store. Marker: DELETE-VERIFY-ALL.","source":"memory-governance-policy","updated_at":"2026-08-12T00:00:00Z"} metadata=`

### G19 - mixed

`<LONG_TERM> <USER_SUMMARY> Lan's project is LOTUS-88. They prioritize Java and Spring Boot for backend examples and do not use Python for the backend. </USER_SUMMARY>  <EPISODES> Episodes are source message or document excerpts shown in selection order.   - Created At: 2026-08-01 11:00:00     Source: message     Content: [user] {   "user_id": "lan-lab17",   "first_name": "Lan",   "last_name": "Tran",   "user_alias": "Lan Tran" }: Toi la Lan. Du an cua toi la LOTUS-88. Toi uu tien Java va Spring Boot, va khong dung Python trong vi du backend.   - Created At: 2026-08-01 11:00:20     Source: message     Content: Lab Assistant (assistant): Da hieu: LOTUS-88, Java + Spring Boot cho backend exampl`

### G03 - long_term

`<USER_SUMMARY> Minh Nguyen's personal project is named ORCHID-27.  Minh Nguyen prefers Python and dislikes Java.  When explaining coroutine and Task, the AI will prioritize using a timeline. The AI will use short examples when explaining code. </USER_SUMMARY>  <EPISODES> Episodes are source message or document excerpts shown in selection order.   - Created At: 2026-08-01 09:00:00     Source: message     Content: [user] {   "user_id": "minh-lab17",   "first_name": "Minh",   "last_name": "Nguyen",   "user_alias": "Minh Nguyen" }: Ten du an ca nhan cua toi la ORCHID-27. Toi thich Python va khong thich Java. Khi giai thich code, hay dung vi du ngan.   - Created At: 2026-08-01 09:00:20     Source`

### G04 - long_term

`<USER_SUMMARY> Minh Nguyen's personal project is named ORCHID-27.  Minh Nguyen prefers Python and dislikes Java.  When explaining coroutine and Task, the AI will prioritize using a timeline. The AI will use short examples when explaining code. </USER_SUMMARY>  <EPISODES> Episodes are source message or document excerpts shown in selection order.   - Created At: 2026-08-01 09:04:00     Source: message     Content: [user] {   "user_id": "minh-lab17",   "first_name": "Minh",   "last_name": "Nguyen",   "user_alias": "Minh Nguyen" }: TODO: hoan thanh benchmark report truoc thu Sau luc 16:00. Day la open loop LAB-REPORT-1600.   - Created At: 2026-08-01 09:02:20     Source: message     Content: Lab `

### G05 - long_term

`<USER_SUMMARY> Minh Nguyen's personal project is named ORCHID-27.  Minh Nguyen prefers Python and dislikes Java.  When explaining coroutine and Task, the AI will prioritize using a timeline. The AI will use short examples when explaining code. </USER_SUMMARY>  <EPISODES> Episodes are source message or document excerpts shown in selection order.   - Created At: 2026-08-01 09:02:00     Source: message     Content: [user] {   "user_id": "minh-lab17",   "first_name": "Minh",   "last_name": "Nguyen",   "user_alias": "Minh Nguyen" }: Toi dang hoc async/await va hay nham coroutine voi Task. Neu sau nay gap chu de nay, hay giai thich bang timeline.   - Created At: 2026-08-01 09:02:20     Source: mes`

### G10 - episodic

`EPISODE: TODO: hoan thanh benchmark report truoc thu Sau luc 16:00. Day la open loop LAB-REPORT-1600. EPISODE: Ten du an ca nhan cua toi la ORCHID-27. Toi thich Python va khong thich Java. Khi giai thich code, hay dung vi du ngan. EPISODE: Da hieu: demo ca nhan ORCHID-27, uu tien Python, tranh Java, vi du ngan. EPISODE: Toi dang hoc async/await va hay nham coroutine voi Task. Neu sau nay gap chu de nay, hay giai thich bang timeline. EPISODE: Toi se uu tien timeline khi giai thich coroutine va Task.`

### G11 - episodic

`EPISODE: TODO: hoan thanh benchmark report truoc thu Sau luc 16:00. Day la open loop LAB-REPORT-1600. EPISODE: Ten du an ca nhan cua toi la ORCHID-27. Toi thich Python va khong thich Java. Khi giai thich code, hay dung vi du ngan. EPISODE: Da hieu: demo ca nhan ORCHID-27, uu tien Python, tranh Java, vi du ngan. EPISODE: Toi dang hoc async/await va hay nham coroutine voi Task. Neu sau nay gap chu de nay, hay giai thich bang timeline. EPISODE: Toi se uu tien timeline khi giai thich coroutine va Task.`

### G13 - semantic

`EPISODE: {"id":"kb-memory-privacy","entity":"Agent Memory Privacy Rule","summary":"Do not persist personal data without explicit opt-in. A deletion request must remove user-scoped memory and be verified across every store. Marker: DELETE-VERIFY-ALL.","source":"memory-governance-policy","updated_at":"2026-08-12T00:00:00Z"} metadata= EPISODE: {"id":"kb-async-http","entity":"Async HTTP Incident Playbook","summary":"When async HTTP calls time out, inspect connection pooling, downstream saturation and concurrency before increasing timeout. Reuse a long-lived client session where possible. Marker: CONN-POOL-FIRST.","source":"incident-playbook-2026","updated_at":"2026-08-11T00:00:00Z"} metadata= EP`

### G16 - mixed

`<LONG_TERM> <USER_SUMMARY> Minh Nguyen's personal project is named ORCHID-27.  Minh Nguyen prefers Python and dislikes Java.  When explaining coroutine and Task, the AI will prioritize using a timeline. The AI will use short examples when explaining code. </USER_SUMMARY>  <EPISODES> Episodes are source message or document excerpts shown in selection order.   - Created At: 2026-08-01 09:00:00     Source: message     Content: [user] {   "user_id": "minh-lab17",   "first_name": "Minh",   "last_name": "Nguyen",   "user_alias": "Minh Nguyen" }: Ten du an ca nhan cua toi la ORCHID-27. Toi thich Python va khong thich Java. Khi giai thich code, hay dung vi du ngan.   - Created At: 2026-08-01 09:00:2`

### G18 - mixed

`<EPISODIC> EPISODE: TODO: hoan thanh benchmark report truoc thu Sau luc 16:00. Day la open loop LAB-REPORT-1600. EPISODE: Ten du an ca nhan cua toi la ORCHID-27. Toi thich Python va khong thich Java. Khi giai thich code, hay dung vi du ngan. EPISODE: Da hieu: demo ca nhan ORCHID-27, uu tien Python, tranh Java, vi du ngan. EPISODE: Toi dang hoc async/await va hay nham coroutine voi Task. Neu sau nay gap chu de nay, hay giai thich bang timeline. EPISODE: Toi se uu tien timeline khi giai thich coroutine va Task. </EPISODIC>  <SEMANTIC> EPISODE: {"id":"kb-async-http","entity":"Async HTTP Incident Playbook","summary":"When async HTTP calls time out, inspect connection pooling, downstream saturati`

### G20 - mixed

`<LONG_TERM> <USER_SUMMARY> Minh Nguyen's personal project is named ORCHID-27.  Minh Nguyen prefers Python and dislikes Java.  When explaining coroutine and Task, the AI will prioritize using a timeline. The AI will use short examples when explaining code. </USER_SUMMARY>  <EPISODES> Episodes are source message or document excerpts shown in selection order.   - Created At: 2026-08-01 09:02:00     Source: message     Content: [user] {   "user_id": "minh-lab17",   "first_name": "Minh",   "last_name": "Nguyen",   "user_alias": "Minh Nguyen" }: Toi dang hoc async/await va hay nham coroutine voi Task. Neu sau nay gap chu de nay, hay giai thich bang timeline.   - Created At: 2026-08-01 09:00:20    `

### G06 - long_term

`<USER_SUMMARY> Minh Nguyen's personal project is named ORCHID-27.  Minh Nguyen prefers Python and dislikes Java.  When explaining coroutine and Task, the AI will prioritize using a timeline. The AI will use short examples when explaining code. </USER_SUMMARY>  <EPISODES> Episodes are source message or document excerpts shown in selection order.   - Created At: 2026-08-01 09:00:00     Source: message     Content: [user] {   "user_id": "minh-lab17",   "first_name": "Minh",   "last_name": "Nguyen",   "user_alias": "Minh Nguyen" }: Ten du an ca nhan cua toi la ORCHID-27. Toi thich Python va khong thich Java. Khi giai thich code, hay dung vi du ngan.   - Created At: 2026-08-01 09:02:20     Source`

### G07 - long_term

`<USER_SUMMARY> Minh Nguyen's personal project is named ORCHID-27.  Minh Nguyen prefers Python and dislikes Java.  When explaining coroutine and Task, the AI will prioritize using a timeline. The AI will use short examples when explaining code. </USER_SUMMARY>  <EPISODES> Episodes are source message or document excerpts shown in selection order.   - Created At: 2026-08-01 09:00:00     Source: message     Content: [user] {   "user_id": "minh-lab17",   "first_name": "Minh",   "last_name": "Nguyen",   "user_alias": "Minh Nguyen" }: Ten du an ca nhan cua toi la ORCHID-27. Toi thich Python va khong thich Java. Khi giai thich code, hay dung vi du ngan.   - Created At: 2026-08-01 09:00:20     Source`

### G17 - mixed

`<LONG_TERM> <USER_SUMMARY> Minh Nguyen's personal project is named ORCHID-27.  Minh Nguyen prefers Python and dislikes Java.  When explaining coroutine and Task, the AI will prioritize using a timeline. The AI will use short examples when explaining code. </USER_SUMMARY>  <EPISODES> Episodes are source message or document excerpts shown in selection order.   - Created At: 2026-08-01 09:00:00     Source: message     Content: [user] {   "user_id": "minh-lab17",   "first_name": "Minh",   "last_name": "Nguyen",   "user_alias": "Minh Nguyen" }: Ten du an ca nhan cua toi la ORCHID-27. Toi thich Python va khong thich Java. Khi giai thich code, hay dung vi du ngan.   - Created At: 2026-08-01 09:02:2`
