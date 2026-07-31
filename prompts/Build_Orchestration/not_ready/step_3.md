Tiếp tục remediation dựa trên báo cáo readiness gần nhất đang có trạng thái `NOT_READY`.

## Mục tiêu

Chỉ xử lý **một blocker ưu tiên cao nhất** trong lần này. Không xử lý lại toàn bộ báo cáo và không mở rộng sang các vấn đề không trực tiếp liên quan.

`.kiro/steering/angular-skills` là source of truth sau remediation.

## Bước 1 — Chọn blocker

1. Đọc báo cáo readiness gần nhất.
2. Liệt kê ngắn gọn các blocker chưa được giải quyết.
3. Sắp xếp blocker theo thứ tự ưu tiên:
   - Steering không được kích hoạt hoặc không được đọc đúng.
   - Rule mâu thuẫn hoặc không có precedence rõ ràng.
   - Thiếu convention bắt buộc.
   - Metadata, inclusion hoặc `fileMatchPattern` không chính xác.
   - Trùng lặp hoặc vấn đề tối ưu token.

4. Chọn đúng một blocker có mức ưu tiên cao nhất.
5. Nếu nhiều blocker thực chất có cùng một root cause và bắt buộc phải sửa cùng nhau, có thể gom chúng thành một remediation unit nhỏ; phải giải thích rõ lý do.

## Bước 2 — Phân tích root cause

Với blocker được chọn, xác định:

- Blocker ID hoặc mô tả tương ứng trong báo cáo.
- File và section liên quan.
- Expected behavior.
- Actual behavior.
- Root cause.
- Vì sao remediation trước đó chưa giải quyết được.
- Acceptance criteria cụ thể để blocker được coi là resolved.

Không thay đổi file trong bước này nếu vẫn chưa xác định chắc chắn root cause.

Nếu blocker cần quyết định nghiệp vụ hoặc có nhiều solution ảnh hưởng khác nhau, hãy dừng và hỏi tôi trước khi implement.

## Bước 3 — Implement remediation nhỏ nhất

Sau khi root cause đã rõ:

1. Chỉ chỉnh sửa những file trực tiếp cần thiết để giải quyết blocker.
2. Không chỉnh sửa source code của application.
3. Không xử lý thêm blocker khác.
4. Không đưa convention cũ từ `.amazonq/rules/coding_conventions` trở lại nếu convention đó đã bị thay thế hoặc loại bỏ có chủ đích.
5. Không scan source code để suy luận convention; steering đã được phê duyệt là source of truth.
6. Không duplicate nguyên bộ rule sang nhiều steering.
7. Giữ nguyên các quyết định đã được phê duyệt trong những remediation batch trước.
8. Không tạo skill hoặc review agent tại bước này.

## Bước 4 — Targeted validation

Chỉ chạy validation cần thiết cho blocker vừa sửa:

- Kiểm tra syntax và metadata của các file thay đổi.
- Kiểm tra `inclusion` và `fileMatchPattern` nếu có liên quan.
- Kiểm tra rule conflict hoặc precedence nếu có liên quan.
- Kiểm tra convention còn thiếu nếu có liên quan.
- Mô phỏng một tình huống kích hoạt nhỏ nếu blocker liên quan đến việc load steering.

Không chạy lại toàn bộ readiness audit trong bước này.

## Kết quả bắt buộc

Báo cáo theo cấu trúc:

### Selected blocker

- Blocker ID:
- Priority:
- Root cause:
- Files affected:

### Changes

- File:
- Thay đổi:
- Lý do:

### Targeted validation

- Check:
- Result:
- Evidence:

### Status

Chỉ trả về một trong các trạng thái:

- `BLOCKER_RESOLVED`
- `BLOCKER_PARTIALLY_RESOLVED`
- `BLOCKED_BY_DECISION`
- `BLOCKER_NOT_RESOLVED`

Nếu chưa resolved, nêu chính xác phần còn thiếu. Không tự động chuyển sang blocker tiếp theo.

Nếu đã `BLOCKER_RESOLVED`, hãy dừng lại để tôi kiểm tra trước khi xử lý blocker kế tiếp.
