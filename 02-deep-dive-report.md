# Báo cáo Deep-Dive & Đánh giá Tính khả thi AI (Vin Smart Future)
**Đơn vị:** Vin Smart Future (Trung tâm Công nghệ Tập đoàn Vingroup)  
**Dự án:** Co-pilot Hỗ trợ Điều phối Sự cố Pin & Cứu hộ Xanh SM (GSM Intelligent Dispatcher)  
**Tác giả:** Phạm Minh Đức (MSSV: 2A202603758)  
**Nhóm công tác:** Vin Smart Future AI Engineering Team  
**Branch:** `PhamMinhDuc-2A202603758`

---

## 1. Current-State Workflow Mapping (Quy trình Hiện tại)

Khảo sát thực tế tại Trung tâm Điều vận Xanh SM (GSM), quy trình xử lý sự cố taxi điện hết pin trên đường đón trả khách gồm 5 bước thủ công:

```text
┌─────────────────┐       ┌─────────────────┐       ┌─────────────────┐       ┌─────────────────┐
│     Bước 1      │       │     Bước 2      │       │     Bước 3      │       │     Bước 4      │
│  Tiếp nhận cuộc │       │  Tra cứu vị trí │       │  Tra cứu trạm   │       │   Soạn thảo     │
│   gọi khẩn cấp  │ ────> │    GPS xe       │ ────> │   sạc VinFast   │ ────> │  tin hướng dẫn  │
│                 │       │                 │       │   còn trụ trống │       │    cho tài xế   │
│ Actor: Dispatch │       │ Actor: Dispatch │       │ Actor: Dispatch │       │ Actor: Dispatch │
│ ⏱ 2 phút        │       │ ⏱ 2 phút        │       │ ⏱ 5 phút 🔴     │       │ ⏱ 5 phút 🔴     │
│ In: Cuộc gọi    │       │ In: Biển số xe  │       │ In: GPS, Loại xe│       │ In: Thông tin   │
│ Out: Ticket sự cố│      │ Out: Tọa độ     │       │ Out: DS trạm sạc│       │ Out: SMS gửi đi │
└─────────────────┘       └─────────────────┘       └─────────────────┘       └─────────────────┘
                                                                                       │
                                                                                       ▼
                                                                              ┌─────────────────┐
                                                                              │     Bước 5      │
                                                                              │  Điều phối xe   │
                                                                              │   sạc di động   │
                                                                              │  (Nếu pin < 5%) │
                                                                              │ Actor: Dispatch │
                                                                              │ ⏱ 1 phút        │
                                                                              │ Out: Lệnh cứu hộ│
                                                                              └─────────────────┘

🔴 = Bottlenecks (Bước 3 & Bước 4 gây tắc nghẽn nghiêm trọng, chiếm 10/15 phút).
🔄 = Handoff (Chuyển giao giữa Tài xế gọi thoại <-> Điều phối viên <-> Hệ thống trạm sạc đối tác).
⏱ Tổng thời gian xử lý thủ công: 15 phút / sự cố.
```

### Phân tích điểm nghẽn (Bottlenecks & Handoffs):
- **Bước 3 (Tra cứu trạm sạc VinFast):** Điều phối viên phải chuyển đổi qua lại giữa Dashboard bản đồ xe và cổng thông tin quản lý trạm sạc VinFast, lọc loại cổng sạc (CCS2 cho VF e34/VF5/VF8), kiểm tra số trụ đang trống và ước lượng khoảng cách.
- **Bước 4 (Soạn thảo văn bản chỉ dẫn):** Soạn tin nhắn hướng dẫn tài xế bằng ngôn ngữ tự nhiên, giải thích cung đường ngắn nhất, tránh khu vực kẹt xe giờ cao điểm. Áp lực tâm lý lớn khi tài xế đang hoang mang vì xe có nguy cơ dừng giữa cầu hoặc đường vành đai.
- **Handoff rủi ro:** Nếu pin dưới 5%, tài xế cố chạy tới trạm sạc xa hơn 5km sẽ dẫn đến cạn pin chết máy giữa đường (Stranded EV), gây ùn tắc giao thông và chi phí kéo xe tốn kém.

---

## 2. Problem Statement 6-Field (Chuẩn Vin Smart Future)

| Trường thông tin (Field) | Nội dung chi tiết |
|---|---|
| **1. Actor / Operator** | Điều phối viên (Dispatcher) tại Trung tâm Vận hành và Điều vận thông minh Xanh SM (GSM Hà Nội / TP.HCM). |
| **2. Current Workflow** | Khi tài xế báo cáo pin xe dưới ngưỡng an toàn (hoặc có nguy cơ cạn pin giữa chừng khi phục vụ khách), điều phối viên thao tác 5 bước thủ công: nghe điện thoại tiếp nhận -> tra tọa độ xe trên bản đồ -> tra danh sách trạm sạc VinFast còn trụ trống tương thích -> tự gõ tin nhắn hướng dẫn lộ trình -> nếu pin nguy cấp (< 5%) thì gọi đội xe cứu hộ sạc di động. Toàn bộ quy trình tốn trung bình 15 phút/lượt. |
| **3. Bottleneck** | **Bước 3 & Bước 4:** Mất 10 phút để tra cứu chéo trạng thái trạm sạc theo thời gian thực và tự gõ tin nhắn hướng dẫn cho từng tài xế. Trong giờ cao điểm mưa gió, hàng chục sự cố xảy ra cùng lúc gây quá tải hoàn toàn tổng đài. |
| **4. Business Impact** | Tại Hà Nội ghi nhận trung bình 70–80 sự cố pin yếu/ngày. Tốn khoảng 18–20 giờ công làm việc của đội điều phối mỗi ngày; thời gian xe dừng chờ kéo dài làm giảm 15% hiệu suất khai thác cuốc xe; tài xế stress và gia tăng tỷ lệ khách hủy chuyến; xe chết máy giữa đường gây tổn hại thương hiệu taxi điện hiện đại. |
| **5. Success Metric** | 1. **Thời gian xử lý sự cố:** Giảm từ **15 phút xuống dưới 3 phút/lượt** (giảm 80% thời gian xử lý).<br>2. **Độ an toàn vận hành:** **0% sự cố** xe pin < 5% bị hướng dẫn đến trạm sạc xa (> 5km); 100% trường hợp pin < 5% được kích hoạt điều xe cứu hộ sạc di động.<br>3. **Chất lượng phản hồi:** 98% tin nhắn hướng dẫn đúng loại trụ sạc và hiển thị rõ trạng thái trụ trống. |
| **6. Operational Boundary (Ranh giới cấm)** | **Ranh giới an toàn tuyệt đối:**<br>1. AI **TUYỆT ĐỐI KHÔNG ĐƯỢC** tự động gửi tin nhắn cho tài xế nếu chưa có sự phê duyệt của Điều phối viên. Bắt buộc mọi phản hồi phải có tag `[DRAFT_ONLY]` ở đầu để bắt buộc quy trình Human-in-the-loop (HITL).<br>2. Khi pin xe dưới 5% (`battery < 5%`), AI **TUYỆT ĐỐI CẤM** đề xuất trạm sạc xa hơn 5km, mà phải ngay lập tức chuyển sang chế độ kích hoạt điều xe sạc di động `{"action": "dispatch_mobile_charger", "reason": "..."}`.<br>3. AI không được phép can thiệp trực tiếp vào hệ thống phanh hoặc khóa động cơ của xe. |

---

## 3. AI Fit & Future-State Flow

### 3.1. Ma trận lựa chọn kiến trúc (AI-Fit Matrix)

| Tiêu chí | Rule-based Engine | **LLM Feature (Lựa chọn)** | Autonomous Multi-Agent |
|---|---|---|---|
| **Khả năng tổng hợp ngôn ngữ tự nhiên** | Rất kém, bản tin cố định rập khuôn, khó linh hoạt theo từng ngữ cảnh đường phố/tài xế | **Rất tốt:** Soạn tin nhắn hướng dẫn chi tiết, ân cần, tóm tắt tình trạng giao thông linh hoạt | Tốt nhưng quá phức tạp, độ trễ phản hồi cao (15-30s) |
| **Độ tin cậy & Rủi ro an toàn** | Cao (dựa theo if/else cứng), nhưng khó xử lý yêu cầu biến đổi linh hoạt | **Tốt khi có System Prompt nghiêm ngặt + Output Structured JSON + Ranh giới kiểm tra [DRAFT_ONLY]** | Thấp: Agent tự trị dễ gặp hiện tượng loop vô tận hoặc tự ý đưa ra quyết định gửi tin sai |
| **Chi phí & Độ phức tạp triển khai** | Thấp | **Vừa phải:** Tích hợp Gemini 2.5 Flash API nhanh, chi phí token cực thấp (~$0.0001/lần gọi) | Rất cao, khó kiểm soát hành vi và debug |

**Kết luận:** Nhóm chọn kiến trúc **LLM Feature có Human-in-the-loop (Co-pilot)**. Đây là kiến trúc tối ưu nhất giữa độ linh hoạt ngôn ngữ, chi phí và mức độ an toàn vận hành.

### 3.2. Future-State Flow (Quy trình tương lai)

```text
┌─────────────────┐       ┌─────────────────┐       ┌─────────────────┐       ┌─────────────────┐
│     Bước 1      │       │     Bước 2      │       │     Bước 3      │       │     Bước 4      │
│  Tài xế bấm nút │       │  🔵 Hệ thống tự │       │  🔵 Gemini Flash│       │  🟢 Điều phối   │
│  báo pin yếu    │ ────> │  động kéo GPS & │ ────> │  sinh draft tin │ ────> │  viên 1-click   │
│  hoặc gọi tổng  │       │  trạm sạc trống │       │  hoặc lệnh cứu  │       │  phê duyệt và   │
│  đài điều vận   │       │  thời gian thực │       │  hộ [DRAFT_ONLY]│       │  gửi tới tài xế │
│ (10 giây)       │       │ (5 giây)        │       │ (2 giây)        │       │ (15 giây)       │
└─────────────────┘       └─────────────────┘       └─────────────────┘       └─────────────────┘
                                                             │                         │
                                                             │                         ▼
                                                             │                  ↩️ Fallback Plan:
                                                             │                  Nếu model timeout,
                                                             │                  lỗi API hoặc output
                                                             │                  thiếu [DRAFT_ONLY],
                                                             │                  hệ thống rơi về màn
                                                             │                  hình Dashboard cũ để
                                                             │                  điều phối viên tự gõ.
```

- 🔵 **AI Step (Bước AI):** Gemini 2.5 Flash nhận thông tin xe (dòng xe, vị trí GPS, % pin) và danh sách trạm sạc lân cận để tạo bản nháp hướng dẫn có cấu trúc chuẩn.
- 🟢 **Human Step (HITL):** Điều phối viên xem bản nháp, bấm nút "Duyệt & Gửi" (Approve & Dispatch) trong 5-15 giây.
- ↩️ **Fallback Plan (Dự phòng):** Nếu API trả về lỗi hoặc thiếu tag an toàn `[DRAFT_ONLY]`, hệ thống tự động kích hoạt Rule-based Fallback hiển thị trạm sạc gần nhất trên bản đồ tĩnh để con người xử lý như quy trình truyền thống.

---

## 4. Phase 5 — EVALUATE & Quyết định của Vin Smart Future

### 4.1. Bảng kiểm tra độ sẵn sàng AI (AI Readiness Checklist)
- [x] **Dữ liệu / Telemetry:** Đã có API vị trí xe Xanh SM qua thiết bị giám sát hành trình và API trạng thái trạm sạc thời gian thực từ VinFast.
- [x] **Kiểm soát rủi ro:** Đã thiết lập cơ chế Human-in-the-loop bắt buộc với tiền tố `[DRAFT_ONLY]`, ngăn chặn AI tự động gửi tin nhắn khi chưa được duyệt; kiểm soát pin < 5% bằng lệnh cứu hộ xe sạc di động.
- [x] **Sự sẵn sàng của Stakeholder:** Đội ngũ điều phối viên Xanh SM rất hoan nghênh công cụ giúp giảm áp lực trực điện thoại và rút ngắn thời gian xử lý sự cố.

### 4.2. Quyết định cuối cùng: **GO** (Tiến hành phát triển Prototype & Pilot diện hẹp)

**Lý giải quyết định (Justification):**
1. **Bài toán rõ ràng, tính cấp thiết cao:** Sự cố pin xe taxi điện là vấn đề cốt lõi ảnh hưởng trực tiếp đến SLA và doanh thu của Xanh SM.
2. **Hiệu quả kinh tế & Kỹ thuật:** Giảm thời gian xử lý từ 15 phút xuống dưới 3 phút giúp tiết kiệm hàng chục giờ công mỗi ngày, trong khi chi phí gọi Gemini 2.5 Flash chỉ khoảng vài nghìn đồng cho toàn bộ hệ thống mỗi ngày.
3. **Ranh giới an toàn đã được kiểm chứng bằng Code:** Đã chạy thử nghiệm thành công prompt prototype, mô hình tuân thủ tuyệt đối ranh giới an toàn: giữ vững tag `[DRAFT_ONLY]` và tự động điều phối xe sạc pin di động khi pin dưới 5%.

