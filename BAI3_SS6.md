
# Prompt Chaining: Feynman + Rubric

## 1. Thiết kế
Chuỗi: Tóm tắt -> Feynman -> Chấm Rubric -> Sửa lỗi. Mỗi prompt 1 nhiệm vụ, output trước chỉ lấy lõi để đưa sang bước sau, gặp input lỗi thì trả ERROR và dừng, không bịa.

## 2. Prompt thực thi

**P1 - Tóm tắt:**
> Tóm tắt tài liệu sau thành 5 ý chính, chỉ dùng gạch đầu dòng "-". Cấm chào hỏi, kết luận. Nếu tài liệu trống/lỗi: Trả về "ERROR: DỮ LIỆU KHÔNG HỢP LỆ" và dừng. Tài liệu: """[Dán vào đây]"""

**P2 - Feynman Gia sư:**
> Bạn là gia sư Socratic. Dựa vào 5 ý ở P1, chỉ đặt 1 câu hỏi để tôi tự giải thích lại như dạy cho trẻ 12 tuổi. Nếu tôi trả lời sai/trống: Gợi ý 1 ví dụ đời thường và hỏi lại, không đưa đáp án.

**P3 - Rubric chấm bài:**
> Chấm bài sau theo Rubric: Correctness 40%, Security 20%, Code Quality 20%, Documentation 20%. Cho điểm từng tiêu chí + tổng /10. Không nịnh hót. Nếu bài trống: 0 điểm + "ERROR: BÀI TRỐNG". Bài làm: """[Dán bài vào đây]"""

**P4 - Sửa lỗi:**
> Dựa vào kết quả P3, chỉ liệt kê lỗi theo mẫu: - Lỗi: [dòng nào] - Nguyên nhân: [vì sao] - Cách sửa: [code cụ thể]. Cấm khen chung chung.

## 3. Kết quả chạy thử
**Input:** Bài có lỗi `age = int(input_str)`
**Output P3:** Correctness 30/40, Security 0/20, Code Quality 15/20, Documentation 5/20. Tổng 5.0/10.
**Output P4:**
- Lỗi: Dòng `age = int(input_str)` không bắt ngoại lệ
- Nguyên nhân: Nhập chữ gây ValueError crash app
- Cách sửa: Dùng try-except hoặc kiểm tra isdigit() trước khi ép kiểu

**Kết luận:** Chaining tốt hơn 1 prompt dài vì dễ kiểm soát, tiết kiệm token, chống crash, đầu ra sạch và có điểm + cách sửa cụ thể.
