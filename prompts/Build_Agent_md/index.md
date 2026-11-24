# Nhiệm vụ: Phân tích repository và xây dựng AGENTS.md cho NBE

## 1. Mục tiêu

Phân tích toàn bộ cấu hình AI-assisted development hiện có trong repository, bao gồm:

- Steering files.
- Skills.
- Custom Agents (nếu có).
- Coding conventions và các tài liệu hướng dẫn phát triển.
- Cấu trúc source code và các quy chuẩn kiến trúc thực tế.

Dựa trên kết quả phân tích, tự xây dựng file `AGENTS.md` tại project root nhằm giúp Kiro Default Agent hiểu và tuân thủ các quy chuẩn của NBE trong quá trình triển khai task.

## 2. Nội dung cần phân tích

### 2.1. Project standards

- Xác định Tech Stack thực tế và các phiên bản đang sử dụng.
- Xác định kiến trúc và cấu trúc repository.
- Xác định coding conventions và các nguyên tắc phát triển bắt buộc.
- Xác định các giới hạn kỹ thuật mà code mới phải tuân thủ.

### 2.2. Steering

- Liệt kê toàn bộ Steering hiện có.
- Phân tích phạm vi và điều kiện áp dụng của từng file.
- Xác định Steering nào cần được áp dụng cho mọi task.
- Xác định Steering nào chỉ áp dụng theo từng phạm vi công việc.
- Kiểm tra các quy định trùng lặp hoặc mâu thuẫn.

### 2.3. Skills

- Liệt kê toàn bộ Skill hiện có.
- Phân tích mục đích, phạm vi và điều kiện kích hoạt của từng Skill.
- Xác định các tình huống mà Default Agent cần chủ động lựa chọn Skill.
- Xác định các Skill có thể cần kết hợp trong cùng một task.
- Đánh giá khả năng nhận diện Skill dựa trên description hiện tại.

## 3. Yêu cầu xây dựng AGENTS.md

AGENTS.md phải đóng vai trò là Global Execution Policy dành cho Default Agent.

Nội dung cần hướng dẫn Agent:

1. Luôn xác định và tuân thủ Tech Stack của project.
2. Phân loại task trước khi triển khai.
3. Chủ động xác định và đọc các Steering liên quan.
4. Chủ động tìm kiếm và kích hoạt các Skill phù hợp.
5. Không bỏ qua Skill phù hợp chỉ vì người dùng không chỉ định.
6. Kết hợp nhiều Skill khi task yêu cầu nhiều workflow.
7. Kiểm tra các implementation hiện có trước khi lựa chọn cách triển khai.
8. Kiểm tra tính tuân thủ quy chuẩn trước khi hoàn thành task.

Không sao chép toàn bộ nội dung Steering hoặc Skill vào AGENTS.md. Chỉ định nghĩa nguyên tắc điều phối và tham chiếu đến các tài liệu hiện có.

## 4. Nguyên tắc thực hiện

- Phải phân tích repository thực tế, không suy đoán từ tên file.
- Không tự ý sửa đổi hoặc tạo thêm Steering, Skill, Agent khác.
- Không thay đổi source code.
- Không tạo ra các quy chuẩn mới trái với convention hiện tại.
- Không biến AGENTS.md thành tài liệu mô tả toàn bộ project.
- Ưu tiên hướng dẫn ngắn gọn, rõ ràng, có tính bắt buộc và có thể thực thi.
- Nếu phát hiện thiếu thông tin hoặc có mâu thuẫn, ghi nhận và giải thích thay vì tự đưa ra quyết định.

## 5. Kết quả đầu ra

Tạo duy nhất file `AGENTS.md` tại project root.

Sau khi tạo, báo cáo:

- Những tài liệu và quy chuẩn quan trọng đã phân tích.
- Cách AGENTS.md điều phối Steering và Skills.
- Những vấn đề hoặc mâu thuẫn phát hiện được.
- Những nội dung chưa đủ cơ sở để đưa vào AGENTS.md.

Không tự động chỉnh sửa các file khác.
