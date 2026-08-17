# Báo Cáo Thu Hoạch Lab 17: Multi-Memory Agent với Zep Cloud V3

## 1. Phân Tích Benchmark
- **Layer có Hit Rate thấp nhất**: `episodic` (0/2 Pass - E04, E05) do Zep Cloud V3 async graph indexer xử lý bất đồng bộ, chưa kịp trích xuất đầy đủ entity graph/reflections tại thời điểm query.
- **Query retrieve nhiều token nhất**: E08 (`long_term`, 1113 tokens) và E02/E03 (`long_term`, 1102 tokens) do chứa toàn bộ `UserContext` block tự động tổng hợp cùng danh sách fact edges.
- **Ca E07 (mixed)**: Phối hợp `long_term` (truy xuất preference `Python` của `minh-lab17`) và `semantic` (truy xuất quy tắc `Idempotency-Key` từ `vinuni-lab17-domain-kb`).
- **Token Reduction**: Mô hình `Student` đạt **22.4%**. Baseline `No-memory` báo giảm 81.8% token chỉ vì hoàn toàn không truy xuất bộ nhớ bền vững, dẫn tới hit rate cực thấp (**18.2%**, 2/11 Pass).

## 2. Trả Lời Suy Ngẫm (Reflection) & Kiến Trúc
- **Layer quan trọng nhất**: `long_term` (E02, E03, E08, E09) vì đóng vai trò cốt lõi trong việc duy trì tính cá nhân hóa xuyên suốt các session và quản lý các open loop/task chưa đóng.
- **Trade-off Zep Context Block vs Redis/Qdrant**: Zep Managed Service tự động hóa việc tổng hợp đồ thị quan hệ, trích xuất thực thể và nén context tự nhiên nhưng phụ thuộc API cloud và độ trễ async. Redis + Qdrant local cho phép kiểm soát 100% độ trễ (sub-ms), TTL và schema local nhưng đòi hỏi tự phát triển pipeline trích xuất thủ công.
- **Guardrail chống Memory Poisoning**: Áp dụng xác thực quyền (Policy/Human Review) trước khi write-back, yêu cầu đầy đủ metadata (`source`, `timestamp`, `confidence`, `TTL`), và chỉ tiến hành nén khi phát hiện áp lực (pressure detection).
- **Phân Tích Chuyên Sâu Case E08 & E10**:
  - **Case E08 (Recency & Scope Conflict)**: Ràng buộc công nghệ riêng của dự án `BLUEBIRD-42` (`TypeScript`, `NestJS`) được ưu tiên cao hơn preference cá nhân chung (`Python`), thể hiện đúng quy tắc Scope-specific Conflict.
  - **Case E10 (Short-term Compaction)**: Chiến lược `sliding` trích xuất constraint `REVIEW-DEADLINE-1600` vào `<DURABLE_NOTES>`, giúp thông tin bền vững ngay cả khi các lượt thoại thô cũ bị evict khỏi recent turns.
