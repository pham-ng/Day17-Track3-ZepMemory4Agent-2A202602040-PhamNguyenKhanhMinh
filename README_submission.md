# Báo Cáo Thu Hoạch Lab 17: Multi-Memory Agent với Zep Cloud V3

## 1. Phân Tích Benchmark
- **Layer có Hit Rate thấp nhất**: `episodic` (0/2 Pass - E04, E05) do Zep Cloud V3 async graph indexer chưa kịp hoàn tất trích xuất entity graph tại thời điểm truy vấn.
- **Query retrieve nhiều token nhất**: E08 (`long_term`, 1113 tokens) và E02/E03 (`long_term`, 1102 tokens) do chứa toàn bộ `UserContext` block và fact edges.
- **Ca E07 (mixed)**: Kết hợp `long_term` (lấy preference `Python`) và `semantic` (lấy quy tắc `Idempotency-Key`).
- **Token Reduction**: `Student` đạt **22.4%**. `No-memory` đạt 81.8% token reduction nhưng hit rate chỉ **18.2%** (2/11 PASS) vì bỏ qua hoàn toàn bộ nhớ bền vững.

## 2. Reflection Bắt Buộc
- **Layer quan trọng nhất**: `long_term` (E02, E03, E08, E09) vì đảm bảo tính cá nhân hóa và theo dõi open loops xuyên các phiên thoại.
- **Trade-off Zep Context Block vs Redis/Qdrant**: Zep tự động tổng hợp đồ thị quan hệ và nén ngữ cảnh dạng văn bản tự nhiên nhưng phụ thuộc API cloud và độ trễ async. Redis/Qdrant làm chủ 100% latency (sub-ms), TTL local nhưng phải tự quản lý pipeline trích xuất thủ công.
- **Guardrail chống Memory Poisoning**: Enforce xác thực quyền (Policy/Human review), lưu kèm metadata (`source`, `timestamp`, `confidence`, `TTL`), và chỉ nén khi phát hiện pressure.
- **E08 Recency & E10 Compaction**: Trong E08, constraint mới của dự án `BLUEBIRD-42` (`TypeScript`, `NestJS`) được ưu tiên cao hơn preference chung (`Python`). Trong E10, cơ chế `sliding` trích xuất deadline `REVIEW-DEADLINE-1600` vào `<DURABLE_NOTES>`, giúp thông tin bền vững kể cả khi raw turns bị evict.
