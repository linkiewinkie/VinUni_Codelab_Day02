# 🔍 Phase 1 — SCAN (Cá nhân)

Dùng **4 Lenses** quét qua hoạt động vận hành của các công ty thành viên Vingroup. Ghi lại **ít nhất 5 bài toán/bottleneck** thực tế.

### 4 Lenses tìm bài toán AI cho Vingroup:
1. **Lặp lại (Repetitive):** Tác vụ lặp đi lặp lại nhiều lần hằng ngày.
2. **Tốn thời gian (Time-consuming):** Tác vụ ngốn thời gian xử lý thủ công của nhân viên.
3. **AI có thể tốt hơn (AI-upgrade):** Dịch vụ khách hàng hiện tại còn chậm hoặc phản hồi rập khuôn.
4. **Pain từ người khác (Stakeholder Pain):** Bottleneck khiến khách hàng hoặc nhân viên thực địa phàn nàn.

### 📝 List bài toán của tôi:

| # | Subsidiary | Lens | Mô tả ngắn bài toán |
|---|------------|------|---------------------|
| 1 | **Xanh SM** | Lặp lại | Bộ phận vận doanh phải đối soát thủ công mỗi ngày giữa cuốc hoàn thành, doanh thu, ví và điều kiện thưởng của hàng nghìn tài xế, dẫn đến dễ sai sót và chậm trả thưởng. |
| 2 | **Xanh SM** | Tốn thời gian | Khi tài xế bị tạm dừng nhận chuyến, nhân viên phải tự tra GPS, cảm biến và log để xác minh; trong lúc tài xế chờ giải trình thì mất cuốc và có thể mất thưởng. |
| 3 | **Xanh SM** | AI-upgrade | Chính sách thưởng và ưu đãi sạc thay đổi thường xuyên, nằm rải rác trong các bài đăng nên tài xế khó nắm điều kiện và phải hỏi lại đội xe. |
| 4 | **Xanh SM** | Pain từ người khác | Điểm đón khách chọn trên bản đồ thường lệch vị trí thực tế (cổng, hẻm, tòa nhà lớn), khiến tài xế mất thời gian tìm khách, khách chờ lâu và dễ hủy chuyến. |
| 5 | **Xanh SM** | Pain từ người khác | Tài xế tự chọn trạm và giờ sạc theo kinh nghiệm nên phải xếp hàng chờ lâu ở giờ cao điểm, dẫn đến mất cuốc và giảm thu nhập. |

---

# 🃏 Phase 2 — QUICK-ASSESS (Cá nhân)

Chọn 3 bài toán tiềm năng nhất từ bảng trên. Lưu ý: các con số dưới đây là **giả định ước tính** để có metric kiểm thử, chưa được đối chiếu với số liệu thật của Xanh SM.

### 🃏 Quick Card #1 — Đối soát thưởng tài xế (Lens: Lặp lại)

- **Bài toán (1 câu):** Đối soát thủ công cuốc hoàn thành, doanh thu, ví và điều kiện thưởng của hàng nghìn tài xế mỗi ngày nên dễ sai sót và chậm trả thưởng.
- **Công ty thành viên:** Xanh SM
- **Ai đang đau (Actor):** Nhân viên vận doanh/đối soát; tài xế chờ thưởng.
- **Workflow thủ công hiện tại (3-5 bước):**
  1. Xuất dữ liệu cuốc hoàn thành trong ngày ──> 2. Khớp với doanh thu và số dư ví ──> 3. Kiểm tra từng tài xế có đạt điều kiện thưởng ──> 4. Duyệt và chuyển thưởng
- **Bước tốn thời gian/lỗi nhất:** Bước 3, phải đối chiếu nhiều nguồn và nhiều điều kiện thưởng khác nhau. (giả định ~3 giờ/ngày)
- **AI có thể nhảy vào hỗ trợ ở bước nào?** Không cần LLM. Điều kiện thưởng là quy tắc rõ ràng nên hợp với rule-based đối soát, AI chỉ có thể hỗ trợ giải thích các trường hợp lệch.
- **Metric thành công:** Thời gian đối soát từ ~3 giờ/ngày xuống dưới 30 phút/ngày; tỷ lệ sai lệch dưới 0,5% số tài xế (giả định, cần kiểm chứng).
- **Quick Architecture:** [ ] No AI  [x] Rule  [ ] LLM  [ ] Agent

### 🃏 Quick Card #2 — Xác minh khi tài xế bị tạm dừng nhận chuyến (Lens: Tốn thời gian)

- **Bài toán (1 câu):** Khi tài xế bị tạm dừng nhận chuyến, nhân viên mất nhiều thời gian tự tra GPS, cảm biến và log để xác minh, trong lúc đó tài xế mất cuốc và có thể mất thưởng.
- **Công ty thành viên:** Xanh SM
- **Ai đang đau (Actor):** Nhân viên hỗ trợ tài xế; tài xế bị tạm dừng.
- **Workflow thủ công hiện tại (3-5 bước):**
  1. Nhận báo tạm dừng ──> 2. Tra GPS hành trình ──> 3. Tra cảm biến và log hệ thống ──> 4. Tổng hợp và gửi giải trình ──> 5. Quyết định gỡ hoặc giữ tạm dừng
- **Bước tốn thời gian/lỗi nhất:** Bước 2-3, phải tự ghép timeline từ nhiều nguồn. (giả định ~35 phút; tính cả bước 4 là ~45 phút/trường hợp)
- **AI có thể nhảy vào hỗ trợ ở bước nào?** Bước 2-4: LLM tóm tắt timeline từ log đã được xuất sẵn và soạn nháp giải trình [DRAFT_ONLY]. Người vẫn là người quyết định ở bước 5.
- **Metric thành công:** Thời gian xác minh một trường hợp từ ~45 phút xuống dưới 10 phút; hơn 70% nháp giải trình được nhân viên duyệt mà không cần sửa lớn (giả định).
- **Quick Architecture:** [ ] No AI  [ ] Rule  [x] LLM  [ ] Agent

### 🃏 Quick Card #3 — Tra cứu chính sách thưởng và ưu đãi sạc (Lens: AI-upgrade)

- **Bài toán (1 câu):** Chính sách thưởng và ưu đãi sạc thay đổi thường xuyên, nằm rải rác trong nhiều bài đăng, khiến tài xế khó nắm điều kiện và phải hỏi lại đội xe.
- **Công ty thành viên:** Xanh SM
- **Ai đang đau (Actor):** Tài xế; đội xe phải trả lời lặp lại cùng một câu hỏi.
- **Workflow thủ công hiện tại (3-5 bước):**
  1. Tài xế nhớ hoặc tìm lại bài đăng chính sách ──> 2. Tự đọc và suy ra điều kiện ──> 3. Nhắn hỏi đội xe ──> 4. Chờ phản hồi
- **Bước tốn thời gian/lỗi nhất:** Bước 2-4, tài xế phải tự suy luận và chờ người trả lời. (giả định: nhiều câu hỏi lặp lại mỗi ngày)
- **AI có thể nhảy vào hỗ trợ ở bước nào?** Bước 2: LLM trả lời câu hỏi dựa trên kho chính sách đã được đội xe duyệt, có trích dẫn nguồn. Khi không chắc chắn hoặc câu hỏi nằm ngoài chính sách, chuyển cho người.
- **Metric thành công:** Ít nhất 80% câu hỏi chính sách được trả lời đúng và có trích dẫn trong 10 giây; số câu hỏi chuyển đến đội xe giảm 50% (giả định).
- **Quick Architecture:** [ ] No AI  [ ] Rule  [x] LLM  [ ] Agent
