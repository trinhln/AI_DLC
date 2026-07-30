Hãy đọc báo cáo audit về sự đồng nhất giữa:

- `.amazonq/rules/coding_conventions/`
- `.kiro/steering/angular-skills/`

Bây giờ hãy tạo remediation plan để chuẩn hóa `steering/angular-skills`.

Đây chỉ là bước lập kế hoạch. Không được chỉnh sửa bất kỳ file nào.

## Mục tiêu cuối cùng

Sau quá trình remediation:

1. `.kiro/steering/angular-skills/` trở thành source of truth duy nhất về Angular coding convention của project.
2. Toàn bộ project-specific convention quan trọng được bảo toàn.
3. Các bổ sung hữu ích từ Angular official skills được giữ lại nếu tương thích Angular 16.
4. Không còn conflict, duplicate hoặc rule không thể kích hoạt.
5. Nội dung steering được tối ưu để hạn chế token.

## Thứ tự ưu tiên

Áp dụng thứ tự ưu tiên mặc định sau:

1. Quyết định đã được user xác nhận.
2. Coding convention đặc thù của project.
3. Constraint Angular 16 và dependency thực tế.
4. Angular official recommendation tương thích.
5. Source code hiện hữu không được dùng làm coding convention.

Không tự động giải quyết conflict nếu lựa chọn có thể thay đổi cách team viết code.

## Phân loại remediation

Phân loại từng finding trong báo cáo thành:

### `AUTO_SAFE`

Có thể sửa an toàn mà không thay đổi ý nghĩa convention, ví dụ:

- Lỗi front matter rõ ràng.
- Pattern sai cú pháp.
- Duplicate hoàn toàn.
- Typo.
- Link/reference hỏng.
- Hai rule có cùng ý nghĩa và có thể hợp nhất mà không mất thông tin.

### `HUMAN_DECISION`

Cần user quyết định, ví dụ:

- Hai nguồn đưa ra pattern khác nhau.
- Thay đổi mức độ `MUST`/`SHOULD`.
- Thay đổi phạm vi áp dụng.
- Bổ sung Angular official best practice chưa từng được team thống nhất.
- Loại bỏ một convention legacy nhưng vẫn đang được áp dụng.
- Có nhiều phương án `fileMatchPattern`.

### `NEEDS_VERIFICATION`

Chưa đủ bằng chứng, ví dụ:

- Không chắc API có được Angular 16 hỗ trợ không.
- Không rõ dependency hoặc cấu hình project.
- Không xác định được rule là intentional exception hay migration error.

## Prioritization

Gán priority:

- `P0`: Conflict hoặc incompatibility có thể sinh code sai/build fail.
- `P1`: Rule bắt buộc bị thiếu, strength mismatch hoặc activation gap nghiêm trọng.
- `P2`: Partial coverage, scope mismatch hoặc enhancement cần chuẩn hóa.
- `P3`: Duplicate, wording và tối ưu token.

## Remediation matrix

Tạo bảng:

| Finding ID | Priority | Category | File steering | Vấn đề | Nguồn gốc | Đề xuất | Loại quyết định | Rủi ro |
| ---------- | -------- | -------- | ------------- | ------ | --------- | ------- | --------------- | ------ |

Với mỗi finding `P0` và `P1`, phải cung cấp:

- Nội dung convention cũ.
- Nội dung steering hiện tại.
- Nội dung đề xuất sau remediation.
- Lý do đề xuất.
- Ảnh hưởng tới implement.
- Ảnh hưởng tới code review.
- Câu hỏi cụ thể cần user quyết định nếu có.

## Chia thành các batch

Tạo kế hoạch sửa theo các batch nhỏ:

### Batch 1 — Critical compatibility and conflicts

- Angular 16 incompatibility.
- Project convention conflicts.
- Các rule có thể làm code build fail hoặc sai kiến trúc.

### Batch 2 — Missing mandatory conventions

- Missing rule.
- Partial rule quan trọng.
- Strength mismatch.

### Batch 3 — Activation

- `inclusion`.
- `fileMatchPattern`.
- Phạm vi file.
- Core rule cần luôn được kích hoạt.

### Batch 4 — Consolidation

- Duplicate.
- Nội dung chồng chéo.
- Chuẩn hóa terminology.
- Tối ưu token.

### Batch 5 — Validation

- Kiểm tra lại coverage.
- Kiểm tra conflict.
- Kiểm tra Angular 16 compatibility.
- Kiểm tra activation.
- Tạo báo cáo so sánh sau remediation.

Mỗi batch phải có:

- Danh sách file dự kiến sửa.
- Danh sách finding được xử lý.
- Điều kiện hoàn thành.
- Cách rollback hoặc đối chiếu với bản cũ.

## Yêu cầu đầu ra

Chỉ tạo remediation plan, không sửa steering.

Cuối cùng cung cấp:

1. Executive summary.
2. Số lượng finding theo `P0`–`P3`.
3. Danh sách `AUTO_SAFE`.
4. Danh sách `HUMAN_DECISION`.
5. Danh sách `NEEDS_VERIFICATION`.
6. Các câu hỏi cần tôi quyết định, sắp xếp theo priority.
7. Kế hoạch các batch.
8. Đề xuất batch nên thực hiện đầu tiên.
