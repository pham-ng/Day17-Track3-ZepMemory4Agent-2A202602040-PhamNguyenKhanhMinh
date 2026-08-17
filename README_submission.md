# Báo Cáo Thu Hoạch Lab 17: Multi-Memory Agent với Zep Cloud V3

## 1. Trả Lời Câu Hỏi Suy Ngẫm (Reflection)

1. **Vì sao `short_term` không bị ảnh hưởng bởi việc xóa durable memory?**
   `short_term` memory quản lý cửa sổ hội thoại gần nhất nằm trực tiếp trong bộ nhớ tạm thời của thread hiện tại (`recent_messages`). Khi xóa durable memory (dữ liệu người dùng trên Zep Cloud/Redis), các tin nhắn trong phiên hội thoại ngắn hạn chưa đẩy vào durable graph hoặc vẫn được giữ trực tiếp trong context window của LLM nên Agent vẫn truy cập bình thường.

2. **Vì sao `episodic` và `semantic` cần hai chiến lược retrieval khác nhau?**
   - `episodic` lưu trữ các sự kiện, trải nghiệm cá nhân nối tiếp theo thời gian của từng `user_id`. Cần tìm kiếm theo phạm vi `episodes` trên user graph để tái hiện ngữ cảnh sự cố/lịch sử cá nhân.
   - `semantic` lưu trữ tri thức miền chung (domain/incident playbooks, rules) áp dụng cho mọi người dùng. Cần tìm kiếm trên độc lập `graph_id` (`vinuni-lab17-domain-kb`) theo phạm vi `episodes`/`nodes` để tra cứu quy trình chuẩn.

3. **Trade-off lớn nhất giữa context budget và latency/cost là gì?**
   Tăng context budget giúp tăng độ bao phủ thông tin (hit rate cao hơn), nhưng làm tăng số lượng token đầu vào làm tăng chi phí API (cost) và thời gian xử lý phản hồi (latency). Ngược lại, nén budget quá mức làm giảm latency/cost nhưng dễ gây trôi mất thông tin quan trọng.

## 2. Phân Tích Kết Quả Benchmark

### Kết Quả So Sánh (`student` vs `no_memory`)

| Metric | Memory-enabled (Student) | No-memory Baseline | Chênh lệch (Delta) |
| --- | ---: | ---: | ---: |
| **Evidence Hit Rate** | **72.7%** (8/11) | **18.2%** (2/11) | **+54.5%** |
| **Số ca Pass** | **8/11** | **2/11** | **+6 ca** |
| **Thời gian phản hồi TB (Latency)** | 1348.8 ms | 0.0 ms | +1348.8 ms |
| **Tỷ lệ giảm Token (Token Reduction)** | 22.4% | 81.8% | -59.4% |

### Phân Tích Chi Tiết
- **Ca Pass (`student`)**:
  - `short_term` (E01, E10): Đạt 100% nhờ lấy trực tiếp `recent_messages`.
  - `semantic` (E06, E11): Đạt 100% nhờ truy vấn chính xác tri thức miền từ standalone graph `vinuni-lab17-domain-kb`.
  - `long_term` & `mixed` (E02, E03, E07, E09): Đạt kết quả xuất sắc nhờ sự phối hợp giữa `user_context` (Context Block) và fact edges.
- **Ca Fail (E04, E05, E08)**: Các ca đòi hỏi truy vết chi tiết mã lỗi phức tạp trùng với thời điểm Zep Cloud V3 async graph indexer chưa hoàn tất trích xuất thực thể.
- **Privacy Drill**: Xác nhận `python -m src.forget --user-id minh-lab17` xóa sạch dữ liệu cá nhân (`Zep user absent: True`), trong khi tri thức chung `vinuni-lab17-domain-kb` được bảo vệ nguyên vẹn.
