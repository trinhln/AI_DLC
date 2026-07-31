Tiếp tục remediation từ báo cáo readiness `NOT_READY` và kết quả xử lý blocker gần nhất.

## Mục tiêu

Xử lý blocker chưa hoàn thành có mức ưu tiên cao nhất tiếp theo. Chỉ thực hiện một remediation unit nhỏ trong lần này.

## Trước khi thực hiện

1. Đọc báo cáo readiness gần nhất.
2. Đọc kết quả các remediation unit đã hoàn thành.
3. Không chọn lại blocker đã có trạng thái `BLOCKER_RESOLVED`.
4. Xác định các blocker vẫn còn mở.
5. Chọn blocker có độ ưu tiên cao nhất theo thứ tự:
   - Steering không được kích hoạt hoặc đọc đúng.
   - Rule mâu thuẫn hoặc precedence không rõ ràng.
   - Thiếu convention bắt buộc.
   - Metadata, `inclusion` hoặc `fileMatchPattern` không chính xác.
   - Trùng lặp hoặc vấn đề tối ưu token.

Nếu nhiều blocker có cùng root cause và không thể xử lý riêng, được phép gom thành một remediation unit nhỏ, nhưng phải giải thích rõ.

## Phân tích blocker

Trước khi chỉnh sửa, báo cáo:

- Blocker ID.
- Mức độ ưu tiên.
- File và section liên quan.
- Expected behavior.
- Actual behavior.
- Root cause.
- Acceptance criteria để blocker được coi là resolved.
- Những file dự kiến thay đổi.

Nếu cần quyết định về convention hoặc có nhiều hướng xử lý làm thay đổi ý nghĩa rule, hãy dừng và hỏi tôi.

## Implement

Sau khi root cause đã rõ:

1. Chỉ sửa các file trực tiếp liên quan đến blocker.
2. `.kiro/steering/angular-skills` tiếp tục là source of truth.
3. Không suy luận convention từ source code của application.
4. Không sửa application source code.
5. Không đưa lại rule cũ đã bị loại bỏ có chủ đích.
6. Không duplicate convention giữa nhiều steering.
7. Không làm thay đổi các quyết định đã được phê duyệt.
8. Không xử lý thêm blocker khác.
9. Không tạo skill, prompt review hoặc custom agent ở bước này.

## Targeted validation

Sau khi sửa, chỉ kiểm tra những nội dung liên quan trực tiếp:

- Syntax và metadata của file thay đổi.
- `inclusion` và `fileMatchPattern`, nếu có liên quan.
- Conflict và precedence, nếu có liên quan.
- Coverage của convention, nếu có liên quan.
- Một tình huống kích hoạt nhỏ, nếu blocker liên quan đến việc load steering.
- Không phát sinh regression với các blocker đã được resolve.

Không chạy lại full readiness audit.

## Kết quả

### Selected blocker

- Blocker ID:
- Priority:
- Root cause:
- Acceptance criteria:

### Changes

- File:
- Thay đổi:
- Lý do:

### Targeted validation

- Check:
- Result:
- Evidence:

### Regression check

- Các blocker đã resolve có bị ảnh hưởng không:
- Kết quả:

### Status

Chỉ trả về một trạng thái:

- `BLOCKER_RESOLVED`
- `BLOCKER_PARTIALLY_RESOLVED`
- `BLOCKED_BY_DECISION`
- `BLOCKER_NOT_RESOLVED`

Nếu trả về `BLOCKER_RESOLVED`, hãy liệt kê số blocker còn mở và dừng lại. Không tự động xử lý blocker tiếp theo.
