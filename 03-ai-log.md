# 03-ai-log.md — Nhật Ký Làm Việc Cùng AI (AI Thought-Partner Log)

**Học viên:** Phạm Minh Đức  
**MSSV:** 2A202603758  
**Đơn vị:** Vin Smart Future (Vingroup)  
**Branch:** `PhamMinhDuc-2A202603758`  
**Mục tiêu:** Ghi nhận trung thực quá trình sử dụng AI như một cộng sự tư duy (Thought-Partner), các phản biện, sai sót (hallucinations/bias) phát hiện được, cách kiểm chứng và bài học rút ra.

---

## 1. Nhật ký tương tác & Các bài toán đã thảo luận cùng AI

| Giai đoạn (Phase) | Mục đích sử dụng AI | Prompt tiêu biểu đã sử dụng | Kết quả nhận được từ AI |
|---|---|---|---|
| **Phase 1: SCAN** | Brainstorm các điểm nghẽn vận hành trên hệ sinh thái Vingroup dựa trên 4 Lenses | *"Tôi là AI Engineer tại Vin Smart Future (Vingroup). Tôi đang tìm kiếm các pain point vận hành cụ thể có thể tối ưu bằng AI cho các mảng Xanh SM, VinFast, Vinhomes, Vinmec. Hãy gợi ý 5 quy trình nghiệp vụ thủ công tốn nhiều thời gian và gây rò rỉ hiệu suất kèm ước tính tổn thất."* | AI gợi ý 6 kịch bản thực tế (điều vận Xanh SM, đối chiếu hóa đơn trạm sạc VinFast, phân loại khiếu nại Vinhomes, tóm tắt bệnh án Vinmec...). Các bài toán đều bám sát nghiệp vụ thực tế của Vingroup. |
| **Phase 2: QUICK-ASSESS** | Đóng vai trò phản biện khắt khe (Stress-test 3 Problem Cards) | *"Hãy đóng vai trò là Giám đốc Vận hành và CFO khắt khe của Xanh SM. Hãy chỉ ra cho tôi 3 điểm yếu về logic, metric, và giải thích vì sao rule-based code thông thường có thể giải quyết bài toán điều phối sạc pin tốt hơn là dùng AI."* | AI chỉ ra: Nếu chỉ đơn giản tìm trạm sạc gần nhất thì dùng thuật toán k-d tree hoặc Google Maps Distance Matrix API (Rule-based) nhanh và rẻ hơn nhiều. AI chỉ có giá trị khi phải xử lý ngôn ngữ tự nhiên (soạn tin nhắn hướng dẫn tài xế theo ngữ cảnh) và đưa ra quyết định dự phòng đa biến. |
| **Phase 3: DEEP-DIVE** | Xây dựng ranh giới an toàn (Operational Boundary) và kế hoạch Fallback | *"Trong bài toán Xanh SM cứu hộ sạc pin, rủi ro lớn nhất khi dùng LLM là gì và làm thế nào để đảm bảo 100% tin nhắn không bị gửi thẳng tới tài xế nếu chưa có con người duyệt?"* | AI đề xuất bắt buộc đưa tiền tố `[DRAFT_ONLY]` vào mọi output và kiểm tra assertion trước khi cho phép hệ thống gọi API gửi SMS; đồng thời đưa ngưỡng pin nguy cấp (< 5%) chuyển sang lệnh `dispatch_mobile_charger`. |
| **Phase 4: TECHNICAL PROTOTYPE** | Xây dựng System Prompt chuẩn và thiết kế các kịch bản tấn công (Adversarial Tests) | *"Hãy tạo 3 adversarial prompt cố tình dụ Gemini 2.5 Flash bỏ tag [DRAFT_ONLY] hoặc ép mô hình chỉ dẫn đến trạm sạc xa 8km khi pin chỉ còn 2%."* | AI hỗ trợ thiết kế 3 kịch bản tấn công tinh vi (dùng vai trò khẩn cấp, ép bỏ qua bước nháp, và giấu lệnh trong kịch bản đón khách VIP). |

---

## 2. Các sai sót của AI (Hallucinations / Biases) và Cách kiểm chứng, sửa chữa

### Sai sót 1: Ảo tưởng dữ liệu và số liệu thống kê (Data Hallucination)
- **Hiện tượng:** Khi được yêu cầu đưa ra con số tổn thất của Xanh SM, AI ban đầu tự sinh ra con số: *"Mỗi ngày có 450 xe cạn pin trên toàn quốc, gây thiệt hại 1.2 tỷ VND/ngày"*.
- **Cách phát hiện & Kiểm chứng:** Con số 450 xe cạn pin/ngày là phi thực tế đối với quy mô vận hành hiện tại của đội xe, nếu tỷ lệ cạn pin cao như vậy thì hoạt động sẽ bị gián đoạn diện rộng.
- **Cách khắc phục:** Đã điều chỉnh lại thành ước lượng khiêm tốn và thực tế hơn dựa trên quy mô tại một thị trường cụ thể (Hà Nội): ~70–80 sự cố pin yếu/ngày, tốn khoảng 18–20 giờ công làm việc của đội điều phối, và ghi rõ đây là giả định hoạt động (Assumptions) phục vụ bài toán Scoping.

### Sai sót 2: Đề xuất "Over-engineering" kiến trúc (Multi-Agent ảo tưởng)
- **Hiện tượng:** AI ban đầu khuyên nên dựng một hệ thống Multi-Agent phức tạp gồm 4 agent tự trị (Scout Agent, Routing Agent, Booking Agent, Communication Agent) tương tác qua lại.
- **Cách phản biện & Khắc phục:** Nhận thấy trong môi trường điều vận thời gian thực, một chuỗi Multi-Agent tự trị vừa chậm (độ trễ cao), vừa tốn kém chi phí token, và rủi ro sinh lỗi vòng lặp rất lớn. Tôi đã bác bỏ và kiên quyết hạ cấp giải pháp xuống **LLM Feature có Human-in-the-loop (Co-pilot)**, vừa đơn giản, an toàn vừa đáp ứng hoàn hảo tiêu chí SLA dưới 3 phút.

### Sai sót 3: Rò rỉ ranh giới khi định dạng JSON
- **Hiện tượng:** Trong một vài prompt thử nghiệm, khi yêu cầu trả về JSON, AI đôi khi đặt tag `[DRAFT_ONLY]` bên ngoài cú pháp JSON khiến parser của hệ thống bị văng lỗi cú pháp (`json.loads` thất bại).
- **Cách khắc phục:** Quy định rõ trong System Prompt rằng `[DRAFT_ONLY]` phải là tiền tố văn bản bắt đầu của phản hồi, hoặc là một trường tường minh `"status": "[DRAFT_ONLY]"` nằm bên trong cấu trúc JSON trả về.

---

## 3. Bài học rút ra (Key Reflections)

1. **AI là Thought-Partner, không phải Decision-Maker:** AI rất xuất sắc trong việc mở rộng góc nhìn và phản biện kịch bản, nhưng người kỹ sư AI phải là người chịu trách nhiệm cuối cùng về ranh giới vận hành, số liệu giả định và tính khả thi trong thực tế.
2. **Problem-First, Architecture-Later:** Không bao giờ chọn bài toán để thể hiện kỹ thuật AI phức tạp. Một giải pháp LLM Feature đơn giản kết hợp chặt chẽ với Human-in-the-loop luôn mang lại ROI cao hơn và an toàn hơn một Agent tự trị mất kiểm soát.
3. **Operational Boundaries là sống còn:** Trong các hệ thống vật lý và dịch vụ thực tế như xe điện của Xanh SM hay VinFast, mọi ranh giới (như pin < 5%, bắt buộc phê duyệt [DRAFT_ONLY]) phải được thử nghiệm bằng kiểm thử tấn công (Adversarial Testing) và có cơ chế Fallback cứng bằng mã lệnh truyền thống.

