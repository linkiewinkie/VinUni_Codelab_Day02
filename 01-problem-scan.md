# Lab 02 — Worksheet: AI Product Scoping (Vin Smart Future)

---

## 🏛️ 1. Bối cảnh thực tế: Vin Smart Future (Vingroup)

Tôi là **Quỳnh Anh**, AI Engineer tại **Vin Smart Future**. Nhóm chúng tôi được giao nhiệm vụ phối hợp với Khối Vận Hành của **Vinhomes, VinFast và Vinpearl** để tìm kiếm các cơ hội tối ưu hóa bằng trí tuệ nhân tạo. 

Thông qua quan sát thực tế tại Tòa S2XX (Vinhomes Ocean Park 1) và đọc đánh giá công khai của khách hàng VinFast, Vinpearl, tôi nhận thấy cả nhân viên vận hành lẫn khách hàng đều đang mất nhiều thời gian cho các bước xác minh, tra cứu và xử lý thủ công: tòa nhà có 1–2 lần báo cháy giả mỗi ngày, còn app xe máy điện VinFast chỉ được 2.5/5 trên App Store. Bài toán tôi mang vào buổi Lab hôm nay đến từ chính quan sát thực tế này.

---

# 🔍 Phase 1 — SCAN (Cá nhân, 20 min)

Dùng **4 Lenses** quét qua vận hành của các công ty thành viên Vingroup.

### 📝 List bài toán của tôi:
| # | Subsidiary | Lens | Mô tả ngắn bài toán |
|---|----------------------------------|------|---------------------|
| 1 | Vinhomes | Stakeholder Pain |	Các cảnh báo cháy không xuất phát từ đám cháy thực sự có thể khiến cư dân chung cư Vinhomes hoảng loạn, gián đoạn sinh hoạt và làm tăng khối lượng xác minh cho Ban Quản lý, trong khi vẫn phải bảo đảm không bỏ sót cháy thật. |
| 2 | VinFast | Stakeholder Pain | Người dùng xe máy điện VinFast là phân khúc có doanh số lớn, nhưng chưa được đáp ứng đầy đủ nhu cầu hỗ trợ đặc thù về pin, sạc, chẩn đoán lỗi và bảo trì. Họ phải tự đi tìm địa điểm sạc và bảo trì pin cũng như bảo dưỡng gây mất thời gian và làm giảm trải nghiệm sở hữu. |
| 3 | VinFast | Time-consuming | Nhân viên CSKH và kỹ thuật viên tốn thời gian tiếp nhận và phân loại cũng như tra cứu cách giải quyết khiếu nại khi số lượng yêu cầu hậu mãi của khách hàng càng ngày càng tăng. |
| 4 | Vinpearl | Stakeholder Pain | 2,3 triệu room-nights/năm tạo quy mô housekeeping lớn; public review có ví dụ phòng/tác vụ dọn phòng chưa được thực hiện theo kỳ vọng khiến nhân viên tốn thời gian kiểm tra, dọn dẹp và công ty phải chịu khiếu nại của khách hàng cũng như bồi thường theo yêu cầu.  |
| 5 | Vinpearl | Time-consuming | FAQ chính thức cho biết CSKH có thể cần tới 48 giờ làm việc để kiểm tra với bộ phận liên quan; quy mô khoảng 31 khách sạn làm knowledge fragmentation đáng kể. Làm chậm quy trình xử lý cho khách khiến họ khó chịu và chịu hưởng tới branding. |

---

# 🃏 Phase 2 — QUICK-ASSESS (Cá nhân, 30 min)

Chọn **top 3 bài toán** từ danh sách trên và hoàn thiện **3 Quick Problem Cards** dưới đây (10 phút/card).

```
┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #1                                     │
│                                                             │
│ Bài toán (1 câu): Các cảnh báo cháy không xuất phát từ đám cháy thực sự có thể khiến cư dân chung cư Vinhomes hoảng loạn, gián đoạn sinh hoạt và làm tăng khối lượng xác minh cho Ban Quản lý, trong khi vẫn phải bảo đảm không bỏ sót cháy thật.
│
│ Công ty thành viên: [ x ] Vinhomes  
│                                                             │
│ Ai đang đau (Actor)? Nhân viên xác minh của Ban Quản lý
│                                                             │
│ Workflow thủ công hiện tại (3-5 bước):                      │
│   1. Đầu báo kích hoạt ──> 2. Hệ thống phát cảnh báo ──> 3. Nhân viên xác minh ──> 4. Ứng cứu và ghi nhận                  
│                                                             │
│ Bước nào tốn thời gian/lỗi nhất? Bước 3 — Nhân viên phải xác định vị trí, kiểm tra hiện trường và đối chiếu thông tin cảm biến. Riêng trong một ngày ở Toà S2XX tại Vinhomes OCP1 có ít nhất 1-2 báo cháy giả. (⏱ ~15 phút/lượt, ước tính)
│
│ AI có thể nhảy vào hỗ trợ ở bước nào? Bước 3 — Kết hợp dữ liệu đầu báo khói, nhiệt, lịch sử cảnh báo và camera tại khu vực chung (nếu có) để phát hiện tín hiệu bất thường, cung cấp bằng chứng và hỗ trợ nhân viên xác minh nhanh; không tự động kết luận an toàn.
│                                                             │
│ Đo thành công bằng gì (Metric có số)? 
│- Giảm thời gian xác minh 1 cảnh báo từ ~15 phút xuống ≤ 10 phút (giảm ≥30%).
- Phân loại đúng nguyên nhân ≥90% trên bộ sự cố đã được xác minh.
- 0 trường hợp AI làm chậm, hủy hoặc vô hiệu hóa báo cháy, 0 vụ cháy thật bị bỏ sót trong tập kiểm thử.
│                                                             │
│ Quick Architecture:  [ x ] Rule
└─────────────────────────────────────────────────────────────┘
```

```
┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #2                                     │
│                                                             │
│ Bài toán (1 câu): Quy mô khách hàng xe máy điện lớn cùng sự đa dạng về dòng xe, pin và chính sách dịch vụ có thể khiến người dùng mất thời gian tự tìm thông tin hỗ trợ phù hợp, làm giảm trải nghiệm sở hữu.
│
│ Công ty thành viên: [ x ] VinFast  
│                                                             │
│ Ai đang đau (Actor)? Chủ xe máy điện
│                                                             │
│ Workflow thủ công hiện tại (3-5 bước):                      │
│   1. Phát sinh câu hỏi/lỗi ──> 2. Tự tra cứu tài liệu ──> 3. Liên hệ hỗ trợ ──> 4. Được hướng dẫn/xử lý
│                                                             │
│ Bước nào tốn thời gian/lỗi nhất? Bước 2–3 — Chủ xe phải tự xác định mẫu xe, loại pin, chính sách bảo hành rồi tự tìm trạm sạc hoặc xưởng gần nhất. App VinFast E-Scooter hiện chỉ đạt 2.5/5 (946 lượt đánh giá trên App Store VN), có review phàn nàn định vị lỗi và xe "rất dễ nằm đường"; hãng cũng phải thêm nút báo trụ sạc sai thông tin. (⏱ ~40 phút/lượt, ước tính)
│
│ AI có thể nhảy vào hỗ trợ ở bước nào? Bước 2-3: RAG AI Agent nhận diện mẫu xe và triệu chứng, truy xuất tài liệu chính thức, giải thích chính sách, hướng dẫn kiểm tra an toàn hoặc chuyển yêu cầu tới kỹ thuật viên dựa trên dynamic routing cập nhật chính xác địa điểm của chủ xe và allow họ tiếp cận real-time với các dịch vụ CSKH cần thiết, đặc biệt là khi xảy ra vấn đề trên đường.
│                                                             │
│ Đo thành công bằng gì (Metric có số)?
│- Gợi ý đúng trạm sạc/xưởng dịch vụ phù hợp, còn hoạt động, gần nhất ≥90% trên bộ test vị trí.
- Giảm thời gian từ lúc gặp sự cố đến khi có hướng xử lý từ ~40 phút xuống dưới 3 phút.
- Trả lời đúng theo tài liệu chính thức ≥85% trên bộ 100 câu hỏi test.
│                                                             │
│ Quick Architecture: [ x ] Agent
└─────────────────────────────────────────────────────────────┘
```

```
┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #3                                    │
│                                                             │
│ Bài toán (1 câu): Với gần 2,3 triệu đêm phòng bán ra năm 2025, khối lượng dọn phòng của Vinpearl rất lớn; phòng dọn chưa đạt chuẩn mà không được phát hiện kịp sẽ khiến khách khiếu nại, nhân viên phải dọn lại và công ty có thể phải bồi thường.
│
│ Công ty thành viên: [ x ] Vinpearl
│                                                             │
│ Ai đang đau (Actor)? Giám sát buồng phòng và nhân viên housekeeping
│                                                             │
│ Workflow thủ công hiện tại (3-5 bước):                      │
│   1. Lễ tân báo phòng check-out ──> 2. Nhân viên dọn phòng ──> 3. Giám sát kiểm tra phòng ──> 4. Cập nhật phòng sẵn sàng ──> 5. Xử lý khiếu nại (nếu có)
│                                                             │
│ Bước nào tốn thời gian/lỗi nhất? Bước 3 — Giám sát phải kiểm tra từng phòng bằng mắt theo checklist, dễ bỏ sót vào giờ cao điểm check-out; lỗi thường chỉ lộ ra khi khách đã vào phòng và phàn nàn. (⏱ ~6 phút/phòng, ước tính)
│
│ AI có thể nhảy vào hỗ trợ ở bước nào? Bước 3 — Nhân viên chụp ảnh phòng sau khi dọn, AI đối chiếu với ảnh chuẩn để đánh dấu các điểm chưa đạt (giường, khăn, rác, minibar) cho giám sát kiểm tra lại; giám sát vẫn là người duyệt cuối.
│                                                             │
│ Đo thành công bằng gì (Metric có số)?
│- Phát hiện đúng ≥90% lỗi dọn phòng phổ biến trên bộ ảnh test đã gán nhãn.
- Giảm thời gian kiểm tra 1 phòng từ ~6 phút xuống ≤ 3 phút.
- Giảm ≥30% khiếu nại về vệ sinh phòng sau 3 tháng thử nghiệm tại 1 khách sạn.
│                                                             │
│ Quick Architecture: [ x ] LLM
└─────────────────────────────────────────────────────────────┘
```
