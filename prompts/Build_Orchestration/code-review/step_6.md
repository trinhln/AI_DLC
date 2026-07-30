Hãy đọc:

1. Báo cáo audit ban đầu.
2. Remediation plan.
3. Decision register.
4. Kết quả Batch 1 và Batch 2 đã được user xác nhận.
5. Các targeted re-audit tương ứng.

Bây giờ thực hiện `Batch 3 — Steering activation`.

## Điều kiện bắt đầu

Chỉ thực hiện khi:

- Batch 1 và Batch 2 đã được user xác nhận.
- Không còn P0/P1 unresolved ảnh hưởng tới activation.
- Working tree không có thay đổi ngoài phạm vi đã biết.

Nếu chưa đáp ứng, dừng và báo lại.

## Mục tiêu

1. Mỗi steering được kích hoạt đúng với loại file cần áp dụng.
2. Không bỏ sót convention do `fileMatchPattern` quá hẹp hoặc sai.
3. Không nạp steering không liên quan do pattern quá rộng.
4. Core rule chỉ đặt `always` khi thực sự áp dụng cho mọi task.
5. Front matter hợp lệ và được Kiro nhận diện.
6. Tối ưu context và token nhưng không làm mất coverage.

## Phạm vi

Chỉ xử lý:

- `inclusion`.
- `fileMatchPattern`.
- YAML front matter.
- Activation gap đã được báo cáo.
- Scope mismatch liên quan trực tiếp đến loại file được match.

Không thay đổi:

- Nội dung coding convention.
- Mức độ `MUST`, `MUST NOT`, `SHOULD`, `MAY`.
- Source code.
- `.amazonq/rules/coding_conventions/`.
- Agent.
- Skill.
- Prompt review.
- Build hoặc test configuration.

## Nguyên tắc activation

### `inclusion: always`

Chỉ sử dụng cho rule:

- Áp dụng thực sự cho mọi task hoặc mọi loại source file.
- Là constraint nền tảng không được phép bỏ qua.
- Không phụ thuộc component, service, template, styling hoặc testing.

Không chuyển toàn bộ `angular-skills` thành `always`.

### `inclusion: fileMatch`

Sử dụng cho convention chuyên biệt theo:

- File extension.
- File suffix.
- Folder.
- Loại artifact.
- Combination của nhiều pattern liên quan.

Nếu một steering áp dụng cho nhiều loại file, dùng danh sách pattern rõ ràng thay vì pattern quá rộng.

## Trước khi chỉnh sửa

Tạo activation matrix:

| Steering file | Chủ đề | Inclusion hiện tại | Pattern hiện tại | Loại file cần áp dụng | Pattern đề xuất | Lý do |
| ------------- | ------ | ------------------ | ---------------- | --------------------- | --------------- | ----- |

Với mỗi file:

1. Đọc nội dung để xác định phạm vi convention.
2. Không suy luận phạm vi chỉ từ filename.
3. Kiểm tra front matter nằm ngay đầu file.
4. Kiểm tra cú pháp YAML.
5. Xác định các loại file thực sự cần match.
6. Xác định pattern có quá rộng hoặc quá hẹp không.

Có thể đọc cây thư mục và tên file của project để kiểm tra glob, nhưng không đọc nội dung source code để suy luận convention.

## Kiểm tra pattern

Đối với mỗi pattern đề xuất, tạo các representative path:

- Path phải match.
- Path không được match.
- File mới dự kiến sẽ được tạo.
- File hiện hữu.
- File test nếu convention có hoặc không áp dụng cho test.
- Template và style file nếu có liên quan.

Ví dụ kết quả:

| Pattern | Representative path | Expected | Actual |
| ------- | ------------------- | -------: | -----: |

Không thay đổi pattern nếu chưa chứng minh được hành vi match.

## Thực hiện

1. Chỉ chỉnh sửa front matter của các steering thuộc Batch 3.
2. Dùng pattern nhỏ nhất nhưng bao phủ đủ phạm vi.
3. Không sử dụng glob toàn project nếu rule chỉ áp dụng cho một artifact.
4. Không duplicate pattern không cần thiết.
5. Giữ nguyên phần body của steering.
6. Không đổi tên hoặc tổ chức lại file trong batch này.
7. Không thêm blank line hoặc content trước YAML front matter.

## File mới chưa tồn tại

`fileMatch` có thể không được kích hoạt trước khi file mới được tạo.

Vì vậy:

1. Không giải quyết vấn đề này bằng cách đổi tất cả steering thành `always`.
2. Ghi rõ đây là trách nhiệm của implementation agent:
   - Xác định đường dẫn file dự kiến.
   - Chọn steering có pattern tương ứng.
   - Chủ động đọc steering trước khi tạo file.

3. Batch này chỉ bảo đảm pattern mô tả đúng phạm vi cần áp dụng.

## Validation

Sau khi cập nhật:

1. Kiểm tra diff chỉ thay đổi front matter đã được phê duyệt.
2. Validate cú pháp YAML của mọi steering đã chỉnh sửa.
3. Chạy lại representative path matching.
4. Kiểm tra:
   - Không còn activation gap trong Batch 3.
   - Không tạo overlap không cần thiết.
   - Không tăng số lượng `always` nếu không có lý do.
   - Không làm mất coverage.
   - Body convention không bị thay đổi.

5. Thực hiện targeted re-audit riêng cho activation.
6. Không chạy unit test hoặc Angular build.

## Kết quả đầu ra

Cung cấp:

1. Activation matrix trước và sau.
2. Danh sách file đã thay đổi.
3. Diff để user review.
4. Kết quả glob matching.
5. Số activation gap đã xử lý.
6. Pattern còn cần `HUMAN_DECISION`, nếu có.
7. Ước tính steering nào sẽ được nạp cho từng nhóm:
   - Component TypeScript.
   - Template.
   - Style.
   - Service/API.
   - Model.
   - Routing.
   - Unit test.

8. Xác nhận body convention không bị thay đổi.
9. Chờ user xác nhận; không tự chuyển sang Batch 4.
