# Báo Cáo Thu Hoạch Lab 17: Multi-Memory Agent với Zep Cloud V3

## 1. Luồng Xử Lý Kiến Trúc (Workflow)
Chat JSON ➔ Ingest (Zep Graph + Standalone Graph) ➔ User Query ➔ LangGraph Memory Router ➔ Truy vấn 4 Memory Layers:
- `Short-term`: Local sliding window + compaction giữ `<DURABLE_NOTES>`.
- `Long-term`: Zep `UserContext` block + fact edges theo `user_id`.
- `Episodic`: Zep user episodes recall trajectory, outcome & reflection.
- `Semantic`: Standalone `vinuni-lab17-domain-kb` scope `episodes` giữ mã literal.
➔ `ContextBudgetManager` phân bổ (10%/4%/3%/3%) ➔ Merged Context ➔ Evaluator/UI.

## 2. Phân Tích Benchmark & Chỉ Số
- **Hit Rate thấp nhất**: `episodic` (0/2 Pass - E04, E05) do Zep Cloud V3 async graph indexer chưa hoàn tất trích xuất entity graph tại thời điểm query.
- **Query retrieve nhiều token nhất**: E08 (`long_term`, 1113 tokens) và E02/E03 (1102 tokens) do chứa full `UserContext` block.
- **Ca E07 (mixed)**: Phối hợp `long_term` (preference `Python`) và `semantic` (`Idempotency-Key`).
- **Token Reduction**: `Student` đạt **22.4%**. `No-memory` giảm 81.8% token nhưng hit rate rất thấp (**18.2%**, 2/11 Pass) do bỏ bộ nhớ bền vững.

## 3. Reflection & Guardrails
- **Layer quan trọng nhất**: `long_term` (E02, E03, E08, E09) duy trì cá nhân hóa và quản lý open loops.
- **Trade-off Zep Context Block vs Redis/Qdrant**: Zep tự động hóa tổng hợp đồ thị quan hệ và nén context tự nhiên nhưng phụ thuộc cloud API. Redis/Qdrant kiểm soát 100% độ trễ (sub-ms), TTL local nhưng phải tự phát triển pipeline thủ công.
- **Guardrail Memory Poisoning**: Xác thực quyền (Policy/Human Review) trước write-back, lưu metadata (`source`, `timestamp`, `confidence`, `TTL`), nén khi phát hiện pressure.
- **Case E08 & E10**: E08 ràng buộc `BLUEBIRD-42` (`TypeScript`, `NestJS`) ưu tiên hơn preference chung (`Python`). E10 `sliding` trích xuất `REVIEW-DEADLINE-1600` vào `<DURABLE_NOTES>`.
