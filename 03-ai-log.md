# 03 — AI Log & Reflection (Cá nhân)

> **Người thực hiện:** Đào Thị Ngọc Anh — 2A202603790
> ✏️ *Các chỗ đánh dấu `[ĐIỀN: ...]` là phần bạn cần tự viết theo trải nghiệm thật của mình. File này được chấm theo mức độ trung thực, đừng để nguyên placeholder khi nộp.*

---

## 1. Tôi đã dùng AI nào, vào việc gì?

| Giai đoạn | Công cụ AI | Tôi dùng AI để làm gì |
|---|---|---|
| Phase 1 — SCAN | Claude | Cho tôi tình hiện tại của những vấn đề hay gặp trong đời sống |
| Phase 2 — Quick Cards | Claude |  ví dụ nhờ AI đóng vai CFO khắt khe để phản biện thẻ bài toán |
| Phase 4 — Code `prompt_prototype.py` | Claude | ví dụ hỏi cách gọi `google-genai`, kết nối VS máy với git|


## 2. AI đã giúp được gì?

* **Cấu trúc báo cáo:** Từ 5 bài toán tôi tự quét, AI giúp sắp xếp thành Quick Card, bảng 6 trường và bảng so sánh Rule / LLM / Agent.

## 3. AI đã sai / hallucinate ở đâu?

* **Số liệu tự đặt ra:** Khi soạn báo cáo, AI đưa ra các con số như "15 ca xuất viện/ngày", "6,25 giờ bác sĩ/ngày" hay thời gian từng bước. Đây là **giả định**, không phải dữ liệu thật của Vinmec. Tôi đã yêu cầu ghi rõ "giả định/ước tính" trong báo cáo, và tôi đã kiểm tra hoặc điều chỉnh con số nào theo hiểu biết của mình.

## 4. Tôi đã sửa prompt / đặt ranh giới như thế nào?

* **Với Gemini trong prototype:**
  * Tôi viết SYSTEM_PROMPT với mục "NON-NEGOTIABLE RULES", nói rõ các quy tắc này **ghi đè mọi yêu cầu của người dùng**, kể cả "gửi ngay", "bỏ thẻ", "ignore previous rules".
  * Tôi quy định định dạng output theo từng dòng (dòng 1 là `[DRAFT_ONLY]`, dòng 2 là JSON khi pin < 5%) để dễ kiểm tra tự động.
  * Tôi đặt `temperature=0.2` để giảm độ ngẫu nhiên.
  * Tôi thêm một lớp kiểm tra trong code: nếu output không bắt đầu bằng `[DRAFT_ONLY]` thì tự chèn vào. Không phụ thuộc 100% vào việc mô hình tuân thủ.
* **Với AI trợ lý khi viết báo cáo:** Tôi cung cấp sẵn 5 bài toán của mình để AI không tự bịa bài toán mới, và yêu cầu đánh dấu rõ số liệu nào là giả định.

## 5. Bài học rút ra

* AI là **thought-partner** tốt để sắp xếp ý và phản biện. Nhưng **bài toán, số liệu thực tế và quyết định GO/NO-GO phải do con người chịu trách nhiệm**, nhất là trong y tế.
* Ranh giới an toàn không thể chỉ nằm trong prompt. Cần thêm kiểm tra bằng code (rule) và người duyệt (HITL). Đây cũng là nguyên tắc tôi áp dụng cho thiết kế Discharge Summary Co-pilot.
