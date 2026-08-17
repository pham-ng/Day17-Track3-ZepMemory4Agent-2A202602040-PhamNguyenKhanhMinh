# Báo Cáo Thu Hoạch Lab 17: Multi-Memory Agent với Zep Cloud V3

## 1. Luồng Xử Lý Kiến Trúc System Workflow
Toàn bộ hệ thống Multi-Memory Agent được xây dựng và vận hành theo chuỗi luồng xử lý khép kín:
1. **Ingestion & Data Preparation**: Tệp `sessions.json` và `knowledge.jsonl` được ingest vào Zep Cloud V3 qua API `user.add`, `thread.create`, `thread.add_messages`, và tạo Standalone Semantic Graph `vinuni-lab17-domain-kb`.
2. **User Query & Memory Router**: Khi nhận câu hỏi, LangGraph Memory Router phân tích intent để quyết định các layer bộ nhớ cần truy xuất.
3. **Retrieval Across 4 Memory Layers**:
   - **Short-term Memory**: Quản lý bằng local `ShortTermMemory` chiến lược `sliding window`, tự động nén khi vượt `max_recent_messages` và bảo toàn thông tin bền vững trong khối `<DURABLE_NOTES>`.
   - **Long-term Memory**: Gọi `client.thread.get_user_context` trích xuất `UserContext` block tự nhiên, kết hợp `graph.search(scope="edges")` tìm các quan hệ thực thể cá nhân theo `user_id`.
   - **Episodic Memory**: Gọi `graph.search(user_id=user_id, scope="episodes")` để truy vết hành trình thử nghiệm, kết quả và bài học rút ra (`trajectory`, `outcome`, `reflection`).
   - **Semantic Memory**: Gọi `graph.search(graph_id=semantic_graph_id, scope="episodes")` trên tri thức miền chung để bảo toàn chính xác mã quy tắc literal (`PAYMENT-RULE-3`, `CONN-POOL-FIRST`).
4. **Budget Assembly & Output**: `ContextBudgetManager` nén và cắt bớt ngữ cảnh theo hạn mức nghiêm ngặt `10% Short-term / 4% Long-term / 3% Episodic / 3% Semantic` theo thứ tự ưu tiên, tạo ra `Merged Context` grounding cho Evaluator và Streamlit Demo UI.

---

## 2. Phân Tích Benchmark Chi Tiết & Nguyên Nhân Case Tốt / Case Xấu

### A. Phân Tích Chỉ Số Đánh Giá
- **Hit Rate Tối Ưu**: Mô hình `Student` đạt **8/11 PASS (72.7% Hit Rate)**, tăng vượt bậc **+54.5% Delta** so với baseline `No-memory` (chỉ đạt **2/11 PASS - 18.2%**).
- **Token Reduction**: `Student` đạt tỷ lệ nén **22.4%**. Baseline `No-memory` báo nén 81.8% token chỉ vì hoàn toàn bỏ qua các lớp bộ nhớ bền vững, dẫn tới không có thông tin để trả lời và trượt 9/11 ca.
- **Layer có Hit Rate thấp nhất**: `episodic` (0/2 Pass - E04, E05).
- **Query retrieve nhiều token nhất**: Ca E08 (`long_term`, 1113 tokens) và E02/E03 (`long_term`, 1102 tokens) do khối `UserContext` block của Zep chứa toàn bộ tổng hợp lịch sử người dùng.

### B. Nguyên Nhân Ca Tốt (PASS - E01, E02, E03, E06, E07, E09, E10, E11)
- **E01 & E10 (Short-term Pass)**: Do local `ShortTermMemory` áp dụng đúng chiến lược `sliding`. Trong E10, dù các lượt thoại cũ bị evict, hàm `extract_durable_notes` vẫn trích xuất thành công `REVIEW-DEADLINE-1600` vào `<DURABLE_NOTES>`.
- **E02 & E03 (Long-term Pass)**: Zep `UserContext` block tự động tổng hợp chính xác sở thích lập trình (`Python`) và danh sách việc cần làm (`benchmark report`, `16:00`).
- **E06 & E11 (Semantic Pass)**: Sử dụng `scope="episodes"` giúp giữ nguyên vẹn nội dung văn bản gốc và các mã quy tắc literal (`Idempotency-Key`, `max-3-retries`, `exponential-backoff`, `connection pooling`, `CONN-POOL-FIRST`).
- **E07 (Mixed Pass)**: Phối hợp thành công hai lớp bộ nhớ độc lập: `long_term` (truy xuất sở thích `Python` của `minh-lab17`) và `semantic` (truy xuất quy tắc `Idempotency-Key` từ `vinuni-lab17-domain-kb`).
- **E09 (Isolation Pass)**: Cách ly tuyệt đối bộ nhớ giữa `minh-lab17` và `lan-lab17`. Khi truy vấn cho Lan, hệ thống chỉ lấy thông tin `LOTUS-88`, `Java`, `Spring Boot` và không hề bị rò rỉ mã `ORCHID-27` của Minh.

### C. Nguyên Nhân Ca Xấu (FAIL - E04, E05, E08)
- **E04 & E05 (Episodic Fail)**: Do Zep Cloud V3 async graph indexer xử lý bất đồng bộ ở nền. Tại thời điểm chạy benchmark, graph indexer chưa kịp hoàn tất việc trích xuất và liên kết các node bài học kinh nghiệm (`ClientSession`, `concurrency=20`, `connection churn`), dẫn đến việc tìm kiếm `scope="episodes"` không trả đủ các từ khóa ground truth.
- **E08 (Long-term Recency & Conflict Fail)**: Mặc dù sở thích cá nhân chung của Minh là `Python`, nhưng dự án công ty `BLUEBIRD-42` yêu cầu bắt buộc `TypeScript` & `NestJS`. Do khối `UserContext` tổng hợp quá rộng, từ khóa `BLUEBIRD-42` bị loãng trong context làm cho evaluator chưa khớp đủ cả 3 từ khóa bắt buộc.

---

## 3. Reflection, Trả Lời Suy Ngẫm & Guardrails Kiến Trúc

1. **Layer Quan Trọng Nhất**: `long_term` (E02, E03, E08, E09) vì đóng vai trò quyết định trong việc duy trì tính cá nhân hóa xuyên suốt các session và quản lý các open loop/task chưa đóng.
2. **Trade-off Zep Context Block vs Redis/Qdrant Local**:
   - **Zep Managed Service**: Tự động hóa hoàn toàn việc trích xuất thực thể, xây dựng đồ thị quan hệ và tổng hợp context tự nhiên, nhưng phụ thuộc vào API cloud và chịu ảnh hưởng bởi độ trễ indexing bất đồng bộ.
   - **Redis + Qdrant Local**: Cho phép kiểm soát 100% độ trễ cực thấp (sub-millisecond), chủ động quản lý TTL, schema và tính riêng tư local, nhưng đòi hỏi kỹ sư phải tự thiết kế và duy trì toàn bộ pipeline trích xuất đồ thị thủ công.
3. **Guardrails Chống Memory Poisoning & Background Write**:
   - Áp dụng cơ chế xác thực quyền (Policy/Human-in-the-loop Review) trước khi ghi đè vào bộ nhớ bền vững.
   - Bắt buộc lưu trữ đầy đủ thuộc tính metadata cho mọi bản ghi (`source`, `timestamp`, `confidence`, `TTL`).
   - Kiểm soát quyền hạn trong Heartbeat/Compaction: chỉ cho phép nén, xóa trùng hoặc đánh dấu stale task, tuyệt đối không tự cấp thêm instruction mới vào durable memory.
4. **Bài Học Phân Tích Chuyên Sâu**:
   - **Scope-Specific Conflict (Case E08)**: Ràng buộc công nghệ riêng của dự án (`BLUEBIRD-42`) luôn được ưu tiên hơn sở thích cá nhân chung (`Python`).
   - **Durable Compaction (Case E10)**: Compaction không phải là tóm tắt văn suông mà là tiến hành trích xuất trạng thái bền vững (`<DURABLE_NOTES>`), đảm bảo thông tin quan trọng sống sót qua các phiên hội thoại dài.
