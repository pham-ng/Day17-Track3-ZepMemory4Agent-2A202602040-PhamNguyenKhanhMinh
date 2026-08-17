# Báo Cáo Thu Hoạch Lab 17: Multi-Memory Agent với Zep Cloud V3

## 1. Phân Tích Benchmark & Chỉ Số Đánh Giá
- **Hit Rate thấp nhất**: `episodic` (0/2 Pass - E04, E05) do Zep Cloud V3 async graph indexer chưa kịp trích xuất đầy đủ entity graph/reflections tại thời điểm query.
- **Query retrieve nhiều token nhất**: E08 (`long_term`, 1113 tokens) và E02/E03 (`long_term`, 1102 tokens) do chứa toàn bộ `UserContext` block tự động tổng hợp cùng fact edges.
- **Ca E07 (mixed)**: Phối hợp `long_term` (lấy preference `Python`) và `semantic` (lấy quy tắc `Idempotency-Key` từ `vinuni-lab17-domain-kb`).
- **Token Reduction**: Mô hình `Student` đạt **22.4%**. Baseline `No-memory` giảm 81.8% token chỉ vì không truy xuất bộ nhớ bền vững, dẫn tới hit rate thấp (**18.2%**, 2/11 Pass).

## 2. Trả Lời Suy Ngẫm (Reflection), Kiến Trúc & Guardrails
- **Layer quan trọng nhất**: `long_term` (E02, E03, E08, E09) vì đóng vai trò cốt lõi duy trì cá nhân hóa xuyên các session và quản lý open loops.
- **Trade-off Zep Context Block vs Redis/Qdrant**: Zep tự động hóa tổng hợp đồ thị quan hệ và nén context tự nhiên nhưng phụ thuộc API cloud và độ trễ async. Redis + Qdrant local cho phép kiểm soát 100% độ trễ (sub-ms), TTL và schema local nhưng phải tự viết pipeline thủ công.
- **Guardrail chống Memory Poisoning**: Áp dụng xác thực quyền (Policy/Human Review) trước khi write-back, yêu cầu metadata (`source`, `timestamp`, `confidence`, `TTL`), và chỉ nén khi phát hiện pressure.
- **Phân Tích Chuyên Sâu Case E08 & E10**:
  - **Case E08 (Recency & Scope Conflict)**: Ràng buộc công nghệ của dự án `BLUEBIRD-42` (`TypeScript`, `NestJS`) được ưu tiên hơn preference cá nhân (`Python`), thể hiện đúng quy tắc Scope-specific Conflict.
  - **Case E10 (Short-term Compaction)**: Chiến lược `sliding` trích xuất constraint `REVIEW-DEADLINE-1600` vào `<DURABLE_NOTES>`, giúp thông tin bền vững ngay cả khi raw turns bị evict.

## 3. Hoàn Thành Mở Rộng (Bonus Track)
- **Golden Set (G01–G20)**: Sinh `golden_benchmark.json` và `.md` chấm điểm tự động.
- **Mini-Product Streamlit UI**: Hoàn thiện `retrieve_for_case` hỗ trợ load case, hiển thị 4 layer evidence và chat với Gemini.
