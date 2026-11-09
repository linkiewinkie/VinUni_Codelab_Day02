# 02 — Deep-Dive Report: Vinmec AI Discharge Summary Co-pilot

> **Người thực hiện:** Đào Thị Ngọc Anh — 2A202603790
> **Bài toán:** Bác sĩ Vinmec mất 20–30 phút/bệnh nhân để tự viết bản tóm tắt xuất viện.
> *Các số liệu khối lượng và chi phí dưới đây là giả định ước tính để scoping, cần thay bằng số liệu thật từ HIS/EMR của Vinmec trước khi trình Ban Giám đốc.*

---

## 🏗️ Phase 3.1 — Current-State Workflow

Sơ đồ chi tiết: [04-workflow-diagram.png](04-workflow-diagram.png)

```text
┌──────────────┐    ┌──────────────┐    ┌──────────────┐    ┌──────────────┐    ┌──────────────┐    ┌──────────────┐
│ Bước 1       │    │ Bước 2       │    │ Bước 3       │    │ Bước 4       │    │ Bước 5       │    │ Bước 6       │
│ Đọc lại bệnh │    │ Tra kết quả  │    │ Đối chiếu y  │    │ Tự viết bản  │    │ Trưởng khoa /│    │ Điều dưỡng in│
│ án EMR, diễn │──→ │ XN (LIS) &   │──→ │ lệnh, đơn    │──→ │ tóm tắt + HD │──→ │ BS ký duyệt  │──→ │ & giải thích │
│ biến điều trị│ 🔄 │ CĐHA (PACS)  │    │ thuốc ra viện│    │ sau ra viện  │ 🔄 │              │ 🔄 │ cho bệnh nhân│
│ Ai: BS điều  │    │ Ai: BS điều  │    │ Ai: BS điều  │    │ Ai: BS điều  │    │ Ai: BS/Trưởng│    │ Ai: Điều     │
│ trị          │    │ trị          │    │ trị          │    │ trị          │    │ khoa         │    │ dưỡng        │
│ ⏱ 7 phút 🔴  │    │ ⏱ 5 phút     │    │ ⏱ 3 phút     │    │ ⏱ 10 phút 🔴 │    │ ⏱ 2 phút     │    │ ⏱ 5 phút     │
│ In: EMR      │    │ In: Mã BN    │    │ In: Y lệnh   │    │ In: Ghi chú  │    │ In: Bản nháp │    │ In: Bản ký   │
│ Out: Ghi chú │    │ Out: KQ chính│    │ Out: DS thuốc│    │ Out: Bản nháp│    │ Out: Bản ký  │    │ Out: BN hiểu │
└──────────────┘    └──────────────┘    └──────────────┘    └──────────────┘    └──────────────┘    └──────────────┘
🔴 = Bottleneck   🔄 = Handoff (giữa hệ thống ↔ người, hoặc giữa các vai trò)
⏱ Thời gian của bác sĩ: ~25 phút/bệnh nhân (bước 1–4). Tổng end-to-end: ~32 phút/lượt.
```

**Các điểm handoff:**
* 🔄 **EMR/LIS/PACS → Bác sĩ:** Dữ liệu nằm rải rác ở 3 hệ thống, bác sĩ phải tự mở từng nơi.
* 🔄 **Bác sĩ điều trị → Trưởng khoa:** Bản tóm tắt chờ ký. Vào giờ cao điểm xuất viện (10h–12h), bước này thường bị dồn.
* 🔄 **Bác sĩ → Điều dưỡng → Bệnh nhân:** Điều dưỡng phải "dịch" thuật ngữ chuyên môn sang ngôn ngữ dễ hiểu bằng lời.

**Bottleneck:**
* 🔴 **Bước 1 (7 phút):** Đọc lại toàn bộ diễn biến của những ca nằm viện dài (5–10 ngày) để chọn ra thông tin quan trọng.
* 🔴 **Bước 4 (10 phút):** Viết văn bản tự do. Phải viết 2 lần: một bản chuyên môn lưu hồ sơ, và hướng dẫn sau ra viện cho bệnh nhân (thuốc, tái khám, dấu hiệu cần quay lại viện).

---

## 📌 Phase 3.2 — Problem Statement (6-field)

| Field | Nội dung chi tiết |
|---|---|
| **1. Actor / Operator** | Bác sĩ điều trị nội trú tại các khoa lâm sàng của Vinmec (pilot: khoa Nội tổng hợp). Liên quan: Trưởng khoa (ký duyệt), Điều dưỡng (giải thích cho bệnh nhân), bệnh nhân và người nhà. |
| **2. Current Workflow** | Khi có chỉ định xuất viện, bác sĩ mở EMR đọc lại diễn biến điều trị, tra kết quả xét nghiệm (LIS) và chẩn đoán hình ảnh (PACS), đối chiếu y lệnh và đơn thuốc ra viện, rồi tự gõ bản tóm tắt vào template của EMR. Trưởng khoa ký duyệt, điều dưỡng in và giải thích cho bệnh nhân. Gồm 6 bước, hoàn toàn thủ công, khoảng 32 phút/lượt (riêng bác sĩ khoảng 25 phút). |
| **3. Bottleneck** | Bước 1 (đọc lại và chọn lọc thông tin, 7 phút) và Bước 4 (soạn văn bản hai phiên bản chuyên môn và dễ hiểu, 10 phút). Cả hai đều là xử lý ngôn ngữ tự nhiên từ dữ liệu rải rác, tốn 17/25 phút thời gian bác sĩ. |
| **4. Business Impact** | *Giả định:* khoa pilot có ~15 ca xuất viện/ngày → 15 × 25 phút ≈ **6,25 giờ bác sĩ/ngày** chỉ để viết tóm tắt. Bệnh nhân phải chờ thêm 1–2 giờ ở giờ cao điểm, giường không được giải phóng sớm cho bệnh nhân mới. Bản tóm tắt nhiều thuật ngữ khiến bệnh nhân dùng sai thuốc hoặc bỏ tái khám, làm tăng nguy cơ tái nhập viện. |
| **5. Success Metric** | 1. **Hiệu suất:** Thời gian bác sĩ dành cho mỗi bản tóm tắt giảm từ ~25 phút xuống **≤ 8 phút** (chỉ review và ký).<br>2. **Chất lượng:** **≥ 95%** bản nháp AI không có lỗi về thuốc, liều, dị ứng hoặc chẩn đoán khi bác sĩ review; **0 trường hợp** AI bịa thông tin không có trong hồ sơ (đo bằng audit ngẫu nhiên 10% bản nháp).<br>3. **Trải nghiệm bệnh nhân:** **≥ 80%** bệnh nhân trả lời đúng 3 câu teach-back (uống thuốc gì, khi nào tái khám, dấu hiệu nào cần quay lại viện). |
| **6. Operational Boundary** | **AI được phép:** đọc dữ liệu EMR/LIS/PACS của đúng bệnh nhân đang xuất viện (chỉ đọc); soạn **bản nháp** tóm tắt chuyên môn và bản hướng dẫn dễ hiểu; đánh dấu các trường thiếu dữ liệu.<br>**TUYỆT ĐỐI CẤM:** tự lưu hoặc gửi bản tóm tắt cho bệnh nhân khi chưa có chữ ký bác sĩ; đưa ra chẩn đoán mới hoặc tiên lượng; thêm, bớt hay đổi thuốc và liều so với đơn thuốc ra viện; bỏ qua thông tin dị ứng; dùng bất kỳ thông tin nào không có trong hồ sơ; gửi dữ liệu bệnh nhân ra mô hình bên ngoài khi chưa được phê duyệt bảo mật.<br>**Điểm duyệt bắt buộc (HITL):** Bác sĩ điều trị review và ký mọi bản tóm tắt. Bản nào có cờ ⚠️ cần Trưởng khoa duyệt thêm. |

---

## 🤖 Phase 3.3 — AI Fit & Future-State Flow

### So sánh mức AI Fit

| Phương án | Làm được gì | Hạn chế | Đánh giá |
|---|---|---|---|
| **Rule / State-Machine** | Tự điền template từ các trường có cấu trúc: tên, ngày vào/ra, mã ICD, danh sách thuốc. | Không tóm tắt được diễn biến điều trị dạng văn bản tự do, không viết lại được ngôn ngữ dễ hiểu cho bệnh nhân. | ✅ Dùng làm **lớp nền**: các trường quan trọng (thuốc, liều, dị ứng) lấy thẳng từ dữ liệu, không để LLM tự viết. |
| **LLM Feature** | Đọc ghi chú tự do, tóm tắt diễn biến, viết bản hướng dẫn dễ hiểu cho bệnh nhân. | Có thể hallucinate, cần ranh giới chặt và bác sĩ review. | ✅ **Lựa chọn chính.** |
| **Agentic Loop** | Tự truy vấn nhiều hệ thống, tự quyết định và tự gửi cho bệnh nhân. | Quy trình vốn cố định nên không cần agent tự trị. Rủi ro y khoa và pháp lý quá cao nếu AI tự hành động. | ❌ Không phù hợp. |

**Kết luận AI Fit:** **LLM Feature trên nền Rule.** Rule trích xuất các trường an toàn quan trọng, LLM chỉ viết phần diễn giải, bác sĩ luôn duyệt cuối.

### Future-State Flow

```text
┌──────────────┐    ┌──────────────────┐    ┌──────────────────┐    ┌──────────────────┐    ┌──────────────┐
│ Bước 1       │    │ Bước 2 🔵 + Rule │    │ Bước 3 🔵 AI     │    │ Bước 4 🟢 HITL   │    │ Bước 5       │
│ BS chỉ định  │──→ │ Auto-pull EMR,   │──→ │ LLM soạn bản     │──→ │ BS review, sửa  │──→ │ Điều dưỡng   │
│ xuất viện    │    │ LIS, PACS; Rule  │    │ NHÁP [DRAFT_ONLY]│    │ & ký (cờ ⚠️ →    │    │ in & giải    │
│ trên EMR     │    │ trích thuốc/liều/│    │ chuyên môn + bản │    │ Trưởng khoa     │    │ thích (bản   │
│              │    │ dị ứng (cố định) │    │ dễ hiểu cho BN   │    │ duyệt thêm)     │    │ dễ hiểu)     │
│ ⏱ 0 phút     │    │ ⏱ < 30 giây      │    │ ⏱ < 1 phút       │    │ ⏱ ≤ 8 phút      │    │ ⏱ 3 phút     │
└──────────────┘    └──────────────────┘    └──────────────────┘    └──────────────────┘    └──────────────┘
                                                     │
                                                     ▼
                                     ↩️ Fallback:
                                     • Thiếu dữ liệu → để trống, ghi [CẦN BS BỔ SUNG]
                                     • Thuốc/dị ứng trong nháp lệch với dữ liệu Rule
                                       → chặn bản nháp, gắn cờ ⚠️
                                     • LLM lỗi / timeout → mở template EMR cũ,
                                       bác sĩ viết tay như hiện tại
```

🔵 = AI Step · 🟢 = Human Step (HITL) · ↩️ = Fallback

**Ước tính sau khi áp dụng:** thời gian bác sĩ giảm từ ~25 phút xuống ≤ 8 phút/ca, end-to-end từ ~32 phút xuống ~13 phút/lượt.

---

## 🏁 Phase 5 — EVALUATE

### AI Readiness Checklist

1. [x] **Có sẵn dữ liệu mẫu/logs để test?** Có một phần. Vinmec đã dùng EMR nên có sẵn các bản tóm tắt xuất viện cũ do bác sĩ viết, dùng làm "đáp án chuẩn" để so sánh. **Điều kiện:** phải ẩn danh hóa dữ liệu và được Hội đồng đạo đức / bộ phận bảo mật phê duyệt trước khi dùng.
2. [x] **Rủi ro khi AI sai nằm trong tầm kiểm soát?** Có. Bác sĩ đã ký duyệt bản tóm tắt sẵn trong quy trình hiện tại (HITL). Thuốc, liều, dị ứng do Rule lấy thẳng từ dữ liệu, không để LLM tự viết. Khi LLM lỗi thì quay về template cũ.
3. [ ] **Stakeholders sẵn sàng thay đổi quy trình?** Chưa chắc chắn. Bác sĩ có thể không tin bản nháp AI, hoặc mất thời gian kiểm tra lại toàn bộ (làm mất lợi ích về thời gian). Cần chạy thử và lấy ý kiến bác sĩ khoa pilot.

### Quyết định cuối cùng

[x] **GO (Bắt đầu xây dựng Prototype) — scope hẹp, có điều kiện**
[ ] NOT YET
[ ] NO-GO

**Justification:**
> * **Vì sao GO:** Bottleneck rõ ràng (17/25 phút bác sĩ là công việc xử lý ngôn ngữ), đúng thế mạnh của LLM. Quy trình đã có sẵn bước bác sĩ ký duyệt nên thêm HITL không phát sinh vai trò mới. Rule-based một mình không giải quyết được phần tóm tắt diễn biến và viết ngôn ngữ dễ hiểu.
> * **Vì sao scope hẹp:** Y tế là lĩnh vực rủi ro cao. Prototype chỉ triển khai ở **1 khoa (Nội tổng hợp)** và chạy **4 tuần "shadow mode"**: AI soạn nháp song song, bác sĩ vẫn tự viết như cũ, so sánh hai bản để đo tỷ lệ lỗi trước khi cho bác sĩ dùng bản nháp thật.
> * **Điều kiện dừng (kill criteria):** Nếu sau shadow mode tỷ lệ bản nháp có lỗi thuốc, liều hoặc dị ứng **> 5%**, hoặc phát hiện **bất kỳ** trường hợp AI bịa thông tin lâm sàng, dự án chuyển sang **NOT YET** để cải thiện dữ liệu và prompt.
> * **Chi phí:** Prototype chỉ cần LLM API, kết nối chỉ đọc vào EMR và một prompt có ranh giới chặt, không cần huấn luyện mô hình riêng. Nếu tiết kiệm được ~17 phút × 15 ca/ngày ≈ 4 giờ bác sĩ/ngày tại một khoa, chi phí API là rất nhỏ so với chi phí giờ làm của bác sĩ. *(Cần tính lại khi có số liệu thật.)*
> * **Rủi ro lớn nhất cần xử lý trước khi mở rộng:** bảo mật dữ liệu bệnh nhân. Phải dùng mô hình được triển khai trong hạ tầng đạt chuẩn bảo mật của Vinmec, hoặc ẩn danh hóa dữ liệu trước khi gửi.
