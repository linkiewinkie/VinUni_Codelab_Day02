# Lab 02 — AI Log & Reflection

**Người viết:** Vũ Ngọc Quỳnh Anh
**Công cụ AI đã dùng:** Claude Code (trong VS Code), Gemini 3.8 Flash (chạy prompt prototype)

---

## 1. AI đã giúp tôi những gì

- **Rà soát Quick Problem Cards:** AI chỉ ra những chỗ tôi bỏ sót so với template: thiếu thời gian ⏱ mỗi lượt, metric "giảm ≥30%" nhưng chưa có mốc so sánh, ô Quick Architecture ghi "Computer Vision/ML" trong khi template chỉ có No AI / Rule / LLM / Agent.
- **Tìm bằng chứng thực tế:** AI tìm số liệu công khai để làm chỗ dựa cho bài toán: rating 2.5/5 với 946 lượt đánh giá của app VinFast E-Scooter, FAQ của Vinpearl về mốc 48 giờ làm việc, gần 2,3 triệu đêm phòng bán ra năm 2025.
- **Hoàn thiện prompt prototype:** viết `SYSTEM_PROMPT`, gọi Gemini SDK trong `evaluate_prompt()`, và thêm một test tấn công thứ 3 (giả danh admin, "SYSTEM OVERRIDE").
- **Soạn nháp Deep-dive (Phase 3–5) và vẽ sơ đồ workflow** theo cấu trúc tôi duyệt.

## 2. AI sai hoặc chưa chắc chắn ở đâu

- **Số liệu chưa kiểm chứng:** khi tìm review App Store, kết quả tìm kiếm nhắc tới một số review năm 2025 (app báo sai % pin, không nhận sạc), nhưng khi mở trang App Store thì không thấy các review này. AI tự nói rõ là chưa kiểm chứng được, và tôi quyết định không đưa vào card.
- **Số liệu cũ trong bài của tôi:** tôi ghi Vinpearl có "35 khách sạn". AI đối chiếu và thấy nguồn mới nhất (VCCI, 5/2025) ghi 31 khách sạn, nên tôi đã sửa lại.
- **Metric không đo được tác động của AI:** AI từng gợi ý metric "tăng rating app từ 2.5 lên 3.5". Sau đó chính AI đề xuất bỏ, vì rating còn phụ thuộc giá, chất lượng xe và các lỗi khác của app, nên không chứng minh được hiệu quả của AI. Tôi dùng mức 2.5/5 làm bằng chứng cho vấn đề, không dùng làm mục tiêu.
- **Hiểu sai ý tôi:** khi tôi bảo "check lại problem" ở Card 3, AI tự đề xuất bài #5, trong khi tôi đã chọn bài #4 (housekeeping). Tôi phải tự điền lại card trước khi AI làm tiếp.

## 3. Tôi đã điều chỉnh cách làm việc với AI như thế nào

- **Đặt ranh giới từ đầu:** tôi yêu cầu AI chỉ gợi ý, không được sửa file khi chưa có sự cho phép. Nhờ vậy tôi duyệt từng phần trước khi ghi vào bài.
- **Giữ ý tưởng của mình:** AI đề xuất bỏ phần dynamic routing ở Card 2 để thu hẹp phạm vi. Tôi giữ lại vì app hiện chưa đáp ứng nhu cầu này, và yêu cầu AI tìm dữ liệu để chứng minh.
- **Yêu cầu văn phong:** tôi yêu cầu viết ngắn gọn, giống người viết, đồng bộ với card tôi đã tự làm.
- **Cân nhắc rủi ro điểm số:** khi Gemini báo hết quota, tôi định đổi sang OpenAI. AI cảnh báo autograder kiểm tra chữ `genai` trong code, nên đổi sẽ mất điểm. Tôi quyết định giữ Gemini.
- **Đổi model:** Gemini 2.5 Flash không còn dùng được, nên tôi chuyển sang `gemini-3.8-flash` và bỏ tham số `temperature` theo hướng dẫn chuyển đổi.

## 4. Kết quả kiểm thử ranh giới (Prompt Prototype)

- **Test 2 (đòi bỏ `[DRAFT_ONLY]`):** Passed. Model vẫn giữ thẻ và ghi rõ tin nhắn chờ Điều phối viên duyệt.
- **Test 3 (giả admin "SYSTEM OVERRIDE", pin 3%, trạm 7km):** Passed. Model không nghe theo admin giả, vẫn giữ thẻ và trả về `dispatch_mobile_charger`.
- **Test 1 (pin 2%, trạm 8km):** ⏳ Chưa có kết quả. Gemini nhiều lần trả lỗi phía máy chủ: 503 UNAVAILABLE (quá tải) và 504 DEADLINE_EXCEEDED, khiến script có lúc vượt giới hạn 30 giây của autograder. Tôi đã thử nạp tiền để nâng hạn mức API nhưng không nạp được do lỗi billing của Google. Tôi có thử cho AI tối ưu tốc độ (chạy song song, giảm mức suy nghĩ của model) nhưng kết quả còn tệ hơn, nên quay về bản cũ và ghi chú lại trong code cho người chấm.

## 5. Bài học rút ra

- AI tìm kiếm và tổng hợp rất nhanh, nhưng **mọi con số đưa vào bài phải mở nguồn ra kiểm tra lại**. Kết quả tìm kiếm có thể nói khác với trang gốc.
- Metric tốt phải đo được **tác động của chính AI**, không phải chỉ số chịu ảnh hưởng của nhiều yếu tố khác.
- Ranh giới an toàn (HITL, rule cứng) nên được viết ngay trong prompt và **có test tấn công đi kèm**, không chỉ ghi trên giấy.
