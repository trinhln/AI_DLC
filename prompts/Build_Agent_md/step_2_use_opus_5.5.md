# Nhiệm vụ: Independent Review – AGENTS.md

## 1. Vai trò

Bạn là Senior AI Agent Architect, chịu trách nhiệm đánh giá chất lượng cấu hình AI-assisted development trong một Angular enterprise project.

Một model khác đã phân tích repository và tạo file `AGENTS.md`.

Nhiệm vụ của bạn là thực hiện một cuộc review độc lập, khách quan và có tính phản biện đối với file này.

**Không mặc định rằng các quyết định hoặc hướng dẫn trong AGENTS.md đều chính xác.**

## 2. Mục tiêu review

Mục tiêu cuối cùng của AGENTS.md là cải thiện khả năng Kiro Default Agent chủ động nhận diện, lựa chọn và tuân thủ các quy chuẩn của project khi thực hiện task.

Tập trung vào các vấn đề:

- Default Agent có hiểu và tuân thủ Tech Stack không?
- Agent có chủ động xác định và đọc Steering phù hợp trước khi triển khai không?
- Agent có khả năng nhận diện và kích hoạt Skill phù hợp mà không cần người dùng chỉ định không?
- Agent có biết kết hợp nhiều Skill khi task yêu cầu không?
- Agent có thực sự tuân thủ hướng dẫn trong Skill và Steering trong quá trình generate code không?
- Agent có cơ chế kiểm tra tính tuân thủ trước khi hoàn thành task không?

## 3. Phạm vi kiểm tra

### 3.1. Review AGENTS.md

Đánh giá:

- Tính chính xác của các hướng dẫn.
- Tính rõ ràng và khả năng thực thi.
- Mức độ đầy đủ của các quy trình.
- Những nội dung mơ hồ hoặc có thể dẫn đến nhiều cách hiểu.
- Những hướng dẫn trùng lặp không cần thiết.
- Những yêu cầu vượt quá khả năng thực tế của Kiro Default Agent.
- Khả năng gây tiêu tốn context không cần thiết.

### 3.2. Đối chiếu với repository

Không chỉ review nội dung AGENTS.md một cách độc lập.

Hãy kiểm tra lại các nguồn quy chuẩn thực tế:

- Toàn bộ Steering files.
- Toàn bộ Skills và YAML frontmatter.
- Coding conventions.
- Tech Stack và các phiên bản dependency.
- Cấu trúc source code và implementation hiện tại.
- Các tài liệu hướng dẫn phát triển liên quan.

Xác minh rằng những gì AGENTS.md mô tả thực sự tồn tại trong repository.

Đặc biệt kiểm tra:

- Skill nào chưa được AGENTS.md đề cập hoặc định hướng kích hoạt.
- Steering nào có thể bị bỏ sót.
- Description của Skill có đủ rõ để Default Agent nhận diện đúng task không.
- Có sự mâu thuẫn giữa AGENTS.md và các quy chuẩn hiện có không.

### 3.3. Review khả năng tương thích với Kiro

Đánh giá các chỉ dẫn dựa trên cơ chế hoạt động thực tế của Kiro IDE và Kiro CLI.

Không giả định rằng chỉ cần viết một câu lệnh trong AGENTS.md là Kiro chắc chắn thực hiện được.

Phân biệt rõ:

- Khả năng được nạp vào context.
- Khả năng nhận diện tài liệu.
- Khả năng kích hoạt Skill.
- Khả năng tuân thủ chỉ dẫn trong quá trình thực thi.

Nếu một yêu cầu không được Kiro hỗ trợ trực tiếp hoặc không thể đảm bảo bằng prompt, hãy chỉ rõ giới hạn đó.

## 4. Nguyên tắc review

- Phải có bằng chứng từ repository cho mỗi finding quan trọng.
- Không đưa ra nhận xét chung chung.
- Không tự suy đoán các quy chuẩn không tồn tại.
- Không đề xuất tạo thêm nhiều tài liệu nếu chưa chứng minh được sự cần thiết.
- Không đánh giá AGENTS.md tốt chỉ vì nội dung có vẻ đầy đủ.
- Ưu tiên tính chính xác, khả năng thực thi và hiệu quả sử dụng context.
- Phân biệt lỗi thực sự với các đề xuất tối ưu không bắt buộc.

## 5. Định dạng báo cáo

Trình bày kết quả theo cấu trúc:

### A. Overall Assessment

Đánh giá tổng thể AGENTS.md và mức độ đáp ứng mục tiêu ban đầu.

### B. Critical Findings

Các vấn đề nghiêm trọng có thể khiến Default Agent tiếp tục bỏ qua Steering hoặc Skill.

Mỗi finding phải có:

- Mô tả vấn đề.
- Bằng chứng.
- Tác động.
- Đề xuất xử lý.

### C. Missing Rules

Những quy tắc hoặc quy trình quan trọng còn thiếu.

### D. Conflicts & Ambiguities

Các nội dung mâu thuẫn, trùng lặp hoặc chưa đủ rõ ràng.

### E. Kiro Compatibility

Những hướng dẫn không tương thích hoặc không được đảm bảo bởi cơ chế Kiro.

### F. Improvement Recommendations

Các đề xuất cải thiện, sắp xếp theo mức độ ưu tiên:

- P0: Cần sửa trước khi sử dụng.
- P1: Nên sửa để tăng độ tin cậy.
- P2: Tối ưu thêm.

### G. Final Verdict

Kết luận AGENTS.md:

- Ready: Có thể sử dụng.
- Ready with changes: Cần chỉnh sửa trước khi sử dụng.
- Not ready: Cần thiết kế lại.

## 6. Giới hạn hành động

Đây là nhiệm vụ REVIEW, không phải IMPLEMENTATION.

- Không chỉnh sửa AGENTS.md.
- Không chỉnh sửa Steering hoặc Skill.
- Không thay đổi source code.
- Không tự tạo thêm file.
- Chỉ trả về báo cáo review và các đề xuất có bằng chứng.

Kết thúc bằng danh sách các thay đổi cụ thể mà model chịu trách nhiệm xây dựng AGENTS.md cần thực hiện ở vòng tiếp theo.
