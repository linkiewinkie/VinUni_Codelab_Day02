# Phase 1 — SCAN & Phase 2 — QUICK-ASSESS
**Họ và tên:** Phạm Minh Đức  
**Mã số sinh viên:** 2A202603758  
**Đơn vị:** Vin Smart Future (Vingroup)  
**Branch cá nhân:** `PhamMinhDuc-2A202603758`

---

## 🔍 Phase 1 — SCAN: Danh sách bài toán vận hành Vingroup

Sử dụng **4 Lenses** (1. Repetitive / Lặp lại, 2. Time-consuming / Tốn thời gian, 3. AI-upgrade / Có thể nâng cấp bằng AI, 4. Stakeholder Pain / Điểm đau từ người khác) để quét qua toàn bộ hệ sinh thái Vingroup:

| # | Đơn vị thành viên (Subsidiary) | Lens áp dụng | Mô tả bài toán kinh doanh cụ thể |
|---|-------------------------------|--------------|----------------------------------|
| 1 | **Xanh SM (GSM)** | Tốn thời gian / Time-consuming | **Điều phối cứu hộ và trạm sạc khẩn cấp khi xe taxi điện cạn pin giữa đường:** Điều phối viên mất 12–15 phút/sự cố để tra cứu tọa độ GPS, đối chiếu trụ sạc trống phù hợp cổng CCS2 trên Dashboard VinFast, và soạn thảo tin nhắn hướng dẫn cho tài xế đang hoảng loạn. |
| 2 | **VinFast** | Lặp lại / Repetitive | **Đối chiếu & So khớp hóa đơn điện năng trạm sạc đối tác:** Hàng tuần nhân viên kế toán vận hành phải đối chiếu thủ công hàng nghìn giao dịch sạc xe điện từ các trạm sạc bên thứ ba liên kết với log hệ thống telemetry của xe VinFast, mất 20–25 giờ/tuần và dễ sai sót số liệu công nợ. |
| 3 | **Vinhomes** | AI-upgrade / Nâng cấp bằng AI | **Phân loại đa tầng và định tuyến phản ánh cư dân trên App Vinhomes Resident:** Cư dân gửi phản ánh tự do bằng tiếng Việt (tiếng ồn, rò rỉ nước, bảo trì thang máy, phí dịch vụ). CSKH đọc thủ công, mất 8–12 tiếng để phân loại và chuyển tiếp về đúng Ban Quản lý từng tòa nhà, phản hồi thường rập khuôn, gây bức xúc. |
| 4 | **Vinpearl / VinWonders** | Điểm đau / Stakeholder Pain | **Tổng hợp và phân tích đa kênh đánh giá của du khách (Review Analytics):** Ban quản lý khu nghỉ dưỡng nhận hàng nghìn review trên Google Maps, TripAdvisor, Booking.com mỗi tuần; nhân viên CSKH mất 3–4 ngày/tháng tổng hợp thủ công, dẫn đến bỏ lỡ các cảnh báo nóng về vệ sinh/thái độ nhân viên để xử lý tức thời. |
| 5 | **Vinmec** | Tốn thời gian / Time-consuming | **Tự động trích xuất và dự thảo tóm tắt hồ sơ xuất viện (Discharge Summary):** Bác sĩ điều trị mất 20–30 phút cho mỗi bệnh nhân xuất viện để gom chẩn đoán, kết quả xét nghiệm, đơn thuốc từ EHR và viết bản tóm tắt y khoa bằng ngôn ngữ thân thiện cho bệnh nhân và gia đình hiểu rõ cách theo dõi tại nhà. |
| 6 | **Xanh SM (GSM)** | Điểm đau / Stakeholder Pain | **Tự động phân loại lý do khách hàng hủy cuốc xe giờ cao điểm:** Hàng ngày tổng đài ghi nhận hàng nghìn cuộc gọi hoặc lý do hủy chuyến ngắn; nhân viên QA mất hàng chục giờ nghe lại ghi âm để gắn nhãn lý do (do tài xế đến chậm, lỗi app định vị sai, hay pin xe yếu không nhận cuốc xa). |

---

## 🃏 Phase 2 — QUICK-ASSESS: 3 Quick Problem Cards

Chọn lọc top 3 bài toán tiềm năng nhất từ danh sách trên để phân tích thẻ nghiệp vụ:
- **Card 1 (Chọn ưu tiên):** Xanh SM — Điều phối cứu hộ & trạm sạc thông minh khi xe cạn pin
- **Card 2:** Vinhomes — Phân loại & Điều hướng phản ánh cư dân trên App Vinhomes Resident
- **Card 3:** Vinmec — Dự thảo tóm tắt hồ sơ xuất viện (Discharge Summary)

---

### 📇 QUICK PROBLEM CARD #1

```text
┌─────────────────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #1 (Ưu tiên lựa chọn Deep-Dive)                     │
│                                                                         │
│ Bài toán (1 câu): Điều phối viên Xanh SM mất quá nhiều thời gian tra cứu│
│ thủ công vị trí và trạng thái trạm sạc để chỉ dẫn tài xế khi xe sắp hết│
│ pin trên đường đón trả khách.                                           │
│ Công ty thành viên: [x] Xanh SM    [ ] VinFast   [ ] Vinhomes           │
│                     [ ] Vinmec     [ ] Vinpearl                         │
│                                                                         │
│ Ai đang đau (Actor)?                                                    │
│ - Điều phối viên (Dispatcher): Quá tải xử lý cuộc gọi khẩn cấp.        │
│ - Tài xế Xanh SM: Nguy cơ cạn kiệt pin giữa đường, bị trừ điểm KPI.     │
│ - Khách hàng: Chờ xe lâu hoặc bị hủy chuyến đột ngột.                   │
│                                                                         │
│ Workflow thủ công hiện tại (5 bước):                                    │
│   1. Tài xế gọi hotline điều vận báo pin yếu (< 10%)                    │
│   ──> 2. Dispatcher mở phần mềm GPS tìm tọa độ xe                       │
│   ──> 3. Mở portal trạm sạc VinFast tìm trụ sạc trống tương thích       │
│   ──> 4. Dispatcher gõ tin nhắn SMS hướng dẫn lộ trình và gửi tài xế    │
│   ──> 5. Nếu xe cạn pin (< 5%), gọi điện thoại điều xe sạc di động      │
│                                                                         │
│ Bước nào tốn thời gian/lỗi nhất?                                         │
│   Bước 3 và 4 (⏱ 10 - 12 phút/lượt xử lý sự cố).                       │
│ AI có thể nhảy vào hỗ trợ ở bước nào?                                   │
│   Bước 3 & 4: Tự động gom dữ liệu xe + trạm trống, AI sinh bản nháp    │
│   hướng dẫn ([DRAFT_ONLY]) hoặc đề xuất lệnh điều xe sạc di động        │
│   (dispatch_mobile_charger) nếu pin < 5%.                               │
│                                                                         │
│ Đo thành công bằng gì (Metric có số)?                                    │
│   - Giảm thời gian xử lý sự cố từ 15 phút xuống dưới 3 phút/lượt.       │
│   - 100% tin nhắn bắt buộc phải qua Dispatcher duyệt trước khi gửi.     │
│   - 0 trường hợp tài xế xe pin < 5% bị chỉ định đến trạm sạc > 5km.     │
│                                                                         │
│ Quick Architecture: [ ] No AI  [ ] Rule  [x] LLM Feature  [ ] Agent     │
└─────────────────────────────────────────────────────────────────────────┘
```

---

### 📇 QUICK PROBLEM CARD #2

```text
┌─────────────────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #2                                                   │
│                                                                         │
│ Bài toán (1 câu): Cư dân gửi khiếu nại/yêu cầu dịch vụ bằng văn bản tự │
│ do trên app nhưng mất nửa ngày mới được phân loại và phản hồi ban đầu. │
│ Công ty thành viên: [ ] Xanh SM    [ ] VinFast   [x] Vinhomes           │
│                     [ ] Vinmec     [ ] Vinpearl                         │
│                                                                         │
│ Ai đang đau (Actor)?                                                    │
│ - Cư dân Vinhomes: Bức xúc vì sự cố rò rỉ nước/ồn ào không được phản hồi│
│   ngay lập tức.                                                         │
│ - Nhân viên CSKH/Ban Quản Lý: Mất hàng giờ phân loại hàng trăm ticket.  │
│                                                                         │
│ Workflow thủ công hiện tại (4 bước):                                    │
│   1. Cư dân gửi khiếu nại văn bản kèm ảnh qua Vinhomes Resident App     │
│   ──> 2. Nhân viên CSKH trung tâm đọc nội dung và phân loại thủ công    │
│   ──> 3. Tạo ticket nội bộ chuyển cho Kỹ thuật/An ninh từng khu đô thị │
│   ──> 4. Viết email/tin nhắn xác nhận gửi lại cư dân                    │
│                                                                         │
│ Bước nào tốn thời gian/lỗi nhất?                                         │
│   Bước 2 & 4 (⏱ 8 - 12 tiếng để tiếp nhận và phản hồi ban đầu).        │
│ AI có thể nhảy vào hỗ trợ ở bước nào?                                   │
│   Bước 2: Phân loại ý định (Intent Classification), đánh giá mức độ khẩn│
│   cấp và tự động draft câu trả lời xác nhận ban đầu cho nhân viên duyệt.│
│                                                                         │
│ Đo thành công bằng gì (Metric có số)?                                    │
│   - Giảm thời gian phản hồi ban đầu từ 12 giờ xuống dưới 15 phút.       │
│   - Độ chính xác định tuyến phòng ban đạt trên 92%.                     │
│                                                                         │
│ Quick Architecture: [ ] No AI  [ ] Rule  [x] LLM Feature  [ ] Agent     │
└─────────────────────────────────────────────────────────────────────────┘
```

---

### 📇 QUICK PROBLEM CARD #3

```text
┌─────────────────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #3                                                   │
│                                                                         │
│ Bài toán (1 câu): Bác sĩ mất nhiều thời gian tổng hợp bệnh án điện tử để│
│ viết tóm tắt hồ sơ xuất viện dễ hiểu cho bệnh nhân trước khi ra viện.  │
│ Công ty thành viên: [ ] Xanh SM    [ ] VinFast   [ ] Vinhomes           │
│                     [x] Vinmec     [ ] Vinpearl                         │
│                                                                         │
│ Ai đang đau (Actor)?                                                    │
│ - Bác sĩ điều trị: Quá tải hành chính, giảm thời gian khám lâm sàng.    │
│ - Bệnh nhân/Thân nhân: Chờ đợi giấy xuất viện lâu (2-3 tiếng).         │
│                                                                         │
│ Workflow thủ công hiện tại (4 bước):                                    │
│   1. Bác sĩ rà soát toàn bộ lịch sử khám, xét nghiệm, chẩn đoán trên EHR│
│   ──> 2. Gõ tay bản tóm tắt quá trình điều trị và hướng dẫn chăm sóc   │
│   ──> 3. Điều dưỡng in phiếu, kiểm tra lại chữ ký và đơn thuốc         │
│   ──> 4. Bàn giao và giải thích trực tiếp cho bệnh nhân                 │
│                                                                         │
│ Bước nào tốn thời gian/lỗi nhất?                                         │
│   Bước 1 & 2 (⏱ 25 - 30 phút/bệnh nhân).                               │
│ AI có thể nhảy vào hỗ trợ ở bước nào?                                   │
│   Bước 2: Trích xuất các trường dữ liệu y khoa chuẩn và draft bản tóm   │
│   tắt song ngữ/ngôn ngữ phổ thông dưới sự phê duyệt tuyệt đối của Bác sĩ│
│                                                                         │
│ Đo thành công bằng gì (Metric có số)?                                    │
│   - Giảm thời gian bác sĩ soạn tóm tắt từ 25 phút xuống dưới 5 phút.    │
│   - 100% bản tóm tắt phải được bác sĩ phụ trách ký duyệt xác nhận.      │
│                                                                         │
│ Quick Architecture: [ ] No AI  [ ] Rule  [x] LLM Feature  [ ] Agent     │
└─────────────────────────────────────────────────────────────────────────┘
```

