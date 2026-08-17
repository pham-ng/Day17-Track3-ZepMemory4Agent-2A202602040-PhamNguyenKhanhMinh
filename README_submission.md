# Báo Cáo Thu Hoạch Lab 17: Multi-Memory Agent với Zep Cloud V3

## 1. Reflection

1. **Vì sao `short_term` không bị ảnh hưởng khi xóa durable memory?**
   `short_term` memory lưu ở `recent_messages`. Khi xóa durable memory trên cloud, tin nhắn ngắn hạn chưa bị nén vẫn tồn tại trong context window nên Agent vẫn truy cập bình thường.

2. **Vì sao `episodic` và `semantic` cần hai chiến lược retrieval khác nhau?**
   - `episodic`: Lưu sự kiện cá nhân theo thời gian của từng `user_id`, cần truy vấn scope `episodes` trên user graph.
   - `semantic`: Lưu tri thức miền chung (incident playbooks, rules) cho mọi người dùng, cần truy vấn scope `episodes`/`nodes` trên standalone `graph_id` (`vinuni-lab17-domain-kb`).

3. **Trade-off giữa context budget và latency/cost?**
   Tăng budget giúp tăng hit rate nhưng làm tăng token, chi phí API và độ trễ (latency). Giảm budget tiết kiệm chi phí nhưng dễ bỏ sót thông tin.

4. **Short-term Compaction & lý do Buffer không bền vững?**
   Chiến lược `buffer` giữ toàn bộ lịch sử khiến token bùng nổ. `sliding` kết hợp `extract_durable_notes` chủ động trích xuất các constraint (như `REVIEW-DEADLINE-1600`) vào `<DURABLE_NOTES>`, giúp giữ thông tin ngay cả khi lượt thoại thô bị evict.

## 2. Benchmark Analysis

### So Sánh (`student` vs `no_memory`)

| Metric | Student (Memory) | No-memory Baseline | Delta |
| --- | ---: | ---: | ---: |
| **Evidence Hit Rate** | **72.7%** (8/11) | **18.2%** (2/11) | **+54.5%** |
| **Số ca Pass** | **8/11** | **2/11** | **+6 ca** |
| **Latency TB** | 1348.8 ms | 0.0 ms | +1348.8 ms |
| **Giảm Token** | 22.4% | 81.8% | -59.4% |

### Phân Tích Chi Tiết
- **Ca Pass**: `short_term` (E01, E10) và `semantic` (E06, E11) đạt 100%. `long_term` và `mixed` (E02, E03, E07, E09) đạt nhờ trích xuất `user_context` và fact edges từ Zep.
- **Ca Fail (E04, E05, E08)**: Do Zep Cloud V3 async graph indexer chưa kịp trích xuất đầy đủ entity graph tại thời điểm truy vấn.
- **Privacy Drill**: Lệnh `python -m src.forget --user-id minh-lab17` xóa sạch dữ liệu cá nhân (`Zep user absent: True`), giữ nguyên vẹn tri thức miền chung `vinuni-lab17-domain-kb`.
