# BÁO CÁO CHIẾN LƯỢC THU THẬP YÊU CẦU & MÔ TẢ QUY TRÌNH TO-BE CHO TELEMEDICINE
**Khóa học:** Phân tích & Thiết kế Hệ thống (IT105)  
**Session:** 03 - Lựa chọn Chiến lược Thu thập Yêu cầu & Mô tả Quy trình TO-BE cho Telemedicine  
**Vai trò thực hiện:** Lead System Analyst (Lead SA)

---

## PHẦN 1: LỰA CHỌN KỸ THUẬT THU THẬP YÊU CẦU PHÙ HỢP NHẤT

### 1. Hai Kỹ thuật Lựa chọn Phù hợp nhất
Để phát hiện và làm rõ khoảng chênh lệch giữa **22 phút (thực tế hiện trường)** so với **7 phút (báo cáo lý thuyết)** trong mỗi ca khám Telemedicine, 2 kỹ thuật phù hợp nhất là:

1. **Quan sát hiện trường (Observation / Job Shadowing):** Quan sát trực tiếp các phiên tư vấn Video thực tế giữa Bác sĩ và Bệnh nhân mà không can thiệp vào tiến trình khám.
2. **Phỏng vấn sâu (In-depth Interview):** Phỏng vấn 1-1 với Bác sĩ điều trị và Điều dưỡng ngay sau phiên khám để đào sâu nguyên nhân gốc rễ (Root Cause Analysis).

---

### 2. Biện luận chi tiết về lựa chọn Kỹ thuật

- **Vì sao chọn Quan sát hiện trường & Phỏng vấn sâu:**
  - *Quan sát hiện trường* là kỹ thuật duy nhất giúp ghi nhận **các thao tác ngầm và thời gian trễ thực tế** (như thời gian app giật lag, thời gian bác sĩ loay hoay tìm bệnh án cũ, thời gian hướng dẫn bệnh nhân lớn tuổi bật micro) — những dữ liệu không bao giờ xuất hiện trên báo cáo lý thuyết hay tài liệu quy trình.
  - *Phỏng vấn sâu* cho phép SA đào sâu bản chất nguyên nhân rào cản qua các câu hỏi "Tại sao?", giúp bác sĩ giãi bày những ức chế về mặt công nghệ (UI phức tạp, nút bấm giấu sâu, thao tác thừa) mà họ không thể phản ánh qua báo cáo hành chính.

- **Vì sao KHÔNG chọn Phân tích tài liệu & Khảo sát diện rộng trong tình huống này:**
  - *Phân tích tài liệu* hoàn toàn thất bại trong tình huống này vì tài liệu/báo cáo lý thuyết chính là nguồn dữ liệu sai lệch (chỉ ghi nhận 7 phút/ca), không phản ánh được diễn biến thực tế tại hiện trường.
  - *Khảo sát diện rộng* chỉ mang lại các con số thống kê tổng quan (như % bác sĩ thấy chậm), nhưng không thể chỉ ra chính xác thao tác hay bước nghiệp vụ nào trong phiên khám gây lãng phí thời gian (không thể phát hiện lỗi do micro hay lỗi do tra cứu bệnh án).

---

## PHẦN 2: ĐẶC TẢ YÊU CẦU HỆ THỐNG TELEMEDICINE

### 1. Hai Câu hỏi Phỏng vấn Sâu "Tại sao?" cho Bác sĩ Điều trị

1. **Câu hỏi 1 (Về rào cản tra cứu dữ liệu bệnh án cũ):**  
   *"Tại sao Bác sĩ phải mất trung bình hơn 5 phút trong phiên khám tư vấn video để tra cứu lại lịch sử bệnh án và đơn thuốc cũ của bệnh nhân trên hệ thống?"*  
   *(Mục đích: Tìm nguyên nhân gốc rễ về thiết kế giao diện màn hình tư vấn video đang bị tách rời khỏi phân hệ EHR/Lịch sử khám, forcing Bác sĩ phải chuyển tab hoặc mở nhiều cửa sổ ứng dụng).*

2. **Câu hỏi 2 (Về sự cố kết nối và thao tác hỗ trợ người dùng):**  
   *"Tại sao Bác sĩ gặp khó khăn và lúng túng trong việc hỗ trợ các bệnh nhân lớn tuổi bật micro/camera hoặc kết nối lại khi tín hiệu mạng bị chập chờn trong phiên khám?"*  
   *(Mục đích: Khai thác nguyên nhân UX ứng dụng phía bệnh nhân thiếu tính năng trợ năng/hướng dẫn trực quan, và hệ thống chưa có nút bấm hỗ trợ nhanh từ phía Bác sĩ).*

---

### 2. Đặc tả Yêu cầu FR và NFR Xử lý Bẫy Sự cố Đường truyền (Connection Interruption Trap)

#### A. Yêu cầu Chức năng (Functional Requirement - FR)
- **Mã yêu cầu:** `FR-TELE-01` (Tự động Lưu bản nháp & Khôi phục Phiên khám Khẩn cấp)
- **Nội dung đặc tả:**  
  *"Khi xảy ra sự cố mất kết nối mạng Internet trong lúc Bác sĩ đang thực hiện phiên tư vấn Telemedicine, hệ thống phải tự động lưu tạm (Auto-draft) toàn bộ ghi chú chẩn đoán, dữ liệu sinh hiệu và đơn thuốc đang kê vào bộ nhớ đệm an toàn; đồng thời khi kết nối Internet được khôi phục, hệ thống phải hiển thị thông báo cho phép Bác sĩ bấm **'Khôi phục phiên làm việc' (Resume Session)** để nạp lại đầy đủ dữ liệu nháp mà không bị mất dữ liệu đã nhập trước đó."*

#### B. Yêu cầu Phi chức năng (Non-Functional Requirement - NFR)
- **Mã yêu cầu:** `NFR-TELE-01` (Thời gian Lưu đệm & Phôi phục Trạng thái Kết nối)
- **Nội dung đặc tả:**  
  *"Hệ thống Telemedicine phải duy trì cơ chế lưu bản nháp tự động với tần suất **$\le 3.0$ giây/lần** (Real-time Auto-save) và đảm bảo thời gian khôi phục toàn bộ trạng thái phiên làm việc (bao gồm cả dữ liệu ghi chú và luồng Video Call) trong thời gian **$\le 2.0$ giây** ngay khi thiết bị của Bác sĩ/Bệnh nhân kết nối lại mạng Internet thành công."*
