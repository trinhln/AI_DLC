# Nhiệm vụ: Final Revision – AGENTS.md

## 1. Vai trò

Bạn là Senior AI Agent Architect, chịu trách nhiệm hoàn thiện cấu hình AI-assisted development cho một Angular enterprise project.

Trước đó:

- GPT-5.6 Sol đã phân tích repository và xây dựng AGENTS.md.
- Claude Opus 5.5 đã thực hiện Independent Review và đưa ra báo cáo findings, các vấn đề và đề xuất cải thiện.

Nhiệm vụ hiện tại là tiếp nhận kết quả review, kiểm chứng và hoàn thiện AGENTS.md.

## 2. Mục tiêu

Xây dựng phiên bản AGENTS.md cuối cùng nhằm cải thiện khả năng Kiro Default Agent:

1. Hiểu và tuân thủ chính xác Tech Stack của project.
2. Chủ động xác định và áp dụng các Steering phù hợp.
3. Chủ động nhận diện và kích hoạt các Skill cần thiết.
4. Kết hợp nhiều Skill khi task yêu cầu.
5. Tuân thủ các quy chuẩn trong suốt quá trình triển khai.
6. Tự kiểm tra tính tuân thủ trước khi hoàn thành task.

AGENTS.md phải hoạt động như Global Execution Policy dành cho Default Agent trên cả Kiro IDE và Kiro CLI.

## 3. Xử lý kết quả Independent Review

Đọc đầy đủ báo cáo review của Claude Opus 5.5.

Đối với từng finding:

- Kiểm tra lại bằng chứng trong repository.
- Xác minh vấn đề có thực sự tồn tại không.
- Đánh giá mức độ ảnh hưởng đến mục tiêu của AGENTS.md.
- Quyết định có cần sửa hay không.
- Không áp dụng máy móc các đề xuất chỉ mang tính tối ưu hoặc sở thích cá nhân.

Phân loại:

- Accepted: Finding chính xác và cần xử lý.
- Partially Accepted: Chỉ một phần nhận xét hoặc đề xuất phù hợp.
- Rejected: Finding không chính xác hoặc không có đủ bằng chứng.

Mọi finding bị Rejected phải có giải thích rõ ràng.

## 4. Nguyên tắc hoàn thiện AGENTS.md

### 4.1. Tính chính xác

- Mọi quy chuẩn phải có căn cứ từ repository.
- Không tạo ra Tech Stack, architecture hoặc convention mới.
- Không đưa vào những yêu cầu mà Kiro không hỗ trợ hoặc không thể thực hiện đáng tin cậy.

### 4.2. Khả năng điều phối

AGENTS.md phải hướng dẫn Default Agent thực hiện quy trình trước khi triển khai:

1. Phân tích và phân loại task.
2. Xác định Steering liên quan.
3. Đọc và áp dụng các Steering cần thiết.
4. Tìm kiếm và kích hoạt Skill phù hợp.
5. Đọc đầy đủ hướng dẫn Skill trước khi thực hiện workflow tương ứng.
6. Triển khai theo các quy chuẩn đã xác định.
7. Kiểm tra tính tuân thủ trước khi hoàn thành.

Không được bỏ qua các bước này chỉ vì người dùng không chỉ định Steering hoặc Skill.

### 4.3. Tính hiệu quả

- Không sao chép nội dung của Steering và Skill.
- Không biến AGENTS.md thành tài liệu mô tả toàn bộ project.
- Tránh các chỉ dẫn trùng lặp.
- Ưu tiên hướng dẫn ngắn gọn, rõ ràng, có tính bắt buộc.
- Không làm context nặng không cần thiết.
- Không sử dụng các câu lệnh chung chung như "follow project conventions" nếu không hướng dẫn Agent cách tìm và áp dụng các quy chuẩn đó.

### 4.4. Khả năng tương thích

Kiểm tra lại tính tương thích với cơ chế hoạt động thực tế của Kiro Default Agent trên IDE và CLI.

Phân biệt rõ:

- Instructions được load.
- Tài liệu được nhận diện.
- Skill được kích hoạt.
- Hướng dẫn thực sự được tuân thủ.

Không tuyên bố AGENTS.md đảm bảo tuyệt đối việc kích hoạt hoặc tuân thủ nếu cơ chế Kiro không hỗ trợ điều đó.

## 5. Kiểm tra sau khi hoàn thiện

Sau khi chỉnh sửa, tự review lại AGENTS.md theo các tiêu chí:

- Không còn Critical Findings chưa được xử lý.
- Không có quy định mâu thuẫn với Steering và Skill.
- Không bỏ sót các quy chuẩn quan trọng.
- Không có hướng dẫn không khả thi.
- Quy trình lựa chọn Steering và Skill đủ rõ ràng.
- Không chứa các nội dung dư thừa không phục vụ mục tiêu.

Nếu phát hiện vấn đề mới trong quá trình chỉnh sửa, tiếp tục xử lý trước khi hoàn thành.

## 6. Phạm vi thay đổi

- Chỉ chỉnh sửa file AGENTS.md tại project root.
- Không thay đổi source code.
- Không sửa đổi Steering.
- Không sửa đổi Skill.
- Không tạo thêm Agent hoặc tài liệu điều phối khác.

Nếu phát hiện vấn đề thuộc các file khác, ghi nhận trong báo cáo thay vì tự ý chỉnh sửa.

## 7. Kết quả đầu ra

Hoàn thành hai phần:

### A. Final AGENTS.md

Cập nhật trực tiếp file AGENTS.md tại project root.

### B. Revision Report

Báo cáo:

1. Tổng số findings đã review.
2. Số lượng Accepted, Partially Accepted và Rejected.
3. Những thay đổi quan trọng trong AGENTS.md.
4. Các vấn đề chưa thể giải quyết bằng AGENTS.md.
5. Những giới hạn còn tồn tại của Kiro.
6. Kết quả self-review sau khi hoàn thiện.

Không kết luận rằng AGENTS.md đã giải quyết triệt để vấn đề nếu chưa có kiểm chứng thực tế.
