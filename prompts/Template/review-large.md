Hãy sử dụng Bitbucket MCP đã được kết nối để thực hiện review toàn bộ code changes của PR mà tôi cung cấp.

## Mục tiêu

Đọc và đánh giá **toàn bộ các file có thay đổi trên PR**, không chỉ một phần nhỏ. Phân tích chất lượng code, lỗi tiềm ẩn và ảnh hưởng của các thay đổi đến hệ thống NBE, đặc biệt đối với PR nâng cấp Angular 22 có khối lượng thay đổi lớn.

## Quy trình bắt buộc

### 1. Lấy danh sách đầy đủ các file thay đổi

- Sử dụng các công cụ Bitbucket MCP hiện có để xác định PR, source branch, target branch và danh sách đầy đủ các file thay đổi.
- Xác định tất cả file được thêm mới, chỉnh sửa, xóa hoặc đổi tên.
- Ghi nhận tổng số file thay đổi để làm cơ sở kiểm soát coverage.
- Không chỉ sử dụng một phần diff được trả về từ lần gọi MCP đầu tiên nếu kết quả đó chưa bao gồm toàn bộ PR.

### 2. Đọc toàn bộ diff của từng file

- Sử dụng Bitbucket MCP để lấy và đọc diff của từng file thay đổi.
- Nếu một lần gọi MCP không trả về toàn bộ diff, tiếp tục truy xuất theo từng file hoặc từng nhóm nhỏ bằng các công cụ MCP phù hợp hiện có.
- Không bỏ qua file vì diff quá dài, nội dung bị cắt ngắn hoặc PR có quá nhiều thay đổi.
- Với file bị xóa hoặc đổi tên, đánh giá tác động tương ứng.
- Với file không thể lấy đầy đủ nội dung diff, thử phương án truy xuất khác nếu có. Nếu vẫn không thể, ghi rõ file đó chưa được review đầy đủ.

### 3. Review code của từng file

Đối với mỗi file, phân tích các vấn đề phù hợp với nội dung thay đổi:

- Lỗi logic, bug và hành vi không mong muốn.
- Breaking changes và nguy cơ regression.
- Tương thích với Angular 22, TypeScript và các dependency liên quan.
- Thay đổi về API, routing, dependency injection, lifecycle, RxJS và cấu hình build nếu có liên quan.
- Ảnh hưởng đến component, service, shared code, module và các luồng nghiệp vụ phụ thuộc.
- Vấn đề về bảo mật, hiệu năng và khả năng bảo trì nếu có bằng chứng cụ thể.

Không chỉ kiểm tra các dòng thêm mới. Cần đọc ngữ cảnh liên quan để xác định thay đổi có phá vỡ hành vi hiện có hay không.

### 4. Chia nhỏ công việc khi PR quá lớn

Nếu toàn bộ PR không thể được xử lý trong một lượt:

- Chia danh sách file thành các nhóm nhỏ và review lần lượt.
- Theo dõi những file đã review và chưa review.
- Tiếp tục các lượt truy xuất MCP cho đến khi toàn bộ file trong phạm vi PR đã được xử lý hoặc đã xác định rõ là không thể xử lý.
- Không kết thúc sớm chỉ vì đã tìm thấy một số lỗi hoặc đã có đủ nội dung cho một báo cáo sơ bộ.
- Sau khi review từng nhóm, kiểm tra thêm ảnh hưởng chéo giữa các nhóm.

### 5. Kiểm soát độ bao phủ

Trước khi kết luận, đối chiếu danh sách file ban đầu với kết quả review.

Báo cáo chính xác:

- Tổng số file thay đổi trên PR.
- Số file đã đọc đầy đủ diff.
- Số file đã review.
- Số file chưa review hoặc không thể lấy đầy đủ diff.
- Những giới hạn thực tế của dữ liệu MCP.

Không được tuyên bố đã review toàn bộ PR nếu chưa có bằng chứng đối chiếu. Không được đánh dấu một file là đã review chỉ vì đã đọc tên file hoặc metadata.

## Định dạng báo cáo

### A. Coverage summary

Thống kê tổng số file và trạng thái review, kèm danh sách các file còn thiếu nếu có.

### B. Findings

Với mỗi vấn đề được phát hiện, cung cấp:

- Mức độ nghiêm trọng: Critical / High / Medium / Low.
- File và vị trí code cụ thể.
- Bằng chứng từ diff hoặc code liên quan.
- Mô tả vấn đề và điều kiện phát sinh.
- Ảnh hưởng đến hệ thống.
- Hướng xử lý đề xuất.

Chỉ báo cáo các vấn đề có cơ sở; phân biệt lỗi đã xác minh với rủi ro cần điều tra thêm.

### C. Đánh giá ảnh hưởng hệ thống

Tổng hợp các thành phần và luồng nghiệp vụ bị ảnh hưởng trực tiếp hoặc gián tiếp, đặc biệt là các dependency dùng chung.

### D. Regression test đề xuất

Liệt kê các chức năng và kịch bản cần kiểm thử dựa trên những thay đổi thực tế.

### E. Kết luận

Nêu các vấn đề quan trọng cần xử lý trước khi merge và trạng thái hoàn tất của review.

## Nguyên tắc thực thi

- Thực hiện review ngay bằng các công cụ Bitbucket MCP hiện có; không chỉ đưa ra hướng dẫn hoặc kế hoạch.
- Không sửa source code, không tạo commit và không thay đổi cấu hình dự án.
- Không tự giả định tên công cụ MCP hoặc khả năng của công cụ khi chưa xác minh.
- Nếu gặp giới hạn truy xuất, phải nêu rõ giới hạn và tiếp tục bằng phương án khả thi khác.
- Ưu tiên tính đầy đủ, độ chính xác và bằng chứng hơn tốc độ.
- Không bỏ qua các file khó đọc hoặc diff lớn mà không ghi nhận rõ trạng thái.

**Bắt đầu bằng việc xác định PR và kiểm kê toàn bộ file thay đổi, sau đó lần lượt lấy diff, review code và đối chiếu coverage trước khi tạo báo cáo cuối cùng.**
