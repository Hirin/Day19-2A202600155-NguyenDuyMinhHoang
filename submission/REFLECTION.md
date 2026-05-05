# Reflection — Lab 19

**Tên:** Nguyễn Duy Minh Hoàng
**MSSV:** 2A202600155
**Cohort:** A20-K1
**Path đã chạy:** both

---

## Câu hỏi (≤ 200 chữ)

> Trên golden set 50 queries, mode nào thắng ở loại query nào (`exact` /
> `paraphrase` / `mixed`), và tại sao? Khi nào bạn **không** dùng hybrid
> (i.e. khi nào pure BM25 hoặc pure vector là lựa chọn đúng)?

Kết quả benchmark cho thấy mỗi mode có thế mạnh riêng:

- **Exact queries** (n=15): BM25 keyword thắng (96.7%) vì query chứa đúng từ khóa có trong document. Vector search (88.7%) bị nhiễu do embedding gộp các khái niệm tương tự nhau. Hybrid đạt ngang BM25 (96.7%) nhờ RRF giữ lại tín hiệu keyword.
- **Paraphrase queries** (n=15): Cả 3 mode đều thấp (kw: 33.3%, sem: 24.0%, hyb: 32.0%) vì corpus có 100 docs/topic nên paraphrase dễ lạc sang topic khác. BM25 lại nhỉnh hơn vector ở đây do fastembed (all-MiniLM-L6-v2) chưa tối ưu cho tiếng Việt.
- **Mixed queries** (n=20): Hybrid chiến thắng tuyệt đối (100%) vì kết hợp được cả tín hiệu lexical và semantic. Đây chính là kịch bản thực tế nhất.

**Khi nào KHÔNG dùng hybrid?**
1. **Latency-critical** (P99 < 5ms): Pure BM25 chỉ ~5ms, hybrid ~32ms. Nếu cần tốc độ tối đa, BM25 là lựa chọn.
2. **Query hoàn toàn chính xác** (mã sản phẩm, ID): BM25 đủ tốt, vector search gây nhiễu không cần thiết.

---

## Điều ngạc nhiên nhất khi làm lab này

Fastembed all-MiniLM-L6-v2 chạy hoàn toàn trên CPU mà vẫn đạt P99 < 35ms cho hybrid search trên 1000 docs — nhanh hơn kỳ vọng rất nhiều. Feast materialize với SQLite online store cũng cho P99 < 10ms, chứng minh lightweight stack đủ mạnh cho prototype.

---

## Bonus challenge

- [ ] Đã làm bonus (xem `bonus/`)
- [ ] Pair work với: _không_
