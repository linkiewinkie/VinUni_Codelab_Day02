# 01 — Problem Scan & Quick Cards (Cá nhân)

> **Người thực hiện:** Đào Thị Ngọc Anh — 2A202603790
> **Mảng kinh doanh tập trung:** 🏥 **Vinmec** — Y tế thông minh
> *Các con số thời gian/khối lượng dưới đây là ước tính từ quan sát và mô tả quy trình, cần xác nhận lại bằng dữ liệu vận hành thực tế của Vinmec.*

---

## 🔍 Phase 1 — SCAN: 5 bài toán thực tế tại Vinmec

Sử dụng 4 Lenses: **Lặp lại (Repetitive)**, **Tốn thời gian (Time-consuming)**, **AI có thể tốt hơn (AI-upgrade)**, **Pain từ người khác (Stakeholder Pain)**.

| # | Subsidiary | Lens | Mô tả ngắn bài toán |
|---|------------|------|---------------------|
| 1 | **Vinmec** | Tốn thời gian | Bác sĩ phải đọc lại bệnh án điện tử, kết quả xét nghiệm và ghi chú điều trị để tự viết **bản tóm tắt xuất viện**, mất 20–30 phút/bệnh nhân. Bản tóm tắt thường dùng thuật ngữ chuyên môn nên bệnh nhân khó hiểu. Bác sĩ quá tải, bệnh nhân xuất viện chậm. |
| 2 | **Vinmec** | Pain từ người khác | Bệnh nhân có **bảo hiểm tư nhân** phải chờ 2–4 giờ lúc xuất viện. Nhân viên gom chứng từ thủ công; hồ sơ thiếu giấy tờ thường bị bảo hiểm trả về để bổ sung. Bệnh nhân không được giải thích rõ chi phí, người làm hóa đơn mất thêm thời gian giải đáp cho bệnh nhân và người nhà. |
| 3 | **Vinmec** | Lặp lại | Nhân viên phải tra cứu và **gán mã bệnh ICD-10** cho từng hồ sơ rồi đối chiếu với danh mục BHYT. Việc này lặp lại hằng ngày với khối lượng lớn. Gán sai mã dẫn đến bị xuất toán, bệnh viện thất thu, số liệu thống kê bệnh tật sai lệch và phải làm lại hồ sơ. |
| 4 | **Vinmec** | AI có thể tốt hơn | **Chatbot và tổng đài** chạy theo kịch bản cứng, chỉ đặt lịch theo yêu cầu có sẵn, không hỏi triệu chứng để gợi ý đúng chuyên khoa và mức độ khẩn cấp. Bệnh nhân có thể bị xếp sai phòng khám, phải chờ lâu; nhân viên mất thời gian làm lại hồ sơ nhập viện. |
| 5 | **Vinmec** | Tốn thời gian | Cuối mỗi ca, **điều dưỡng tổng hợp thủ công** tình trạng từng bệnh nhân (sinh hiệu, thuốc, dị ứng, y lệnh) vào sổ bàn giao, mất 30–45 phút/ca. Thông tin quan trọng như dị ứng hay thuốc chờ dùng dễ bị sót, ảnh hưởng an toàn người bệnh. Điều dưỡng phải tăng ca. |

---

## 🃏 Phase 2 — QUICK-ASSESS: 3 Quick Problem Cards

Top 3 được chọn: **#1 (Tóm tắt xuất viện)**, **#3 (Gán mã ICD-10)**, **#5 (Bàn giao ca điều dưỡng)**.

### Card #1 — Soạn tóm tắt xuất viện (Discharge Summary)

```text
┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #1                                       │
│                                                             │
│ Bài toán: Bác sĩ mất 20-30 phút/bệnh nhân để đọc lại bệnh   │
│ án và tự viết bản tóm tắt xuất viện, bệnh nhân khó hiểu.    │
│ Công ty thành viên: [ ] VinFast  [ ] Xanh SM  [ ] Vinhomes  │
│                     [x] Vinmec   [ ] Khác                   │
│                                                             │
│ Ai đang đau? Bác sĩ điều trị (quá tải giấy tờ), bệnh nhân   │
│ (chờ xuất viện, không hiểu hướng dẫn sau ra viện).          │
│                                                             │
│ Workflow thủ công hiện tại (5 bước):                        │
│   1. Mở bệnh án điện tử (EMR), đọc diễn biến điều trị       │
│   → 2. Tra kết quả xét nghiệm / chẩn đoán hình ảnh          │
│   → 3. Đối chiếu y lệnh, đơn thuốc ra viện                  │
│   → 4. Tự viết bản tóm tắt + hướng dẫn sau ra viện          │
│   → 5. Ký duyệt, điều dưỡng in và giải thích cho bệnh nhân  │
│                                                             │
│ Bước nào tốn nhất? Bước 1 & 4 (⏱ ~17 phút/lượt)             │
│ AI có thể nhảy vào hỗ trợ ở bước nào? Bước 1-4: tự trích    │
│ xuất dữ liệu từ EMR và soạn bản NHÁP (bản chuyên môn + bản   │
│ ngôn ngữ dễ hiểu cho bệnh nhân). Bác sĩ chỉ review & ký.    │
│                                                             │
│ Đo thành công bằng gì (Metric có số)?                        │
│ Giảm thời gian bác sĩ viết tóm tắt từ ~25 phút ──> ≤ 8 phút │
│ (chỉ review & ký); ≥ 95% bản nháp không cần sửa thông tin   │
│ thuốc/liều/dị ứng.                                          │
│                                                             │
│ Quick Architecture: [ ] No AI  [ ] Rule  [x] LLM  [ ] Agent │
└─────────────────────────────────────────────────────────────┘
```

### Card #2 — Gán mã bệnh ICD-10 & đối chiếu danh mục BHYT

```text
┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #2                                       │
│                                                             │
│ Bài toán: Nhân viên mã hóa phải tra cứu và gán mã ICD-10    │
│ thủ công cho từng hồ sơ; gán sai bị BHYT xuất toán.         │
│ Công ty thành viên: [x] Vinmec                              │
│                                                             │
│ Ai đang đau? Nhân viên mã hóa bệnh / phòng Kế hoạch tổng    │
│ hợp, phòng Tài chính (thất thu), bác sĩ (bị hỏi lại).       │
│                                                             │
│ Workflow thủ công hiện tại (4 bước):                        │
│   1. Đọc chẩn đoán xuất viện + bệnh kèm trong hồ sơ         │
│   → 2. Tra cứu mã ICD-10 tương ứng (sách / phần mềm tra)    │
│   → 3. Đối chiếu danh mục BHYT, kiểm tra điều kiện thanh toán│
│   → 4. Nhập mã vào HIS, gửi hồ sơ giám định                 │
│                                                             │
│ Bước nào tốn nhất? Bước 2-3 (⏱ ~5-8 phút/hồ sơ)             │
│ AI có thể nhảy vào hỗ trợ ở bước nào? Bước 2: LLM đọc chẩn  │
│ đoán dạng văn bản tự do và GỢI Ý top-3 mã ICD-10 kèm lý do; │
│ Bước 3: Rule engine kiểm tra mã với danh mục BHYT.          │
│                                                             │
│ Đo thành công bằng gì (Metric có số)?                        │
│ Mã gợi ý top-1 đúng ≥ 90% (so với chuyên viên mã hóa);      │
│ giảm thời gian mã hóa từ ~6 phút ──> ≤ 2 phút/hồ sơ;        │
│ giảm 50% số hồ sơ bị BHYT trả về do sai mã.                 │
│                                                             │
│ Quick Architecture: [ ] No AI  [x] Rule  [x] LLM  [ ] Agent │
│ (LLM gợi ý + Rule kiểm tra danh mục, người duyệt cuối)       │
└─────────────────────────────────────────────────────────────┘
```

### Card #3 — Tổng hợp bàn giao ca điều dưỡng

```text
┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #3                                       │
│                                                             │
│ Bài toán: Cuối ca, điều dưỡng tổng hợp thủ công tình trạng  │
│ từng bệnh nhân vào sổ bàn giao, mất 30-45 phút, dễ sót      │
│ thông tin dị ứng / thuốc chờ dùng.                          │
│ Công ty thành viên: [x] Vinmec                              │
│                                                             │
│ Ai đang đau? Điều dưỡng (tăng ca), bệnh nhân (rủi ro an     │
│ toàn khi thông tin bị sót), điều dưỡng ca sau.              │
│                                                             │
│ Workflow thủ công hiện tại (4 bước):                        │
│   1. Xem lại sinh hiệu, phiếu chăm sóc của từng bệnh nhân   │
│   → 2. Đối chiếu y lệnh, thuốc đã dùng / chờ dùng           │
│   → 3. Viết tóm tắt vào sổ bàn giao (giấy / Excel)          │
│   → 4. Bàn giao miệng tại giường cho ca sau                 │
│                                                             │
│ Bước nào tốn nhất? Bước 1-3 (⏱ ~3-4 phút/bệnh nhân,         │
│ 30-45 phút/ca cho 10-12 bệnh nhân)                          │
│ AI có thể nhảy vào hỗ trợ ở bước nào? Bước 1-3: tự tổng hợp │
│ dữ liệu EMR thành bản nháp theo chuẩn SBAR; tự đánh dấu đỏ  │
│ dị ứng, thuốc chờ dùng, sinh hiệu bất thường.               │
│                                                             │
│ Đo thành công bằng gì (Metric có số)?                        │
│ Giảm thời gian chuẩn bị bàn giao từ 30-45 ──> ≤ 10 phút/ca; │
│ 100% thông tin dị ứng & thuốc chờ dùng có mặt trong bản     │
│ bàn giao (0 lỗi bỏ sót).                                    │
│                                                             │
│ Quick Architecture: [ ] No AI  [x] Rule  [x] LLM  [ ] Agent │
│ (Rule trích xuất trường bắt buộc + LLM tóm tắt ghi chú)      │
└─────────────────────────────────────────────────────────────┘
```

---

## 🗳️ Lựa chọn để Deep-Dive

Tôi chọn **Card #1 — Soạn tóm tắt xuất viện** để phân tích sâu trong `02-deep-dive-report.md`.

**Lý do chọn:**
* Đây là tác vụ sinh văn bản dài từ nhiều nguồn dữ liệu, đúng thế mạnh của LLM. Rule-based chỉ điền được template cứng, không viết lại được ngôn ngữ dễ hiểu cho bệnh nhân.
* Bác sĩ luôn ký duyệt cuối cùng, nên có sẵn một bước Human-in-the-loop trong quy trình hiện tại. Rủi ro khi AI sai vì vậy kiểm soát được.
* Bài toán tác động cùng lúc tới bác sĩ (tiết kiệm thời gian) và bệnh nhân (xuất viện nhanh hơn, hiểu rõ hướng dẫn).

**Lý do chưa chọn các thẻ khác:**
* **Card #2 (ICD-10):** Phần kiểm tra danh mục BHYT là bài toán Rule-based. Nên chuẩn hóa rule engine trước, sau đó mới thêm LLM gợi ý mã.
* **Card #3 (Bàn giao điều dưỡng):** Giá trị cao nhưng phụ thuộc vào việc điều dưỡng nhập phiếu chăm sóc lên EMR đầy đủ và kịp thời. Nếu dữ liệu đầu vào thiếu, AI tóm tắt cũng thiếu, mà bỏ sót dị ứng là rủi ro an toàn nghiêm trọng.
* **#2 (Bảo hiểm tư nhân)** và **#4 (Chatbot phân luồng)** không vào top 3. #2 phụ thuộc quy trình của nhiều công ty bảo hiểm bên ngoài. #4 liên quan đánh giá mức độ khẩn cấp lâm sàng, rủi ro rất cao nếu phân luồng sai.
